# CSS

## Basic Styles

There are several overarching style rules which should be kept to for the sake of consistency across projects.

Let’s look at some basic examples:

```css
// Avoid
.foo, .baz, .bar{
    color: #000000; background-color: linear-gradient(to bottom, #ffffff, #cccccc);
    font-size: 16px; font-family: Helvetica Neue, sans-serif }
.foo-bar{/* … */}

// Prefer
.foo,
.baz,
.bar {
    color: #000;
    background-color: linear-gradient( to bottom, #fff, #ccc );
    font-size: 1rem;
    font-family: "Helvetica Neue", sans-serif;
}

.foo-bar { /* … */ }
```

**Note several features about the above preferred code style**

* Each selector is on its own line
* There is a space before the opening `{`
* The trailing `}` is on its own line
* Each declaration is on its own line
* Each declaration has an ending semicolon, even the last in the set
* There is one blank line between selector sets
* Double quotes around values
* Spacing inside parenthesis

Unless stated, we follow the [WordPress CSS coding standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/css/).

## Shorthand Notation

If you don’t need to set all the values, avoid shorthand notation. This avoids writing over-broad styles that accidentally overwrite inherited styles.

```css
// Avoid:
.some-component {
    background: blue;
    margin: 0 0 0 10px;
}

// Prefer:
.some-component {
    background-color: blue;
    margin-left: 10px;
}
```

## Font Weights

Font weights should be expressed numerically, avoiding the the keywords `normal` and `bold`.

The numerical values `100` to `900` roughly correspond to the following common weight names:

| Value        | Common Weight Name        |
| ------------ | ------------------------- |
| 100          | Thin (Hairline)           |
| 200          | Extra Light (Ultra Light) |
| 300          | Light                     |
| 400 (normal) | Normal                    |
| 500          | Medium                    |
| 600          | Semi Bold                 |
| 700 (bold)   | Bold                      |
| 800          | Extra Bold                |
| 900          | Black                     |

## Font Sizes

Font sizes should not be written in px notation, but instead use the preferred rem or em values. Using relative units allows inheritance to work more gracefully and prevents having to override static values repeatedly.

```css
// Avoid:
h1 {
    font-size: 32px;
}

// Prefer:
h1 {
    font-size: 2.5rem;
}
```

## Timing Values

Timing values in CSS should use milliseconds instead of seconds. While `400ms` and `.4s` represent the same amount of time, the millisecond value is more explicit and easily readable.

```css
// Avoid:
button {
    transition-duration: .15s;
}

// Prefer:
button {
    transition-duration: 150ms;
}
```

## Quotes Within The URL Function

The `url()` function within CSS accepts parameters with both quotes and without quotes in the v3 spec. However, quoteless parameters require more escaping and special care; using quotes avoids having to remember special escaping rules and makes the values work more reliably.

```css
// Avoid:
div {
    background-image: url( ../my-background.jpg );
}

// Prefer:
div {
    background-image: url( "../my-background.jpg" );
}
```

## Naming Things

We use the BEM naming convention as a general guide for class naming. BEM stands for Block-Element-Modifier and follows the following style of delineation to denote which level of item that we are modifying.

An element is delineated by underscores `__`, and a modifier by dashes `--`. All elements of the BEM class should be lowercased.

`.block__element--modifier { }`

This would carry out in the following example.

```css
.car { /* … */ }
.car__door { /* … */ }
.car--blue { /* … */ }
```

Our **block** is a car – this represents the whole of what we’re styling.
Then, we have an **element** of `__door` – this represents a single piece of the larger block that we want to style.

Finally, we have a **modifier** of `--blue` to describe a variation of the car. Perhaps our car was originally red, but we need to style this one as blue.

