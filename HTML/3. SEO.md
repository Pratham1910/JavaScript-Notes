
# 🔍 SEO — Complete Tutorial (Basic → Advanced)

  

> **SEO** (Search Engine Optimization) is the practice of improving a website so that search engines rank it higher in results — driving more **organic (free) traffic**. Good SEO combines technical correctness, quality content, and authority signals.

  

---

  

## 📚 Table of Contents

  

1. [How Search Engines Work](#1-how-search-engines-work)

2. [Types of SEO](#2-types-of-seo)

3. [Keyword Research](#3-keyword-research)

4. [On-Page SEO — HTML Fundamentals](#4-on-page-seo--html-fundamentals)

5. [Meta Tags for SEO](#5-meta-tags-for-seo)

6. [URL Structure](#6-url-structure)

7. [Content SEO](#7-content-seo)

8. [Technical SEO](#8-technical-seo)

9. [Core Web Vitals & Page Speed](#9-core-web-vitals--page-speed)

10. [Mobile SEO](#10-mobile-seo)

11. [Structured Data (Schema Markup)](#11-structured-data-schema-markup)

12. [Open Graph & Social SEO](#12-open-graph--social-seo)

13. [Image SEO](#13-image-seo)

14. [Link Building (Off-Page SEO)](#14-link-building-off-page-seo)

15. [Local SEO](#15-local-seo)

16. [`robots.txt` & `sitemap.xml`](#16-robotstxt--sitemapxml)

17. [SEO for Single Page Apps (SPA)](#17-seo-for-single-page-apps-spa)

18. [Measuring & Monitoring SEO](#18-measuring--monitoring-seo)

19. [SEO Checklist](#19-seo-checklist)

20. [Quick Reference Cheat Sheet](#20-quick-reference-cheat-sheet)

  

---

  

## 1. How Search Engines Work

  

Understanding the process helps you optimize for each stage.

  

### The 3-Step Process

  

```

1. CRAWLING

   Googlebot follows links across the web, discovering pages.

   Like a spider exploring a web of links.

  

2. INDEXING

   Google parses the page content, stores it in its index database.

   Pages that can't be indexed can't rank.

  

3. RANKING

   When someone searches, Google scores all indexed pages using

   200+ factors and returns the most relevant results.

```

  

### What Googlebot Reads

  

- HTML content (text, headings, links)

- HTTP response codes (200 OK, 301 Redirect, 404 Not Found)

- `robots.txt` directives

- Structured data (Schema.org JSON-LD)

- Page speed metrics

- Mobile-friendliness

  

### What It Cannot Read (Without Help)

  

- JavaScript-rendered content (if not server-side rendered)

- Content inside `<canvas>` or images (without alt text)

- Flash content

- PDFs (partially)

  

---

  

## 2. Types of SEO

  

| Type | Focus Area |

|------|------------|

| **On-Page SEO** | Content, HTML tags, keywords on the page |

| **Technical SEO** | Site speed, crawlability, indexability, HTTPS |

| **Off-Page SEO** | Backlinks, authority, brand signals |

| **Local SEO** | Geographic targeting, Google Business Profile |

| **Content SEO** | Topic depth, freshness, E-E-A-T |

| **Mobile SEO** | Mobile-first indexing, responsive design |

  

---

  

## 3. Keyword Research

  

Keywords are the queries users type into search engines. Targeting the right ones is the foundation of SEO.

  

### Keyword Types

  

| Type | Description | Example |

|------|-------------|---------|

| **Short-tail** | 1–2 words, high volume, high competition | `css tutorial` |

| **Long-tail** | 3+ words, lower volume, easier to rank | `css flexbox tutorial for beginners` |

| **LSI Keywords** | Semantically related terms | `stylesheet`, `web design`, `selectors` |

| **Branded** | Include your brand name | `Google Analytics tutorial` |

| **Informational** | User wants to learn | `how to center a div` |

| **Transactional** | User wants to buy | `buy web hosting cheap` |

| **Navigational** | User wants a specific site | `MDN CSS reference` |

  

### Keyword Metrics to Track

  

| Metric | What It Means |

|--------|---------------|

| **Search Volume** | Monthly searches |

| **Keyword Difficulty (KD)** | How hard to rank (0–100) |

| **CPC** | Cost per click (signals commercial value) |

| **CTR** | Click-through rate in search results |

| **Intent** | Informational / Navigational / Transactional |

  

### Free Keyword Research Tools

  

- **Google Search** — Autocomplete & "People Also Ask"

- **Google Search Console** — Queries your site already ranks for

- **Google Keyword Planner** — Volume estimates

- **Ubersuggest** — Free tier available

- **AnswerThePublic** — Question-based queries

- **Ahrefs / SEMrush** — Industry standard (paid)

  

### Process

  

```

1. Seed keywords → brainstorm core topics

2. Expand → use tools to find related & long-tail

3. Analyze → volume, difficulty, intent

4. Prioritize → high volume + low difficulty + matching intent

5. Map → one primary keyword per page

```

  

---

  

## 4. On-Page SEO — HTML Fundamentals

  

### Title Tag `<title>` — Most Important On-Page Factor

  

```html

<!-- ✅ Good -->

<title>CSS Flexbox Tutorial for Beginners — MyWebsite</title>

  

<!-- ❌ Bad — too short, vague -->

<title>Page</title>

  

<!-- ❌ Bad — keyword stuffed -->

<title>CSS CSS Flexbox CSS Tutorial Learn CSS Flexbox CSS</title>

```

  

| Rule | Guideline |

|------|-----------|

| Length | 50–60 characters |

| Primary keyword | Near the start |

| Brand | At the end, separated by `—` or `\|` |

| Unique | Every page must have a different title |

  

### Heading Hierarchy

  

```html

<!-- One <h1> per page — exact or near-match of primary keyword -->

<h1>CSS Flexbox Tutorial for Beginners</h1>

  

<!-- <h2> for main sections -->

<h2>What is Flexbox?</h2>

<h2>Flexbox Container Properties</h2>

  

  <!-- <h3> for subsections under h2 -->

  <h3>justify-content</h3>

  <h3>align-items</h3>

  

<!-- Never skip heading levels (h1 → h3 without h2) -->

```

  

### Anchor Text for Internal Links

  

```html

<!-- ✅ Descriptive anchor text -->

<a href="/css-flexbox">Learn CSS Flexbox</a>

  

<!-- ❌ Non-descriptive -->

<a href="/css-flexbox">Click here</a>

<a href="/css-flexbox">Read more</a>

```

  

### Semantic HTML for SEO

  

```html

<!-- ✅ Semantic — signals page structure to Google -->

<article>

  <header><h1>Article Title</h1></header>

  <main>...</main>

  <footer>By Jane Doe · Published <time datetime="2024-06-01">June 1, 2024</time></footer>

</article>

  

<!-- ❌ Non-semantic — Google understands less structure -->

<div class="article">

  <div class="title">Article Title</div>

  <div class="content">...</div>

</div>

```

  

---

  

## 5. Meta Tags for SEO

  

### Essential Meta Tags

  

```html

<head>

  <!-- Character encoding — must be first -->

  <meta charset="UTF-8" />

  

  <!-- Viewport — required for mobile-first indexing -->

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  

  <!-- Meta description — shown in search results snippet -->

  <!-- 150–160 characters, includes keyword, compelling CTA -->

  <meta name="description" content="Learn CSS Flexbox from scratch with this beginner's tutorial. Includes interactive examples, exercises, and a complete reference guide." />

  

  <!-- Canonical — prevents duplicate content issues -->

  <link rel="canonical" href="https://example.com/css-flexbox-tutorial" />

  

  <!-- Robots — control crawling/indexing per page -->

  <meta name="robots" content="index, follow" />          <!-- Default — allow everything -->

  <meta name="robots" content="noindex, follow" />        <!-- Don't index this page -->

  <meta name="robots" content="index, nofollow" />        <!-- Don't follow links -->

  <meta name="robots" content="noindex, nofollow" />      <!-- Block everything -->

  <meta name="robots" content="noarchive" />              <!-- No cached version -->

  

  <!-- Language -->

  <meta http-equiv="content-language" content="en" />

  

  <!-- Author -->

  <meta name="author" content="Jane Doe" />

</head>

```

  

### What Meta Description Does (and Doesn't Do)

  

```

✅ Affects: Click-through rate (CTR) in search results

❌ Does NOT directly affect: Rankings

  

Google may rewrite your description anyway — write for humans, not robots.

Best: Include primary keyword + a clear value proposition.

```

  

### Hreflang — Multi-language Pages

  

```html

<!-- Tell Google which language version to serve to which users -->

<link rel="alternate" hreflang="en"    href="https://example.com/page" />

<link rel="alternate" hreflang="fr"    href="https://example.com/fr/page" />

<link rel="alternate" hreflang="es"    href="https://example.com/es/page" />

<link rel="alternate" hreflang="x-default" href="https://example.com/page" />

```

  

---

  

## 6. URL Structure

  

### Good vs Bad URLs

  

```

✅ Good URLs:

https://example.com/css-flexbox-tutorial

https://example.com/blog/seo-guide-2024

https://example.com/products/blue-running-shoes

  

❌ Bad URLs:

https://example.com/p?id=482&cat=3&ref=home   (parameters, no keywords)

https://example.com/CSS_Flexbox_TUTORIAL       (uppercase, underscores)

https://example.com/this-is-a-very-long-url-with-too-many-words-in-it

```

  

### URL Best Practices

  

| Rule | Example |

|------|---------|

| Use hyphens (not underscores) | `css-flexbox` not `css_flexbox` |

| All lowercase | `css-flexbox` not `CSS-Flexbox` |

| Keep it short | 3–5 words max |

| Include primary keyword | `/css-flexbox-tutorial` |

| No stop words (usually) | `css-flexbox` not `a-guide-to-css-flexbox` |

| No special characters | Avoid `?`, `#`, `%`, `&` in the slug |

| Use subfolders for hierarchy | `/blog/2024/seo-guide` |

  

### Canonical URLs

  

Canonical tags tell Google which version of a URL is the "original" to prevent duplicate content penalties:

  

```html

<!-- On https://example.com/css-flexbox?ref=twitter -->

<link rel="canonical" href="https://example.com/css-flexbox" />

  

<!-- On https://www.example.com/page (www vs non-www) -->

<link rel="canonical" href="https://example.com/page" />

```

  

---

  

## 7. Content SEO

  

### E-E-A-T (Google's Quality Framework)

  

Google evaluates content on **E-E-A-T**:

  

| Letter | Stands For | How to Signal |

|--------|-----------|---------------|

| **E** | Experience | First-hand experience, personal stories, case studies |

| **E** | Expertise | Author credentials, technical depth, accuracy |

| **A** | Authoritativeness | Backlinks, brand mentions, citations |

| **T** | Trustworthiness | HTTPS, clear authorship, sources cited, reviews |

  

### Content Length

  

```

Blog posts:    1,500–2,500+ words for competitive topics

Product pages: 300–800 words + specs + reviews

Homepage:      500–1,000 words

Landing pages: 800–1,500 words relevant to the offer

  

Longer ≠ better — quality and completeness beats word count.

```

  

### Content Best Practices

  

```

1. Cover the topic comprehensively — answer all related questions

2. Use subheadings (h2, h3) to organize content

3. Write for humans first, optimize for crawlers second

4. Use your primary keyword:

   - In the <title>

   - In the <h1>

   - In the first 100 words

   - Naturally 2–4x in the body

5. Include LSI / related keywords naturally

6. Update content regularly (freshness factor)

7. Use short paragraphs (2–3 sentences max)

8. Include FAQ sections (targets featured snippets)

9. Add a table of contents for long articles

10. Link to authoritative external sources

```

  

### Featured Snippets

  

Featured snippets appear above position 1 ("position zero"):

  

```html

<!-- Target paragraph snippets -->

<h2>What is CSS Flexbox?</h2>

<p>CSS Flexbox (Flexible Box Layout) is a one-dimensional layout model

   that distributes space and aligns items in a container, even when their

   sizes are unknown or dynamic.</p>

  

<!-- Target list snippets -->

<h2>How to Center a Div with Flexbox</h2>

<ol>

  <li>Set the parent to <code>display: flex</code></li>

  <li>Add <code>justify-content: center</code></li>

  <li>Add <code>align-items: center</code></li>

</ol>

  

<!-- Target table snippets -->

<h2>Flexbox Properties Comparison</h2>

<table>...</table>

```

  

---

  

## 8. Technical SEO

  

### HTTPS

  

```

Google uses HTTPS as a ranking signal since 2014.

  

✅ https://example.com  ← Secure

❌  http://example.com  ← Insecure (browsers show "Not Secure" warning)

  

Always redirect HTTP → HTTPS with a 301 redirect.

```

  

### HTTP Status Codes

  

| Code | Meaning | SEO Impact |

|------|---------|------------|

| `200 OK` | Page exists | Normal — indexed |

| `301 Moved Permanently` | Permanent redirect | Passes ~100% link equity |

| `302 Found` | Temporary redirect | Does NOT pass full equity |

| `404 Not Found` | Page missing | Dropped from index eventually |

| `410 Gone` | Permanently deleted | Faster deindexing than 404 |

| `500 Server Error` | Server problem | Crawl fails — bad for SEO |

| `503 Service Unavailable` | Temporary downtime | OK if brief — Googlebot retries |

  

### Crawl Budget

  

Large sites need to manage how many pages Googlebot crawls per day:

  

```

1. Remove or noindex low-value pages (duplicate, thin, admin)

2. Fix broken links (4xx errors waste crawl budget)

3. Optimize robots.txt to block non-SEO paths

4. Use sitemaps to guide crawlers to important pages

5. Improve server response time (slow servers get fewer crawls)

```

  

### Redirect Chains — Avoid These

  

```

❌ Bad — 3 hops wastes link equity and slows crawlers:

/old-page → /intermediate → /another-page → /final-page

  

✅ Good — direct 301:

/old-page → /final-page

```

  

### Duplicate Content

  

```

Causes:

- www vs non-www   → Fix: canonical + redirect to one version

- HTTP vs HTTPS    → Fix: redirect all to HTTPS + canonical

- Trailing slash   → Fix: example.com/page vs example.com/page/  → pick one

- URL parameters   → Fix: canonical or noindex parameter URLs

- Syndicated content → Add canonical pointing to original

  

Use: <link rel="canonical" href="[master-url]" />

```

  

### Core HTML File: Putting Technical Tags Together

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="UTF-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>Primary Keyword — Brand Name</title>

  <meta name="description" content="150–160 char description with keyword." />

  <link rel="canonical" href="https://example.com/page-slug" />

  <meta name="robots" content="index, follow" />

  

  <!-- Preconnect to speed up 3rd party resources -->

  <link rel="preconnect" href="https://fonts.googleapis.com" />

  <link rel="dns-prefetch" href="https://fonts.googleapis.com" />

  

  <!-- Favicon -->

  <link rel="icon" href="/favicon.ico" />

  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />

</head>

<body>

  <header>

    <nav aria-label="Main navigation">...</nav>

  </header>

  <main>

    <article>

      <h1>Primary Keyword in H1</h1>

      <p>First paragraph includes primary keyword within first 100 words.</p>

    </article>

  </main>

  <footer>...</footer>

</body>

</html>

```

  

---

  

## 9. Core Web Vitals & Page Speed

  

Google uses **Core Web Vitals** as ranking signals (since 2021).

  

### The Three Core Web Vitals

  

| Metric | Measures | Good | Needs Work | Poor |

|--------|----------|------|------------|------|

| **LCP** — Largest Contentful Paint | Loading speed of main content | ≤ 2.5s | 2.5–4s | > 4s |

| **INP** — Interaction to Next Paint | Responsiveness to interactions | ≤ 200ms | 200–500ms | > 500ms |

| **CLS** — Cumulative Layout Shift | Visual stability (elements jumping) | ≤ 0.1 | 0.1–0.25 | > 0.25 |

  

### How to Improve LCP (Largest Contentful Paint)

  

```html

<!-- 1. Preload the hero image — most impactful for LCP -->

<link rel="preload" as="image" href="hero.webp" />

  

<!-- 2. Use modern image formats -->

<img src="hero.webp" alt="Hero image" width="1200" height="600" fetchpriority="high" />

  

<!-- 3. Don't lazy-load above-the-fold images -->

<img src="hero.webp" alt="..." loading="eager" />  <!-- NOT lazy for hero -->

  

<!-- 4. Inline critical CSS -->

<style>

  /* Only CSS needed to render above-the-fold content */

  body { font-family: system-ui; }

  .hero { background: #6366f1; }

</style>

```

  

```

Other LCP improvements:

- Use a CDN to reduce server response time (TTFB < 800ms)

- Enable HTTP/2 or HTTP/3

- Compress images (WebP, AVIF)

- Enable browser caching (Cache-Control headers)

- Minify HTML, CSS, JavaScript

```

  

### How to Improve CLS (Cumulative Layout Shift)

  

```html

<!-- Always specify width and height on images -->

<img src="photo.jpg" alt="Photo" width="800" height="450" />

<!-- Browser reserves space before image loads — no layout shift -->

  

<!-- Reserve space for ads/embeds -->

<div style="aspect-ratio: 16/9; width: 100%;">

  <!-- Ad or embed goes here -->

</div>

```

  

```css

/* Avoid inserting content above existing content */

/* Use transform for animations, not top/left/margin */

```

  

### How to Improve INP (Interaction to Next Paint)

  

```javascript

// Break up long tasks (> 50ms blocks the main thread)

// ❌ One long task

function processAll(items) {

  items.forEach(item => heavyProcess(item));

}

  

// ✅ Yield to the browser between chunks

async function processAll(items) {

  for (const item of items) {

    heavyProcess(item);

    await new Promise(r => setTimeout(r, 0)); // yield

  }

}

```

  

### Tools to Measure

  

| Tool | What It Measures |

|------|-----------------|

| **PageSpeed Insights** | Real + lab CWV data |

| **Lighthouse** (Chrome DevTools) | Lab-only performance audit |

| **Google Search Console** (CWV report) | Real-world field data |

| **WebPageTest** | Detailed waterfall analysis |

| **GTmetrix** | Speed + CWV combined |

  

---

  

## 10. Mobile SEO

  

Google uses **mobile-first indexing** — it primarily uses the **mobile version** of your site for indexing and ranking.

  

### Responsive Design (Required)

  

```html

<!-- Viewport meta tag — without this, mobile SEO fails -->

<meta name="viewport" content="width=device-width, initial-scale=1.0" />

```

  

```css

/* Mobile-first CSS */

body { font-size: 1rem; }

  

@media (min-width: 768px) {

  body { font-size: 1.125rem; }

}

  

/* Tap target size — Google recommends minimum 48x48px */

button, a {

  min-width:  48px;

  min-height: 48px;

  padding:    12px;

}

```

  

### Mobile SEO Checklist

  

```

✅ Responsive layout (works on all screen sizes)

✅ Tap targets ≥ 48×48px with 8px spacing

✅ Font size ≥ 16px to avoid zooming

✅ No horizontal scrolling

✅ No intrusive interstitials (pop-ups that block content)

✅ Fast on mobile networks (test with 3G throttling)

✅ Same content on mobile and desktop

```

  

---

  

## 11. Structured Data (Schema Markup)

  

Structured data uses **JSON-LD** format to tell Google exactly what your content is. It can unlock **rich results** (stars, FAQs, breadcrumbs, recipes in search).

  

### JSON-LD Format (Recommended by Google)

  

```html

<script type="application/ld+json">

{

  "@context": "https://schema.org",

  "@type": "Article",

  "headline": "CSS Flexbox Tutorial for Beginners",

  "author": {

    "@type": "Person",

    "name": "Jane Doe",

    "url": "https://example.com/author/jane-doe"

  },

  "datePublished": "2024-06-01",

  "dateModified": "2024-06-15",

  "image": "https://example.com/images/flexbox-tutorial.jpg",

  "publisher": {

    "@type": "Organization",

    "name": "MyWebsite",

    "logo": {

      "@type": "ImageObject",

      "url": "https://example.com/logo.png"

    }

  },

  "description": "Learn CSS Flexbox from scratch with this beginner tutorial."

}

</script>

```

  

### FAQ Schema (Unlocks FAQ Rich Results)

  

```html

<script type="application/ld+json">

{

  "@context": "https://schema.org",

  "@type": "FAQPage",

  "mainEntity": [

    {

      "@type": "Question",

      "name": "What is CSS Flexbox?",

      "acceptedAnswer": {

        "@type": "Answer",

        "text": "CSS Flexbox is a one-dimensional layout model for distributing space among items in a container."

      }

    },

    {

      "@type": "Question",

      "name": "When should I use Flexbox vs Grid?",

      "acceptedAnswer": {

        "@type": "Answer",

        "text": "Use Flexbox for one-dimensional layouts (rows or columns). Use Grid for two-dimensional layouts."

      }

    }

  ]

}

</script>

```

  

### Breadcrumb Schema

  

```html

<script type="application/ld+json">

{

  "@context": "https://schema.org",

  "@type": "BreadcrumbList",

  "itemListElement": [

    { "@type": "ListItem", "position": 1, "name": "Home",     "item": "https://example.com" },

    { "@type": "ListItem", "position": 2, "name": "Blog",     "item": "https://example.com/blog" },

    { "@type": "ListItem", "position": 3, "name": "CSS Guide", "item": "https://example.com/blog/css-guide" }

  ]

}

</script>

```

  

### Other Common Schema Types

  

| Schema Type | Rich Result Unlocked |

|-------------|----------------------|

| `Product` | Price, availability, reviews |

| `Recipe` | Cook time, calories, ratings |

| `Event` | Date, location, ticket info |

| `LocalBusiness` | Address, phone, hours, map pin |

| `Review` / `AggregateRating` | Star ratings |

| `HowTo` | Step-by-step in results |

| `VideoObject` | Video thumbnail in results |

| `SoftwareApplication` | App rating, OS, price |

| `Person` | Author profile card |

| `Organization` | Brand knowledge panel |

  

> Validate your markup at: **[search.google.com/test/rich-results](https://search.google.com/test/rich-results)**

  

---

  

## 12. Open Graph & Social SEO

  

Open Graph tags control how your page looks when shared on Facebook, LinkedIn, Twitter, Slack, etc.

  

```html

<head>

  <!-- Open Graph (Facebook, LinkedIn, Slack, Discord, WhatsApp) -->

  <meta property="og:type"        content="article" />

  <meta property="og:title"       content="CSS Flexbox Tutorial for Beginners" />

  <meta property="og:description" content="Learn Flexbox from scratch with interactive examples." />

  <meta property="og:image"       content="https://example.com/og-flexbox.jpg" />

  <meta property="og:image:width" content="1200" />

  <meta property="og:image:height"content="630" />

  <meta property="og:url"         content="https://example.com/css-flexbox-tutorial" />

  <meta property="og:site_name"   content="MyWebsite" />

  <meta property="og:locale"      content="en_US" />

  

  <!-- Twitter / X Card -->

  <meta name="twitter:card"        content="summary_large_image" />

  <meta name="twitter:site"        content="@MyWebsite" />

  <meta name="twitter:creator"     content="@JaneDoe" />

  <meta name="twitter:title"       content="CSS Flexbox Tutorial for Beginners" />

  <meta name="twitter:description" content="Learn Flexbox from scratch with interactive examples." />

  <meta name="twitter:image"       content="https://example.com/og-flexbox.jpg" />

</head>

```

  

### OG Image Best Practices

  

```

Dimensions: 1200 × 630 pixels (1.91:1 ratio)

Format:     JPG or PNG

File size:  < 1 MB

Content:    Title + logo + visuals (no important info at edges)

Test with:  Facebook Sharing Debugger, Twitter Card Validator

```

  

---

  

## 13. Image SEO

  

### Optimized Image Markup

  

```html

<!-- Always include alt text — describes the image for screen readers AND Google -->

<img

  src="flexbox-diagram.webp"

  alt="CSS Flexbox container with justify-content space-between diagram"

  width="800"

  height="450"

  loading="lazy"

  decoding="async"

/>

  

<!-- fetchpriority for above-the-fold hero images -->

<img

  src="hero.webp"

  alt="Developer coding on a laptop"

  width="1200"

  height="600"

  fetchpriority="high"

  loading="eager"

/>

```

  

### Image File Best Practices

  

| Rule | Detail |

|------|--------|

| **Format** | WebP first (30–35% smaller than JPEG), AVIF for best compression, JPEG for photos, PNG for transparency |

| **Filename** | `css-flexbox-diagram.webp` (descriptive, hyphenated) not `IMG_4829.jpg` |

| **Alt text** | Describe the image; include keyword naturally if relevant |

| **Compression** | Compress to ≤ 150KB for most images |

| **Dimensions** | Serve at display size — don't serve 3000px if displaying 800px |

| **Lazy loading** | `loading="lazy"` on below-the-fold images |

| **Responsive** | Use `srcset` to serve appropriate sizes |

  

### `srcset` for Responsive Images

  

```html

<img

  src="image-800.webp"

  srcset="image-400.webp 400w, image-800.webp 800w, image-1600.webp 1600w"

  sizes="(max-width: 640px) 400px, (max-width: 1200px) 800px, 1600px"

  alt="Descriptive alt text here"

  width="800"

  height="450"

/>

```

  

---

  

## 14. Link Building (Off-Page SEO)

  

Backlinks (links from other websites to yours) are one of Google's strongest ranking signals. A link from a trusted site is a "vote of confidence."

  

### Link Quality Factors

  

| Factor | High Quality | Low Quality |

|--------|-------------|-------------|

| **Domain Authority** | High DA site (news, gov, edu) | Spam/low-DA site |

| **Relevance** | Same niche/topic | Unrelated topic |

| **Anchor Text** | Descriptive + natural | Over-optimized keyword |

| **Link Position** | In the body content | Footer / sidebar |

| **Follow vs Nofollow** | `dofollow` passes equity | `rel="nofollow"` limited equity |

| **Link placement** | Editorial (earned naturally) | Paid / directory |

  

### Ethical Link Building Strategies

  

```

CONTENT-BASED:

✅ Write comprehensive guides others will cite

✅ Create original research, surveys, or data

✅ Publish infographics (others embed + link back)

✅ Guest posting on reputable sites in your niche

  

RELATIONSHIP-BASED:

✅ HARO (Help a Reporter Out) — get cited by journalists

✅ Broken link building — find broken links, offer your content as replacement

✅ Resource page link building — get added to curated resource lists

✅ Partnership / co-marketing with complementary brands

  

BRAND-BASED:

✅ Unlinked brand mentions → ask for a link

✅ PR campaigns → news sites cover you and link

✅ Podcast appearances → get a link from show notes

```

  

### Link Attributes

  

```html

<!-- dofollow (default) — passes link equity to destination -->

<a href="https://example.com">Anchor Text</a>

  

<!-- nofollow — does NOT pass equity (use on paid/untrusted links) -->

<a href="https://example.com" rel="nofollow">Sponsored Link</a>

  

<!-- Sponsored — marks paid links (required by Google for paid placements) -->

<a href="https://example.com" rel="sponsored">Affiliate Link</a>

  

<!-- UGC — marks user-generated content links -->

<a href="https://example.com" rel="ugc">Comment Link</a>

```

  

---

  

## 15. Local SEO

  

Local SEO optimizes for geographically based searches like "coffee shop near me" or "plumber in Chicago".

  

### Google Business Profile

  

```

Most impactful local SEO action:

1. Claim your Google Business Profile (free) at business.google.com

2. Fill in ALL fields: name, address, phone, hours, photos, category

3. Respond to every review (positive and negative)

4. Post updates weekly

5. Verify your listing

```

  

### Local Schema Markup

  

```html

<script type="application/ld+json">

{

  "@context": "https://schema.org",

  "@type": "LocalBusiness",

  "name": "Jane's CSS Academy",

  "url": "https://janescssacademy.com",

  "telephone": "+1-555-123-4567",

  "address": {

    "@type": "PostalAddress",

    "streetAddress": "123 Web Street",

    "addressLocality": "New York",

    "addressRegion": "NY",

    "postalCode": "10001",

    "addressCountry": "US"

  },

  "openingHoursSpecification": [

    {

      "@type": "OpeningHoursSpecification",

      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],

      "opens": "09:00",

      "closes": "18:00"

    }

  ],

  "geo": {

    "@type": "GeoCoordinates",

    "latitude": 40.7128,

    "longitude": -74.0060

  }

}

</script>

```

  

### NAP Consistency

  

**NAP** = **N**ame, **A**ddress, **P**hone number  

Must be **identical** across your website, Google Business Profile, Yelp, directories, and social profiles.

  

```

❌ Inconsistent:

Website:  123 Web Street, New York, NY

Yelp:     123 Web St., New York, NY 10001

Google:   123 Web Street Suite 4, New York

  

✅ Consistent everywhere:

123 Web Street, New York, NY 10001

```

  

---

  

## 16. `robots.txt` & `sitemap.xml`

  

### `robots.txt`

  

Controls which pages search engine bots can crawl. Lives at the **root** of your domain: `https://example.com/robots.txt`

  

```

# Allow all bots to crawl everything

User-agent: *

Allow: /

  

# Block all bots from admin and private areas

User-agent: *

Disallow: /admin/

Disallow: /dashboard/

Disallow: /private/

Disallow: /api/

  

# Block only a specific bot

User-agent: Bingbot

Disallow: /

  

# Block specific file types

User-agent: *

Disallow: /*.pdf$

  

# Point to your sitemap

Sitemap: https://example.com/sitemap.xml

```

  

> **Warning:** `robots.txt` controls **crawling** but NOT **indexing**.  

> A blocked URL can still appear in results if other sites link to it.  

> To block indexing, use `<meta name="robots" content="noindex" />` on the page.

  

### `sitemap.xml`

  

A sitemap tells Google about all pages on your site so they can be discovered and crawled efficiently.

  

```xml

<?xml version="1.0" encoding="UTF-8"?>

<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">

  

  <!-- Homepage -->

  <url>

    <loc>https://example.com/</loc>

    <lastmod>2024-06-01</lastmod>

    <changefreq>weekly</changefreq>

    <priority>1.0</priority>

  </url>

  

  <!-- Blog post -->

  <url>

    <loc>https://example.com/blog/css-flexbox-tutorial</loc>

    <lastmod>2024-05-20</lastmod>

    <changefreq>monthly</changefreq>

    <priority>0.8</priority>

  </url>

  

  <!-- Contact page -->

  <url>

    <loc>https://example.com/contact</loc>

    <lastmod>2024-01-01</lastmod>

    <changefreq>yearly</changefreq>

    <priority>0.5</priority>

  </url>

  

</urlset>

```

  

### Sitemap Index (Large Sites)

  

```xml

<?xml version="1.0" encoding="UTF-8"?>

<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">

  <sitemap>

    <loc>https://example.com/sitemap-pages.xml</loc>

    <lastmod>2024-06-01</lastmod>

  </sitemap>

  <sitemap>

    <loc>https://example.com/sitemap-blog.xml</loc>

    <lastmod>2024-06-01</lastmod>

  </sitemap>

  <sitemap>

    <loc>https://example.com/sitemap-products.xml</loc>

    <lastmod>2024-06-01</lastmod>

  </sitemap>

</sitemapindex>

```

  

```

Submit your sitemap in Google Search Console:

Sitemaps → Enter sitemap URL → Submit

```

  

---

  

## 17. SEO for Single Page Apps (SPA)

  

SPAs (React, Vue, Angular) are harder to index because content is rendered by JavaScript.

  

### The Problem

  

```

Traditional HTML:  Googlebot fetches HTML → sees content immediately ✅

SPA (React/Vue):   Googlebot fetches HTML → sees empty <div id="root"> ❌

                   Must wait for JS to render — often fails or is delayed

```

  

### Solutions

  

#### 1. Server-Side Rendering (SSR) — Best

  

```

Next.js (React), Nuxt.js (Vue), SvelteKit:

→ HTML fully rendered on the server

→ Googlebot gets complete content immediately

→ Fastest time-to-index

```

  

#### 2. Static Site Generation (SSG) — Best for Content Sites

  

```

Next.js (getStaticProps), Gatsby, Astro, Eleventy:

→ HTML pre-built at build time

→ Served as static files (extremely fast)

→ Perfect for blogs, docs, marketing sites

```

  

#### 3. Dynamic Rendering — Fallback

  

```

Serve pre-rendered HTML to bots, SPA to users:

→ Detect user-agent (Googlebot, Bingbot)

→ Use a renderer (Puppeteer, Rendertron) to generate HTML

→ Serve cached HTML to bots

```

  

#### 4. Prerendering Services

  

```

Prerender.io, Netlify On-Demand Builders, Vercel ISR

→ Middleware intercepts bot requests → returns pre-rendered HTML

```

  

### Essential Meta Tags for SPAs

  

```javascript

// React with Helmet or Next.js Head

import Head from 'next/head';

  

export default function BlogPost({ post }) {

  return (

    <>

      <Head>

        <title>{post.title} — MyBlog</title>

        <meta name="description" content={post.excerpt} />

        <link rel="canonical" href={`https://example.com/blog/${post.slug}`} />

        <meta property="og:title" content={post.title} />

        <meta property="og:description" content={post.excerpt} />

        <meta property="og:image" content={post.ogImage} />

      </Head>

      <article>...</article>

    </>

  );

}

```

  

---

  

## 18. Measuring & Monitoring SEO

  

### Essential Free Tools

  

| Tool | What to Monitor |

|------|----------------|

| **Google Search Console** | Impressions, clicks, CTR, position, index coverage, Core Web Vitals, crawl errors |

| **Google Analytics 4** | Organic traffic, landing pages, user behavior, conversions |

| **Bing Webmaster Tools** | Same as Search Console for Bing |

| **PageSpeed Insights** | Core Web Vitals, real + lab data |

| **Ahrefs Webmaster Tools** | Backlinks, broken links (free for your own site) |

  

### Key SEO Metrics to Track

  

```

Organic Traffic:    Sessions from search engines (Google Analytics)

Impressions:        How often you appear in results (Search Console)

CTR:                Clicks ÷ Impressions (Search Console) — target 3–5%+

Average Position:   Your average ranking position (Search Console)

Backlinks:          Number and quality of inbound links (Ahrefs/Moz)

Domain Rating (DR): Site authority score (Ahrefs)

Core Web Vitals:    LCP, INP, CLS scores (Search Console CWV report)

Index Coverage:     How many pages are indexed (Search Console)

```

  

### Ranking Tracking

  

```

Free:  Google Search Console (only your site)

Paid:  Ahrefs, SEMrush, Moz, SERPWatcher

       → Enter your keywords → monitor daily/weekly position changes

```

  

### Search Console Key Reports

  

```

Performance → Queries:       What keywords bring traffic

Performance → Pages:         Which pages rank

Coverage:                    Indexed vs non-indexed pages + errors

Core Web Vitals:             LCP/INP/CLS by URL

Sitemaps:                    Sitemap submission + discovery

Links → Top linked pages:    Internal + external link data

```

  

---

  

## 19. SEO Checklist

  

### ✅ Technical SEO

  

```

[ ] HTTPS enabled (force redirect HTTP → HTTPS)

[ ] Mobile-friendly (responsive design tested)

[ ] Page load < 3s (PageSpeed Insights score > 90)

[ ] Core Web Vitals all green (LCP, INP, CLS)

[ ] robots.txt configured correctly

[ ] XML sitemap submitted to Google Search Console

[ ] No crawl errors in Search Console

[ ] All pages return correct HTTP status codes

[ ] No broken internal links

[ ] Canonical tags on all pages

[ ] No duplicate content issues

[ ] URL structure clean and keyword-friendly

[ ] GZIP/Brotli compression enabled

[ ] Browser caching configured

```

  

### ✅ On-Page SEO

  

```

[ ] Unique <title> tag (50–60 chars) with keyword

[ ] Meta description (150–160 chars) with keyword + CTA

[ ] One H1 per page with primary keyword

[ ] H2/H3 headings used logically

[ ] Primary keyword in first 100 words

[ ] Keyword used 2–4x naturally in content

[ ] All images have descriptive alt text

[ ] Internal links to related pages

[ ] External links to authoritative sources

[ ] Content is comprehensive and original

[ ] Schema markup where applicable

[ ] Open Graph / Twitter Card tags present

[ ] Favicon set

```

  

### ✅ Content SEO

  

```

[ ] Target keyword has clear user intent match

[ ] Content answers the search query completely

[ ] Updated and accurate information

[ ] Author clearly identified

[ ] Sources cited for claims

[ ] FAQ section for featured snippet opportunities

[ ] Long-tail keywords used naturally

[ ] Reading level appropriate for audience

```

  

### ✅ Off-Page SEO

  

```

[ ] Google Business Profile claimed and complete (local)

[ ] NAP consistent across all directories

[ ] Active link building strategy

[ ] Social profiles linked to website

[ ] No spammy or toxic backlinks (disavow if needed)

```

  

---

  

## 20. Quick Reference Cheat Sheet

  

### Title & Meta

  

```html

<title>Keyword — Brand (50–60 chars)</title>

<meta name="description" content="150–160 chars with keyword + CTA" />

<link rel="canonical" href="https://example.com/slug" />

<meta name="robots" content="index, follow" />

```

  

### Open Graph

  

```html

<meta property="og:title"       content="Title" />

<meta property="og:description" content="Description" />

<meta property="og:image"       content="https://example.com/og.jpg" />

<meta property="og:url"         content="https://example.com/page" />

<meta name="twitter:card"       content="summary_large_image" />

```

  

### Structured Data Skeleton

  

```html

<script type="application/ld+json">

{ "@context":"https://schema.org", "@type":"Article",

  "headline":"...", "author":{"@type":"Person","name":"..."},

  "datePublished":"YYYY-MM-DD", "image":"...", "description":"..." }

</script>

```

  

### Heading Structure

  

```

H1 → Primary keyword (one per page)

  H2 → Main section (with LSI keywords)

    H3 → Subsection

```

  

### robots.txt Essentials

  

```

User-agent: *

Disallow: /admin/

Allow: /

Sitemap: https://example.com/sitemap.xml

```

  

### Core Web Vitals Targets

  

```

LCP < 2.5s  →  Preload hero image, use CDN, compress assets

INP < 200ms →  Break up long JS tasks, use requestAnimationFrame

CLS < 0.1   →  Set width/height on images, reserve space for embeds

```

  

---

  

> **The Golden Rule of SEO:**  

> *"Create the best possible resource for the person searching — Google's job is to find it."*

>

> Rankings are a byproduct of genuinely useful, fast, trustworthy content.

  

*Happy optimizing! 🚀*