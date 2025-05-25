# JavaScript Variables and Data Types

JavaScript is a dynamically-typed language, meaning variables can hold different types of data without explicitly declaring their type. Understanding variables and data types is foundational to writing effective JS code.

---

## Variables in JavaScript

Variables are containers for storing data values. In JS, there are three ways to declare variables: var, let, and const. Each has unique behaviors and use cases.

### Types of Variables

> [!NOTE] Click the sections below to expand/collapse details in Obsidian.

#### var - The Old School Variable

- **Scope**: Function-scoped (available throughout the function it’s declared in).
- **Hoisting**: Declared variables are "hoisted" to the top of their scope, initialized as undefined.
- **Re-declarable**: Can be re-declared in the same scope without errors.
- **Re-assignable**: Can be updated with new values.

javascript

CollapseWrapCopy

`var name = "Alice"; console.log(name); // "Alice" var name = "Bob"; // Re-declaration is allowed console.log(name); // "Bob" name = "Charlie"; // Re-assignment is allowed console.log(name); // "Charlie" function example() { var x = 10; if (true) { var x = 20; // Overwrites the outer x console.log(x); // 20 } console.log(x); // 20 (not 10, due to function scope) } example();`

- **Why Use?**: Rarely recommended today due to its quirks (hoisting, lack of block scope). Use it only in legacy code or when you explicitly need function-level scope.
- **Why Not?**: Can lead to bugs due to hoisting and accidental re-declarations.

#### let - Block-Scoped and Modern

- **Scope**: Block-scoped (limited to the {} block it’s declared in).
- **Hoisting**: Hoisted but not initialized (leads to "Temporal Dead Zone" errors if accessed before declaration).
- **Re-declarable**: Cannot be re-declared in the same scope.
- **Re-assignable**: Can be updated with new values.

javascript

`let age = 25; console.log(age); // 25 // let age = 30; // Error: Cannot re-declare 'age' in the same scope age = 30; // Re-assignment is fine console.log(age); // 30 if (true) { let score = 100; console.log(score); // 100 } console.log(score); // Error: 'score' is not defined (block scope)`

- **Why Use?**: Preferred for variables that need to change value, with safer block scoping.
- **Why Not?**: Don’t use if the value should never change (use const instead).

#### const - Constants with Block Scope

- **Scope**: Block-scoped (like let).
- **Hoisting**: Hoisted but not initialized (Temporal Dead Zone applies).
- **Re-declarable**: Cannot be re-declared in the same scope.
- **Re-assignable**: Cannot be re-assigned, but objects/arrays declared with const can have their contents modified.

javascript


`const pi = 3.14; console.log(pi); // 3.14 // pi = 3.14159; // Error: Assignment to constant variable const person = { name: "Alice" }; person.name = "Bob"; // Allowed: Modifying object properties console.log(person.name); // "Bob" // person = { name: "Charlie" }; // Error: Cannot re-assign the object itself`

- **Why Use?**: Use for values that shouldn’t change (e.g., constants, configuration values) or when you want to signal intent.
- **Why Not?**: Avoid if the variable needs to be re-assigned (use let instead).

---

### When to Use Which Variable Type?

|Scenario|var|let|const|
|---|---|---|---|
|Value won’t change|❌|❌|✅|
|Value will change|⚠️ (risky)|✅|❌|
|Block scope needed|❌|✅|✅|
|Legacy code compatibility|✅|❌|❌|
|Loop counters|❌|✅|❌ (unless constant)|

- **Use const by default**: Promotes immutability and reduces bugs.
- **Switch to let when re-assignment is needed**: Keeps code predictable with block scope.
- **Avoid var unless required**: Modern JS rarely needs it.

---

## Data Types in JavaScript

JavaScript has **primitive** and **non-primitive** (reference) data types.

### Primitive Data Types

These are immutable, basic building blocks.

1. **Number**
    
    - Represents integers and floating-point numbers.
    - Example: let num = 42; let decimal = 3.14;
    - Special values: Infinity, -Infinity, NaN.
    
    javascript
    
    CollapseWrapCopy
    
    `let x = 10; let y = 3.5; console.log(x + y); // 13.5 console.log(1 / 0); // Infinity console.log("text" * 2); // NaN`
    
2. **String**
    
    - Represents text, enclosed in ', ", or ` (template literals).
    - Example: let greeting = "Hello";
    
    javascript
    
    CollapseWrapCopy
    
    ``let name = "Alice"; let message = `Hi, ${name}!`; // Template literal console.log(message); // "Hi, Alice!"``
    
3. **Boolean**
    
    - Represents true or false.
    - Example: let isActive = true;
    
    javascript
    
    CollapseWrapCopy
    
    `let isAdult = age >= 18; console.log(isAdult); // true (if age = 25)`
    
4. **Undefined**
    
    - A variable that’s declared but not assigned a value.
    - Example: let x; console.log(x); // undefined
5. **Null**
    
    - Represents intentional absence of a value.
    - Example: let data = null;
    
    javascript
    
    CollapseWrapCopy
    
    `let value = null; console.log(value); // null`
    
6. **Symbol** (ES6)
    
    - Unique and immutable identifiers, often used as object keys.
    - Example: let id = Symbol("id");
    
    javascript
    
    CollapseWrapCopy
    
    `let sym1 = Symbol("key"); let sym2 = Symbol("key"); console.log(sym1 === sym2); // false (unique even with same description)`
    
7. **BigInt** (ES2020)
    
    - For arbitrarily large integers, suffix with n.
    - Example: let bigNum = 12345678901234567890n;
    
    javascript
    

    
    `let big = 10n; console.log(big * 2n); // 20n`
    

### Non-Primitive (Reference) Data Types

These are mutable and stored by reference.

1. **Object**
    
    - Key-value pairs, used for structured data.
    - Example: let user = { name: "Alice", age: 25 };
    
    javascript
    
    
    `let car = { make: "Toyota", model: "Camry" }; car.model = "Corolla"; // Mutable console.log(car); // { make: "Toyota", model: "Corolla" }`
    
2. **Array**
    
    - Ordered list of values, technically an object.
    - Example: let numbers = [1, 2, 3];
    
    javascript
    
    CollapseWrapCopy
    
    `let fruits = ["apple", "banana"]; fruits.push("orange"); console.log(fruits); // ["apple", "banana", "orange"]`
    
3. **Function**
    
    - A callable object that executes code.
    - Example: function greet() { console.log("Hello"); }
    
    javascript
    
    
    `function add(a, b) { return a + b; } console.log(add(2, 3)); // 5`
    

