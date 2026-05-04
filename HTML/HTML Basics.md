
# 🏗️ HTML — Complete Tutorial (Tags & Attributes)

  

> **HTML** (HyperText Markup Language) is the standard language for creating web pages. It uses **tags** to define the structure and meaning of web content.

  

---

  

## 📚 Table of Contents

  

1. [HTML Document Structure](#1-html-document-structure)

2. [Head Elements](#2-head-elements)

3. [Text & Headings](#3-text--headings)

4. [Links](#4-links)

5. [Images](#5-images)

6. [Lists](#6-lists)

7. [Tables](#7-tables)

8. [Forms & Input Elements](#8-forms--input-elements)

9. [Semantic HTML5 Elements](#9-semantic-html5-elements)

10. [Media Elements](#10-media-elements)

11. [Inline Elements](#11-inline-elements)

12. [Block & Container Elements](#12-block--container-elements)

13. [Scripting & Embedding](#13-scripting--embedding)

14. [Global Attributes](#14-global-attributes)

15. [ARIA & Accessibility Attributes](#15-aria--accessibility-attributes)

16. [Meta Tags & SEO](#16-meta-tags--seo)

17. [Full Page Example](#17-full-page-example)

18. [Quick Reference Cheat Sheet](#18-quick-reference-cheat-sheet)

  

---

  

## 1. HTML Document Structure

  

Every HTML page follows this skeleton:

  

```html

<!DOCTYPE html>

<html lang="en">

  <head>

    <meta charset="UTF-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Page Title</title>

  </head>

  <body>

    <!-- Page content goes here -->

  </body>

</html>

```

  

| Tag | Purpose |

|-----|---------|

| `<!DOCTYPE html>` | Declares HTML5 document type |

| `<html lang="en">` | Root element; `lang` sets the document language |

| `<head>` | Container for metadata (not displayed) |

| `<body>` | Container for all visible content |

  

---

  

## 2. Head Elements

  

### `<meta>` — Metadata

  

```html

<meta charset="UTF-8" />                          <!-- Character encoding -->

<meta name="viewport" content="width=device-width, initial-scale=1.0" />  <!-- Responsive -->

<meta name="description" content="A great website." />  <!-- SEO description -->

<meta name="author" content="John Doe" />

<meta name="keywords" content="html, css, javascript" />

<meta http-equiv="refresh" content="30" />        <!-- Auto-refresh every 30s -->

```

  

### `<title>` — Browser Tab Title

  

```html

<title>My Awesome Website</title>

```

  

### `<link>` — External Resources

  

```html

<link rel="stylesheet" href="styles.css" />           <!-- CSS file -->

<link rel="icon" href="favicon.ico" type="image/x-icon" />  <!-- Favicon -->

<link rel="canonical" href="https://example.com/page" />    <!-- SEO canonical -->

<link rel="preload" href="font.woff2" as="font" />    <!-- Preload resource -->

```

  

| Attribute | Description |

|-----------|-------------|

| `rel` | Relationship between document and linked resource |

| `href` | URL of the linked resource |

| `type` | MIME type of the linked resource |

| `media` | Media query (e.g., `"print"`, `"screen"`) |

  

### `<style>` — Internal CSS

  

```html

<style>

  body { font-family: Arial, sans-serif; }

  h1   { color: navy; }

</style>

```

  

---

  

## 3. Text & Headings

  

### Headings `<h1>` – `<h6>`

  

```html

<h1>Main Heading (only one per page)</h1>

<h2>Section Heading</h2>

<h3>Subsection Heading</h3>

<h4>Sub-subsection</h4>

<h5>Smaller Heading</h5>

<h6>Smallest Heading</h6>

```

  

> Always use headings in **hierarchical order**. Only one `<h1>` per page for SEO.

  

### Paragraphs & Line Breaks

  

```html

<p>This is a paragraph. HTML collapses whitespace automatically.</p>

<p>Second paragraph.</p>

  

<!-- Force a line break (use sparingly) -->

<p>Line one.<br />Line two.</p>

  

<!-- Horizontal rule (thematic break) -->

<hr />

```

  

### Text Formatting Tags

  

```html

<strong>Bold / important text</strong>

<b>Bold (no semantic importance)</b>

  

<em>Italic / emphasis</em>

<i>Italic (no semantic emphasis)</i>

  

<u>Underlined text</u>

<s>Strikethrough</s>

<del>Deleted text</del>       <!-- For editorial deletions -->

<ins>Inserted text</ins>      <!-- For editorial insertions -->

  

<mark>Highlighted text</mark>

  

<small>Smaller text (fine print)</small>

<big>Larger text</big>

  

<sub>Subscript</sub>  e.g. H<sub>2</sub>O

<sup>Superscript</sup> e.g. E=mc<sup>2</sup>

```

  

### Quotations

  

```html

<!-- Block quote (multi-line) -->

<blockquote cite="https://source.com">

  "To be or not to be, that is the question."

</blockquote>

  

<!-- Inline quote -->

<p>As Shakespeare wrote, <q>To be or not to be</q>.</p>

  

<!-- Citation / title of a work -->

<cite>Hamlet</cite> by William Shakespeare.

  

<!-- Abbreviation with tooltip -->

<abbr title="HyperText Markup Language">HTML</abbr>

  

<!-- Address (contact info) -->

<address>

  123 Main St, New York, NY<br />

  <a href="mailto:info@example.com">info@example.com</a>

</address>

```

  

### Preformatted & Code Text

  

```html

<!-- Preformatted text: preserves spaces and line breaks -->

<pre>

  Name:   John

  Age:    30

</pre>

  

<!-- Inline code -->

<p>Use the <code>console.log()</code> function to debug.</p>

  

<!-- Keyboard input -->

<p>Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to copy.</p>

  

<!-- Sample output -->

<p>Output: <samp>Hello, World!</samp></p>

  

<!-- Variable -->

<p>The variable <var>x</var> holds the value.</p>

```

  

---

  

## 4. Links

  

### `<a>` — Anchor / Hyperlink

  

```html

<!-- External link -->

<a href="https://www.google.com" target="_blank" rel="noopener noreferrer">

  Visit Google

</a>

  

<!-- Internal link -->

<a href="/about.html">About Us</a>

  

<!-- Anchor link (jump to section) -->

<a href="#section2">Go to Section 2</a>

<h2 id="section2">Section 2</h2>

  

<!-- Email link -->

<a href="mailto:hello@example.com">Send Email</a>

  

<!-- Phone link -->

<a href="tel:+11234567890">Call Us</a>

  

<!-- Download link -->

<a href="report.pdf" download="Annual-Report.pdf">Download Report</a>

```

  

| Attribute | Values | Description |

|-----------|--------|-------------|

| `href` | URL, `#id`, `mailto:`, `tel:` | Destination |

| `target` | `_blank`, `_self`, `_parent`, `_top` | Where to open |

| `rel` | `noopener`, `noreferrer`, `nofollow` | Relationship |

| `download` | filename string | Triggers file download |

| `title` | string | Tooltip on hover |

  

---

  

## 5. Images

  

### `<img>` — Image

  

```html

<!-- Basic image -->

<img src="photo.jpg" alt="A scenic mountain view" />

  

<!-- With dimensions -->

<img src="logo.png" alt="Company Logo" width="200" height="100" />

  

<!-- Lazy loading (modern best practice) -->

<img src="large-photo.jpg" alt="Large Photo" loading="lazy" />

  

<!-- Responsive image with multiple sources -->

<picture>

  <source srcset="image-large.webp" media="(min-width: 800px)" type="image/webp" />

  <source srcset="image-small.webp" media="(max-width: 799px)" type="image/webp" />

  <img src="image-fallback.jpg" alt="Responsive image fallback" />

</picture>

  

<!-- Srcset for resolution switching -->

<img

  src="image-400.jpg"

  srcset="image-400.jpg 400w, image-800.jpg 800w, image-1200.jpg 1200w"

  sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"

  alt="Responsive image"

/>

```

  

| Attribute | Description |

|-----------|-------------|

| `src` | Image URL (**required**) |

| `alt` | Alternative text (**required** for accessibility) |

| `width` / `height` | Dimensions in pixels |

| `loading` | `lazy` (defer) or `eager` (default) |

| `srcset` | List of image sources for different sizes |

| `sizes` | Hints for which srcset image to use |

  

### `<figure>` & `<figcaption>`

  

```html

<figure>

  <img src="chart.png" alt="Sales chart for Q1 2024" />

  <figcaption>Figure 1: Q1 2024 Sales Data</figcaption>

</figure>

```

  

---

  

## 6. Lists

  

### Unordered List `<ul>`

  

```html

<ul>

  <li>HTML</li>

  <li>CSS</li>

  <li>JavaScript</li>

</ul>

```

  

### Ordered List `<ol>`

  

```html

<!-- Default (1, 2, 3...) -->

<ol>

  <li>First step</li>

  <li>Second step</li>

</ol>

  

<!-- Custom start and type -->

<ol type="A" start="3">       <!-- C, D, E... -->

  <li>Item C</li>

  <li>Item D</li>

</ol>

  

<!-- Reversed -->

<ol reversed>

  <li>Third</li>

  <li>Second</li>

  <li>First</li>

</ol>

```

  

| `type` Value | Display |

|-------------|---------|

| `1` | 1, 2, 3 (default) |

| `A` | A, B, C |

| `a` | a, b, c |

| `I` | I, II, III |

| `i` | i, ii, iii |

  

### Definition List `<dl>`

  

```html

<dl>

  <dt>HTML</dt>

  <dd>HyperText Markup Language — the structure of web pages.</dd>

  

  <dt>CSS</dt>

  <dd>Cascading Style Sheets — the styling of web pages.</dd>

</dl>

```

  

### Nested Lists

  

```html

<ul>

  <li>Frontend

    <ul>

      <li>HTML</li>

      <li>CSS</li>

      <li>JavaScript</li>

    </ul>

  </li>

  <li>Backend

    <ul>

      <li>Node.js</li>

      <li>Python</li>

    </ul>

  </li>

</ul>

```

  

---

  

## 7. Tables

  

```html

<table border="1">

  <!-- Caption -->

  <caption>Monthly Sales Report</caption>

  

  <!-- Column groups for styling -->

  <colgroup>

    <col style="background-color: #f0f8ff;" />

    <col span="2" />

  </colgroup>

  

  <!-- Table head -->

  <thead>

    <tr>

      <th scope="col">Month</th>

      <th scope="col">Sales</th>

      <th scope="col">Revenue</th>

    </tr>

  </thead>

  

  <!-- Table body -->

  <tbody>

    <tr>

      <td>January</td>

      <td>150</td>

      <td>$15,000</td>

    </tr>

    <tr>

      <td>February</td>

      <td>200</td>

      <td>$20,000</td>

    </tr>

  </tbody>

  

  <!-- Table footer -->

  <tfoot>

    <tr>

      <td>Total</td>

      <td>350</td>

      <td>$35,000</td>

    </tr>

  </tfoot>

</table>

```

  

### Spanning Cells

  

```html

<table>

  <tr>

    <td colspan="2">Spans 2 columns</td>  <!-- Merge horizontally -->

  </tr>

  <tr>

    <td rowspan="2">Spans 2 rows</td>     <!-- Merge vertically -->

    <td>Normal cell</td>

  </tr>

  <tr>

    <td>Another cell</td>

  </tr>

</table>

```

  

| Tag | Description |

|-----|-------------|

| `<table>` | Table container |

| `<caption>` | Table title |

| `<thead>` | Header row group |

| `<tbody>` | Body row group |

| `<tfoot>` | Footer row group |

| `<tr>` | Table row |

| `<th>` | Header cell (bold + centered) |

| `<td>` | Data cell |

| `<colgroup>` / `<col>` | Column grouping |

  

---

  

## 8. Forms & Input Elements

  

### `<form>`

  

```html

<form action="/submit" method="POST" enctype="multipart/form-data" novalidate>

  <!-- Form contents -->

</form>

```

  

| Attribute | Values | Description |

|-----------|--------|-------------|

| `action` | URL | Where to send data |

| `method` | `GET`, `POST` | HTTP method |

| `enctype` | `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` | Encoding (use `multipart` for file uploads) |

| `novalidate` | boolean | Skip browser validation |

| `autocomplete` | `on`, `off` | Enable/disable autocomplete |

  

### Text Inputs

  

```html

<input type="text"     name="username"  placeholder="Enter username" />

<input type="email"    name="email"     placeholder="you@example.com" />

<input type="password" name="pwd"       placeholder="Password" />

<input type="tel"      name="phone"     placeholder="+1 (555) 000-0000" />

<input type="url"      name="website"   placeholder="https://example.com" />

<input type="search"   name="query"     placeholder="Search..." />

<input type="number"   name="qty"       min="1" max="100" step="1" value="1" />

<input type="range"    name="volume"    min="0" max="100" step="5" value="50" />

```

  

### Date & Time Inputs

  

```html

<input type="date"           name="birthday"   value="1990-01-15" />

<input type="time"           name="meeting"    value="09:00" />

<input type="datetime-local" name="event"      value="2024-06-15T10:30" />

<input type="month"          name="period"     value="2024-06" />

<input type="week"           name="week"       value="2024-W24" />

```

  

### Selection Inputs

  

```html

<!-- Checkbox -->

<input type="checkbox" id="agree" name="agree" value="yes" checked />

<label for="agree">I agree to the terms</label>

  

<!-- Radio buttons -->

<input type="radio" id="male"   name="gender" value="male" />

<label for="male">Male</label>

<input type="radio" id="female" name="gender" value="female" checked />

<label for="female">Female</label>

  

<!-- Color picker -->

<input type="color" name="themeColor" value="#ff6600" />

  

<!-- File upload -->

<input type="file" name="avatar" accept="image/*" multiple />

  

<!-- Hidden input -->

<input type="hidden" name="userId" value="42" />

```

  

### `<textarea>` — Multi-line Text

  

```html

<textarea

  name="message"

  id="message"

  rows="5"

  cols="40"

  maxlength="500"

  placeholder="Type your message here..."

  required

>

</textarea>

```

  

### `<select>` — Dropdown

  

```html

<select name="country" id="country" multiple size="4">

  <option value="">-- Select Country --</option>

  <optgroup label="Europe">

    <option value="uk">United Kingdom</option>

    <option value="de">Germany</option>

  </optgroup>

  <optgroup label="Asia">

    <option value="in" selected>India</option>

    <option value="jp">Japan</option>

  </optgroup>

</select>

```

  

### `<datalist>` — Input with Suggestions

  

```html

<input type="text" list="browsers" name="browser" placeholder="Pick a browser" />

<datalist id="browsers">

  <option value="Chrome" />

  <option value="Firefox" />

  <option value="Safari" />

  <option value="Edge" />

</datalist>

```

  

### `<label>` — Accessible Labels

  

```html

<!-- Explicit association (preferred) -->

<label for="email">Email Address:</label>

<input type="email" id="email" name="email" />

  

<!-- Implicit association (wrapping) -->

<label>

  Username:

  <input type="text" name="username" />

</label>

```

  

### Buttons

  

```html

<button type="submit">Submit Form</button>

<button type="reset">Reset Form</button>

<button type="button" onclick="doSomething()">Click Me</button>

  

<!-- Input-based buttons (older style) -->

<input type="submit" value="Submit" />

<input type="reset"  value="Reset" />

<input type="button" value="Click Me" />

```

  

### Common Input Attributes

  

| Attribute | Description |

|-----------|-------------|

| `name` | Field name sent to server |

| `id` | Unique identifier (for `<label for>`) |

| `value` | Default / current value |

| `placeholder` | Hint text when empty |

| `required` | Field must be filled |

| `disabled` | Greys out and disables the field |

| `readonly` | Visible but not editable |

| `maxlength` | Maximum characters allowed |

| `minlength` | Minimum characters required |

| `min` / `max` | Number/date range limits |

| `step` | Increment for number/range |

| `pattern` | Regex pattern for validation |

| `autocomplete` | `on` / `off` |

| `autofocus` | Focus this field on page load |

| `multiple` | Allow multiple values (file, select) |

| `checked` | Pre-check checkbox/radio |

| `selected` | Pre-select option |

| `accept` | Allowed file types for `type="file"` |

  

### `<fieldset>` & `<legend>` — Grouping

  

```html

<fieldset>

  <legend>Personal Information</legend>

  <label for="fname">First Name:</label>

  <input type="text" id="fname" name="fname" />

  

  <label for="lname">Last Name:</label>

  <input type="text" id="lname" name="lname" />

</fieldset>

```

  

### `<progress>` & `<meter>`

  

```html

<!-- Task progress (0–100) -->

<progress value="70" max="100">70%</progress>

  

<!-- Scalar measurement within a known range -->

<meter value="0.75" min="0" max="1" low="0.25" high="0.75" optimum="0.9">

  75%

</meter>

```

  

---

  

## 9. Semantic HTML5 Elements

  

Semantic elements carry meaning, improving SEO and accessibility.

  

```html

<body>

  

  <header>

    <nav>

      <ul>

        <li><a href="#home">Home</a></li>

        <li><a href="#about">About</a></li>

        <li><a href="#contact">Contact</a></li>

      </ul>

    </nav>

  </header>

  

  <main>

  

    <article>

      <header>

        <h2>Article Title</h2>

        <time datetime="2024-06-15">June 15, 2024</time>

      </header>

      <p>Article body content...</p>

      <footer>By <address>Jane Doe</address></footer>

    </article>

  

    <section>

      <h2>About Us</h2>

      <p>We are a technology company...</p>

    </section>

  

    <aside>

      <h3>Related Links</h3>

      <ul>

        <li><a href="#">Link 1</a></li>

      </ul>

    </aside>

  

  </main>

  

  <footer>

    <p>&copy; 2024 My Company. All rights reserved.</p>

  </footer>

  

</body>

```

  

| Tag | Semantic Role |

|-----|--------------|

| `<header>` | Page or section header (logo, nav, title) |

| `<nav>` | Navigation links |

| `<main>` | Primary unique content of the page |

| `<article>` | Self-contained, independent content (blog post, news) |

| `<section>` | Thematic grouping of content with a heading |

| `<aside>` | Tangentially related content (sidebar) |

| `<footer>` | Footer of a page or section |

| `<figure>` | Self-contained media with optional caption |

| `<figcaption>` | Caption for a `<figure>` |

| `<time>` | Machine-readable date/time |

| `<details>` | Expandable disclosure widget |

| `<summary>` | Visible heading of `<details>` |

  

### `<details>` & `<summary>` — Accordion

  

```html

<details>

  <summary>Click to expand</summary>

  <p>Hidden content revealed when expanded.</p>

</details>

  

<details open>              <!-- "open" attribute = expanded by default -->

  <summary>Already Open</summary>

  <p>This is visible without clicking.</p>

</details>

```

  

### `<dialog>` — Native Modal

  

```html

<dialog id="myDialog">

  <h2>Dialog Title</h2>

  <p>This is a native HTML dialog box.</p>

  <button onclick="document.getElementById('myDialog').close()">Close</button>

</dialog>

  

<button onclick="document.getElementById('myDialog').showModal()">Open Dialog</button>

```

  

---

  

## 10. Media Elements

  

### `<audio>` — Audio Player

  

```html

<audio controls autoplay loop muted preload="auto">

  <source src="song.mp3" type="audio/mpeg" />

  <source src="song.ogg" type="audio/ogg" />

  Your browser does not support the audio element.

</audio>

```

  

### `<video>` — Video Player

  

```html

<video controls width="640" height="360" poster="thumbnail.jpg" preload="metadata">

  <source src="movie.mp4"  type="video/mp4" />

  <source src="movie.webm" type="video/webm" />

  <!-- Subtitles -->

  <track src="subtitles-en.vtt" kind="subtitles" srclang="en" label="English" default />

  Your browser does not support the video element.

</video>

```

  

| Attribute | Description |

|-----------|-------------|

| `controls` | Show play/pause controls |

| `autoplay` | Start playing automatically |

| `loop` | Loop the media |

| `muted` | Start muted |

| `preload` | `auto`, `metadata`, `none` |

| `poster` | Thumbnail image (video only) |

| `width` / `height` | Dimensions (video only) |

  

### `<iframe>` — Embedded Frame

  

```html

<!-- YouTube embed -->

<iframe

  src="https://www.youtube.com/embed/dQw4w9WgXcQ"

  width="560"

  height="315"

  title="YouTube video"

  frameborder="0"

  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope"

  allowfullscreen

  loading="lazy"

></iframe>

  

<!-- Google Maps embed -->

<iframe

  src="https://maps.google.com/maps?q=New+York&output=embed"

  width="400"

  height="300"

  title="Map of New York"

></iframe>

```

  

### `<canvas>` — Drawing Surface

  

```html

<canvas id="myCanvas" width="400" height="200">

  Your browser does not support canvas.

</canvas>

  

<script>

  const canvas = document.getElementById('myCanvas');

  const ctx    = canvas.getContext('2d');

  ctx.fillStyle = 'blue';

  ctx.fillRect(10, 10, 150, 80);

</script>

```

  

### `<svg>` — Scalable Vector Graphics

  

```html

<svg width="200" height="100" xmlns="http://www.w3.org/2000/svg">

  <rect x="10" y="10" width="80" height="80" fill="tomato" rx="10" />

  <circle cx="150" cy="50" r="40" fill="steelblue" />

  <text x="50" y="90" font-size="14" fill="black">Hello SVG</text>

</svg>

```

  

---

  

## 11. Inline Elements

  

Inline elements flow within text content.

  

```html

<span class="highlight">Highlighted span</span>        <!-- Generic inline container -->

<a href="#">Link</a>

<strong>Bold</strong>

<em>Italic</em>

<code>inline code</code>

<img src="icon.png" alt="icon" />

<br />                   <!-- Line break -->

<wbr />                  <!-- Optional line break hint for long words -->

<label>Label</label>

<button>Button</button>

<select></select>

<input type="text" />

  

<!-- Ruby annotation (East Asian typography) -->

<ruby>

  漢 <rt>Kan</rt>

  字 <rt>ji</rt>

</ruby>

  

<!-- Bidirectional text -->

<bdi>مرحبا</bdi>

<bdo dir="rtl">Reversed text</bdo>

```

  

---

  

## 12. Block & Container Elements

  

Block elements start on a new line and take full width.

  

```html

<div class="container">Generic block container</div>

  

<p>Paragraph</p>

  

<blockquote>Quoted block</blockquote>

  

<pre>Preformatted block</pre>

  

<hr />  <!-- Horizontal rule -->

  

<!-- Headings h1–h6 -->

<h1>Main Heading</h1>

  

<!-- Lists -->

<ul><li>Item</li></ul>

<ol><li>Item</li></ol>

<dl><dt>Term</dt><dd>Definition</dd></dl>

  

<!-- Table -->

<table>...</table>

  

<!-- Semantic blocks -->

<header>, <nav>, <main>, <section>, <article>, <aside>, <footer>

```

  

---

  

## 13. Scripting & Embedding

  

### `<script>` — JavaScript

  

```html

<!-- Inline script -->

<script>

  console.log('Hello from inline script');

</script>

  

<!-- External script -->

<script src="app.js"></script>

  

<!-- Deferred — runs after HTML parsed -->

<script src="app.js" defer></script>

  

<!-- Async — runs as soon as downloaded -->

<script src="analytics.js" async></script>

  

<!-- Specify type -->

<script type="module" src="module.js"></script>

```

  

### `<noscript>` — Fallback for No JS

  

```html

<noscript>

  <p>Please enable JavaScript to use this website.</p>

</noscript>

```

  

### `<template>` — Reusable HTML Template

  

```html

<template id="cardTemplate">

  <div class="card">

    <h3 class="card-title"></h3>

    <p class="card-body"></p>

  </div>

</template>

  

<script>

  const template = document.getElementById('cardTemplate');

  const clone    = template.content.cloneNode(true);

  clone.querySelector('.card-title').textContent = 'My Card';

  document.body.appendChild(clone);

</script>

```

  

### `<object>` & `<embed>` — Embedding External Content

  

```html

<!-- PDF -->

<object data="document.pdf" type="application/pdf" width="600" height="400">

  <p><a href="document.pdf">Download PDF</a></p>

</object>

  

<!-- Plugin content (older usage) -->

<embed src="flash.swf" type="application/x-shockwave-flash" width="400" height="300" />

```

  

---

  

## 14. Global Attributes

  

These attributes are valid on **any** HTML element.

  

| Attribute | Description | Example |

|-----------|-------------|---------|

| `id` | Unique identifier | `id="header"` |

| `class` | CSS class name(s) | `class="btn primary"` |

| `style` | Inline CSS | `style="color:red"` |

| `title` | Tooltip text | `title="More info"` |

| `lang` | Language of content | `lang="fr"` |

| `dir` | Text direction | `dir="rtl"` |

| `hidden` | Hides the element | `hidden` |

| `tabindex` | Tab order | `tabindex="0"` |

| `contenteditable` | Make content editable | `contenteditable="true"` |

| `draggable` | Enable drag & drop | `draggable="true"` |

| `spellcheck` | Browser spellcheck | `spellcheck="true"` |

| `translate` | Content translatable | `translate="no"` |

| `data-*` | Custom data attributes | `data-user-id="42"` |

  

```html

<!-- contenteditable — makes any element editable -->

<div contenteditable="true" style="border:1px solid #ccc; padding:8px;">

  Click here to edit this text!

</div>

  

<!-- draggable -->

<div draggable="true" ondragstart="event.dataTransfer.setData('text', 'Hello')">

  Drag me!

</div>

  

<!-- hidden -->

<p hidden>This is not visible.</p>

  

<!-- tabindex: -1 = not in tab order, 0 = natural order, 1+ = priority order -->

<button tabindex="1">First Tab Stop</button>

<button tabindex="2">Second Tab Stop</button>

```

  

---

  

## 15. ARIA & Accessibility Attributes

  

**ARIA** (Accessible Rich Internet Applications) attributes make web content more accessible to assistive technologies (screen readers).

  

```html

<!-- role: defines element's purpose -->

<div role="button" tabindex="0">Custom Button</div>

<nav role="navigation" aria-label="Main Navigation">...</nav>

  

<!-- aria-label: text label (when no visible label exists) -->

<button aria-label="Close dialog">✕</button>

  

<!-- aria-labelledby: points to another element as the label -->

<h2 id="section-title">Our Services</h2>

<section aria-labelledby="section-title">...</section>

  

<!-- aria-describedby: points to descriptive text -->

<input type="password" aria-describedby="pwd-help" />

<p id="pwd-help">Must be at least 8 characters with a number.</p>

  

<!-- aria-hidden: hides from screen readers -->

<span aria-hidden="true">🎉</span> Congratulations!

  

<!-- aria-live: announce dynamic changes -->

<div aria-live="polite" id="status">Form submitted successfully!</div>

  

<!-- aria-expanded: toggle state -->

<button aria-expanded="false" aria-controls="menu">Menu</button>

<ul id="menu" hidden>...</ul>

  

<!-- aria-checked / aria-disabled / aria-required -->

<div role="checkbox" aria-checked="true">Agree</div>

<button aria-disabled="true">Submit</button>

<input type="text" aria-required="true" />

  

<!-- Common ARIA Roles -->

<!-- role="alert", "banner", "complementary", "contentinfo" -->

<!-- role="dialog", "form", "heading", "list", "listitem" -->

<!-- role="main", "navigation", "region", "search", "status" -->

```

  

---

  

## 16. Meta Tags & SEO

  

### Essential Meta Tags

  

```html

<head>

  <meta charset="UTF-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <meta name="description" content="Learn HTML with this complete tutorial covering all tags." />

  <meta name="author" content="Jane Doe" />

  <meta name="robots" content="index, follow" />

  <title>HTML Complete Tutorial</title>

  <link rel="canonical" href="https://example.com/html-tutorial" />

  

  <!-- Open Graph (Facebook, LinkedIn sharing) -->

  <meta property="og:title"       content="HTML Complete Tutorial" />

  <meta property="og:description" content="Learn all HTML tags and attributes." />

  <meta property="og:image"       content="https://example.com/thumbnail.jpg" />

  <meta property="og:url"         content="https://example.com/html-tutorial" />

  <meta property="og:type"        content="article" />

  

  <!-- Twitter Card -->

  <meta name="twitter:card"        content="summary_large_image" />

  <meta name="twitter:title"       content="HTML Complete Tutorial" />

  <meta name="twitter:description" content="Learn all HTML tags and attributes." />

  <meta name="twitter:image"       content="https://example.com/thumbnail.jpg" />

</head>

```

  

---

  

## 17. Full Page Example

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="UTF-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <meta name="description" content="A demo HTML page showcasing all major tags." />

  <title>HTML Demo Page</title>

  <link rel="stylesheet" href="styles.css" />

</head>

<body>

  

  <header>

    <h1>My Website</h1>

    <nav aria-label="Main navigation">

      <ul>

        <li><a href="#about">About</a></li>

        <li><a href="#blog">Blog</a></li>

        <li><a href="#contact">Contact</a></li>

      </ul>

    </nav>

  </header>

  

  <main>

  

    <section id="about">

      <h2>About Us</h2>

      <p>We build <strong>amazing</strong> websites with <em>pure</em> HTML.</p>

      <figure>

        <img src="team.jpg" alt="Our team at the office" width="600" loading="lazy" />

        <figcaption>Our talented team, 2024</figcaption>

      </figure>

    </section>

  

    <section id="blog">

      <h2>Latest Articles</h2>

      <article>

        <header>

          <h3>Getting Started with HTML</h3>

          <time datetime="2024-06-01">June 1, 2024</time>

        </header>

        <p>HTML is the backbone of the web. Every website starts with it...</p>

        <a href="/articles/html-basics">Read More →</a>

      </article>

    </section>

  

    <section id="contact">

      <h2>Contact Us</h2>

      <form action="/contact" method="POST">

        <fieldset>

          <legend>Your Details</legend>

  

          <label for="name">Full Name:</label>

          <input type="text" id="name" name="name" placeholder="John Doe" required />

  

          <label for="email">Email:</label>

          <input type="email" id="email" name="email" placeholder="you@example.com" required />

  

          <label for="subject">Subject:</label>

          <select id="subject" name="subject">

            <option value="">Select topic</option>

            <option value="general">General Inquiry</option>

            <option value="support">Technical Support</option>

          </select>

  

          <label for="message">Message:</label>

          <textarea id="message" name="message" rows="5" placeholder="Write your message..." required></textarea>

  

          <button type="submit">Send Message</button>

        </fieldset>

      </form>

    </section>

  

  </main>

  

  <aside>

    <h3>Quick Links</h3>

    <ul>

      <li><a href="https://developer.mozilla.org" target="_blank" rel="noopener">MDN Web Docs</a></li>

      <li><a href="https://www.w3schools.com" target="_blank" rel="noopener">W3Schools</a></li>

    </ul>

  </aside>

  

  <footer>

    <p>&copy; 2024 My Website. All rights reserved.</p>

    <address>Contact: <a href="mailto:info@mywebsite.com">info@mywebsite.com</a></address>

  </footer>

  

  <script src="app.js" defer></script>

</body>

</html>

```

  

---

  

## 18. Quick Reference Cheat Sheet

  

### Document Structure

```html

<!DOCTYPE html>  <html lang="en">  <head>  <body>

<meta>  <title>  <link>  <style>  <script>

```

  

### Text

```html

<h1>–<h6>  <p>  <br>  <hr>  <strong>  <em>  <b>  <i>

<u>  <s>  <del>  <ins>  <mark>  <sub>  <sup>  <small>

<blockquote>  <q>  <cite>  <abbr>  <address>  <pre>  <code>

<kbd>  <samp>  <var>

```

  

### Links & Media

```html

<a href target rel download>

<img src alt width height loading srcset>

<picture>  <source>

<audio controls>  <video controls poster>

<iframe src width height>  <canvas>  <svg>

```

  

### Lists

```html

<ul>  <ol type start reversed>  <li>

<dl>  <dt>  <dd>

```

  

### Tables

```html

<table>  <caption>  <thead>  <tbody>  <tfoot>

<tr>  <th scope>  <td colspan rowspan>

<colgroup>  <col>

```

  

### Forms

```html

<form action method enctype>

<input type name value placeholder required disabled readonly>

<textarea rows cols>

<select>  <option>  <optgroup>

<datalist>  <label for>

<button type>  <fieldset>  <legend>

<progress value max>  <meter value min max>

```

  

### Semantic

```html

<header>  <nav>  <main>  <article>  <section>

<aside>  <footer>  <figure>  <figcaption>

<time datetime>  <details>  <summary>  <dialog>

```

  

### Global Attributes

```html

id  class  style  title  lang  dir  hidden

tabindex  contenteditable  draggable  data-*

```

  

---

  

> **Key Rules to Remember:**

> - Always use `alt` on `<img>` for accessibility

> - One `<h1>` per page for SEO

> - Use semantic elements over generic `<div>` / `<span>` when possible

> - Always pair `<label>` with form inputs using `for` + `id`

> - Validate your HTML at [validator.w3.org](https://validator.w3.org)

  

*Happy building! 🚀*