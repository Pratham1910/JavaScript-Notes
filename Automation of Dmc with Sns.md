
Of course. Automating the assignment of S1000D Data Module Codes (DMCs) is an excellent use case for a Large Language Model (LLM). It involves understanding the context of a technical document and mapping it to a structured coding system.

Here is a comprehensive approach using Python to achieve your goal. This solution includes:
1.  **Parsing S1000D Standards**: Loading the SNS and Information Code definitions you provided.
2.  **Document Content Extraction**: Reading the text from `.docx` files.
3.  **LLM-Powered Code Assignment**: A primary method that uses your local Ollama LLM to analyze the text and suggest DMC codes.
4.  **Keyword-Based Fallback**: A secondary method that attempts to find codes based on keyword matching if the LLM fails.
5.  **Logging**: Detailed logging of both successful and failed attempts.

### Prerequisites

1.  **Ollama Installation**: Ensure your Ollama service is running. You can verify this by opening a terminal and running `ollama list` to see your available models. This code assumes the service is available at `http://localhost:11434`.
2.  **Python Environment**: You need Python installed, along with a few libraries. You can install them using pip:
    ```bash
    pip install python-docx requests beautifulsoup4
    ```
3.  **Project Structure**: For this to work, organize your files as follows:

    ```
    dmc_automation/
    |-- s1000d_data/
    |   |-- sns_generic.xml
    |   |-- sns_sea_vehicles.xml
    |   |-- sns_support_training.xml
    |   |-- info_codes.txt
    |-- documents_to_process/
    |   |-- your_document_1.docx
    |   |-- your_document_2.docx
    |-- main.py
    |-- logs/
    ```
    *   Place the three SNS XML files and the `DMTypes` text file (I've named it `info_codes.txt`) into the `s1000d_data` directory.
    *   Place the `.docx` files you want to process into the `documents_to_process` directory.
    *   The `logs` directory will be created automatically to store the output logs.

---

### The Python Code (`main.py`)

Copy the following code into a file named `main.py` in your `dmc_automation` directory.

```python
import os
import re
import json
import logging
from datetime import datetime
from bs4 import BeautifulSoup
import docx
import requests

# --- CONFIGURATION ---
OLLAMA_API_URL = "http://localhost:11434/api/generate"
OLLAMA_MODEL = "gemma:7b"  # <-- IMPORTANT: Change this to your 20b model name
DOCS_DIRECTORY = "documents_to_process"
DATA_DIRECTORY = "s1000d_data"
LOGS_DIRECTORY = "logs"

# --- SETUP LOGGING ---
if not os.path.exists(LOGS_DIRECTORY):
    os.makedirs(LOGS_DIRECTORY)

log_filename = os.path.join(LOGS_DIRECTORY, f"dmc_processing_log_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json")

# Basic logger for console output
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')


# --- DATA PARSING AND PREPARATION ---

def parse_sns_xml(file_path):
    """Parses an S1000D SNS XML file to extract system codes and titles."""
    with open(file_path, 'r', encoding='utf-8') as f:
        soup = BeautifulSoup(f, 'xml')

    sns_data = {}
    for system in soup.find_all('snsSystem'):
        sys_code = system.find('snsCode').text
        sys_title = system.find('snsTitle').text
        sns_data[sys_code] = {'title': sys_title, 'subsystems': {}}
        for subsys in system.find_all('snsSubSystem'):
            sub_code = subsys.find('snsCode').text
            sub_title = subsys.find('snsTitle').text
            sns_data[sys_code]['subsystems'][sub_code] = {'title': sub_title, 'subsubsystems': {}}
            for subsubsys in subsys.find_all('snsSubSubSystem'):
                subsub_code = subsubsys.find('snsCode').text
                subsub_title = subsubsys.find('snsTitle').text
                sns_data[sys_code]['subsystems'][sub_code]['subsubsystems'][subsub_code] = {'title': subsub_title}
    return sns_data

def parse_info_codes(file_path):
    """Parses the DMTypes text file to extract info codes and their descriptions."""
    info_codes = {}
    with open(file_path, 'r', encoding='utf-8') as f:
        for line in f:
            line = line.strip()
            if not line:
                continue
            match = re.match(r'^([0-9A-Z]{3})\s+([a-z]+)\s+(.*)', line)
            if match:
                code, type, desc = match.groups()
                info_codes[code] = {'type': type, 'description': desc}
    return info_codes

def prepare_context_for_llm(sns_data, info_codes):
    """Creates a simplified string representation of the S1000D data for the LLM prompt."""
    sns_context = "SNS (System, Sub-System, Sub-Sub-System):\n"
    for code, data in sns_data.items():
        sns_context += f"- {code}: {data['title']}\n"

    info_context = "Information Codes (describes document type):\n"
    for code, data in info_codes.items():
        info_context += f"- {code} ({data['type']}): {data['description']}\n"

    return sns_context, info_context


# --- DOCUMENT PROCESSING ---

def extract_text_from_docx(file_path):
    """Extracts all text from a .docx file."""
    try:
        doc = docx.Document(file_path)
        full_text = []
        for para in doc.paragraphs:
            full_text.append(para.text)
        # You could also extract text from tables if needed
        # for table in doc.tables:
        #     for row in table.rows:
        #         for cell in row.cells:
        #             full_text.append(cell.text)
        return '\n'.join(full_text)
    except Exception as e:
        logging.error(f"Could not read docx file {file_path}: {e}")
        return None


# --- CORE LOGIC: LLM AND FALLBACK ---

def generate_dmc_with_llm(content, sns_context, info_context):
    """Uses Ollama to analyze content and generate DMC codes."""
    prompt = f"""
    You are an expert technical author specializing in the S1000D specification.
    Your task is to analyze the technical document content provided and assign the correct S1000D Data Module Code (DMC) components.

    **CONTEXT - Standard Numbering System (SNS):**
    {sns_context}

    **CONTEXT - Information Codes:**
    {info_context}

    **DOCUMENT CONTENT:**
    ---
    {content[:4000]}
    ---

    **TASK:**
    Based on the document content, determine the most appropriate values for the following DMC components.
    The most important codes are 'systemCode' and 'infoCode'.
    - systemCode: The 2-character primary system code from the SNS.
    - subSystemCode: The 1-character sub-system code. Use '0' if not applicable.
    - subSubSystemCode: The 1-character sub-sub-system code. Use '0' if not applicable.
    - infoCode: The 3-character code that best describes the type of information (e.g., '040' for description, '520' for a removal procedure).
    - disassyCode: Default to '00'.
    - disassyCodeVariant: Default to 'A'.

    Provide ONLY the JSON object in your response.

    Example Response:
    {{
      "systemCode": "B1",
      "subSystemCode": "1",
      "subSubSystemCode": "0",
      "infoCode": "663",
      "disassyCode": "00",
      "disassyCodeVariant": "A"
    }}
    """

    try:
        logging.info("Querying LLM for DMC codes...")
        payload = {
            "model": OLLAMA_MODEL,
            "prompt": prompt,
            "stream": False,
            "format": "json"
        }
        response = requests.post(OLLAMA_API_URL, json=payload, timeout=300)
        response.raise_for_status()
        
        # The response from Ollama is a JSON string itself.
        response_text = response.json().get('response', '{}')
        dmc_parts = json.loads(response_text)

        # Basic validation
        if 'systemCode' in dmc_parts and 'infoCode' in dmc_parts:
            logging.info(f"LLM successfully returned codes: {dmc_parts}")
            return dmc_parts
        else:
            logging.warning("LLM response was malformed.")
            return None

    except requests.exceptions.RequestException as e:
        logging.error(f"API call to Ollama failed: {e}")
        return None
    except json.JSONDecodeError as e:
        logging.error(f"Failed to parse JSON from LLM response: {e}")
        return None


def generate_dmc_with_fallback(content, sns_data, info_codes):
    """A simple keyword-based fallback if the LLM fails."""
    logging.warning("Executing fallback mechanism...")
    content_lower = content.lower()
    
    # Simple scoring for best match
    best_sns = None
    max_sns_score = 0
    for code, data in sns_data.items():
        score = 1 if data['title'].lower() in content_lower else 0
        if score > max_sns_score:
            max_sns_score = score
            best_sns = code

    best_info = None
    max_info_score = 0
    for code, data in info_codes.items():
        # Give higher weight to keywords like "procedure", "description", etc.
        keywords = data['description'].lower().split()
        score = sum(1 for keyword in keywords if keyword in content_lower)
        if score > max_info_score:
            max_info_score = score
            best_info = code

    if best_sns and best_info:
        dmc_parts = {
            "systemCode": best_sns,
            "subSystemCode": "0",
            "subSubSystemCode": "0",
      		"infoCode": best_info,
            "disassyCode": "00",
            "disassyCodeVariant": "A"
        }
        logging.info(f"Fallback mechanism found potential codes: {dmc_parts}")
        return dmc_parts
    
    logging.error("Fallback mechanism could not determine codes.")
    return None

def format_dmc(parts):
    """Formats the final DMC string from its component parts."""
    # These parts are placeholders as their logic is not defined in the prompt.
    # You would need to add logic to determine these.
    model_ident_code = "EXAMPLE"
    system_diff_code = "A"
    assy_code = "0000"
    item_location_code = "D"
    
    return (
        f'DMC-{model_ident_code}-{system_diff_code}-{parts["systemCode"]}-'
        f'{parts["subSystemCode"]}{parts["subSubSystemCode"]}-{assy_code}-'
        f'{parts["disassyCode"]}{parts["disassyCodeVariant"]}-{parts["infoCode"]}A-{item_location_code}'
    )

# --- MAIN EXECUTION ---

def main():
    """Main function to process all documents."""
    logging.info("--- Starting DMC Automation Process ---")

    # 1. Load and parse S1000D data
    try:
        # Combining all SNS data into one dictionary
        sns_data = {}
        sns_files = [f for f in os.listdir(DATA_DIRECTORY) if f.startswith('sns_') and f.endswith('.xml')]
        for sns_file in sns_files:
            sns_data.update(parse_sns_xml(os.path.join(DATA_DIRECTORY, sns_file)))

        info_codes = parse_info_codes(os.path.join(DATA_DIRECTORY, "info_codes.txt"))

        if not sns_data or not info_codes:
            logging.error("Failed to load S1000D data. Check data directory. Exiting.")
            return

        logging.info(f"Successfully loaded {len(sns_data)} SNS systems and {len(info_codes)} info codes.")
    except FileNotFoundError as e:
        logging.error(f"Required data file not found: {e}. Exiting.")
        return

    # 2. Prepare context for LLM
    sns_context_str, info_context_str = prepare_context_for_llm(sns_data, info_codes)

    # 3. Process documents
    files_to_process = [f for f in os.listdir(DOCS_DIRECTORY) if f.endswith(".docx")]
    if not files_to_process:
        logging.warning(f"No .docx files found in '{DOCS_DIRECTORY}'. Exiting.")
        return
    
    log_data = {"successful": [], "failed": []}

    for filename in files_to_process:
        logging.info(f"--- Processing file: {filename} ---")
        filepath = os.path.join(DOCS_DIRECTORY, filename)
        
        content = extract_text_from_docx(filepath)
        if not content:
            log_data["failed"].append({
                "file": filename,
                "issue": "Could not read or extract content from the file."
            })
            continue

        # Primary attempt: Use LLM
        dmc_parts = generate_dmc_with_llm(content, sns_context_str, info_context_str)

        # Fallback if LLM fails
        if not dmc_parts:
            dmc_parts = generate_dmc_with_fallback(content, sns_data, info_codes)

        # Log results
        if dmc_parts:
            final_dmc = format_dmc(dmc_parts)
            log_data["successful"].append({
                "file": filename,
                "assigned_dmc": final_dmc,
                "dmc_parts": dmc_parts
            })
            logging.info(f"Successfully assigned DMC: {final_dmc}")
        else:
            log_data["failed"].append({
                "file": filename,
                "issue": "Failed to determine DMC codes using both LLM and fallback mechanism."
            })
            logging.error("Could not assign DMC for this file.")

    # 4. Write final log file
    with open(log_filename, 'w', encoding='utf-8') as f:
        json.dump(log_data, f, indent=4)
        
    logging.info(f"--- Process Finished. Log file created at: {log_filename} ---")


if __name__ == "__main__":
    main()

```

### How to Run It

1.  Make sure your project structure is set up as described above.
2.  Open your terminal or command prompt.
3.  Navigate to the `dmc_automation` directory.
4.  Run the script:
    ```bash
    python main.py
    ```

### Explanation

1.  **Configuration**: At the top of the script, you can easily change the Ollama model name, API URL, and directory paths. **Remember to change `OLLAMA_MODEL` to match your 20b model's name** (e.g., `mistral:20b` or whatever you have named it).
2.  **Data Parsing**:
    *   `parse_sns_xml` uses the `BeautifulSoup` library to read the XML structure of your SNS files and convert them into a nested Python dictionary, which is easy to work with.
    *   `parse_info_codes` uses regular expressions to read the `info_codes.txt` file and store the codes and descriptions.
3.  **Content Extraction**: `extract_text_from_docx` uses the `python-docx` library to open a Word document and pull out all the text from its paragraphs.
4.  **LLM Interaction (`generate_dmc_with_llm`)**:
    *   This is the core of the primary approach. It constructs a detailed **prompt** for your LLM.
    *   The prompt includes the document's content and the parsed S1000D standards as context. This helps the LLM make an informed decision.
    *   It instructs the LLM to return the output in a strict **JSON format**, which makes it easy for the Python script to parse the results reliably.
    *   It sends the request to your local Ollama API endpoint.
5.  **Fallback Mechanism (`generate_dmc_with_fallback`)**:
    *   This function is called only if the LLM function returns `None` (due to an API error, bad response, etc.).
    *   It performs a very simple keyword search. It checks if the titles from the SNS data or keywords from the info code descriptions appear in the document text. It's not as smart as the LLM but serves as a basic safety net.
6.  **Main Execution Loop**:
    *   The `main()` function orchestrates the entire process.
    *   It first loads all the S1000D data into memory.
    *   It then iterates through every `.docx` file in the specified directory.
    *   For each file, it tries the LLM first, then the fallback.
    *   It records the outcome (success or failure) in the `log_data` dictionary.
    *   Finally, it writes this dictionary to a timestamped JSON log file in the `logs` directory. This file gives you a clean, machine-readable record of what happened.
Of course. Let's break down the script's functionality step-by-step and then look at the exact output you can expect.

### How the Script Works (The "Working")

Think of the script as a digital technical author that follows a logical workflow. Here is the process from start to finish:

#### Phase 1: Learning the Rules

Before it can do any work, the script needs to understand the S1000D standard you've provided.

1.  **Parse SNS Codes**: It reads the three XML files (`sns_generic.xml`, `sns_sea_vehicles.xml`, `sns_support_training.xml`). Using the `BeautifulSoup` library, it extracts every `<snsSystem>`, `<snsSubSystem>`, etc., and organizes them into a large Python dictionary. This dictionary acts as its "knowledge base" of all possible system and subsystem codes and their meanings.
2.  **Parse Information Codes**: It then reads the `info_codes.txt` file. It goes through this file line-by-line, extracting the 3-character code (e.g., `040`), the type (`descript`), and the description ("Description"). This is stored in another dictionary.
3.  **Prepare Context for the LLM**: It takes these large dictionaries and formats them into a simplified, readable text block. This text is specifically designed to be part of the prompt, giving the LLM the essential context it needs to make an informed decision.

#### Phase 2: Processing the Documents

Now that it has the S1000D knowledge, it begins processing your `.docx` files one by one. For each file in the `documents_to_process` folder:

1.  **Extract Text**: The script opens the `.docx` file and extracts all the plain text from its paragraphs. This text is the raw data it will analyze.
2.  **Attempt 1: The AI Expert (Ollama LLM)**:
    *   It constructs a detailed **prompt**. This prompt is a set of instructions for your local 20b-parameter LLM.
    *   The prompt includes:
        *   The role it should play ("You are an expert technical author...").
        *   The context it needs (the simplified S1000D rules from Phase 1).
        *   The content of the document.
        *   A very specific task (identify the `systemCode`, `infoCode`, etc., and return **only a JSON object**).
    *   It sends this entire package to your local Ollama API.
    *   It waits for the LLM to "think" and send back a response. If the response is a valid JSON object with the required codes, this attempt is considered a **success**.

3.  **Attempt 2: The Fallback Plan (Keyword Matching)**:
    *   This step only runs if the LLM fails for any reason (e.g., the API is down, the model returns a malformed response, or the request times out).
    *   It performs a much simpler, non-AI analysis. It reads the document text and counts how many times the keywords from the SNS titles and Information Code descriptions appear.
    *   It picks the `systemCode` and `infoCode` that have the highest keyword scores. This is less accurate than the LLM but is a good safety net to get a potential match.

4.  **Record the Result**:
    *   If either the LLM or the fallback was successful, it formats the final DMC string (e.g., `DMC-EXAMPLE-A-B1-10-0000-00A-663A-D`).
    *   It then stores the result—including the filename, the final DMC, and the individual code parts—in a "successful" list.
    *   If both attempts fail, it records the filename and the reason for failure in a "failed" list.

#### Phase 3: Final Report

After the script has looped through every single document:

1.  **Generate Log File**: It takes the "successful" and "failed" lists it has been building.
2.  **Write to JSON**: It writes this data into a new file in the `logs` directory. The file is named with the current date and time to ensure every run has a unique log (e.g., `dmc_processing_log_20250918_180100.json`).

---

### What to Expect as Output

You will see two types of output: what appears in your terminal as the script runs, and the final JSON log file that is generated.

#### 1. Console/Terminal Output

While the script is running, it will print status messages to your screen, keeping you informed of its progress. It will look something like this:

```
2025-09-18 18:01:00,123 - INFO - --- Starting DMC Automation Process ---
2025-09-18 18:01:00,456 - INFO - Successfully loaded 215 SNS systems and 388 info codes.
2025-09-18 18:01:00,457 - INFO - --- Processing file: document_about_hull_repair.docx ---
2025-09-18 18:01:00,789 - INFO - Querying LLM for DMC codes...
2025-09-18 18:01:15,321 - INFO - LLM successfully returned codes: {'systemCode': 'B1', 'subSystemCode': '1', 'subSubSystemCode': '0', 'infoCode': '663', 'disassyCode': '00', 'disassyCodeVariant': 'A'}
2025-09-18 18:01:15,322 - INFO - Successfully assigned DMC: DMC-EXAMPLE-A-B1-10-0000-00A-663A-D

2025-09-18 18:01:15,323 - INFO - --- Processing file: document_with_no_clear_topic.docx ---
2025-09-18 18:01:15,654 - INFO - Querying LLM for DMC codes...
2025-09-18 18:01:30,987 - WARNING - LLM response was malformed.
2025-09-18 18:01:30,988 - WARNING - Executing fallback mechanism...
2025-09-18 18:01:31,123 - ERROR - Fallback mechanism could not determine codes.
2025-09-18 18:01:31,124 - ERROR - Could not assign DMC for this file.

2025-09-18 18:01:31,125 - INFO - --- Process Finished. Log file created at: logs/dmc_processing_log_20250918_180115.json ---
```

#### 2. The Final Log File (`.json`)

This is the most important output. It is a structured file that clearly separates the successful assignments from the failures. It is designed to be easily read by both humans and other programs.

Here is an example of what the contents of `logs/dmc_processing_log_20250918_180115.json` would look like:

```json
{
    "successful": [
        {
            "file": "document_about_hull_repair.docx",
            "assigned_dmc": "DMC-EXAMPLE-A-B1-10-0000-00A-663A-D",
            "dmc_parts": {
                "systemCode": "B1",
                "subSystemCode": "1",
                "subSubSystemCode": "0",
                "infoCode": "663",
                "disassyCode": "00",
                "disassyCodeVariant": "A"
            }
        },
        {
            "file": "engine_removal_guide.docx",
            "assigned_dmc": "DMC-EXAMPLE-A-A1-50-0000-00A-520A-D",
            "dmc_parts": {
                "systemCode": "A1",
                "subSystemCode": "5",
                "subSubSystemCode": "0",
                "infoCode": "520",
                "disassyCode": "00",
                "disassyCodeVariant": "A"
            }
        }
    ],
    "failed": [
        {
            "file": "document_with_no_clear_topic.docx",
            "issue": "Failed to determine DMC codes using both LLM and fallback mechanism."
        },
        {
            "file": "corrupted_file.docx",
            "issue": "Could not read or extract content from the file."
        }
    ]
}
```

This log file gives you a perfect record of the operation, allowing you to quickly review which documents were processed successfully and which ones require manual intervention.