---

### Why Use Specific Data Types?

- **Number/BigInt**: Use Number for general math; BigInt for huge integers to avoid precision loss.
- **String**: Ideal for text manipulation and display.
- **Boolean**: For logical conditions (e.g., if statements).
- **Null/Undefined**: null for intentional emptiness; undefined for uninitialized variables.
- **Object/Array**: Use for structured or list-based data.
- **Symbol**: For unique keys in objects to avoid naming collisions.

---

## Examples Combining Variables and Data Types

javascript



`// Scenario: User profile const userId = Symbol("id"); // Unique identifier let username = "alice123"; // Can change later const profile = { name: "Alice", age: 25, hobbies: ["reading", "gaming"], isActive: true, lastLogin: null }; function updateAge(newAge) { profile.age = newAge; // Allowed: Object properties are mutable } updateAge(26); console.log(profile); // { name: "Alice", age: 26, hobbies: ["reading", "gaming"], isActive: true, lastLogin: null }`

---



# JavaScript Operators and Type Conversion

Operators and type conversion are essential for manipulating data and performing operations in JavaScript. Operators allow you to perform calculations, comparisons, and logical operations, while type conversion helps manage JavaScript’s dynamic typing.

---

## Operators in JavaScript

Operators are symbols or keywords that perform operations on variables and values. JavaScript categorizes operators into several types.

> [!NOTE] Click the sections below to expand/collapse details in Obsidian.

### Arithmetic Operators

- Perform mathematical operations on numbers.
- Examples: +, -, *, /, % (modulus), ** (exponentiation), ++ (increment), -- (decrement).

javascript


`let a = 10; let b = 3; console.log(a + b); // 13 (addition) console.log(a - b); // 7 (subtraction) console.log(a * b); // 30 (multiplication) console.log(a / b); // 3.333... (division) console.log(a % b); // 1 (remainder) console.log(a ** 2); // 100 (exponentiation) a++; // Increment console.log(a); // 11 b--; // Decrement console.log(b); // 2`

- **Why Use?**: For calculations like totals, averages, or scaling values.
- **Why Not?**: Avoid if operands aren’t numbers (can lead to unexpected type coercion).

### Comparison Operators

- Compare two values and return a boolean (true or false).
- Examples: ==, ===, !=, !==, >, <, >=, <=.

javascript



`let x = 5; let y = "5"; console.log(x == y); // true (loose equality, coerces types) console.log(x === y); // false (strict equality, checks type too) console.log(x != y); // false (loose inequality) console.log(x !== y); // true (strict inequality) console.log(x > 3); // true console.log(x <= 5); // true`

- **Why Use === and !==?**: Strict equality avoids type coercion pitfalls, making code more predictable.
- **Why Not == and !=?**: Loose equality can lead to bugs (e.g., "0" == false is true).

### Logical Operators

- Combine or manipulate boolean values.
- Examples: && (AND), || (OR), ! (NOT).

javascript



`let isAdult = true; let hasID = false; console.log(isAdult && hasID); // false (both must be true) console.log(isAdult || hasID); // true (one must be true) console.log(!isAdult); // false (inverts the value)`

- **Why Use?**: For conditional logic (e.g., if statements).
- **Why Not?**: Overusing can make code harder to read; simplify when possible.

### Assignment Operators

- Assign values to variables, often combined with arithmetic.
- Examples: =, +=, -=, *=, /=, %=.

javascript



`let num = 10; num += 5; // num = num + 5 console.log(num); // 15 num *= 2; // num = num * 2 console.log(num); // 30`

- **Why Use?**: Shorthand for updating variables efficiently.
- **Why Not?**: Avoid if clarity is more important than brevity.

### Bitwise Operators

- Operate on binary representations of numbers.
- Examples: & (AND), | (OR), ^ (XOR), ~ (NOT), << (left shift), >> (right shift).

javascript


`let a = 5; // Binary: 0101 let b = 3; // Binary: 0011 console.log(a & b); // 1 (Binary: 0001) console.log(a | b); // 7 (Binary: 0111) console.log(a << 1); // 10 (Binary: 1010, shifts left)`

- **Why Use?**: For low-level operations (e.g., flags, permissions).
- **Why Not?**: Rarely needed in high-level JS; can confuse beginners.

### Ternary Operator

- A shorthand for if-else statements.
- Syntax: condition ? valueIfTrue : valueIfFalse.

javascript

CollapseWrapCopy

`let age = 20; let status = age >= 18 ? "Adult" : "Minor"; console.log(status); // "Adult"`

- **Why Use?**: Concise for simple conditionals.
- **Why Not?**: Overuse can reduce readability; prefer if for complex logic.

---

### When to Use Which Operator?

|Purpose|Recommended Operators|Avoid When...|
|---|---|---|
|Math calculations|+, -, *, /, %|Operands aren’t numbers|
|Strict comparisons|===, !==|Type coercion is desired|
|Loose comparisons|==, !=|Precision is critical|
|Conditional logic|&&, `||
|Quick assignments|+=, -=, etc.|Clarity trumps brevity|

- **Default to ===**: Safer and more explicit.
- **Simplify Logic**: Break down complex expressions for readability.

---

## Type Conversion in JavaScript

JavaScript automatically converts types (implicit coercion) or allows manual conversion (explicit coercion) because it’s dynamically typed.

### Implicit Type Conversion (Coercion)

- Happens automatically during operations.

#### To String

- Occurs with + when one operand is a string.

javascript



`let num = 5; let str = "The number is " + num; console.log(str); // "The number is 5"`

#### To Number

- Occurs with arithmetic operators (except + with strings) or comparisons.

javascript.



`let strNum = "10"; console.log(strNum - 0); // 10 (string to number) console.log("5" * 2); // 10 console.log("3" > 2); // true`

#### To Boolean

- Occurs in logical contexts (e.g., if, &&).
- Falsy values: 0, "", null, undefined, NaN, false.
- Truthy values: Everything else.

javascript



`console.log(!!0); // false console.log(!!"hello"); // true if ("0") { console.log("Truthy!"); } // Logs "Truthy!"`

- **Why Use?**: Convenient for quick operations.
- **Why Not?**: Can lead to unexpected results (e.g., " " == 0 is true).

### Explicit Type Conversion

- Manually convert types using functions or operators.

#### To String

- Use String() or .toString().

javascript



`let num = 42; console.log(String(num)); // "42" console.log(num.toString()); // "42"`

#### To Number

- Use Number(), parseInt(), parseFloat(), or unary +.

javascript