You can read more on BEM [here](http://getbem.com/naming/).

## Components

CSS at [NAME] should be written in a highly-componentized manner. Each component should have its own file and specific set of styles that relate to it and it alone. These styles can be extended, but generally should be very specific for each use case. This allows us to use a singular set of markup and styles all across a site with predictable results.

Sub-components are indicated with an underscore, variants with a hyphen. For example:

* `.some-component`
* `.some-component__sub-component`
* `.some-component--variation`

In the `styles/components` directory, components should be placed in a file named for the component, `some-component.scss`.

## Utilities

Utilities are prefixed with `util-`, for example `util-clearfix`. All utility classes should be placed in the styles/utilities.scss file and should be sorted alphabetically.

## State

If a component has state (often determined by JavaScript) then you should set a class on the component to indicate the current state.

You should use the BEM modifier syntax, on a per-component basis, with grammatically correct modifier names to indicate the state.

For example `.some-component--is-open`, `.some-component--has-option-selected`.

You should avoid using global classes such as `.is-visible` (refer to the [BEM FAQ for more info](http://getbem.com/faq/#can-i-create-global-modifier)).

You should always use the modifier class on the top level block/component, never on the child element.

```html
// Correct
<div class="some-component some-component--is-active">
    <div class="some-component__heading"></div>
    <div class="some-component__content"></div>
</div>

// Incorrect. Avoid global modifiers.
<div class="some-component is-active">
    <div class="some-component__heading"></div>
    <div class="some-component__content"></div>
</div>

// Incorrect. Set the state on the on the top level component/block and not on a child element.
<div class="some-component">
    <div class="some-component__heading"></div>
    <div class="some-component__content some-component__content--is-visible"></div>
</div>
```

## Vendor files

Class names in vendor files should not be changed, the files should be placed in the `styles/vendor` directory and included as any other file would be included. These files do not need to meet our linting standards.

## JavaScript hooks

JavaScript hooks should be kept separate from those used for styles. This allows future developers to move the JavaScript behaviour without the need to modify the CSS.

Our preference is for JavaScript hooks to make use of data attributes, eg `[data-lightbox]`. See the [JavaScript style guide](JS.md) for further details.

If a JavaScript component must use a class, it should remain separate from styles and be prefixed with `js-`.

## Specificity

Specificity across styles is important to the long-term maintainability. Over-specifying at any point on the stylesheet makes it more difficult to make changes or adjustments in the future.

For this reason, the preferred identifier for rules is to use a highly-distinct class name. Classes are distinct without using a high [specificity index](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) rating and are re-usable where needed. Using the convention mentioned in the Naming section will help avoid the need to use IDs, `!important`, or over-nest your rules.

General tag identifiers `p {}`, `a {}`, `li {}`, `h1 {}`, etc. should be avoided unless the style is a global one. For example, using a general tag when styling all `<h1>` tags on the site is OK, styling a specific page’s H1 using a tag is not.

```css
// Avoid:
#cards { /* … */ }
.card { /* … */ }
.card ul { /* … */ }
.card ul li p a { /* … */ }
.single-post h1 { /* … */ }


// Prefer:
.card-list { /* … */ }
.card-single { /* … */ }
.card-single__link { /* … */ }
.single-post__page-title { /* … */ }
```

The usage of `!important` should always be avoided. There are very rare situations where one would need to use `!important`, such as overriding Gutenberg editor styles which are highly over-specified. However, it should only be used when overriding uncontrolled third-party code, and `!important` styles should always be accompanied by an explanatory comment.

## Documentation

CSS should be documented as robustly as PHP, HTML, and JS. Verbosity and over-documenting is better than leaving an item unexplained or vague and should complement good class and file naming conventions.

Documentation should include the intent of the code, including for complex selectors (nth-child, combinators); describing animation;

If you make changes to an existing stylesheet, review existing comments to ensure documentation is kept up-to-date.

### File Docblocks

Each CSS/SCSS file should have a basic docblock at the top of the file identifying a general purpose.

```css
/**
 * Button styles.
 */
```

Or a more verbose example when a file’s purpose is less immediately clear:

```css
/**
 * WordPress-specific styles
 *
 * Styles for over-riding or adapting WordPress-specific classes and hacks.
 * The HTML used with these styles is out of our control, and thusly may not
 * adhere to our normal standards.
 */
```

### Section Documentation

Distinct sections of CSS should be individually identified for easier parsing.

For example:

```css
/**
 * Media query mixins.
 *
 * All styles should be written mobile-first, only a limited few should be max-width.
 */

/**
 * Colors
 */

/**
 * Breakpoints
 */
```

### Functions and Mixins

Functions and Mixins in SCSS should be documented similarly to PHP functions.

**Sass Function:**

```scss
/**
 * Test if `$value` is a valid direction.
 *
 * @param {*} $value Value to test
 * @return {Bool} Whether `$value` is valid or not.
 */
@function is-direction( $value ) {
    /* … */
}
```

**Sass Mixin:**

```scss
/**
 * Build a linear gradient with a direction and any number of color stops.
 *
 * @param {String} The direction that the gradient should be oriented `120deg` or `to bottom`
 * @param {String} Color stop for each step of the gradient `#fff 0%`
 */
@mixin linear-gradient( $direction, $color-stops... ) {
    /* … */
}
```

### Inline Documentation

Where necessary, individual lines of CSS should be documented as necessary. This documentation is most generally leveraged when a rule breaks the standards, uses a particularly large amount of specificity, or would not make sense to your future self.

In Sass files, `//` may be used for single-line comments in lieu of `/* */`.

For example:

```css
/* The high-specificity is necessary to override the Bootstrap style .xxxxxx */
.my-class__item a:nth-of-type(3n):after { /* … */ }

/* Prevent a sliver of the photo from showing up behind the banner */
transform: translate( 1px, 1px );

/* All WP images should get some margin and start out centered. */
[class*="wp-image"] {
    display: block;
    margin: 10px auto;
}

/*
 * Multiline comments are formatted with asterisk
 * at the start of each line.
 */
```

### Responsive Styles

CSS should be kept as minimal and simple as allowed by the required design. As we generally follow mobile-first styling, basic style rules should directly apply to the mobile-view (that is, avoid using max-width queries to target small devices; have those rules be the default instead) and styles for progressively larger breakpoints can be layered on with successive `min-width:` media queries.

However, there are times where a breakpoint may establish many styles that would need to be overwritten in a larger breakpoint. In cases where a set of complex styles apply to only one intermediate set of screens, aim to reduce complexity in the codebase with a more tightly-scoped query specifying both a min-width and max-width.

This “simplicity first” methodology ensures easier-to-maintain code, maximum performance on mobile devices, and a more fluid experience for all users.

```css
// Base styles applicable to all media - color, etc.
.my-element {
    color: black;
    font-size: 1.25rem;
}

// More complex layout adjustments.
@media screen and (min-width: 768px) {
    .my-element{
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
    }
}
```

## Writing Sass

Sass can be an incredibly powerful tool on large projects. Things like variables, partials, and mixins help us write more maintainable CSS. However it is important to remember 2 things.

* Be aware of the CSS that is created by the SASS you write.
* Just because SASS has lots of cool functionality, be wary of overusing it! Otherwise you may end up with a codebase that is difficult to maintain and hard for other developers to get their heads around!

### Using Libraries

At the scale of projects [NAME] typically takes on, rolling your own is more time-effective and less frustrating than using libraries for styling. However, there are certain situation where a library can be useful and used wholesale.

For example, using [sanitize.css](https://github.com/jonathantneal/sanitize.css) can give a project a solid baseline and consistency. It will be used across an entire site and the whole library is used wholesale. It provides consistency and can simply be imported and forgotten.

A large frameworks or library such as Bootstrap or Font Awesome is unlikely to be the right solution for the projects that we build and should not be used. You will likely only use a tiny portion of the entire library, leaving the rest as unused orphan code that provides no benefit and adds to the complexity of the codebase.

### Variable Usage

Variables in Sass and CSS should be used sparingly and with specific purpose. Each variable should represent a single distinct value without side effects. Variables are incredibly useful, but can also add a lot of deleterious side-effects if not carefully managed.

```scss
// Prefer:
$breakpoint-tablet: 768px;
$ss-red:            #D24632;
$font-family-body:  "proxima-nova", sans-serif !default;
$images-path:       "../images" !default;
```

Each of the above represents a variable that is likely to stay the same and can be used without side-effects. You could change any of these values and expect a consistent result and have full knowledge that the effect would be consistent.

```scss
// Avoid:
$blue-btn-color: $blue;
$font-medium:    18px;
```

Let’s look at these examples:

* `$blue-btn-color` is simply referencing another variable and is hyper-specific to a specific button. This variable is unlikely to be used more than once or twice. This is not a hard no-no, but generally the better path here would be to simply reference `$blue`.
* `$font-medium` is usable globally, but if the variable is ever changed down the road then there would be numerous side effects that would be quite difficult to test for.

#### CSS Variables

Unlike Sass variables, CSS custom properties (variables) are subject to the cascade and can be overwritten for elements matching certain selectors. A CSS custom property defined once and used globally may be better as a Sass variable following the rules above.

```css
// Avoid:
:root {
    --ss-red: #D24632; /* Better as a Sass variable */
}

// Prefer:
.car {
    --transparent: rgba( 0, 0, 0, 0 );
    --solid: rgba( 0, 0, 0, 1 );
    background-image: linear-gradient( var( --solid ), var( --transparent ) );
}

.car--blue {
    --transparent: rgba( 0, 0, 255, 0 );
    --solid: rgba( 0, 0, 255, 1 );
}
```

### Nesting

Avoid nesting, and if nesting is required than absolutely nothing over 2 levels. Nesting more deeply generally indicates that the naming convention used wasn’t robust enough or is not being used appropriately.

The use of BEM for class names can help avoid nesting:

```scss
// Avoid:
.car__door {
    .car__door--gullwing { /* … */}
}

// Prefer:
.car__door { /* … */ }
.car__door--gullwing { /* … */ }
```

The more specific modifier rule will take precedence over the element, but both rules only maintain one class’ worth of specificity.

Nesting modifiers within element classes duplicates the existing meaning of the BEM class and adds unnecessary selector specificity.

### Autoprefixing

To save ourselves from writing extra CSS with browser prefixes, all CSS should be run through an autoprefixer. Autoprefixing our styles allows to simply write the basics future-proof code and know that previous versions of browsers will work as expected.

Autoprefixer should be specifically tuned to the browser requirements of the project using [browserslist](https://github.com/ai/browserslist). This can be put in one of many places, a .browserslist file, package.json, and more. We generally recommend placing this in `package.json`, but use what is best for your project’s build pipeline.

Autoprefixing via PostCSS is available through Webpack, Grunt, and most other front-end build systems and task runners.

### Preparing for Production

When compiling Sass, you’re generally preparing for one of 2 contexts: development or production. How and what you compile to development will generally depend on the project and technology used. So long as the styles themselves are the same as on production, the style of output is irrelevant.

However, when compiling for production, we need to be much more stringent about our output. The styles need to be as small as possible traveling over the wire.

#### When compiling to production:

* All comments should be removed
* CSS should be minified as much as possible, and no source maps should be pushed

## Related Reading

* [CSS Guidelines](https://cssguidelin.es/) by Harry Roberts
* [Sensible CSS Guidelines for large scale projects](https://github.com/chris-pearce/css-guidelines) by Chris Pearce
* [CSS Specificity](http://cssspecificity.com/)
* [10up’s CSS Best Practices](https://10up.github.io/Engineering-Best-Practices/css/)
* [WordPress Core CSS Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/css/)
* [Desktop vs Mobile vs Tablet Market Share Worldwide](http://gs.statcounter.com/platform-market-share/desktop-mobile-tablet)