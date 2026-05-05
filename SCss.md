# 🎨 SCSS — Complete Tutorial (Basic → Advanced)

  

> **SCSS** (Sassy CSS) is a preprocessor language that is compiled into CSS. It adds power and elegance to the basic language by allowing you to use variables, nested rules, mixins, inline imports, and more.

  

---

  

## 📚 Table of Contents

  

1. [What is SCSS?](#1-what-is-scss)

2. [Sass vs SCSS Syntax](#2-sass-vs-scss-syntax)

3. [Variables (`$`)](#3-variables)

4. [Nesting](#4-nesting)

5. [The Parent Selector (`&`)](#5-the-parent-selector-)

6. [Partials and `@use`](#6-partials-and-use)

7. [Mixins (`@mixin` and `@include`)](#7-mixins)

8. [Extend/Inheritance (`@extend`)](#8-extendinheritance)

9. [Operators and Math](#9-operators-and-math)

10. [Functions (`@function`)](#10-functions)

11. [Control Directives (`@if`, `@for`, `@each`, `@while`)](#11-control-directives)

12. [Maps and Lists](#12-maps-and-lists)

13. [Advanced Mixins and Logic](#13-advanced-mixins-and-logic)

14. [Best Practices](#14-best-practices)

15. [Quick Reference Cheat Sheet](#15-quick-reference-cheat-sheet)

  

---

  

## 1. What is SCSS?

  

CSS on its own can be repetitive and hard to maintain in large projects. **SCSS** solves this by providing features that don't exist in standard CSS yet.

  

**How it works:**

1. You write `.scss` files.

2. A compiler (like Dart Sass) converts them into standard `.css` files.

3. The browser reads the `.css` files.

  

---

  

## 2. Sass vs SCSS Syntax

  

There are two syntaxes for Sass:

  

### SCSS (Sassy CSS)

Uses `.scss` extension. It is a superset of CSS, meaning every valid CSS file is a valid SCSS file. Uses curly braces `{}` and semicolons `;`.

  

```scss

$color: red;

.btn {

  color: $color;

}

```

  

### Indented Sass (Original)

Uses `.sass` extension. It uses indentation instead of braces and no semicolons.

  

```sass

$color: red

.btn

  color: $color

```

  

*Most modern projects use **SCSS** because it is easier to transition from standard CSS.*

  

---

  

## 3. Variables (`$`)

  

Variables allow you to store information that you can reuse throughout your stylesheet.

  

```scss

$primary-color: #3498db;

$font-stack: Helvetica, sans-serif;

$spacing-unit: 10px;

  

body {

  font-family: $font-stack;

  background-color: $primary-color;

  padding: $spacing-unit * 2;

}

```

  

### Variable Scope

Variables are only available at the level of selectivity where they are defined.

  

```scss

$global-var: red;

  

.sidebar {

  $local-var: blue;

  color: $local-var; // Works

}

  

.content {

  color: $local-var; // Error!

}

```

  

---

  

## 4. Nesting

  

SCSS lets you nest your CSS selectors in a way that follows the same visual hierarchy of your HTML.

  

**Standard CSS:**

```css

nav ul { margin: 0; }

nav li { display: inline-block; }

nav a { text-decoration: none; }

```

  

**SCSS:**

```scss

nav {

  ul {

    margin: 0;

    padding: 0;

    list-style: none;

  }

  

  li { display: inline-block; }

  

  a {

    display: block;

    padding: 6px 12px;

    text-decoration: none;

  }

}

```

  

---

  

## 5. The Parent Selector (`&`)

  

The `&` character is used in nesting to refer to the parent selector. It’s incredibly useful for hover states, modifiers, and BEM naming.

  

### Hover and Pseudo-elements

```scss

.button {

  background: blue;

  &:hover { background: darkblue; }

  &::before { content: "→"; }

}

```

  

### BEM Naming (Block Element Modifier)

```scss

.card {

  padding: 20px;

  &__title { font-size: 2em; } // Compiles to .card__title

  &__body { color: #333; }

  

  &--featured { background: gold; } // Compiles to .card--featured

}

```

  

---

  

## 6. Partials and `@use`

  

Partials are small snippets of SCSS that you can include in other SCSS files. This helps modularize your code.

  

### Naming Partials

A partial file name always starts with an underscore: `_variables.scss`, `_header.scss`. The underscore tells Sass not to compile it into a standalone CSS file.

  

### Using `@use` (Modern Sass)

`@use` loads another Sass file as a module.

  

**_variables.scss:**

```scss

$primary: #333;

```

  

**main.scss:**

```scss

@use 'variables';

  

body {

  color: variables.$primary; // Access variables via namespace

}

```

  

*Note: Use `@use` instead of the older `@import` which is being deprecated.*

  

---

  

## 7. Mixins (`@mixin` and `@include`)

  

Mixins allow you to define groups of CSS declarations that you can reuse throughout your site.

  

### Basic Mixin

```scss

@mixin flex-center {

  display: flex;

  justify-content: center;

  align-items: center;

}

  

.container {

  @include flex-center;

}

```

  

### Mixin with Arguments

```scss

@mixin border-radius($radius) {

  -webkit-border-radius: $radius;

     -moz-border-radius: $radius;

          border-radius: $radius;

}

  

.box { @include border-radius(10px); }

```

  

### Default Values

```scss

@mixin square($size: 50px) {

  width: $size;

  height: $size;

}

  

.small { @include square; } // Uses 50px

.large { @include square(100px); }

```

  

---

  

## 8. Extend/Inheritance (`@extend`)

  

`@extend` lets you share a set of CSS properties from one selector to another.

  

```scss

%message-shared {

  border: 1px solid #ccc;

  padding: 10px;

  color: #333;

}

  

.message { @extend %message-shared; }

  

.success {

  @extend %message-shared;

  border-color: green;

}

  

.error {

  @extend %message-shared;

  border-color: red;

}

```

  

*Note: `%message-shared` is a **placeholder selector**. It won't compile to CSS unless it is extended.*

  

---

  

## 9. Operators and Math

  

SCSS allows you to do math directly in your stylesheets.

  

```scss

.container {

  width: 100% - 20px;

  margin: 10px + 5px;

  padding: 10px * 2;

}

  

// Division requires parentheses or variables in modern Sass

$width: 1000px;

.sidebar {

  width: ($width / 2);

}

```

  

---

  

## 10. Functions (`@function`)

  

Functions are similar to mixins, but they return a value instead of CSS declarations.

  

```scss

@function calculate-rem($size) {

  @return ($size / 16px) * 1rem;

}

  

h1 {

  font-size: calculate-rem(32px); // returns 2rem

}

```

  

---

  

## 11. Control Directives

  

### `@if` and `@else`

```scss

$theme: dark;

  

body {

  @if $theme == dark {

    background: black;

    color: white;

  } @else {

    background: white;

    color: black;

  }

}

```

  

### `@for`

```scss

@for $i from 1 through 3 {

  .col-#{$i} { width: 100% / $i; }

}

// Generates .col-1, .col-2, .col-3

```

  

### `@each`

```scss

$colors: (red, blue, green);

  

@each $color in $colors {

  .text-#{$color} { color: $color; }

}

```

  

---

  

## 12. Maps and Lists

  

### Lists

A list is a series of values.

```scss

$padding: 10px 20px 10px 20px;

$colors: red, blue, green;

```

  

### Maps

Maps are key-value pairs.

```scss

$font-weights: (

  "regular": 400,

  "medium": 500,

  "bold": 700

);

  

body {

  font-weight: map-get($font-weights, "medium");

}

```

  

---

  

## 13. Advanced Mixins and Logic

  

### Passing Content to Mixins (`@content`)

Useful for media queries.

  

```scss

@mixin mobile {

  @media (max-width: 600px) {

    @content;

  }

}

  

.sidebar {

  width: 300px;

  @include mobile {

    width: 100%;

  }

}

```

  

---

  

## 14. Best Practices

  

1. **Keep nesting shallow**: Don't go deeper than 3 levels. It makes CSS hard to read and increases specificity too much.

2. **Use variables for consistency**: Colors, fonts, and spacing should all be variables.

3. **Use Partials**: Separate your code into `_layout.scss`, `_components.scss`, etc.

4. **Prefer `@use` over `@import`**: `@use` is more predictable and avoids global namespace pollution.

5. **Comment your code**: Explain what complex mixins or functions do.

  

---

  

## 15. Quick Reference Cheat Sheet

  

| Feature | Syntax |

|---------|--------|

| Variable | `$name: value;` |

| Nesting | `parent { child { ... } }` |

| Parent ref | `&:hover { ... }` |

| Mixin def | `@mixin name($arg) { ... }` |

| Mixin use | `@include name(value);` |

| Partial | `_filename.scss` |

| Load module | `@use 'filename';` |

| Extend | `@extend %placeholder;` |

| Function | `@function name() { @return ... }` |

| Math | `+`, `-`, `*`, `/`, `%` |

  

---

  

> **Summary:** SCSS makes CSS more powerful and manageable. By using **variables**, **nesting**, and **mixins**, you can write cleaner code that is easier to maintain and scale.

  

*Happy Preprocessing! 🚀*