CollapseWrapCopy

`let str = "123.45"; console.log(Number(str)); // 123.45 console.log(parseInt(str)); // 123 (integer part only) console.log(parseFloat(str)); // 123.45 console.log(+"50"); // 50`

#### To Boolean

- Use Boolean() or double !!.

javascript

CollapseWrapCopy

`console.log(Boolean("")); // false console.log(Boolean(1)); // true console.log(!!"text"); // true`

- **Why Use?**: Explicit control avoids coercion surprises.
- **Why Not?**: Extra code if implicit conversion suffices.

---

### When to Use Type Conversion?

|Goal|Implicit|Explicit|Best For...|
|---|---|---|---|
|String concatenation|"text" + 5|String(5)|User-facing output|
|Numeric operations|"10" - 0|Number("10")|Calculations|
|Boolean checks|if ("text")|Boolean("text")|Explicit logic|
|Parsing user input|N/A|parseInt(), etc.|Form data, strings to nums|

- **Prefer Explicit**: When precision matters (e.g., user input).
- **Leverage Implicit**: For quick, predictable operations.

---

## Examples Combining Operators and Type Conversion

javascript


`// Scenario: Calculate discount let price = "100"; // From user input let discount = 20; let finalPrice = Number(price) - (price * discount) / 100; console.log(finalPrice); // 80 // Scenario: Check eligibility let userAge = "25"; let isEligible = userAge >= 18 ? "Yes" : "No"; console.log(isEligible); // "Yes" // Scenario: Truthy/Falsy check let input = ""; //let hasValue = !!input ? "Filled" : "Empty"; console.log(hasValue); // "Empty"`





# Stack and Heap Memory in JavaScript

JavaScript uses **stack** and **heap** memory to manage data during execution. Understanding these concepts helps explain how variables and objects are stored and accessed.

---

## Stack Memory

> [!NOTE] Click to expand/collapse details in Obsidian.

- **Definition**: A region of memory that stores **primitive values** and **function call frames**. It operates in a Last-In-First-Out (LIFO) manner.
- **Characteristics**:
    - Fixed size, fast access.
    - Stores variables with known, small sizes (e.g., numbers, booleans).
    - Each function call creates a "frame" with local variables, removed when the function finishes.
- **Why Use?**: Efficient for managing simple data and execution context.
- **Why Not?**: Limited space; large or complex data goes to the heap.

### Example of Stack Memory

javascript

CollapseWrapCopy

`function add(a, b) { let result = a + b; return result; } let x = 5; let y = 10; let sum = add(x, y); console.log(sum); // 15`

- **Explanation**:
    1. **Global Stack Frame**: x = 5, y = 10, and sum are stored in the global execution context on the stack.
    2. **Function Call**: When add(5, 10) is called, a new stack frame is pushed:
        - a = 5 (copy of x).
        - b = 10 (copy of y).
        - result = 15 (local variable).
    3. **Return**: result is returned, and the add frame is popped off the stack.
    4. **Back to Global**: sum = 15 is stored in the global frame.
- **Key Point**: Primitive values (x, y, sum) are copied by value and managed on the stack. No references are involved.

---

## Heap Memory

> [!NOTE] Click to expand/collapse details in Obsidian.

- **Definition**: A region of memory for storing **reference types** (e.g., objects, arrays) with dynamic sizes.
- **Characteristics**:
    - Larger, slower access than the stack.
    - Stores complex data; variables on the stack hold references (pointers) to heap locations.
    - Managed by JavaScript’s garbage collector (frees memory when no references remain).
- **Why Use?**: Necessary for dynamic, mutable, or large data.
- **Why Not?**: Slower and more memory-intensive than the stack.

### Example of Heap Memory

javascript

CollapseWrapCopy

`function createPerson(name, age) { let person = { name: name, age: age }; return person; } let p1 = createPerson("Alice", 25); let p2 = p1; p2.name = "Bob"; console.log(p1.name); // "Bob" console.log(p2.name); // "Bob"`

- **Explanation**:
    1. **Stack**:
        - p1 and p2 are variables on the stack, holding references (memory addresses) to the heap.
        - In createPerson, name, age, and person are on the function’s stack frame.
    2. **Heap**:
        - The object { name: "Alice", age: 25 } is created in the heap.
        - person points to this heap location, and p1 gets this reference when returned.
    3. **Reference Copy**: p2 = p1 copies the reference, not the object. Both p1 and p2 point to the same heap object.
    4. **Mutation**: Changing p2.name updates the heap object, affecting p1 too (same reference).
- **Key Point**: Reference types live in the heap, and stack variables only store pointers to them.

---

### Stack vs. Heap: When to Use?

|Aspect|Stack|Heap|
|---|---|---|
|Data Types|Primitives (Number, String, etc.)|Reference (Object, Array)|
|Access Speed|Fast|Slower|
|Size|Fixed, small|Dynamic, large|
|Memory Management|Automatic (scope-based)|Garbage-collected|
|Example Use|Local variables, counters|Complex data, shared objects|

- **Stack**: Use for simple, predictable data (e.g., loop counters).
- **Heap**: Use for structured or shared data (e.g., user profiles).

---

## String in Depth

> [!NOTE] Click to expand/collapse details in Obsidian.

