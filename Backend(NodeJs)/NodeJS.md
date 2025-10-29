
Node.js is a free, open-source, and cross-platform JavaScript runtime environment that allows developers to execute JavaScript code outside of a web browser. It is built on Google Chrome's V8 JavaScript engine, which makes it highly performant.

# 🖥️ Creating Server using HTTP

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.end("Hello world from India");
});

server.listen(3000, () => {
  console.log('Server Running on port 3000...');
});

```


# Express.js

**Light-weight, fast Node.js framework.**

```js
const express = require('express')
const app = express()
app.get('/home', (req, res)=>{
    res.send("Welcome to the Home page!");
    console.log("Running..");  
})
app.get('/about', (req, res)=>{
    res.send("Welcome to the About page!");
    console.log("Running..");  
})
app.listen(3000, ()=>{
    console.log("Server running at port 3000..");
})
```


# API's

# 🧩 What are APIs?

An **API (Application Programming Interface)** is a set of **rules, protocols, and tools** that allows different software applications to communicate with each other.

Think of an API as a **messenger** that takes your request, tells another program what you want, and then returns the result.

---

## 🍽️ Real-Life Example

Imagine you’re at a restaurant:

- **You (User)** → the customer  
- **Menu** → the API documentation (what’s available)  
- **Waiter (API)** → takes your order to the kitchen and brings your food  
- **Kitchen (Server)** → does the actual work and returns the food  

So, the API acts as a **bridge** between your app and another service.

---

## 💻 Technical Definition

An API defines **how one program can request data or services** from another.  
It typically specifies:

- **Endpoints** (URLs for communication)  
- **Methods** (`GET`, `POST`, `PUT`, `DELETE`, etc.)  
- **Data formats** (`JSON`, `XML`, etc.)  
- **Authentication** (API keys, tokens, etc.)

---

## 🌦️ Example: Weather API

If your app needs current weather data, it can call a weather API:

### Request:
```http
GET https://api.weather.com/v3/weather/current?city=Mumbai
```

### Response:

`{   "city": "Mumbai",   "temperature": "32°C",   "condition": "Sunny" }`

Your app then displays:

> ☀️ **32°C – Sunny in Mumbai**

---

## 🧠 Types of APIs

|Type|Description|Example|
|---|---|---|
|**Web APIs**|For communication over the internet|REST, GraphQL|
|**Library APIs**|Functions provided by a software library|TensorFlow API|
|**OS APIs**|Interact with operating systems|Windows API, Android API|
|**Hardware APIs**|Interface with physical devices|Camera API, Bluetooth API|

---

## 🌍 Popular API Examples

- **Google Maps API** → Embed maps, directions
    
- **YouTube API** → Fetch videos, channels, analytics
    
- **OpenAI API** → Use AI models like GPT
    
- **Twitter (X) API** → Read or post tweets
    

---

## 🧑‍💻 Example: Using an API in JavaScript

``fetch("https://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q=Mumbai")   .then(response => response.json())   .then(data => {     console.log(`Temperature in ${data.location.name}: ${data.current.temp_c}°C`);   })   .catch(error => console.error("Error fetching weather data:", error));``

---

## 🏁 Summary

- APIs allow software to **talk to each other**.
    
- They make apps more **modular, reusable, and scalable**.
    
- Almost every modern web or mobile app uses APIs behind the scenes.



# 🌐 What is a REST API?

**REST** stands for **Representational State Transfer** — it’s a **style or set of rules** for building **Web APIs** that use **HTTP** to communicate between a client and a server.

So a **REST API** is a **Web API that follows REST principles**, making it simple, lightweight, and scalable.

---

## ⚙️ REST API Basics

A REST API uses **standard HTTP methods** to perform operations on data (called _resources_).

|HTTP Method|Action|Description|
|---|---|---|
|**GET**|Read|Fetch data from the server|
|**POST**|Create|Add new data to the server|
|**PUT**|Update|Modify existing data|
|**DELETE**|Delete|Remove data|

### Example:

If you have a **user** resource:

|Operation|HTTP Method|Endpoint|Description|
|---|---|---|---|
|Get all users|`GET`|`/api/users`|Returns a list of users|
|Get one user|`GET`|`/api/users/5`|Returns user with ID 5|
|Create a user|`POST`|`/api/users`|Adds a new user|
|Update a user|`PUT`|`/api/users/5`|Updates user 5|
|Delete a user|`DELETE`|`/api/users/5`|Removes user 5|

---

## 🧩 What Is the Difference Between an API and a REST API?

|Feature|**API**|**REST API**|
|---|---|---|
|**Definition**|A general interface that allows two systems to communicate.|A specific type of API that follows REST architectural rules using HTTP.|
|**Protocol**|Can use any protocol (HTTP, WebSocket, gRPC, etc.)|Always uses HTTP/HTTPS.|
|**Data Format**|Can use XML, JSON, SOAP, binary, etc.|Commonly uses JSON or XML.|
|**Architecture**|General concept.|Follows REST constraints like statelessness and resource-based URLs.|
|**Example**|Java SDK API, Windows API, Database API|GitHub REST API, OpenWeather REST API|
|**State**|Can be **stateful** or **stateless**|Always **stateless** (server doesn’t store client context)|

---

## 🧠 In Simple Terms

👉 **All REST APIs are APIs**,  
but **not all APIs are REST APIs**.

- **API** = any interface that connects two software systems.
    
- **REST API** = an API that follows REST rules and works over HTTP (most modern web APIs do).
    

---

## 🔍 Example Comparison

### Generic API (Non-REST)

A **SOAP API** might use XML and a complex structure:

`<soap:Envelope>   <soap:Body>     <getUser>       <userId>5</userId>     </getUser>   </soap:Body> </soap:Envelope>`

### REST API

Uses simple HTTP + JSON:

`GET https://api.example.com/users/5`

