# CSS ![](../assets/images/css3_logo_32.png)

Cascading Style Sheets for formatting and presentation of HTML pages and content.

**Current version: [CSS3](https://www.w3.org/Style/CSS/Overview.en.html)**

---

## Contents

- [Terminology](#terminology)
- [Guidelines](#guidelines)
  - [Typography](#typography)
  - [Z-index](#z-index)
  - [Testing](#testing)
  - [Browser support](#browser-support)
- [Formatting](#formatting)
  - [Properties](#properties)
  - [Comments](#comments)
- [Media queries](#media-queries)
- [Print styles](#print-styles)
- [File management](#file-management)
- [Projects](#projects)
- [Naming conventions](#naming-conventions)
  - [BEM (Block Element Modifier)](#bem-block-element-modifier)
  - [Name things for humans.](#name-things-for-humans)
- [A word on specificity...](#a-word-on-specificity)
- [!important](#important)
- [Namespaces](#namespaces)
- [The Open/Closed Principle](#the-openclosed-principle)
- [The Single Responsibility Principle](#the-single-responsibility-principle)
- [Composition over inheritence](#composition-over-inheritence)
- [Performance](#performance)
- [Critical CSS](#critical-css)
- [Boilerplates](#boilerplates)
- [Choosing the right tools](#choosing-the-right-tools)
- [CSS-in-JS](#css-in-js)
- [Acknowledgements](#acknowledgements)

---

## Terminology

**CSS Rule:** Selector (or group of selectors) with accompanying properties and values:

```css
/** CSS rule */

.selector-a,
.selector-b,
[attribute-selector] {
  property-1: value;
  property-2: value;
}
```

## Guidelines

- Valid CSS must be used
- **Do not use ID selectors (!)**
- **Do not use [qualifying type selectors](https://stylelint.io/user-guide/rules/selector-no-qualifying-type/)**
- Exercise good [Selector Intent](https://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)
- Group styles based on the subject (key selector)
- Avoid "dangerous" selectors with too broad a reach
- Never bind to the same class in both CSS and JavaScript
- Never use `data-*` attributes for styling
- Avoid `@import` — favour multiple links in `<head>`
- Omit protocol and quotes from embedded resources
- Use a solid reset base ([Reset](https://meyerweb.com/eric/tools/css/reset/), [Normalize](https://necolas.github.io/normalize.css/) or [Reboot](https://getbootstrap.com/docs/4.0/content/reboot/))

### Typography

- Use the ["native font stack"](https://www.smashingmagazine.com/2015/11/using-system-ui-fonts-practical-guide/) for default fonts
- Use relative line heights

### Z-index

- Categorise all uses as either "Local" or "Global"
- **Local**: elements to render on top of a sibling or nearby element
  - Must be contained in a new stacking context
  - Rarely have a `z-index` greater than 1
- **Global**: elements to render on top of all other page elements
  - Values must be declared as global variables
  - Should be generally fewer than 10 values per project 
- Declare in increments of 10 for simplicity and flexibility (`0`, `10`, `20` etc. — this allows for `15` if needed)
- Create a new stacking context using `position: relative;` and `z-index: 0;` — where possible make this a mixin with a descriptive name.

### Testing

- Ensure all CSS is W3C compliant
- Use a CSS linter to support this

### Browser support

- Vendor prefix for suitable browsers (ideally using [Autoprefixer](https://autoprefixer.github.io))
- Keep browser specific CSS in seperate files

## Formatting

- Use soft-tabs (2 spaces) for indentation
- 80 character wide columns (where possible)
- Meaningful use of whitespace
- Prefer dashes to camelCasing and PascalCasing
- Use single quotes `''` over double quotes
- Give each selector in a rule its own line
- Put a space before the opening brace `{` in rules
- Put closing braces `}` of rules on a new line
- Rules must be seperated by a new line

### Properties
- Put a space after, but not before, the `:` in properties
- Properties must be followed by a `;`
- List properties in alphabetical order
- Omit unit values after `0` (e.g. px)
- Omit a leading `0` from decimal values
- For borders use `0` instead of `none` 
- Use spaces in comma-separated property values
- Don't uses spaces like this in `rgba()` style values
- Avoid shorthand properties ([source](https://csswizardry.com/2016/12/css-shorthand-syntax-considered-an-anti-pattern/))
- Avoid [magic numbers](https://csswizardry.com/2012/11/code-smells-in-css/#magic-numbers)

_Note: Vendor prefix properties must be placed by their native property and sorted._

### Comments

- **Comments are good, use them more!**
- Prefer line comments (`/**...*/`) to block comments
- Prefer comments on their own line. Avoid end-of-line comments
- Use DocBlock-esque multi-line (C-style) comments where needed
- Write detailed comments for code that is not self-documenting, for example:
  - If code "extends" another file in the project (OOCSS)
  - Browser specific code and hacks
  - Complex uses of z-index

##### Example

```css
/**
 * C-style comment for multi-line use
 */ 
.one,
.selector,
.per-line {
  font-family: 'open sans', arial, sans-serif;
  font-style: italic;
  font-weight: bold;
  font-size: 18px;
  line-height: 1.2;
  margin: 0;
  padding: 0;
  width: 100%;
}

/** inline comment */
.alert {
  background-color: #fff;
  border: 1px solid #000;
  -moz-border-radius: 4px;
  -o-border-radius: 4px;
  -webkit-border-radius: 4px;
  border-radius: 4px;
  color: #000;
  font-size: .8em;
  text-align: center;
}

@media screen {
  .example {
    background-color: #fff;
  }
}

```

There is one exception to the rule of writing CSS across new lines: when related rulesets one carry one selector and declaration each:

```css
.alert {
  background-color: grey;
  color: #fff;
}

.alert--success { background-color: green; };
.alert--warning { background-color: yellow; };
.alert--danger  { background-color: red; };
.alert--info    { background-color: blue; };
```

## Media queries

?How are we approaching this?

- Avoid device-based media query names such as `phone` or `desktop`, these have limited longevity?
- Prefer viewport releated naming such as `xs`, `small`?

## CSS Variables

CSS variable [support is improving](https://caniuse.com/css-variables) — most modern browsers can use them today. They are powerful as they enable the separation of logic from design:

```css
:root {
  --color-red: #ff0000;
  --color-black: #121212;
}

.rule {
  color: var(--color-red);
}
```

However, it's important to remember they differ from Sass variables in that they are **dynamically scoped** and thus are subject to inheritance and cascade.

Notes and best practice:

- CSS variables can only be used on property values.
- Continue to use pre-processor variables for static declarations (such as breakpoint widths).
- If it changes, it probably should be a variable — this applies widely to responsive design and media queries.
- Don't swap one variable for another using a selector or media query — instead define a variable once and then *change it's value with a selector / media query*.
- The approach of defining and then changing variables in media queries should lead to writing less mq's.
- Selectors can be simplified by applying variable changes on a selector level.
- Remembering the cascade, avoid writing generic selector variables — never use `*` and be aware when setting values on large layout selectors such as `.container` etc.

```css
/** default variable values */
:root {
  /** scale for 1.2 */
  --font-size-1: 1rem;
  --font-size-2: 1.2rem;
  --font-size-3: 1.44rem;
  --font-size-4: 1.728rem;
  --font-size-5: 2.074rem;
  --font-size-6: 2.488rem;
  
  --color: #222;
  --background-color: #fff;
}

/** variables set on an selector */
aside {
  --color: #fff;
  --background-color: #222;
}

/** variables set on a media query */
@media screen and (min-width: 800px) {
  :root {
    /** scale for 1.33 */
    --font-size-1: 1rem;     
    --font-size-2: 1.333rem; 
    --font-size-3: 1.777rem;
    --font-size-4: 2.369rem; 
    --font-size-5: 3.157rem;
    --font-size-6: 4.209rem;
  }
}

/**
 * this produce different results despite using the same variables 
 */
main,
aside {
  background-color: var(--background-color);
  color: var(--color);
}
```

*Note: For best support, polyfill your process using a [PostCSS](http://postcss.org) and [cssnext](http://cssnext.io).*

## Print styles

CSS should be provided to optimise the printing process and make printed pages easier to read.

- Strip background colours, change font colour to black and remove `text-shadow`.
- Underline and expand link to show the URL (ignore fragments or psuedo links).
- Expand abbreviations to include a full description.
- Provide instructions on how [supporting browsers](https://en.wikipedia.org/wiki/Comparison_of_layout_engines_%28Cascading_Style_Sheets%29#Grammar_and_rules) should break content into pages and on orphans/widows:
  - ensure `<thead>` is printed on spanned pages
  - prevent `<blockquote>`, `<pre>`, table rows and images from splitting across pages
  - ensure headings never appear on a different page to the text they associate with
  - ensure that orphans and widows do not appear on printed pages.

Print styles should be included along with other stylesheets and an additional HTTP request should be avoided. Print styles must be included last so they can override other styles.

## File management

If working on anything other than a small project, it's a good idea to split code across multiple files — each file having its own scope or meaning.

Even if not using a preprocessor (e.g. [Sass](../sass/sass-guidelines.md)), these files can be merged during a build step using a tool such as [Gulp](../gulp/gulp-guidelines.md) or [Webpack](../webpack/webpack-guidelines.md).

### Table of contents

On larger projects, especially those split across multiple files, it's also a good idea to have a commented, meaningful table of contents at the root of your project "master" stylesheet.

### Tiles and sectioning and titles

Start each CSS file with a useful title (see below). Then split each CSS file into code sections where suitable — the title of each section is capitalized and prefixed witha hash `!` to allow for targeted search and `grep` (as we won't be using `!important`).

Leave 1 empty line after each title and 2 lines between each section for easier scanning.

##### Example

```css
/** =================================
 * !name: [name]
 * !desc: [short description]
 *=================================== */

/** ---------------------------------
 * !SECTION-TITLE
 * ---------------------------------- */
 
.rule {}


/** ---------------------------------
 * !ANOTHER-SECTION
 * ---------------------------------- */

.another-rule {}
```

## Projects

The [Inverted Triangle (ITCSS)](http://www.creativebloq.com/web-design/manage-large-css-projects-itcss-101517528) method is one recommended method to organise project files.

*Note: ITCSS more specifically relates to CSS using preprocessers such as Sass, but can be applied to pure CSS projects too, especially when using a build tool such as [webpack](https://webpack.js.org) or Gulp (see [webpack guidelines](../webpack/webpack-guidelines.md)).*

Split the codebase into the following layers:

1. `settings` — vars, preprocesser settings (optional)
2. `tools` — globally used mixins and functions (optional)
3. `generic` — reset/normalize, etc.
4. `vendor` — vendor libs.
4. `base` — HTML elements (h1, a, etc.) etc.
5. `objects` — class-based undecorated design patterns
6. `layout` — layout styling for specific purposes (articles, products, etc.)
7. `components` — specific UI components
8. `themes` — thematic styling (if required)
9. `utils` — helper classes, overrides, hacks (if we must)

_Note: you should output no CSS in the first 2 layers._

The concept is that as we move "down" the inverted triangle, we move from:

- Generic to explicit
- Low-specificity to specific
- Far-reaching to localised

##### Example structure

```css
/** if using a preprocesser */
@import 'settings/global.css';

@import 'tools/mixins';

/** core */
@import 'generic/normalize';

/** @import 'vendor/[file].css'; */

@import 'base/headings.css';
@import 'base/forms.css';
@import 'base/links.css';
@import 'base/lists.css';
@import 'base/tables.css';

@import 'objects/animations.css';
@import 'objects/media.css';
@import 'objects/overlays.css';

@import 'layout/article.css';
@import 'layout/product.css';

@import 'components/buttons.css';
@import 'components/comments.css';
@import 'components/hero.css';

/** @import 'themes/[file].css'; */

@import 'utils/utils.css';
```
*Note: it goes without saying that you should not use native CSS imports in production code and only use them with build tools that produce a single minified file.*

It is advised that generic animations are defined as objects in an `animations` file, while more specific animations are defined in their parent component file.

CSS project structure is explored in more depth in my [Sass guidelines](../sass/sass-guidelines.md).

## Naming conventions

> "There are only two hard problems in Computer Science: cache invalidation and naming things" — Phil Karlton

Naming things is hard. It's the the biggest source of pain, complexity and struggle when maintaining large CSS based projects.

As such, it's critical to use a consistent, organised and sane methodology to increase development speed, improve debugging and build a maintainable code base.

The goal here is a code base which is both **transparent** and **self-documenting**.

Criteria — any naming system must:

1. Help me instantly know if it's safe to edit a class
2. Help me instantly know where a class fits in the overall system
3. Help me instantly know if a component uses JavaScript
4. Reduce HTML and CSS bloat

There are many methodologies available, but my current preference is for [BEM](http://getbem.com) — it has it's drawbacks but after building a couple of projects with it, the specificity graph convinced me.


### BEM (Block Element Modifier)

- **Block** — Standalone entity, meaningful on its own.
- **Element** — Part of a block (semantically), no standalone meaning.
- **Modifier** — Flag on a block or element, changes appearance, behaviour or state.

BEM is readable, modular, flexible, reusable and maintainable:

- It creates clear, strict relationships between CSS and HTML.
- It allows for less nesting and lower specificity (reducing [cascade problems](http://www.phase2technology.com/blog/used-and-abused-css-inheritance-and-our-misuse-of-the-cascade/)).
- It helps to create reusable, transferable components.
- It helps build scalable stylesheets and reduces code volume.

### Using BEM

#### Block

Encapsulates a standalone entity. Can be nested and interact with other blocks, but without precendence or heirarchy. Entities without DOM representation can also be blocks (containers, models).

- Class name selector only
- Class name cannot contain underscores
- No dependencies

_Note: Perform any additional CSS resets on the block level._

CSS class format is **`block-name`**:

```html
<section class="block">
</section>

<!-- Example -->

<section class="portfolio">
</section>
```

```css
.portfolio {
  ...
}
```

_Note: it's fine to use PascalCasing for blocks in [React](../react/react-guidelines.md) projects._

#### Element

Parts of a block without standalone meaning. Every element is semantically tied to its block.

- Within a block, all elements are semantically equal
- Depend on the element they belong to
- Class name selector only
 
CSS class format is **`block-name__element-name`**:

```html
<section class="block">
  <h1 class="block__element"></h1>
</section>

<!-- Example -->

<section class="portfolio">
  <h1 class="portfolio__title"></h1>
</section>
```

```css
.portfolio__title {
  ...
}

.portfolio .portfolio__title { } /* BAD */
section.portfolio__title { } /* BAD */
```

#### Modifier

Flags on blocks and elements to change appearance, behaviour or state.

- Only add to blocks and elements they modify
- Keep the original class
- Modifier class name selector only


CSS class format is as above plus **`--modifier-name`**:

```html
<section class="block">
  <h1 class="block__element block__element--modifier"></h1>
</section>

<!-- Example -->

<section class="portfolio portfolio--available">
  <header>
    <h1 class="portfolio__title portfolio__title--large"></h1>
  </header>
</section>
```
```css
.portfolio--available {
  ...
}

.portfolio__title--large {
  ...
}

/* alter elements based on block modifier */
.portfolio--available .portfolio__title {
  ...
}
```
_Note: Classes do not have to reflect the full paper-trail of the DOM, so it is correct to use `.portfolio__title` rather than `portfolio__header__title`._

#### Use multiple classes over singular

It can be tempting to write modifier classes that are fully self-contained, but it's important to favour the use of multiple classes so that all styling generic to a block, lives on that block.

```html
<div class="block__element--modifier"></div> <!-- BAD -->

<div class="block__element block__element--modifier"></div> <!-- Good -->
```

#### Mix

Mixes combine several BEM entities to create a semantically new "mix" entity.

For example: a single DOM node combining both a block and an element of another block.

```html
<div class="modal">
  <button class="btn modal__btn"></button>
</div>
```

_Note: there are many approaches available, the one described above is aimed at smaller, non-enterprise projects with few contributors. For larger projects, frameworks should be introduced and [CSS-in-JS](#css-in-js) could be considered._

## Name things for humans.

When writing CSS, **name your classes for people**: they're the only things that actually _read_ code (a parser just matches rules).

Some guidelines:

- Don't use classes like `.red` (unless you adobt a forward-thinking system such as [Tachyons](http://tachyons.io)).
- Avoid classes that describe the nature of content, _content describes itself_.
- Focus on sensibility and longevity, not pure semantics.

##### Examples

```css
.blue { /* bad */ 
  color: blue;
}
.header span {...} /* still bad */
.header-color {...} /* better */
.highlight-color {...} /* GOOD! */

.home-page-panel {} /* bad */
.masthead {} /* good */

.site-nav {} /* bad */
.primary-nav {} /* good */

.btn-login {} /* bad */
.btn-primary {} /* good */
```


## A word on specificity...

Combined selectors are more specific (read: important) than single class selectors. They almost "cost" more in  performance terms as browsers read rules **right-to-left**. Compare these two rules:

```css
body.home div.header > ul {}

.primary-nav {}
```

The second rule "costs" less, doesn't use qualifying or descendant selectors and, importantly, _reads_ better for humans.

Problems caused by over-specificity:

- Limits ability to change and extend a codebase.
- Interrupt and undo CSS cascade and inheritance.
- Prevent things from working as expected.
- **Cause high levels of frustration...**

Keep things simple instead:

- Select what you want explicitly.
- Keep selectors as short as possible.
- Do not use IDs (worth saying twice).
- Do not chain or use qualifying selectors.
- Do not nest selectors unnecessarily.
- Favour multiple class approach over singular.
- When mixing blocks and elements we need to ensure we only affect the block a modifier belongs to.
- Don't fight over-specificity, with more specificity — refactor instead!

This remove ambiguity —- block structure is clear, code is readable and ultimately easier to find and refactor in large projects.

### !important

Poor use of `!important` can ruin a codebase, so tread carefully. The golden rule is:

> `!important` MUST only be used **proactively**, never reactively.

Use it to create single property utlity classes with a specific purpose, but rarely anything else.

## Namespaces

Naming conventions tell us how components relate to each other, but don't tell us how they relate in a global sense.

Two key goals of any CSS system should be:

1. **Clarity** — How much information can we get from a single source? Can we make safe assumptions from a single context?
2. **Confidence** — Do we have enough knowledge about a system to work with it? Are we confident to make changes? Do we know the side-effects of such changes?

Almost all problems in large CSS codebases come down to clarity and confidence — to solve this we need to say what a class does, why it exists, where it can be used and how safe it is to modify.

Namespaces solve these problems by telling us what each class does in _non-relative_ terms.
 
### Primary namespaces

- `o-` — **Object**: An _abstract_ reusable piece of UI. May be used in any number of unrelated contexts. Changes could have knock-on effects in many places you cannot see. Treat with care. Avoid changes without full system knowledge.
- `c-` — **Component**: A self-contained _implementation specific_ piece of UI with a common semantic purpose. Any changes should be visible in the current context. Changes should be safe and have no knock-on effects.

_Examples: `.box` and `.media` would be Objects, while `.button` and `.modal` would be Components. — the golden rule is: "a component shouldn't have to live in a certain place to look a certain way."_

### Secondary namespaces

- `l-` — **Layout**: Specific layout based styling to form recognised design patterns such as articles, products, etc.
- `t-` — **Theme**: Apply theme to a view. Identifies that current appearance may be due to a theme.
- `s-` — **Scope**: Create a styling context for specific encapsulation reasons (such as user-generated content). _Use sparingly_.

### Helper namespaces

- `u-` — **Utility**: Special class with single use and often a single property. Should not be bound onto or changed. Reuable and not tied to any specific piece of user interface. Often provide "hard" overrides.
- `js-` — **JavaScript**: Declare this piece of DOM has JavaScript behaviour or hooks bound to it. Treat with care.
- `is-`, `has-` — **State**: Signify that current styling is due to a state or condition. Informs that the DOM currently has a temporary or optional style applied due to certain conditions being met or invoked.

### Optional namespaces

- `qa-` — **QA**: Special reserved classes for use by QA and Engineering teams during automated UI testing.
- `_` — **Hack**: Signify that class is a (necessary) hack. Aim for zero of these.

 
### Containers

```html
<!-- Semantically meaningful -->
<section class="block-container">
  <div class="l-layout">
    <section class="block">
    </section>
  </div>
</section>

<!-- Purely presentation -->
<div class="container">
</div>
```

### Highlighting namespaces

It's possible to apply debugging styles to highlight all classes of a certain namespace. For example:

```css
/* Highlight component namespace rules */

[class^="c-"],
[class*=" c-"] {
  outline: 5px solid cyan;
}
```

_Note: Namespaces do not provide encapsulation or sandboxing of styles, they exist purely for clarity and information reasons._

## The Open/Closed Principle

Note that BEM follows the _Open/Closed Principle_, defined as:

> "[s]oftware entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."

All modifications, additions, and changes should be opt-in, not mandatory. If something needs an adjustment to take it away from the norm: provide another class which adds this functionality. 

Changing root classes, objects and components could have a huge knock-on effect — remember that.

## The Single Responsibility Principle

> "…the single responsibility principle states that every context (class, function, variable, etc.) should have a single responsibility, and that responsibility should be entirely encapsulated by the context."

In CSS terms, this means composing UI items of smaller classes that focus on specific, limited functionality.

If you find yourself writing lots of properties into a rule, you should probably abstract it and create classes to seperate:

- Layout
- Structure
- Cosmetics

##### Example

```css
.error-message {
  display: block;
  padding: 10px;
  border-top: 1px solid #f00;
  border-bottom: 1px solid #f00;
  background-color: #fee;
  color: #f00;
  font-weight: bold;
}

/* becomes */

.box {} /* layout */
.message {} /* structure */
.message--error {} /* cosmetics */
```

## Composition over inheritence

Create CSS in component parts and combine them like Lego blocks — tiny, single responsibility pieces — to build a multitude of different UI results.

**Always think in terms of reusable blocks, elements, modifiers, objects and components.**

## Performance

Follow best practice for performance:

- Minify for production — ideally remove unusued CSS if possible ([UnCSS](https://uncss-online.com), [Purify](https://github.com/purifycss/purifycss)).
- Generate source maps for debugging minified code.
- Aim for CSS to be as non-blocking as possible.
- Aim to serve over HTTP/2 and split into suitable files.
- _If HTTP/2 not possible:_ concatenate and serve as single file.


### Critical CSS

Where possible a system should look to implement [critical CSS](https://www.smashingmagazine.com/2015/08/understanding-critical-css/), defined as:

> "The minimum set of blocking CSS required to render the first screen's worth of content to the user."

This is achieved through inlining CSS to a page's `<head>` in a `<style>` block. For certain pages this is a valid overall strategy — 404 pages and other static pages — but this usually requires a combination of inline CSS and CSS files created using a build tool such as [webpack](../webpack/webpack-guidelines.md) or Gulp.

## Boilerplates

Any CSS boilerplate should be built on top of [Normalize.css](https://necolas.github.io/normalize.css/) and other sane defaults.

Normalize.css is a modern, HTML5-ready alternative to overly opinionated CSS resets, that:

- only targets styles that need to be normalized;
- preserves useful browser defaults;
- corrects bugs and browser inconsistencies;
- improves usability with subtle improvements;
- doesn't clutter debugging tools;
- has good documentation.

Beyond this, other useful defaults, common helpers, media queries and print styles are used from the [HTML5 Boilerplate](../html/html-guidelines.md#boilerplates)

_Note: for larger projects a customer boilerplate should be considered that builds on a framework and uses a pre-processor such as [Sass](../sass/sass-guidelines.md) and considers other awesome tools such as PostCSS._

## Choosing the right tools

> **"So, should I use Sass, PostCSS or CSS-in-JS?"**

There's no right answer.

They all have their benefits, pros and cons. It's about using the right tool for the job — so, how do you go about choosing the right tool for the job? Some considerations:

- If it doesn't need to be done at run-time, don't do it at run-time — offload it to a build tool and reduce the weight on the server and/or client. Remember that more than 50% of web traffic happens on mobile devices.
- Built-time pre-processing should probably always take precedence... **unless run-time is neccesary** for something like client inout or external data sources.
- Use pre-processing (Sass/LESS) for anything that's algebra heavy — think radians, Bézier curves or other fear-inducing work. `libSass` can get the job done here better than JS (most likely).
- As things change in the future then PostCSS will be more relevant. Sass will need to fill less of the functions it has traditionally as browsers catch up.
- If you're looking for complete *Separation-of-Concerns* on an component level (welcome to the future) thes CSS-in-JS makes total sense...

### CSS-in-JS

> Most of all, I’m excited about the potential of components written in a single language to form the basis of a new, unified styling language—one that unites the front-end community in ways we’ve never seen before.

— Mark Dalgleish

For a complete overview of the topic I emplore you to take the time to read Mark's brilliant and seminal deep-dive  **["A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660)**.

It's also worth reading his 2015 piece ["Block, Element, Modifying Your JavaScript Components"](https://medium.com/seek-blog/block-element-modifying-your-javascript-components-d7f99fcab52b) as a primer.

## Acknowledgements

Credit and thanks to:

- [Harry Roberts](https://twitter.com/csswizardry) at [CSS Wizardry](https://csswizardry.com) for his amazing work on all things CSS-related.
- [Yandex](https://yandex.com/company/) for their work on [BEM](http://getbem.com).

---

## TODO

- [ ] Media queries
- [ ] ?