- **Definition**: A primitive data type representing text, enclosed in single quotes ('), double quotes ("), or backticks (`) for template literals.
- **Storage**: Stored on the stack as a value, but behaves immutably (any "change" creates a new string).
- **Key Features**:
    - Immutable: Cannot be altered in place; methods return new strings.
    - Indexed: Characters can be accessed by index (0-based).
    - Methods: toLowerCase(), toUpperCase(), slice(), concat(), etc.

### String Example

javascript

CollapseWrapCopy

``let greeting = "Hello, World!"; let name = "Alice"; let message = `Hi, ${name}!`; // Template literal console.log(greeting.length); // 13 console.log(greeting[0]); // "H" console.log(greeting.toUpperCase()); // "HELLO, WORLD!" console.log(message); // "Hi, Alice!" console.log(greeting + " " + name); // "Hello, World! Alice"``

- **Explanation**:
    1. **Creation**: greeting and name are primitive strings on the stack.
    2. **Template Literal**: message uses backticks to embed name, creating a new string on the stack.
    3. **Access**: greeting[0] reads the first character; strings are indexed like arrays.
    4. **Method**: toUpperCase() returns a new string (original greeting unchanged due to immutability).
    5. **Concatenation**: + creates a new string combining greeting and name.
- **Why Use?**: Ideal for text display, user input, or static labels.
- **Why Not?**: Avoid for heavy manipulation (use arrays for mutable text operations).

---

## Number in Depth

> [!NOTE] Click to expand/collapse details in Obsidian.

- **Definition**: A primitive data type for integers and floating-point numbers, stored as 64-bit floating-point values (IEEE 754 standard).
- **Storage**: Lives on the stack as a value.
- **Key Features**:
    - Supports basic arithmetic (+, -, *, /, %).
    - Special values: Infinity, -Infinity, NaN.
    - Methods (via wrapper object): toFixed(), toPrecision(), etc.

### Number Example

javascript

CollapseWrapCopy

`let price = 19.99; let quantity = 3; let total = price * quantity; console.log(total); // 59.97 console.log(total.toFixed(2)); // "59.97" (string) console.log(1 / 0); // Infinity console.log("abc" * 2); // NaN console.log(Number.MAX_VALUE); // 1.7976931348623157e+308`

- **Explanation**:
    1. **Creation**: price and quantity are numbers on the stack.
    2. **Arithmetic**: total = price * quantity computes 59.97, stored as a new number on the stack.
    3. **Method**: toFixed(2) converts total to a string with 2 decimal places (returns "59.97").
    4. **Special Cases**:
        - 1 / 0 yields Infinity (beyond representable range).
        - "abc" * 2 results in NaN (invalid operation).
    5. **Constants**: Number.MAX_VALUE shows the largest representable number.
- **Why Use?**: Perfect for calculations, counters, or measurements.
- **Why Not?**: Avoid for very large integers (use BigInt) due to precision loss.

---

## Combined Example: Stack, Heap, String, Number

javascript

CollapseWrapCopy

`function processOrder(itemName, price, quantity) { let order = { name: itemName, // String unitPrice: price, // Number qty: quantity, // Number total: price * quantity // Number }; return order; } let product = "Book"; let cost = 15.50; let amount = 2; let myOrder = processOrder(product, cost, amount); console.log(myOrder.name); // "Book" console.log(myOrder.total); // 31 console.log(myOrder.total.toFixed(2)); // "31.00"`

- **Explanation**:
    1. **Stack**:
        - product (string), cost (number), and amount (number) are primitives on the global stack.
        - In processOrder, itemName, price, quantity, and total are on the function’s stack frame.
    2. **Heap**:
        - order is an object created in the heap; its reference is stored on the stack.
        - Properties (name, unitPrice, qty, total) are stored in the heap object.
    3. **String**: itemName is copied to order.name as a string.
    4. **Number**: price * quantity computes total (31), stored as a number in the heap object.
    5. **Output**: Accessing myOrder.total and using toFixed() shows number-to-string conversion.

---
# Prebuilt Functions in JavaScript

JavaScript provides a rich set of built-in (prebuilt) functions and methods attached to its core objects (e.g., String, Array, Date, Math). These simplify common tasks and can sometimes surprise you with their power or quirks.

---

## Most Commonly Used Prebuilt Functions

> [!NOTE] Click to expand/collapse details in Obsidian.

### console.log()

- **Purpose**: Logs data to the console for debugging or output.
- **Why Common?**: Essential for testing and understanding code behavior.

javascript

CollapseWrapCopy

`let name = "Alice"; console.log("Hello,", name); // Hello, Alice console.log(1 + 2); // 3`

- **Explanation**: Prints multiple arguments with spaces between them. Works with any data type.

### Date (and Related Methods)

- **Purpose**: Creates and manipulates date/time objects.
- **Common Methods**: getFullYear(), getMonth(), getDate(), getTime().

javascript

CollapseWrapCopy

`let now = new Date(); console.log(now); // 2025-03-31T12:00:00Z (example) console.log(now.getFullYear()); // 2025 console.log(now.getMonth()); // 2 (March, 0-based) console.log(now.getTime()); // Milliseconds since 1970-01-01`

- **Explanation**: new Date() gives the current date/time. Methods extract specific parts. Note: getMonth() is 0-based (0 = January).

### String.replace()

- **Purpose**: Replaces a substring with another string (first match by default).
- **Why Common?**: Great for text manipulation.

javascript

CollapseWrapCopy

`let text = "Hello, world!"; let newText = text.replace("world", "Alice"); console.log(newText); // "Hello, Alice!"`

- **Explanation**: Replaces only the first occurrence unless a regex with /g is used (e.g., replace(/l/g, "L")).

### Array.includes()

- **Purpose**: Checks if an array contains a specific value, returns true/false.
- **Why Common?**: Simple alternative to loops for searching.

javascript

CollapseWrapCopy

`let fruits = ["apple", "banana", "orange"]; console.log(fruits.includes("banana")); // true console.log(fruits.includes("grape")); // false`

- **Explanation**: Case-sensitive for strings. Uses strict equality (===).

### String.split()

- **Purpose**: Splits a string into an array based on a delimiter.
- **Why Common?**: Useful for parsing or tokenizing text.

javascript

CollapseWrapCopy

`let sentence = "I like to code"; let words = sentence.split(" "); console.log(words); // ["I", "like", "to", "code"] let csv = "apple,banana,orange"; console.log(csv.split(",")); // ["apple", "banana", "orange"]`

- **Explanation**: The delimiter (e.g., " ") is removed, and the result is an array of substrings.

### Array.splice()

- **Purpose**: Adds/removes elements from an array at a specified index, modifying the original array.
- **Why Common?**: Versatile for array manipulation.

javascript



`let numbers = [1, 2, 3, 4]; numbers.splice(1, 2, "a", "b"); // Start at index 1, remove 2, add "a", "b" console.log(numbers); // [1, "a", "b", 4]`

- **Explanation**: splice(start, deleteCount, ...items) changes the array in place. Returns removed elements if any.

---

## Amazing and "Crazy" Prebuilt Functions

> [!NOTE] Click to expand/collapse details in Obsidian.

### Math.random()

- **Purpose**: Generates a random number between 0 (inclusive) and 1 (exclusive).
- **Why Amazing?**: Foundation for randomness in games, simulations, etc.

javascript



`let rand = Math.random(); console.log(rand); // e.g., 0.7234519... // Random integer between 1 and 10 let dice = Math.floor(Math.random() * 10) + 1; console.log(dice); // e.g., 7`

- **Explanation**: Pseudo-random, not cryptographically secure. Pair with Math.floor() for integers.

### setTimeout() and setInterval()

- **Purpose**: Delays execution (setTimeout) or repeats it (setInterval) in milliseconds.
- **Why Crazy?**: Enables asynchronous behavior, but can lead to wild timing issues.

javascript



`setTimeout(() => console.log("Delayed!"), 2000); // Logs after 2 seconds let count = 0; let interval = setInterval(() => { console.log("Tick", count++); if (count === 3) clearInterval(interval); // Stops after 3 ticks }, 1000);`

- **Explanation**: setTimeout runs once; setInterval repeats until cleared. Timing isn’t exact due to JS’s single-threaded nature.

### Array.flat()

- **Purpose**: Flattens nested arrays into a single-level array.
- **Why Amazing?**: Simplifies complex data structures effortlessly.

javascript



`let nested = [1, [2, 3], [4, [5, 6]]]; console.log(nested.flat()); // [1, 2, 3, 4, [5, 6]] console.log(nested.flat(2)); // [1, 2, 3, 4, 5, 6]`

- **Explanation**: Default depth is 1; specify depth (e.g., flat(2)) for deeper nesting. Doesn’t modify the original.

### Object.freeze()

- **Purpose**: Makes an object immutable (cannot add, remove, or change properties).
- **Why Crazy?**: Unexpectedly strict—shallow freeze only!

javascript



`let obj = { name: "Alice", info: { age: 25 } }; Object.freeze(obj); obj.name = "Bob"; // Fails silently (or throws in strict mode) obj.info.age = 26; // Works! console.log(obj); // { name: "Alice", info: { age: 26 } }`

- **Explanation**: Freezes only the top level. Nested objects remain mutable unless frozen separately.

### String.match()

- **Purpose**: Matches a string against a regular expression, returning matches.
- **Why Amazing?**: Powerful for pattern matching, but regex can get wild.

javascript



`let text = "The year is 2025, not 1999!"; let years = text.match(/\d{4}/g); // Find all 4-digit numbers console.log(years); // ["2025", "1999"]`

- **Explanation**: With /g, returns all matches as an array. Without /g, returns first match with details (index, groups).

### Number.toLocaleString()

- **Purpose**: Formats a number based on a locale (e.g., currency, separators).
- **Why Crazy?**: Surprisingly versatile for internationalization.

javascript



`let price = 1234567.89; console.log(price.toLocaleString("en-US")); // "1,234,567.89" console.log(price.toLocaleString("de-DE")); // "1.234.567,89" console.log(price.toLocaleString("en-US", { style: "currency", currency: "USD" })); // "$1,234,567.89"`

- **Explanation**: Locale-aware formatting. Options like { style: "currency" } make it a hidden gem.

---

## When to Use Which Function?

|Task|Recommended Function|Why Use It?|
|---|---|---|
|Debugging|console.log()|Quick, universal output|
|Time handling|Date, setTimeout|Scheduling, timestamps|
|Text replacement|String.replace()|Simple substitutions|
|Array searching|Array.includes()|Fast presence check|
|Text parsing|String.split()|Break into manageable pieces|
|Array modification|Array.splice()|In-place edits|
|Randomness|Math.random()|Games, unique IDs|
|Deep array handling|Array.flat()|Simplify nested data|
|Immutability|Object.freeze()|Prevent accidental changes|

- **Most Used**: Stick to console.log, split, replace for everyday tasks.
- **Amazing Ones**: Explore flat, toLocaleString, match for creative solutions.

---

## Example Combining Functions

javascript



``function formatOrderSummary(items, date) { let orderDate = new Date(date); let itemList = items.split(","); // Split string into array let updatedList = itemList.splice(1, 1, "Book"); // Replace 2nd item let hasBook = updatedList.includes("Book"); // Check presence let summary = `Order on ${orderDate.toLocaleString("en-US", { dateStyle: "medium" })}: ${ updatedList.join(" & ") }`; console.log(summary); // e.g., "Order on Mar 31, 2025: apple & Book & orange" console.log("Includes Book?", hasBook); // "Includes Book?" true setTimeout(() => console.log("Order processed!"), 1000); // Delay } formatOrderSummary("apple,banana,orange", "2025-03-31");``

- **Explanation**:
    1. **split()**: Turns items string into an array.
    2. **splice()**: Replaces "banana" with "Book".
    3. **includes()**: Confirms "Book" is in the array.
    4. **toLocaleString()**: Formats the date nicely.
    5. **join()**: Combines array back into a string.
    6. **setTimeout()**: Adds a delayed message.

---

# Execution Context

In JavaScript, you write some code and it gets interpreted by the JavaScript engine. To understand what happens behind the scenes, you need to have an understanding of the basics. In this article, we will be focusing on the difference between scope, context, and execution context.

# **Scope**

> Since this article is a part of _Learn JavaScript Fundamentals_ series, check out the rest of the series via the links at the end of article if you would like to learn more about the types of scope or how you can use it.

For a short explanation, scope is closely related to the **visibility of variables**. It is common to confuse scope and context, but in reality they are completely different. With scope, you control the **accessibility of your variables**. Variables created in global scope can be accessed from any scope whereas the local variables are accessible within the function in which they are created.

# **Context**

Context is simply the value of “`this`”**,** the property of _execution context_ which will be explained later in this article**. It also refers to the object that a function/method belongs to.**

Value of `this` differs by how the function is called. In the global scope, value of `this` is always the [window](https://medium.com/swlh/javascript-fundamentals-global-scope-71ba5e48dbae) object.

console.log(this); // window

If the function is a method, the value of `this` is the object that the method belongs to. Of course, the `this` keyword is not that simple to understand. In this article, I am not going into the details of how the `this` keyword works, just stressing out what its relation is to scope, context, execution context. Understanding how `this` works is one of the most difficult and also most important topics in JavaScript. Luckily, there are plenty of resources. If you are unfamiliar, I suggest that you check them out.

- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [Javascript.info](https://javascript.info/object-methods)
- [Mastering The JavaScript This Keyword](https://www.thecodingdelight.com/javascript-this/)

The meaning of the `this` keyword depends on how and where the function is called, not where it is declared. Its meaning changes every time a function is called from different execution context. Why execution context? Because `this` keyword is actually a reference to _function’s current execution context._

# **Execution Context**

Execution context is something that you need to know to understand how JavaScript code runs. First of all, it is an **_abstract_** _concept_ that represents the _environment_ in which JavaScript runs. What happens inside the execution context is two things basically. The first is parsing the code line by line and the second is storing the variables and functions into the memory. In simple terms, it has two types:

- Global Execution Context
- Local Execution Context

**Global Execution Context** is the first thing that is created when you write JavaScript code. It is the default context.

**Local Execution Context** is created when you call a function (not defining a function).

Because it is an abstract concept, I am going to support it with an image.

![](https://miro.medium.com/v2/resize:fit:1050/1*CuL8xsqLb1GhpuHgmDKk0A.png)

Let’s walk through the image. When the JS engine starts reading your code, it creates the global execution context. It starts parsing line by line and adds your variables to memory also known as global variable environment.

Let’s say you have defined a function like this:

function adding(num) {  
  let number=num+2;  
  return number;  
}

If you call the function

adding(3);

While the engine is parsing, if it needs to execute the function, a new local execution context is created. In that execution context, parsing takes place and the `**number**` variable is added to local memory, and then parsing continues. After this, the engine returns to the previous execution context.

Exiting the local execution context and continuing parsing in the previous execution context is achieved with the `return` keyword. Those arrows in the image represents this cycle. Every time a function gets called, this happens again. Every function call results in a different local execution context.

To sum up briefly:

1. Global execution context is created first.
2. Whenever a function gets invoked, called, or executed, a new local execution context gets created.
3. The JavaScript engine starts parsing the code in the global execution context.
4. When the engine comes across a function call, it enters a local execution context and continues parsing in there.
5. When it handles function execution, it exits from that local execution context, returns to the previous one.

This brings us to another important concept. How does engine know which execution context to enter or exit? The answer is the **_Call Stack_**.

The Call Stack is a mechanism for the JavaScript Engine to keep track of execution contexts, which to enter, which to exit or which to return to. At the bottom of the stack is the global execution context. If a function gets invoked, we have a new local execution context and this one is pushed to the top of call stack. Once it’s done, it gets popped off. Let me explain it with an image:

![](https://miro.medium.com/v2/resize:fit:1050/1*lcTk7Ev0gp_H3Krup6G1EA.png)

So, what is going on here?

1. The engine starts parsing, the global execution context is automatically pushed to the call stack.
2. `Function1` gets invoked, a local execution context is pushed to the top of global execution context. The engine will continue to parse after `Function1` is executed.
3. Inside `Function1`, a new execution context is created and gets pushed to the top of call stack. After `Function2` is executed, it gets popped out from call stack.
4. `Function1` gets popped out after it finishes execution.
5. Finally, it returns to global execution context. The engine continues to parse and all of the code is executed.

# **Phases of Execution Context**

Throughout the article, you may be wondering how the execution context is created. Despite being abstract, it is a process and the engine follows some steps in the process.

Basically it has two phases:

1. Creation Phase
2. Execution Phase

In the _creation phase_, the engine first creates _Activation object_ or _Variable object_. This object consists of variables, arguments, function declarations. In this stage, they are assigned the value of `undefined`.

The arguments property is also an object (an array-like object) that has a length property and all arguments passed to the function call are stored in this object.

After that, the engine creates the scope chain. Each execution context knows its scope. It has a reference to its outer scopes all the way to the global scope, and the engine searches variables (if exists or not) starting from the current to the global scope. And this is called scope chain. The scope chain is a list of objects consisting of its own variable object and its parents’ variable objects.

Lastly, the value of `this` is determined. In the global context, its value is the `window`/`global` object whereas in every function call its value may be different.

In the _execution phase_, variables are assigned a value and the engine executes the code.

---

All JavaScript code needs to be hosted and run in some kind of environment. In most cases, that environment would be a web browser.

For any piece of JavaScript code to be executed in a web browser, a lot of processes take place behind the scenes. In this article, we'll take a look at everything that happens behind the scenes for JavaScript code to run in a web browser.

Before we dive in, here are some prerequisites to familiarize yourself with, because we'll use them often in this article.

- **Parser**: A Parser or Syntax Parser is a program that reads your code line-by-line. It understands how the code fits the syntax defined by the Programming Language and what it (the code) is expected to do.
- **JavaScript Engine**: A JavaScript engine is simply a computer program that receives JavaScript source code and compiles it to the binary instructions (machine code) that a CPU can understand. JavaScript engines are typically developed by web browser vendors, and each major browser has one. Examples include the [V8 engine](https://v8.dev/) for Google chrome, [SpiderMonkey](https://firefox-source-docs.mozilla.org/js/index.html) for Firefox, and [Chakra](https://en.wikipedia.org/wiki/Chakra_\(JScript_engine\)) for Internet Explorer.
- **Function Declarations**: These are functions that are assigned a name.

```javascript
function doSomething() { //here "doSomething" is the function's name
statements; 
}
```

- **Function Expressions**: These are anonymous functions, that is functions without a function name like `js function () { statements }`. They are usually used in statements, like assigning a function to a variable. `let someValue = function () { statements }`.

Now, that we've gotten those out of the way, let's dive in.

## **How JavaScript Code Gets Executed**

For does who don't know, the browser doesn't natively understand the high-level JavaScript code that we write in our applications. It needs to be converted into a format that the browser and our computers can understand – machine code.

While reading through HTML, if the browser encounters JavaScript code to run via a `<script>` tag or an attribute that contains JavaScript code like `onClick`, it sends it to its JavaScript engine.

The browser's JavaScript engine then creates a special environment to handle the transformation and execution of this JavaScript code. This environment is known as the **`Execution Context`**.

The Execution Context contains the code that's currently running, and everything that aids in its execution.

During the Execution Context run-time, the specific code gets parsed by a parser, the variables and functions are stored in memory, executable byte-code gets generated, and the code gets executed.

There are two kinds of Execution Context in JavaScript:

- Global Execution Context (GEC)
- Function Execution Context (FEC)

Let's take a detailed look at both.

### **Global Execution Context (GEC)**

Whenever the JavaScript engine receives a script file, it first creates a default Execution Context known as the **`Global Execution Context (GEC)`**.

The GEC is the base/default Execution Context where all JavaScript code that is **not inside of a function** gets executed.

> For every JavaScript file, there can only be one GEC.

### **Function Execution Context (FEC)**

Whenever a function is called, the JavaScript engine creates a different type of Execution Context known as a Function Execution Context (FEC) within the GEC to evaluate and execute the code within that function.

Since every function call gets its own FEC, there can be more than one FEC in the run-time of a script.

## **How are Execution Contexts Created?**

Now that we are aware of what Execution Contexts are, and the different types available, let's look at how the are created.

The creation of an Execution Context (GEC or FEC) happens in two phases:

1. Creation Phase
2. Execution Phase

### Creation Phase

In the creation phase, the Execution Context is first associated with an Execution Context Object (ECO). The Execution Context Object stores a lot of important data which the code in the Execution Context uses during its run-time.

The creation phase occurs in 3 stages, during which the properties of the Execution Context Object are defined and set. These stages are:

1. Creation of the Variable Object (VO)
2. Creation of the Scope Chain
3. Setting the value of the `this` keyword

Let us go over each phase in detail.

### **Creation Phase: Creation Of The Variable Object (VO)**

The Variable Object (VO) is an object-like container created within an Execution Context. It stores the variables and function declarations defined within that Execution Context.

In the GEC, for each variable declared with the `var` keyword, a property is added to VO that points to that variable and is set to 'undefined'.

Also, for every function declaration, a property is added to the VO, pointing to that function, and that property is stored in memory. This means that all the function declarations will be stored and made accessible inside the VO, even before the code starts running.

The FEC, on the other hand, does not construct a VO. Rather, it generates an array-like object called the 'argument' object, which includes all of the arguments supplied to the function. Learn more about the argument object [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments).

This process of storing variables and function declaration in memory prior to the execution of the code is known as **Hoisting**. Since this is an important concept, we'll talk about it briefly before moving on to the next stage.

### **Hoisting in JavaScript**

Function and variable declarations are hoisted in JavaScript. This means that they are stored in memory of the current Execution Context's VO and made available within the Execution Context even before the execution of the code begins.

#### **Function Hoisting**

In most scenarios when building an application, developers can choose to define functions at the top of a script, and only later call them down the code, like so:

![Image](https://www.freecodecamp.org/news/content/images/2022/08/function-before-call.png)

However, due to hoisting, the opposite will still work. Where we can call functions first then define them later down the script.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/call-before-function.png)

In the code above, the `getAge` function declaration will be stored in the memory of the VO, making it available for use even before it is defined.

#### **Variable Hoisting**

Variables initialized with the `var` keyword are stored in the memory of the current Execution Context's VO as a property, and initialized with the value `undefined`. This means, unlike functions, trying to access the value of the variable before it is defined will result in `undefined`.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/log-before-variable.png)

#### **Ground Rules of Hoisting**

Hoisting only works for function declarations, not expressions. Here is an example of a function expression where the code execution will break.

```javascript
getAge(1990); 
var getAge = function (yearOfBirth) {
console.log(new Date().getFullYear - yearOfBirth) 
};
```

The code execution breaks, because with function expressions, `getAge` will be hoisted as a variable not as a function. And with variable hoisting, its value will be set to `undefined`. That's why we get the error:

![Image](https://www.freecodecamp.org/news/content/images/2022/08/getAge-error.png)

Also, variable hoisting does not work for variables initialized with the `let` or `const` keyword. Trying to access a variable ahead of declaration and use the `let` and `const` keywords to declare it later will result in a `ReferenceError`.

In this case, they will be hoisted but not assigned with the default value of `undefined`. `js console.log(name); let name = "Victor";` will throw the error:

![Image](https://www.freecodecamp.org/news/content/images/2022/08/name-not-defined-error.png)

### **Creation Phase: Creation of The Scope Chain**

After the creation of the Variable Object (VO) comes the creation of the Scope Chain as the next stage in the creation phase of an Execution Context.

Scope in JavaScript is a mechanism that determines how accessible a piece of code is to other parts of the codebase. Scope answers the questions: from where can a piece of code be accessed? From where can't it be accessed? What can access it, and what can't?

Each Function Execution Context creates its scope: the space/environment where the variables and functions it defined can be accessed via a process called Scoping.

This means the position of something within a codebase, that is, where a piece of code is located.

When a function is defined in another function, the inner function has access to the code defined in that of the outer function, and that of its parents. This behavior is called **lexical scoping**.

However, the outer function does not have access to the code within the inner function.

This concept of scope brings up an associate phenomenon in JavaScript called closures. These are when inner functions that always get access to the code associated with the outer functions, even after the execution of the outer functions is complete. You can learn more closures [here](https://www.freecodecamp.org/news/scope-and-closures-in-javascript/).

Let's look at some examples to get a better understanding:

![first-scope.png](https://www.freecodecamp.org/news/content/images/2022/02/first-scope.png)

- On the right is the Global Scope. It is the default scope created when a `.js` script is loaded and is accessible from all functions throughout the code.
- The red box is the scope of the `first` function, which defines the variable `b = 'Hello!'` and the `second` function.

![Image](https://www.freecodecamp.org/news/content/images/2022/02/second-scope.png)

- In green is the scope of the `second` function. There is a `console.log` statement which is to print the variables `a`, `b` and `c`.

Now the variables `a` and `b` aren't defined in the `second` function, only `c`. However, due to lexical scoping, it has access to the scope of the function it sits in and that of its parent.

In running the code, the JS engine will not find the variable `b` in the scope of the `second` function. So, it looks up into the scope of its parents, starting with the `first` function. There it finds the variable `b = 'Hello'`. It goes back to the `second` function and resolves the `b` variable there with it.

Same process for the `a` variable. The JS engine looks up through the scope of all its parents all the way to the scope of the GEC, resolving its value in the `second` function.

This idea of the JavaScript engine traversing up the scopes of the execution contexts that a function is defined in in order to resolve variables and functions invoked in them is called the **scope chain**.

![Scope chain](https://www.freecodecamp.org/news/content/images/2022/02/scope-chain.png)

Only when the JS engine can't resolve a variable within the scope chain does it stop executing and throws an error.

However, this doesn't work backward. That is, the global scope will never have access to the inner function’s variables unless they are `returned` from the function.

The scope chain works as a one-way glass. You can see the outside, but people from the outside cannot see you.

And that is why the red arrow in the image above is pointing upwards because that is the only direction the scope chains goes.

### Creation Phase: Setting The Value of The "this" Keyword

The next and final stage after scoping in the creation phase of an Execution Context is setting the value of the `this` keyword.

The JavaScript `this` keyword refers to the scope where an Execution Context belongs.

Once the scope chain is created, the value of `'this'` is initialized by the JS engine.

##### **`"this"` in The Global Context**

In the GEC (outside of any function and object), `this` refers to the global object — which is the `window` object.

Thus, function declarations and variables initialized with the `var` keyword get assigned as properties and methods to the global object – `window` object.

This means that declaring variables and functions outside of any function, like this:

```javascript
var occupation = "Frontend Developer"; 

function addOne(x) { 
    console.log(x + 1) 
}
```

Is exactly the same as:

```javascript
window.occupation = "Frontend Developer"; 
window.addOne = (x) => { 
console.log(x + 1)
};
```

Functions and variables in the GEC get attached as methods and properties to the window object. That's why the snippet below will return true.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/variables-attached-as-properties-to-the-global-object.png)

##### **`"this"` in Functions**

In the case of the FEC, it doesn't create the `this` object. Rather, it get's access to that of the environment it is defined in.

Here that'll be the `window` object, as the function is defined in the GEC:

```javascript
var msg = "I will rule the world!"; 

function printMsg() { 
    console.log(this.msg); 
} 

printMsg(); // logs "I will rule the world!" to the console.
```

In objects, the `this` keyword doesn't point to the GEC, but to the object itself. Referencing `this` within an object will be the same as:

`theObject.thePropertyOrMethodDefinedInIt;`

Consider the code example below:

```js
var msg = "I will rule the world!"; 
const Victor = {
    msg: "Victor will rule the world!", 
    printMsg() { console.log(this.msg) }, 
}; 

Victor.printMsg(); // logs "Victor will rule the world!" to the console.
```

The code logs `"Victor will rule the world!"` to the console, and not `"I will rule the world!"` because in this case, the value of the `this` keyword the function has access to is that of the object it is defined in, not the global object.

With the value of the `this` keyword set, all the properties of the Execution Context Object have been defined. Leading to the end of the creation phase, now the JS engine moves on to the execution phase.

### **The Execution Phase**

Finally, right after the creation phase of an Execution Context comes the execution phase. This is the stage where the actual code execution begins.

Up until this point, the VO contained variables with the values of `undefined`. If the code is run at this point it is bound to return errors, as we can't work with undefined values.

At this stage, the JavaScript engine reads the code in the current Execution Context once more, then updates the VO with the actual values of these variables. Then the code is parsed by a parser, gets transpired to executable byte code, and finally gets executed.

## **JavaScript Execution Stack**

The Execution Stack, also known as the **Call Stack**, keeps track of all the Execution Contexts created during the life cycle of a script.

JavaScript is a single-threaded language, which means that it is capable of only executing a single task at a time. Thus, when other actions, functions, and events occur, an Execution Context is created for each of these events. Due to the single-threaded nature of JavaScript, a stack of piled-up execution contexts to be executed is created, known as the `Execution Stack`.

When scripts load in the browser, the Global context is created as the default context where the JS engine starts executing code and is placed at the bottom of the execution stack.

The JS engine then searches for function calls in the code. For each function call, a new FEC is created for that function and is placed on top of the currently executing Execution Context.

The Execution Context at the top of the Execution stack becomes the active Execution Context, and will always get executed first by the JS engine.

As soon as the execution of all the code within the active Execution Context is done, the JS engine pops out that particular function's Execution Context of the execution stack, moves towards the next below it, and so on.

To understand the working process of the execution stack, consider the code example below:

```javascript
var name = "Victor";

function first() {
  var a = "Hi!";
  second();
  console.log(`${a} ${name}`);
}

function second() {
  var b = "Hey!";
  third();
  console.log(`${b} ${name}`);
}

function third() {
  var c = "Hello!";
  console.log(`${c} ${name}`);
}

first();
```

First, the script is loaded into the JS engine.

After it, the JS engine creates the GEC and places it at the base of the execution stack.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/global-context.png)

The `name` variable is defined outside of any function, so it is in the GEC and stored in it's VO.

The same process occurs for the `first`, `second`, and `third` functions.

Don't get confused as to why they functions are still in the GEC. Remember, the GEC is only for JavaScript code (variables and functions) that are **not inside of any function**. Because they were not defined within any function, the function declarations are in the GEC. Make sense now 😃?

When the JS engine encounters the `first` function call, a new FEC is created for it. This new context is placed on top of the current context, forming the so-called `Execution Stack`.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/execution-context-1.png)

For the duration of the `first` function call, its Execution Context becomes the active context where JavaScript code is first executed.

In the `first` function the variable `a = 'Hi!'` gets stored in its FEC, not in the GEC.

Next, the `second` function is called within the `first` function.

The execution of the `first` function will be paused due to the single-threaded nature of JavaScript. It has to wait until its execution, that is the `second` function, is complete.

Again the JS engine sets up a new FEC for the `second` function and places it at the top of the stack, making it the active context.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/execution-context-2.png)

The `second` function becomes the active context, the variable `b = 'Hey!';` gets store in its FEC, and the `third` function is invoked within the `second` function. Its FEC is created and put on top of the execution stack.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/execution-context-3.png)

Inside of the `third` function the variable `c = 'Hello!'` gets stored in its FEC and the message `Hello! Victor` gets logged to the console.

Hence the function has performed all its tasks and we say it `returns`. Its FEC gets removed from the top of the stack and the FEC of the `second` function which called the `third` function gets back to being the active context.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/execution-context-2-1.png)

Back in the `second` function, the message `Hey! Victor` gets logged to the console. The function completes its task, `returns`, and its Execution Context gets popped off the call stack.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/execution-context-1-1.png)

When the first function gets executed completely, the execution stack of the first function popped out from the stack. Hence, the control reaches back to the GEC of the code.

![Image](https://www.freecodecamp.org/news/content/images/2022/08/global-context-1.png)

And lastly, when the execution of the entire code gets completed, the JS engine removes the GEC from the current stack.

## **Global Execution Context VS. Function Execution Context in JavaScript**

Since you've read all the way until this section, let's summarize the key points between the GEC and the FEC with the table below.

|GLOBAL EXECUTION CONTEXT|Function Execution Context|
|---|---|
|Creates a Global Variable object that stores function and variables declarations.|Doesn't create a Global Variable object. Rather, it creates an argument object that stores all the arguments passed to the function.|
|Creates the `this` object that stores all the variables and functions in the Global scope as methods and properties.|Doesn't create the `this` object, but has access to that of the environment in which it is defined. Usually the `window` object.|
|Can't access the code of the Function contexts defined in it|Due to scoping, has access to the code(variables and functions) in the context it is defined and that of its parents|
|Sets up memory space for variables and functions defined globally|Sets up memory space only for variables and functions defined within the function.|

## **Conclusion**

JavaScript's Execution Context is the basis for understanding many other fundamental concepts correctly.

The Execution Context (GEC and FEC), and the call stack are the processes carried out under the hood by the JS engine that let our code run.

Hope now you have a better understanding in which order your functions/code run and how JavaScript Engine treats them.

As a developer, having a good understanding of these concepts helps you:

- Get a decent understanding of the ins and outs of the language.
- Get a good grasp of a language’s underlying/core concepts.
- Write clean, maintainable, and well-structured code, introducing fewer bugs into production.


---