Response:

`{   "id": 5,   "name": "Alice" }`

---

## 🚀 Summary

- **API** = A general term for any interface that lets software communicate.
    
- **REST API** = A specific type of **Web API** that follows REST architecture using HTTP methods and resource URLs.
    
- **REST APIs** are preferred in web development because they are **simple, fast, and widely supported**.


# ⚙️ REST API Methods

A **REST API** uses standard **HTTP methods** to perform actions on **resources** (like users, products, posts, etc.).  
Each method represents a specific **CRUD operation** (Create, Read, Update, Delete).

---

## 🧩 CRUD and HTTP Mapping

| CRUD Operation | HTTP Method | Description |
|----------------|--------------|--------------|
| **Create** | `POST` | Add a new resource |
| **Read** | `GET` | Retrieve one or more resources |
| **Update** | `PUT` / `PATCH` | Modify an existing resource |
| **Delete** | `DELETE` | Remove a resource |

---

## 🔹 1. GET – Retrieve Data

**Used to:** Read or fetch data from the server.  
**Should not** change or modify anything on the server.

**Example:**
```http
GET /api/users
```

**Response:**

`[   { "id": 1, "name": "Alice" },   { "id": 2, "name": "Bob" } ]`

✅ **Safe & Idempotent:**

- Doesn’t change the data.
    
- Multiple identical GET requests give the same result.
    

---

## 🔹 2. POST – Create Data

**Used to:** Add new data to the server.  
**Example:**

`POST /api/users`

**Request Body:**

`{ "name": "Charlie", "email": "charlie@mail.com" }`

**Response:**

`{ "id": 3, "name": "Charlie", "email": "charlie@mail.com" }`

🚫 **Not Idempotent:**  
Sending the same POST request twice will create **two** new users.

---

## 🔹 3. PUT – Update or Replace Data

**Used to:** Replace an existing resource **entirely**.  
**Example:**

`PUT /api/users/3`

**Request Body:**

`{ "name": "Charlie Brown", "email": "charlieb@mail.com" }`

✅ **Idempotent:**  
Sending the same PUT request multiple times gives the same result.

---

## 🔹 4. PATCH – Partially Update Data

**Used to:** Update only **specific fields** of a resource.  
**Example:**

`PATCH /api/users/3`

**Request Body:**

`{ "email": "newemail@mail.com" }`

Only the `email` field is updated — the rest stays the same.

---

## 🔹 5. DELETE – Remove Data

**Used to:** Delete a resource.  
**Example:**

`DELETE /api/users/3`

✅ **Idempotent:**  
Deleting the same resource twice has the same result (it’s gone).

---

## 🧠 Summary Table

|Method|Action|CRUD|Idempotent|Safe|Typical Use|
|---|---|---|---|---|---|
|**GET**|Read|Read|✅ Yes|✅ Yes|Fetch data|
|**POST**|Create|Create|❌ No|❌ No|Add data|
|**PUT**|Replace|Update|✅ Yes|❌ No|Full update|
|**PATCH**|Modify|Update|✅ Usually|❌ No|Partial update|
|**DELETE**|Remove|Delete|✅ Yes|❌ No|Delete data|

---

## 🧱 Example REST API Resource Endpoints

|Action|Method|Endpoint|
|---|---|---|
|Get all books|`GET`|`/api/books`|
|Get one book|`GET`|`/api/books/1`|
|Add a new book|`POST`|`/api/books`|
|Update book|`PUT`|`/api/books/1`|
|Partially update book|`PATCH`|`/api/books/1`|
|Delete book|`DELETE`|`/api/books/1`|

---

