# Documentation

All code on both the frontend and backend should be thoroughly documented. This includes inline comments on code as well as prose documentation in project wikis and readmes.

## Code Comments

Code should be commented thoroughly throughout to ensure that code remains readable, approachable, and understandable to anyone reading it.

When writing comments, it’s important to ensure that you’re documenting the intent and motivation of the code, rather than merely repeating the code. Well-written code should be inherently understandable, **but** does not convey why the code exists and is written that way.

For example, the following comments merely repeat the code, and are not useful:

```php
// Avoid:

// Set the subtotal to 0
$subtotal = 0;

// Loop over each product
foreach ( $products as $product ) {
    // Add the price of the product to the subtotal
    $subtotal += $product->price;
}

// Multiple subtotal by 0.1 to get taxes
$taxes = $subtotal * 0.1;

// Add subtotal to taxes for total
$total = $subtotal + $taxes;
```

Instead, we should document the intent of the code where it makes sense, and let the code speak for itself in obvious places.

```php
// Prefer:

// Generate subtotal from prices
$subtotal = 0;
foreach ( $products as $product ) {
    $subtotal += $product->price;
}

// Calculate tax based on 10% rate (GST)
$taxes = $subtotal * 0.1;
$total = $subtotal + $taxes;
```

This could be improved further by making the code clearer by removing “magic numbers” and turning them into constants instead.

```php
/**
 * Effective tax rate (Australia)
 *
 * GST tax rate, applied to all non-exempt goods and services.
 */
const TAX_RATE = 0.1;

// [...]

$taxes = $subtotal * TAX_RATE;
```

Inline comments used correctly should add to the readability of the code by providing context on decisions.

### Documentation Blocks

Documentation blocks (or “doc blocks”) are specific types of inline comments with semantic meaning. These are typically attached to a “structural element” (e.g. function, class, etc) and document the element’s behaviour and intent.

Doc blocks have a machine readable structure, allowing most editors and IDEs to pull out their data and display it in the editor. This allows better code completion tooling, and allows automated documentation to be generated for the project.

Our [coding standards](../style-guides/README.md) require the use of documentation blocks where possible.

In doc blocks, you should state the behaviour of the overall element, allowing the element to be understood without viewing the source. Again, this does not mean repeating the code, but rather stating why the element should be used.

For example:

```php
// Avoid:
/**
 * Multiply tax rate by subtotal
 *
 * @param int $subtotal Subtotal
 * @return int Subtotal multiplied by tax rate
 */
function get_tax( $subtotal ) {
    return $subtotal * TAX_RATE;
}

// Prefer:
/**
 * Calculate tax rate for a given subtotal
 *
 * @param int $subtotal
 * @return int Effective tax amount
 */
function get_tax( $subtotal ) {
    return $subtotal * TAX_RATE;
}
```

### Excessive Commenting

When commenting code, you need to exercise judgement on when to comment. Poorly written inline comments can reduce the readability of the code by breaking up the mental flow of reading code, and make the code less maintainable.

For example, the following code contains 9 lines of code, but the comments break it up in to an unreadable 40 lines:

```php
/**
 * This issue only arrises when breadcrumbs are placed inside of The Loop,
 * so we'll first check to see if we're in The Loop, and if not, just bail
 * on this altogether.
 */
$in_the_loop = (bool) in_the_loop();
if ( ! $in_the_loop ) {
    return $breadcrumb_text;
}

/**
 * Grab the post subtitle.
 *
 * Example: (string) "Subtitle"
 */
$post_subtitle = get_the_subtitle();

/**
 * Grab the length of the post subtitle.
 *
 * We're also using a negative value of the legnth of the subtitle.
 *
 * Example: (int) 8
 */
$post_subtitle_length = (int) strlen( $post_subtitle );
$post_subtitle_length_neg = -1 * $post_subtitle_length;

/**
 * Grab the already filtered post title.
 *
 * Example: (string) "Post TitleSubtitle"
 */
$post_title = (string) $breadcrumb_text['text'];

/**
 * Grab the length of the filtered post title.
 *
 * Example: (int) 18
 */
$post_title_length = (int) strlen( $post_title );
```

(Example shortened from [the original](https://github.com/wecobble/Subtitles/blob/9e553731ceec1244b440406ce70e5d011ec16af3/public/class-subtitles.php#L269-L320) for brevity.)

Instead, this code should simply document the intent of the code where necessary, and let the code speak for itself:

```php
// This issue only arises when breadcrumbs are placed inside of The Loop
$in_the_loop = (bool) in_the_loop();
if ( ! $in_the_loop ) {
    return $breadcrumb_text;
}

$post_title = (string) $breadcrumb_text['text'];
$post_subtitle = get_the_subtitle();

// Calculate lengths to avoid repeated calls
$post_subtitle_length = (int) strlen( $post_subtitle );
$post_subtitle_length_neg = -1 * $post_subtitle_length;
$post_title_length = (int) strlen( $post_title );
```

> Note that this example is about the commenting style only. In practice, this code should be rewritten to avoid micro-optimisations and unnecessary code.

## Prose Documentation

Prose documentation is important for understanding the project at a higher level. This is where you can document decisions made about architecture, choice of external libraries and plugins, and process.

Generally speaking, long-lived documentation for a project should live within the project repository in the form of markdown files. This ensures that all documentation is version-controlled along with the project, and any changes needed to documentation can be done as part of a pull request on the project.

As a standard, projects should also contain READMEs at the project root level. These READMEs should lay out the basics of getting started with the project (i.e. instructions for installation and workflow), and no other information. Instead, additional meta information about the project not specifically intended for engineers can be located in other, root level markdown files (e.g. `project.md` or `meta.md`), or link to select pages on a project wiki.

## Further Reading

* [Write The Docs Documenation Guide](https://www.writethedocs.org/guide/)