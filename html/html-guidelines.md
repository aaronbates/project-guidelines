# HTML ![](../assets/images/html5_logo_32.png)

Semantic markup language for structuring content.

**Current version: [HTML5](http://www.w3.org/TR/html5/)**

---

## Table of contents

- [Guidelines](#guidelines)
  - [Accessibility](#accessibility)
  - [Colour](#colour)
  - [Images](#images)
  - [Webfonts](#webfonts)
  - [Misc.](#misc)
  - [Testing](#testing)
  - [Performance](#performance)
  - [Security](#security)
  - [Browser support](#browser-support)
- [Formatting](#formatting)
- [Boilerplates](#boilerplates)
- [Sectioning content](#sectioning-content)
- [Accessibility and ARIA](#aria)
- [Favicons](#favicons)
- [Social](#social)

---

## Guidelines

Structure (markup), presentation (styling: [CSS](../css/css-guidelines.md), [SASS](../sass/sass-guidelines.md)) and behaviour (scripting: [JavaScript](../js/js-guidelines.md)) should be kept strictly separate — unless a technology such as [React](../react/react-guidelines.md) is in use.

- Valid HTML5 must be used for all documents
- Design with graceful degradation (and progressive enhancement) in mind
- Use tags according to their semantic purpose
- Use headings to properly structure content
- Omit type attributes from stylesheet and scripts tags
- Omit the protocol for embedded resources

### Accessibility

- Give links unique and descriptive text
- Use a "skip nav" link if suitable
- Keep clickable areas obvious and large
- Use tables for tabular data, not layout
- Provide alternative content for media tags
- Design with accessibility in mind
- Use progressive enhancement
- Follow [ARIA](#accessibility-and-aria) guidelines

### Colour

- Never rely on colour alone to communicate meaning
- Use colour with care and consider colour-blind users

### Images

- Optimise all images ([ImageOptim](https://imageoptim.com), [SVGO](https://jakearchibald.github.io/svgomg/), etc)
- Provide images at 1x and 2x to support retina devices
- Use media queries to support retina background images
- Implement `srcset` and `sizes` where possible for native responsive images
- Use the `picture` element for art direction
- Use tools like [Picturefill](https://scottjehl.github.io/picturefill/) to polyfill this for older browsers
- "Lazy load" images where possible ([LazySizes](https://github.com/aFarkas/lazysizes))

### Webfonts

- Only provide WOFF, WOFF and TTF options ([source](https://alistapart.com/article/using-webfonts))
- Ensure they don't exceed 2MB in size

### Misc.

- Ensure 404 and 5XX pages exist
- Inline all CSS on 5XX pages (no external calls)
- Use [`rel="noopener"`](https://mathiasbynens.github.io/rel-noopener/) on any `target="_blank"` links

### Testing

- Ensure all pages are W3C compliant
- Use a HTML linter to support this

### Performance

- Server over HTTP/2 wherever possible
- Use `rel="preload"` (and `prefetch`) links in the `<head>` for stylesheets and scripts
- Minify HTML if suitable
- Test using Google PageSpeed

### Security

- **Serve over HTTPS wherever possible**
- HTTP Strict Transport Security (HSTS)
- Cross Site Request Forgery (CSRF)
- Cross Site Scripting (XSS)
- `X-Frame-Options` (XFO)
- `X-Content-Type-Options` set to `"nosniff"`

### Browser support

Aim to support:

- Chrome (latest 2)
- Edge (latest 2)
- Firefox (latest 2)
- Internet Explorer 9+ (ideally 10+)
- Opera (latest 2) — issues here.
- Safari (latest 2) — issues here.

## Formatting

- Use soft-tabs (2 spaces) for indentation
- Meaningful use of whitespace
- All tags must be lowercase (except comments)
- Quote all attributes, even if not needed
- Use double quotes `""` over single quotes
- Don't declare boolean attributes (presence is enough)
- Use a new line for every block, list or table element
- Every child element must be indented
- Trailing white space must be removed

Example:

```html
<!-- HTML -->

<p>Paragraph.</p>

<blockquote>
  <p><em>Emphasis</em>, on this blockquote.</p>
</blockquote>

<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>

<input type="text" disabled>

<table>
  <thead>
    <tr>
      <th scope="col">First</th>
      <th scope="col">Second</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td colspan="2">Footer</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>One</td>
      <td>Two</td>
    </tr>
  </tbody>
</table>
```

### Attribute order

Place HTML attributes in the following order for easier reading:

- `class`
- `id`, `name`
- `data-*`
- `src`, `for`, `type`, `href`, `value`
- `title`, `alt`
- `role`, `aria-*`

*Note: The above models the best practice for using CSS.*

## Boilerplates

Where suitable begin projects using the (ubiqitous) [HTML5 Boilerplate](https://html5boilerplate.com) project.

HTML5BP is "delete-key friendly", so from here we can strip back and create a light-weight customised boilerplate for our own uses.

Further reading: See the [HTML5BP docs on HTML](https://github.com/h5bp/html5-boilerplate/blob/master/dist/doc/html.md).

## Sectioning content

### `<div>`

General purpose. Groups content for targetting by CSS that is not semantically related.

```html
<div class="container">
  <section class="introduction">
    <p>Text.</p>
  </section>
</div>
```

_Use as last resort, meaningless to screen readers._

### `<section>`

Generic section of content grouped together in a semantically meaningful way or "theme" — define this theme using the opening heading element.

```html
<section class="introduction">
  <h1>Title</h1>
  <p>Text.</p>
  
  <h2>Title</h2>
  <p>More text.</p>
</section>
```

_Use when contents would be explicitly listed in a document outline._

### `<article>`

Semantically related content that is more specific than `<section>` and should be "self-contained" — it should be meanigful if separated from the rest of the page.

```html
<article class="article">
  <h1>Article title</h1>
  <p>Text of article.</p>
</article>
```

_Useful for marking up content to be syndicated or re-used._

### div, section or article?

1. **Is the content semantically related?**
  - **No** = `<div>`
  
2. **Is the content self-contained?**
  - **No** = `<section>`
  - **Yes** = `<article>`

### Nesting

- It's suitable to nest multiple `<article>` elements within a `<section>`.
- An `<article>` may contain many `<section>` elements.
- Inner `<article>` elements are assumed to be related to any outer `<article>`.

### A word on `<main>`

Should contain the "main" content of the page, this content should be unique to the page and not appear elsewhere.


```html
<main role="main">
  <article>
    <h1>Heading</h1>
    <p>Text</p>
  </article>
</main>
```

_Currently not fully supported by all browsers and there is divergence between various working group specs, advised instead to use `<section role="main">` or `<article role="main">` or polyfill the element using JavaScript in the document `<head>`:_

```javascript
document.createElement("main");
```

### Supporting elements

#### `<header>`

Defines a header or introductory content for a document or section.

#### `<nav>`

Used to mark up a collection of links to external pages or sections within the current page.

Used for main website navigation, but also a good fit for a table of contents or tabs.

#### `<aside>`

Represents content that is tangibly related to the content surrounding it, but could be considered separate — for example: sidebars, groups of nav elements and pull quotes.

#### `<footer>`

Defines a footer for a document or section.

#### `<address>`

Represent the contact information for an article, page or site. _Not just for physical addresses._

#### `<figure>`

Use `<figure>` and `<figcaption>` to semantically group visual assets.

## Accessibility and ARIA

Designed to provide accessiblity at a technical level, where it currently doesn't exist. Uses three methods:

1. What **role** a component has in the interface
2. What **properties** that component has
3. The **state** of the component

### Roles

#### Landmark

- `application` — web application
- `banner` — masthead of the site
- `complementary` — related information
- `contentinfo` — footer, copyright etc.
- `form` — form
- `main` — main content of the page
- `navigation` — navigation lists
- `search` — site search

#### Document structure

- `article`
- `columnheader`
- `definition`
- `directory` — references to members of a group
- `document` — document content
- `group`
- `heading`
- `img`
- `list` — non-interactive list items
- `listitem`
- `math`
- `note` — ancillary to main content
- `presentation` — purely presentational
- `region` — section important enough to be included in a toc
- `row`
- `rowgroup`
- `rowheader`
- `separator` — separates content or menu items
- `toolbar` — commonly used grouped controls

#### Widget

Such as `alert`, `button`, `menuitem`, `tabpanel` and `tab` — see [the W3C specification](https://www.w3.org/TR/wai-aria/roles#widget_roles) for details. 

### Properties and States

See the [W3C specification](https://www.w3.org/TR/wai-aria/states_and_properties#global_states).

Particularly important are:

- `aria-hidden` (aesthetic elements only)
- `aria-label` (use when no text label is visible for an element, e.g. "X" icons to close)
- `aria-labelledby`

### Testing accessibility

Consider testing using tools such as [WAVE](http://wave.webaim.org) as well as screen readers and keyboard navigation.

A little trick... put the `.qa-testing` namespace class on `<body>` to highlight accessibility issues (expand the rule below to other cases):

```css
img:not([alt]) {
  border: 5px solid red;
}
```
## Favicons

Create the following favicons (sizes in pixels):


| size    | name               | purpose
|---:     |:---                |:---
| *multi* | `favicon.ico`      | Required. 16x16, 32x32, 48x48
| 32x32   | `icon-32.png`      | Most desktop browsers
| 57x57   | `icon-57.png`      | iOS (first to third generation)
| 76x76   | `icon-76.png`      | iPad home screen
| 96x96   | `icon-96.png`      | Desktop shortcut, GoogleTV
| 120x120 | `icon-120.png`     | iPhone Retina
| 128x128 | `icon-128.png`     | Chrome web store
| 144x144 | `icon-144.png`     | Windows 8, IE10 Metro tile
| 152x152 | `icon-152.png`     | iPad Retina
| 167x167 | `icon-167.png`     | General use iOS/Android, iPad Pro
| 180x180 | `icon-180.png`     | iPhone 6+
| 192x192 | `icon-192.png`     | Chrome for Android
| 512x512 | `icon-512.png`     | Chrome for Android
| 558x558 | `icon-558.png`     | Windows 8.1+ tiles
| 558x270 | `icon-558x270.png` | Windows 8.1+ wide tiles

**Optimise all images after creation with a tool such as [ImageOptim](https://imageoptim.com/mac).**

### `browserconfig.xml`
Used to customise tiles on Windows 8.1+. By default points to `tile.png` and `tile-wide.png` which resize automatically as required.

```xml
<?xml version="1.0" encoding="utf-8"?>  
<browserconfig>  
  <msapplication>
    <tile>
      <square70x70logo src="icon-558.png"/>
      <square150x150logo src="icon-558.png"/>
      <square310x310logo src="icon-558.png"/>
      <wide310x150logo src="icon-558x270.png"/>
      <TileColor>[#HEXCODE bg colour]</TileColor>
    </tile>
  </msapplication>
</browserconfig> 
```

### `site.webmanifest`

JSON file that describes how your site should be turned into an "application" on a user's mobile device. Basic format:

```json
{
  "icons": [{
    "src": "icon-192.png",
    "sizes": "192x192",
    "type": "image/png"
  }, {
    "src": "icon-512.png",
    "sizes": "512x512",
    "type": "image/png"
  }],
  "start_url": "/"
}
```

See more here: [https://developer.mozilla.org/en-US/docs/Web/Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest).

### Full example code for `<head>`:

```html
<!-- icons -->
<link rel="icon" href="icon-32.png" sizes="32x32">
<link rel="icon" href="icon-57.png" sizes="57x57">
<link rel="icon" href="icon-76.png" sizes="76x76">
<link rel="icon" href="icon-96.png" sizes="96x96">
<link rel="icon" href="icon-128.png" sizes="128x128">
<link rel="icon" href="icon-228.png" sizes="228x228">

<!-- icons: android -->
<link rel="shortcut icon" href="icon-192.png" sizes="192x192">

<!-- icons: ios -->
<link rel="apple-touch-icon" href="icon-120.png" sizes="120x120">
<link rel="apple-touch-icon" href="icon-152.png" sizes="152x152">
<link rel="apple-touch-icon" href="icon-167.png" sizes="167x167">
<link rel="apple-touch-icon" href="favicon-180.png" sizes="180x180">

<!-- icons: win8+ie10 -->
<meta name="msapplication-TileColor" content="[#HEXCODE bg colour]">
<meta name="msapplication-TileImage" content="icon-144.png">

<!-- icons: win8.1+ie11 and above -->
<meta name="msapplication-config" content="browserconfig.xml" />

<!-- web manifest -->
<link rel="manifest" href="site.webmanifest">
```

We don't explicitly link to `favicon.ico` and remember to ensure your paths are correct (absolute or relative).


## Social

Support Facebook Open Graph and Twitter cards.

### Facebook Open Graph

Test all Facebook Open Graph (OG) tags (no missing or false information). Images need to be at least 600 x 315 pixels, 1200 x 630 pixels recommended.

```html
<meta property="og:type" content="website">
<meta property="og:url" content="https://example.com/page.html">
<meta property="og:title" content="Content Title">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:description" content="Description Here">
<meta property="og:site_name" content="Site Name">
<meta property="og:locale" content="en_GB">
```

### Twitter cards

```html
<meta name="twitter:card" content="summary">
<meta name="twitter:site" content="@site_account">
<meta name="twitter:creator" content="@individual_account">
<meta name="twitter:url" content="https://example.com/page.html">
<meta name="twitter:title" content="Content Title">
<meta name="twitter:description" content="Content description less than 200 characters">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

## SEO

### `robots.txt`

During development always use a universal disallow in `robots.txt` — **(!) but remember to change this before launch (!)**:

```text
User-agent: *
Disallow: /
```

For production use a suitable customised file that grants access to public folders but directs robots away from any unhelpful or useless data or stubs.

## Acknowledgements 

Credit and thanks to:

- [Audrey Roy Greenfeld](https://github.com/audreyr) for her [favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet).

---

## TODO

- [ ] Expand Images section for full responsive info – `srcset`, `sizes`, `picture` etc.
- [ ] SEO