## 🏁 In Summary

- REST APIs use **HTTP methods** to describe what action to take.
    
- Each method is linked to a **CRUD operation**.
    
- Methods like **GET** are safe, while **POST/PUT/DELETE** modify data.
    
- Proper use of these methods makes your API **clean, predictable, and RESTful**.

# 🌐 REST API Methods with Examples

Below are examples of the **four main REST API methods** — `GET`, `POST`, `PATCH`, and `DELETE`.  
We’ll use a sample resource called **/users** to demonstrate each operation.

---

## 🟢 1. GET – Retrieve Data

**Purpose:** Fetch data from the server.  
**Example Request:**
```http
GET /api/users
```


**Response:**

`[   { "id": 1, "name": "Alice", "email": "alice@mail.com" },   { "id": 2, "name": "Bob", "email": "bob@mail.com" } ]`

➡️ **Notes:**

- Does **not** modify any data.
    
- Can be used with query parameters like:
    
    `GET /api/users?name=Alice`
    

---

## 🟠 2. POST – Create Data

**Purpose:** Add a new resource to the server.  
**Example Request:**

`POST /api/users Content-Type: application/json`

**Request Body:**

`{ "name": "Charlie", "email": "charlie@mail.com" }`

**Response:**

`{ "id": 3, "name": "Charlie", "email": "charlie@mail.com" }`

➡️ **Notes:**

- Used to **create** a new user.
    
- Each request usually creates a **new entry**.
    
- **Not idempotent** (multiple calls create duplicates).
    

---

## 🟡 3. PATCH – Update Part of a Resource

**Purpose:** Modify only specific fields of a resource.  
**Example Request:**

`PATCH /api/users/3 Content-Type: application/json`

**Request Body:**

`{ "email": "charlie.new@mail.com" }`

**Response:**

`{ "id": 3, "name": "Charlie", "email": "charlie.new@mail.com" }`

➡️ **Notes:**

- Updates **only** the provided fields.
    
- Common for profile edits or small changes.
    
- Safer than `PUT` when you don’t want to overwrite all data.
    

---

## 🔴 4. DELETE – Remove a Resource

**Purpose:** Delete a resource from the server.  
**Example Request:**

`DELETE /api/users/3`

**Response:**

`{ "message": "User with ID 3 deleted successfully." }`

➡️ **Notes:**

- Deletes the resource permanently (unless soft-delete is implemented).
    
- **Idempotent** (deleting again returns same result — resource already gone).
    

---

## 🧩 Quick Summary Table

|Method|Action|Example Endpoint|Description|
|---|---|---|---|
|**GET**|Read|`/api/users`|Retrieve user data|
|**POST**|Create|`/api/users`|Add a new user|
|**PATCH**|Update (partial)|`/api/users/3`|Modify existing user|
|**DELETE**|Delete|`/api/users/3`|Remove user|

---

## 💻 JavaScript Fetch Example (Optional)

You can use these methods in frontend or backend code.  
Example using the **Fetch API** in JavaScript:

`// Example: GET request fetch("https://api.example.com/users")   .then(res => res.json())   .then(data => console.log(data));  // Example: POST request fetch("https://api.example.com/users", {   method: "POST",   headers: { "Content-Type": "application/json" },   body: JSON.stringify({ name: "Charlie", email: "charlie@mail.com" }) });  // Example: PATCH request fetch("https://api.example.com/users/3", {   method: "PATCH",   headers: { "Content-Type": "application/json" },   body: JSON.stringify({ email: "new@mail.com" }) });  // Example: DELETE request fetch("https://api.example.com/users/3", { method: "DELETE" });`

---

## ✅ In Summary

- `GET` → Read data
    
- `POST` → Create new data
    
- `PATCH` → Update existing data
    
- `DELETE` → Remove data
    

REST APIs make it easy to perform CRUD operations through simple, predictable HTTP methods.

# Examples

```js
const express = require("express");

  

const app = express();

  

app.use(express.json());

  

let notes = [];

  

app.post("/notes", (req, res) => {

  notes.push(req.body);

  console.log(notes);

  res.json({

    message: "Notes added Sucessfully!",

    notes: notes,

  });

});

  

app.get("/notes", (re, res) => {

  res.send("Notes Api !");

});

  

app.delete("/notes/:index", (req, res) => {

  const index = req.params.index;

  delete notes[index];

  res.json({

    message: "Note is Deleted Sucessfully !",

  });

});

  
  

app.patch('/notes/:index',(req,res)=>{

    const index = req.params.index

    const {title} = req.body

    notes[index].title = title

    res.json({

        message: 'Notes Updated Sucessfully !',

    })

  

})

  

app.listen(3000, () => {

  console.log("Server running at port 3000..");

});
```

