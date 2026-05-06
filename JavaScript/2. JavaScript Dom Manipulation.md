
# 🌐 JavaScript DOM — Complete Tutorial

  

> **DOM** stands for **Document Object Model**. It is a programming interface that represents an HTML (or XML) document as a tree of objects, allowing JavaScript to dynamically read, add, modify, or delete elements and their content.

  

---

  

## 📚 Table of Contents

  

1. [What is the DOM?](#1-what-is-the-dom)

2. [DOM Tree Structure](#2-dom-tree-structure)

3. [Node Types](#3-node-types)

4. [Selecting Elements](#4-selecting-elements)

5. [Traversing the DOM](#5-traversing-the-dom)

6. [Manipulating Elements](#6-manipulating-elements)

7. [Working with Attributes](#7-working-with-attributes)

8. [Working with CSS Styles & Classes](#8-working-with-css-styles--classes)

9. [Creating & Inserting Elements](#9-creating--inserting-elements)

10. [Removing & Replacing Elements](#10-removing--replacing-elements)

11. [Event Handling](#11-event-handling)

12. [Event Propagation — Bubbling & Capturing](#12-event-propagation--bubbling--capturing)

13. [Event Delegation](#13-event-delegation)

14. [DOM Content Loading](#14-dom-content-loading)

15. [Forms & Input Handling](#15-forms--input-handling)

16. [Working with the `dataset` API](#16-working-with-the-dataset-api)

17. [DOM Performance Best Practices](#17-dom-performance-best-practices)

18. [Virtual DOM vs Real DOM](#18-virtual-dom-vs-real-dom)

19. [Quick Reference Cheat Sheet](#19-quick-reference-cheat-sheet)

  

---

  

## 1. What is the DOM?

  

When a browser loads an HTML page, it parses the HTML and builds an internal representation called the **DOM Tree**. JavaScript can then interact with this tree via the global `document` object.

  

```html

<!DOCTYPE html>

<html>

  <head><title>My Page</title></head>

  <body>

    <h1 id="heading">Hello, World!</h1>

    <p class="intro">Welcome to DOM tutorial.</p>

  </body>

</html>

```

  

JavaScript sees this as a live, manipulable object hierarchy.

  

```js

console.log(document.title);        // "My Page"

console.log(document.body);         // <body>...</body>

```

  

---

  

## 2. DOM Tree Structure

  

```

document

└── html  (root element)

    ├── head

    │   └── title  → "My Page"

    └── body

        ├── h1#heading  → "Hello, World!"

        └── p.intro     → "Welcome to DOM tutorial."

```

  

Key objects in the tree:

  

| Object | Description |

|--------|-------------|

| `document` | Entry point to the DOM |

| `document.documentElement` | The `<html>` element |

| `document.head` | The `<head>` element |

| `document.body` | The `<body>` element |

  

---

  

## 3. Node Types

  

Everything in the DOM is a **Node**. The most important node types are:

  

| Node Type | Constant | Example |

|-----------|----------|---------|

| Element Node | `Node.ELEMENT_NODE` (1) | `<div>`, `<p>` |

| Text Node | `Node.TEXT_NODE` (3) | Text inside elements |

| Comment Node | `Node.COMMENT_NODE` (8) | `<!-- comment -->` |

| Document Node | `Node.DOCUMENT_NODE` (9) | `document` itself |

  

```js

const h1 = document.querySelector('h1');

console.log(h1.nodeType);   // 1  (ELEMENT_NODE)

console.log(h1.nodeName);   // "H1"

console.log(h1.nodeValue);  // null (elements have no value)

  

const textNode = h1.firstChild;

console.log(textNode.nodeType);  // 3  (TEXT_NODE)

console.log(textNode.nodeValue); // "Hello, World!"

```

  

---

  

## 4. Selecting Elements

  

### 4.1 `getElementById`

  

Returns a **single element** with the matching `id`.

  

```js

const heading = document.getElementById('heading');

console.log(heading); // <h1 id="heading">Hello, World!</h1>

```

  

### 4.2 `getElementsByClassName`

  

Returns a **live HTMLCollection** of elements with the matching class.

  

```js

const items = document.getElementsByClassName('intro');

// HTMLCollection — live (auto-updates if DOM changes)

```

  

### 4.3 `getElementsByTagName`

  

Returns a **live HTMLCollection** of elements with the given tag name.

  

```js

const paragraphs = document.getElementsByTagName('p');

```

  

### 4.4 `querySelector` ⭐ (Most Used)

  

Returns the **first element** matching a CSS selector.

  

```js

const el = document.querySelector('#heading');       // by id

const el2 = document.querySelector('.intro');        // by class

const el3 = document.querySelector('ul > li:first-child'); // complex CSS

```

  

### 4.5 `querySelectorAll` ⭐

  

Returns a **static NodeList** of ALL elements matching a CSS selector.

  

```js

const allParas = document.querySelectorAll('p');

allParas.forEach(p => console.log(p.textContent));

```

  

> **Live vs Static:**  

> `getElementsByClassName` / `getElementsByTagName` return **live** collections (they update automatically).  

> `querySelectorAll` returns a **static** snapshot.

  

---

  

## 5. Traversing the DOM

  

Once you have an element, you can navigate the tree using these properties:

  

### 5.1 Parent

  

```js

const p = document.querySelector('p');

console.log(p.parentNode);    // <body>

console.log(p.parentElement); // <body>  (same, but returns null for non-element parents)

```

  

### 5.2 Children

  

```js

const body = document.body;

  

// All child nodes (includes text nodes & comments)

console.log(body.childNodes);  // NodeList

  

// Only element children

console.log(body.children);    // HTMLCollection

console.log(body.firstElementChild);  // first child element

console.log(body.lastElementChild);   // last child element

console.log(body.childElementCount);  // number of child elements

```

  

### 5.3 Siblings

  

```js

const h1 = document.querySelector('h1');

console.log(h1.nextElementSibling);     // <p class="intro">

console.log(h1.previousElementSibling); // null (h1 is first)

```

  

---

  

## 6. Manipulating Elements

  

### 6.1 Reading & Writing Text Content

  

```js

const p = document.querySelector('p');

  

// Read

console.log(p.textContent);  // "Welcome to DOM tutorial."

console.log(p.innerText);    // Similar, but respects CSS visibility

  

// Write

p.textContent = 'Updated content!';

```

  

> **`textContent` vs `innerText`:**  

> `textContent` gets ALL text including hidden elements; `innerText` gets only visible rendered text.

  

### 6.2 Reading & Writing HTML Content

  

```js

const div = document.querySelector('div');

  

// Read

console.log(div.innerHTML);  // "<p>Hello</p>"

  

// Write (parses the string as HTML)

div.innerHTML = '<strong>Bold Text</strong>';

  

// ⚠️ Never use innerHTML with untrusted user input — risk of XSS attacks!

```

  

### 6.3 `outerHTML`

  

Includes the element tag itself:

  

```js

console.log(p.outerHTML);  // "<p class="intro">Welcome to DOM tutorial.</p>"

```

  

---

  

## 7. Working with Attributes

  

### 7.1 `getAttribute` / `setAttribute` / `removeAttribute`

  

```js

const link = document.querySelector('a');

  

// Get

link.getAttribute('href');    // "https://example.com"

  

// Set

link.setAttribute('href', 'https://google.com');

link.setAttribute('target', '_blank');

  

// Remove

link.removeAttribute('target');

  

// Check existence

link.hasAttribute('href');  // true

```

  

### 7.2 Direct Property Access

  

Many attributes have direct properties:

  

```js

const img = document.querySelector('img');

img.src   = 'photo.jpg';

img.alt   = 'A photo';

img.width = 300;

  

const input = document.querySelector('input');

input.value    = 'Hello';

input.disabled = true;

input.checked  = true;

```

  

### 7.3 `id`, `className`, `tagName`

  

```js

const el = document.querySelector('#heading');

console.log(el.id);        // "heading"

console.log(el.tagName);   // "H1"

console.log(el.className); // "highlight bold"  (string of classes)

```

  

---

  

## 8. Working with CSS Styles & Classes

  

### 8.1 Inline Styles via `.style`

  

```js

const box = document.querySelector('.box');

box.style.color           = 'red';

box.style.backgroundColor = '#f0f0f0';  // camelCase for CSS properties

box.style.fontSize        = '18px';

box.style.display         = 'none';    // hide an element

```

  

### 8.2 `classList` API ⭐ (Best Practice)

  

Rather than toggling `className` strings manually, use `classList`:

  

```js

const el = document.querySelector('.card');

  

el.classList.add('active');           // Add a class

el.classList.remove('active');        // Remove a class

el.classList.toggle('active');        // Toggle on/off

el.classList.contains('active');      // Check — true/false

el.classList.replace('old', 'new');   // Replace a class

```

  

### 8.3 `getComputedStyle`

  

Get the final computed styles (including from stylesheets):

  

```js

const el = document.querySelector('p');

const styles = window.getComputedStyle(el);

console.log(styles.fontSize);    // "16px"

console.log(styles.color);       // "rgb(0, 0, 0)"

```

  

---

  

## 9. Creating & Inserting Elements

  

### 9.1 `createElement`

  

```js

const newDiv = document.createElement('div');

newDiv.textContent = 'I am a new div!';

newDiv.classList.add('box');

```

  

### 9.2 `appendChild`

  

Appends as the **last child**:

  

```js

document.body.appendChild(newDiv);

```

  

### 9.3 `insertBefore`

  

```js

const parent = document.querySelector('ul');

const newLi  = document.createElement('li');

newLi.textContent = 'New Item';

const firstLi = parent.querySelector('li');

parent.insertBefore(newLi, firstLi); // Insert before the first li

```

  

### 9.4 `insertAdjacentHTML` / `insertAdjacentElement` ⭐

  

More powerful insertion with position strings:

  

| Position | Description |

|----------|-------------|

| `'beforebegin'` | Before the element itself |

| `'afterbegin'` | Inside element, before first child |

| `'beforeend'` | Inside element, after last child |

| `'afterend'` | After the element itself |

  

```js

const h1 = document.querySelector('h1');

h1.insertAdjacentHTML('afterend', '<p>Inserted after h1</p>');

h1.insertAdjacentHTML('beforebegin', '<p>Inserted before h1</p>');

```

  

### 9.5 `append` / `prepend` (Modern API)

  

```js

const ul = document.querySelector('ul');

  

// append — adds at end, supports multiple nodes and strings

ul.append('Some text', document.createElement('li'));

  

// prepend — adds at beginning

ul.prepend(newLi);

```

  

### 9.6 `cloneNode`

  

```js

const original = document.querySelector('.card');

const copy = original.cloneNode(true);  // true = deep clone (includes children)

document.body.appendChild(copy);

```

  

---

  

## 10. Removing & Replacing Elements

  

### 10.1 `remove()`

  

```js

const el = document.querySelector('.old-banner');

el.remove();  // removes the element from the DOM

```

  

### 10.2 `removeChild()`

  

```js

const parent = document.querySelector('ul');

const child  = document.querySelector('ul li:last-child');

parent.removeChild(child);

```

  

### 10.3 `replaceChild()`

  

```js

const newEl = document.createElement('h2');

newEl.textContent = 'New Heading';

const oldEl = document.querySelector('h1');

document.body.replaceChild(newEl, oldEl);

```

  

### 10.4 `replaceWith()` (Modern)

  

```js

const oldEl = document.querySelector('h1');

const newEl = document.createElement('h2');

newEl.textContent = 'Replaced!';

oldEl.replaceWith(newEl);

```

  

---

  

## 11. Event Handling

  

Events are actions that happen in the browser — user clicks, key presses, window resizes, etc.

  

### 11.1 `addEventListener` ⭐ (Recommended)

  

```js

const btn = document.querySelector('#myBtn');

  

btn.addEventListener('click', function(event) {

  console.log('Button clicked!');

  console.log(event);           // Event object

  console.log(event.target);    // The element that triggered the event

  console.log(event.type);      // "click"

});

```

  

Using an arrow function:

  

```js

btn.addEventListener('click', (e) => {

  e.preventDefault();   // Prevent default browser action (e.g., form submit)

  console.log('Clicked with arrow function');

});

```

  

### 11.2 `removeEventListener`

  

```js

function handleClick(e) {

  console.log('Clicked!');

}

btn.addEventListener('click', handleClick);

btn.removeEventListener('click', handleClick); // Must pass the same function reference

```

  

### 11.3 Inline Event Handlers (Avoid)

  

```html

<!-- Avoid — hard to maintain -->

<button onclick="alert('clicked')">Click Me</button>

```

  

### 11.4 Common Event Types

  

| Category | Events |

|----------|--------|

| **Mouse** | `click`, `dblclick`, `mousedown`, `mouseup`, `mouseover`, `mouseout`, `mousemove` |

| **Keyboard** | `keydown`, `keyup`, `keypress` |

| **Form** | `submit`, `change`, `input`, `focus`, `blur`, `reset` |

| **Window** | `load`, `DOMContentLoaded`, `resize`, `scroll`, `unload` |

| **Touch** | `touchstart`, `touchend`, `touchmove` |

| **Drag** | `dragstart`, `drag`, `dragend`, `dragover`, `drop` |

| **Clipboard** | `copy`, `cut`, `paste` |

  

### 11.5 The Event Object

  

```js

document.addEventListener('keydown', function(e) {

  console.log(e.key);       // "Enter", "a", "Shift", etc.

  console.log(e.code);      // "KeyA", "Enter", etc.

  console.log(e.ctrlKey);   // true if Ctrl held

  console.log(e.shiftKey);  // true if Shift held

  console.log(e.altKey);    // true if Alt held

});

  

document.addEventListener('click', function(e) {

  console.log(e.clientX, e.clientY); // Mouse position relative to viewport

  console.log(e.pageX, e.pageY);     // Mouse position relative to page

});

```

  

---

  

## 12. Event Propagation — Bubbling & Capturing

  

When an event fires on an element, it travels through three phases:

  

```

Capture phase (top → target)

    ↓

Target phase

    ↓

Bubble phase (target → top)

```

  

### 12.1 Bubbling (Default)

  

Events bubble **up** from the target to the root.

  

```html

<div id="parent">

  <button id="child">Click Me</button>

</div>

```

  

```js

document.getElementById('parent').addEventListener('click', () => {

  console.log('Parent clicked');  // Fires AFTER child handler

});

  

document.getElementById('child').addEventListener('click', () => {

  console.log('Child clicked');   // Fires FIRST

});

// Output: "Child clicked" then "Parent clicked"

```

  

### 12.2 Stopping Propagation

  

```js

document.getElementById('child').addEventListener('click', (e) => {

  e.stopPropagation();  // Prevent event from bubbling up

  console.log('Child only');

});

```

  

### 12.3 Capturing Phase

  

Pass `true` as the third argument to listen in the capturing phase:

  

```js

document.getElementById('parent').addEventListener('click', () => {

  console.log('Parent — CAPTURE phase');

}, true);  // ← capture: true

```

  

---

  

## 13. Event Delegation

  

Instead of adding event listeners to many child elements, add **one listener to the parent** and use `event.target` to identify which child was clicked.

  

```html

<ul id="list">

  <li>Item 1</li>

  <li>Item 2</li>

  <li>Item 3</li>

</ul>

```

  

```js

// ✅ Efficient — one listener handles all <li> clicks

document.getElementById('list').addEventListener('click', function(e) {

  if (e.target.tagName === 'LI') {

    console.log('Clicked:', e.target.textContent);

    e.target.classList.toggle('selected');

  }

});

```

  

**Benefits:**

- Works for dynamically added elements

- Fewer event listeners = better performance

- Less memory usage

  

---

  

## 14. DOM Content Loading

  

### 14.1 `DOMContentLoaded`

  

Fires when the HTML has been fully parsed (before images/CSS load).

  

```js

document.addEventListener('DOMContentLoaded', function() {

  // DOM is ready — safe to query elements

  const btn = document.querySelector('#btn');

  btn.addEventListener('click', handler);

});

```

  

### 14.2 `window.onload`

  

Fires when **everything** (images, scripts, CSS) has loaded.

  

```js

window.addEventListener('load', function() {

  console.log('Full page loaded including images');

});

```

  

### 14.3 Script Placement

  

Best practice — place scripts at the bottom of `<body>` or use `defer`:

  

```html

<!-- defer: script runs after HTML is parsed -->

<script src="app.js" defer></script>

  

<!-- async: script runs as soon as it downloads -->

<script src="analytics.js" async></script>

```

  

---

  

## 15. Forms & Input Handling

  

### 15.1 Accessing Form Elements

  

```html

<form id="myForm">

  <input type="text" id="username" name="username" />

  <input type="password" id="password" name="password" />

  <button type="submit">Login</button>

</form>

```

  

```js

const form  = document.getElementById('myForm');

const uname = document.getElementById('username');

const pass  = document.getElementById('password');

  

// Read value

console.log(uname.value);

  

// Set value

uname.value = 'JohnDoe';

  

// Focus / Blur

uname.focus();

uname.blur();

```

  

### 15.2 Handling Form Submit

  

```js

form.addEventListener('submit', function(e) {

  e.preventDefault();  // Stop page reload

  

  const username = uname.value.trim();

  const password = pass.value;

  

  if (!username) {

    alert('Username is required!');

    return;

  }

  

  console.log('Submitting:', username);

  // Send to server, etc.

});

```

  

### 15.3 `input` vs `change` Events

  

```js

uname.addEventListener('input', (e) => {

  // Fires on EVERY keystroke

  console.log('Current value:', e.target.value);

});

  

uname.addEventListener('change', (e) => {

  // Fires when field loses focus AND value changed

  console.log('Final value:', e.target.value);

});

```

  

### 15.4 Checkboxes & Radio Buttons

  

```js

const checkbox = document.querySelector('input[type="checkbox"]');

console.log(checkbox.checked);   // true / false

checkbox.checked = true;

  

const radios = document.querySelectorAll('input[name="gender"]');

radios.forEach(radio => {

  if (radio.checked) console.log('Selected:', radio.value);

});

```

  

### 15.5 Select Dropdowns

  

```js

const select = document.querySelector('select');

console.log(select.value);           // currently selected value

console.log(select.selectedIndex);   // index of selected option

  

select.addEventListener('change', () => {

  console.log('Selected:', select.value);

});

```

  

---

  

## 16. Working with the `dataset` API

  

HTML `data-*` attributes let you embed custom data in elements, accessible via `dataset`.

  

```html

<div id="user" data-user-id="42" data-role="admin" data-last-login="2024-01-15">

  John Doe

</div>

```

  

```js

const userEl = document.getElementById('user');

  

// Read (camelCase: data-user-id → userId)

console.log(userEl.dataset.userId);    // "42"

console.log(userEl.dataset.role);      // "admin"

console.log(userEl.dataset.lastLogin); // "2024-01-15"

  

// Write

userEl.dataset.userId = '99';

userEl.dataset.newProp = 'value';      // Creates data-new-prop attribute

  

// Delete

delete userEl.dataset.role;

```

  

---

  

## 17. DOM Performance Best Practices

  

### 17.1 Minimize DOM Queries — Cache References

  

```js

// ❌ Bad — queries DOM on every iteration

for (let i = 0; i < 1000; i++) {

  document.getElementById('counter').textContent = i;

}

  

// ✅ Good — cache the reference

const counter = document.getElementById('counter');

for (let i = 0; i < 1000; i++) {

  counter.textContent = i;

}

```

  

### 17.2 Batch DOM Changes with `DocumentFragment`

  

A `DocumentFragment` is an in-memory container. Changes to it don't trigger reflows until you insert it.

  

```js

const fragment = document.createDocumentFragment();

  

for (let i = 0; i < 100; i++) {

  const li = document.createElement('li');

  li.textContent = `Item ${i + 1}`;

  fragment.appendChild(li);  // No reflow yet

}

  

document.querySelector('ul').appendChild(fragment); // One single reflow

```

  

### 17.3 Avoid Layout Thrashing

  

Reading and writing layout properties alternately forces the browser to recalculate layout:

  

```js

// ❌ Bad — causes layout thrashing

const width  = el.offsetWidth;   // Read (forces layout)

el.style.width = width + 10 + 'px'; // Write

  

const height = el.offsetHeight;  // Read (forces layout AGAIN)

el.style.height = height + 10 + 'px'; // Write

  

// ✅ Good — batch reads, then batch writes

const width  = el.offsetWidth;

const height = el.offsetHeight;

el.style.width  = width + 10 + 'px';

el.style.height = height + 10 + 'px';

```

  

### 17.4 Use `classList` Instead of `className` String Manipulation

  

```js

// ❌ Bad

el.className = el.className + ' active';

  

// ✅ Good

el.classList.add('active');

```

  

### 17.5 Debounce Expensive Event Handlers

  

```js

function debounce(fn, delay) {

  let timer;

  return function(...args) {

    clearTimeout(timer);

    timer = setTimeout(() => fn.apply(this, args), delay);

  };

}

  

window.addEventListener('resize', debounce(() => {

  console.log('Resized!');

}, 300));

```

  

---

  

## 18. Virtual DOM vs Real DOM

  

| Feature | Real DOM | Virtual DOM |

|---------|----------|-------------|

| **Manipulation** | Slow for frequent updates | Fast (diffing algorithm) |

| **Memory** | Low (just the real tree) | Higher (keeps a copy) |

| **Re-renders** | Whole subtree may re-render | Only changed nodes update |

| **Usage** | Vanilla JS, jQuery | React, Vue |

  

- The **Real DOM** is the actual browser representation.

- The **Virtual DOM** (used by React/Vue) is a lightweight JS object copy. When state changes, the framework diffs the new virtual DOM against the old one and applies only the minimal set of real DOM updates.

  

---

  

## 19. Quick Reference Cheat Sheet

  

### Selecting Elements

  

```js

document.getElementById('id')

document.querySelector('.class / #id / tag')

document.querySelectorAll('selector')   // → NodeList

document.getElementsByClassName('cls') // → HTMLCollection (live)

document.getElementsByTagName('p')     // → HTMLCollection (live)

```

  

### Traversal

  

```js

el.parentElement

el.children              // child elements

el.firstElementChild

el.lastElementChild

el.nextElementSibling

el.previousElementSibling

```

  

### Content & Attributes

  

```js

el.textContent = 'text'

el.innerHTML   = '<b>bold</b>'

el.getAttribute('attr')

el.setAttribute('attr', 'value')

el.removeAttribute('attr')

el.dataset.myKey = 'value'   // data-my-key

```

  

### Classes & Styles

  

```js

el.classList.add('cls')

el.classList.remove('cls')

el.classList.toggle('cls')

el.classList.contains('cls')

el.style.color = 'red'

getComputedStyle(el).fontSize

```

  

### Creating & Inserting

  

```js

document.createElement('div')

parent.appendChild(child)

parent.prepend(child)

parent.append(child)

el.insertAdjacentHTML('beforeend', '<p>text</p>')

document.createDocumentFragment()

node.cloneNode(true)

```

  

### Removing & Replacing

  

```js

el.remove()

parent.removeChild(child)

parent.replaceChild(newEl, oldEl)

el.replaceWith(newEl)

```

  

### Events

  

```js

el.addEventListener('click', handler)

el.removeEventListener('click', handler)

e.preventDefault()

e.stopPropagation()

e.target      // element that triggered event

e.currentTarget // element listener is on

```

  

---

  

## 🎯 Putting It All Together — Mini Project Example

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="UTF-8" />

  <title>DOM Todo App</title>

</head>

<body>

  <h1>Todo List</h1>

  <input type="text" id="todoInput" placeholder="Enter a task..." />

  <button id="addBtn">Add Task</button>

  <ul id="todoList"></ul>

  

  <script>

    const input    = document.getElementById('todoInput');

    const addBtn   = document.getElementById('addBtn');

    const todoList = document.getElementById('todoList');

  

    // Add item

    addBtn.addEventListener('click', addTask);

    input.addEventListener('keydown', (e) => {

      if (e.key === 'Enter') addTask();

    });

  

    function addTask() {

      const text = input.value.trim();

      if (!text) return;

  

      const li = document.createElement('li');

      li.textContent = text;

      li.dataset.done = 'false';

  

      li.addEventListener('click', () => {

        const done = li.dataset.done === 'true';

        li.dataset.done = String(!done);

        li.style.textDecoration = !done ? 'line-through' : 'none';

        li.style.color          = !done ? 'gray' : '';

      });

  

      todoList.appendChild(li);

      input.value = '';

      input.focus();

    }

  

    // Delegation — remove on double-click

    todoList.addEventListener('dblclick', (e) => {

      if (e.target.tagName === 'LI') {

        e.target.remove();

      }

    });

  </script>

</body>

</html>

```

  

---

  

> **Summary:** The DOM is the bridge between HTML and JavaScript. Mastering it means understanding how to **select**, **traverse**, **create**, **modify**, **delete**, and **listen to events** on elements — efficiently and safely.

  

*Happy coding! 🚀*