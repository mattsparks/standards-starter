# PHP

Unless otherwise mentioned here, we follow the [WordPress style guidelines](https://make.wordpress.org/core/handbook/best-practices/coding-standards/php/).

We also have our own standards for things that aren’t covered by the WP style guidelines, such as namespacing and general project structure.

## File Layout

PHP files should **either** declare symbols (classes, functions, etc) **or** run code (function calls, etc), but not both. Generally speaking, only one file per plugin/theme should contain run code.

An exception is allowed for `require` statements only if there isn’t a better way to include those files. As an example, a namespace file may include the namespace file for subnamespaces, since PHP doesn’t have a way to autoload namespaced functions.

Classes should be in their own file, which should not declare any other functions or classes, or run any code.

Generally, the file should follow the following order:

* `namespace` declaration
* `use` declarations
* `const` declarations for the namespace
* `require` when allowed
* Declarations or run code

For namespaced functions which are primarily action and filter callbacks, the `add_action`/`add_filter` calls should generally live in a `bootstrap()` function in the namespace. This allows the file to be loaded without adding hooks immediately, but still allows the hook declarations to live with the callbacks.

## File Naming

Namespaces are mapped to filesystem directories after lowercasing. Part of the namespace may be excluded from the directory structure, ala PSR-4. For example, classes and functions in the `SS\Some\Example` could live in the `base/ss/some/example` directory, or the `SS` could be excluded for `base/some/example`.

Files containing a class should be named `class-<classname>.php`, all lowercase, to fit with the WordPress coding standards.

The main file for plugins should live in `plugin.php` in the plugin’s directory, while the main file for themes is `functions.php` in the theme directory. This file should be the only one to contain run code. This file should also register necessary autoloaders.

Underneath the top-level directory (either the plugin directory or the theme directory) should be an `inc` directory which represents the top-level namespace for the plugin/theme.

### Namespaced File Naming

Namespaced functions and constants should live in `namespace.php` in the namespace’s directory or in a file named `{namespace}.php` in the parent namespace’s directory.

Choosing the correct namespace naming strategy is important for project organization. Going the `namespace.php` route is standardized and simple, but commonly leads to a collection of un-associated functions, reducing discoverability and clarity. Alternatively, going with `{namespace}.php` will improve discoverability but can lead to many small files which can become difficult to maintain as time goes on. Both philosophies have their place and use.

Generally speaking, if you have classes or sub-namespaces within a namespace, you should use `namespace.php`. This ensures that the code within a namespace lives together in a single directory. Consider using `{namespace}.php` for standalone namespaces with only functions.

As an example, for a plugin called “ss-coffee” with a namespace of `SS\Coffee`, the directory structure should look like this:

* `plugin.php` – Declares autoloader for `SS\Coffee` and runs bootstrap functions
* `inc/` – Contains PHP code for `SS\Coffee` namespace
    * `namespace.php` – Declares functions and constants in the `SS\Coffee` namespace
    * `class-cup.php` – Declares the `SS\Coffee\Cup` class
    * `beans.php` – Declares functions and constants in the `SS\Coffee\Beans` namespace
    * sweeteners/ – Contains sub-namespace SS\Coffee\Sweeteners
        * `namespace.php` – Declares functions and constants in the `SS\Coffee\Sweeteners` namespace
        * `class-sugar.php` – Declares the `SS\Coffee\Sweeteners\Sugar` class

## Namespace and Class Naming

Namespaces should be logical groupings of related functionality. Typically, this means along feature lines, not along technology lines. That is, `SS\Project\Reports` with a `Command` class for CLI, rather than `SS\CLI` with a `ProjectReportsCommand` class.

Reusable code can be prefixed with `SS\`, or can use a project namespace (such as `Mercator\`) if it has a project name. Project names starting with ss-” should be sub-namespaces of `SS\`: if the project is called “ss-something”, the namespace should be `SS\Something.

Client projects or products should generally use their own namespace (e.g. `AwesomeClient\`) rather than under the `SS` namespace.

Namespaces may be named with CamelCase, as _ is historically used as a faux namespace separator. You may avoid underscores in favour of camelisation (heh) **as long as the project is consistent**.

Classes used in a namespaced file should have a `use` statement at the top of the file to allow a quick overview of what is used, and to avoid long absolute references to the classes throughout the file. If you have a lot of functionality from given namespace, you can use the entire namespace to avoid excessive `use` statements.

use statements should never start with a `\`; these are not required, as the namespaces here are already absolute.

```php
// No:
$x = \SS\Some\Thing::method();

// No:
use \SS\Some\Thing;
$x = Thing::method();

// Yes:
use SS\Some\Thing;
$x = Thing::method();

// Allowed:
use SS\Some;

$x = Some\Thing::method();
$y = Some\Other::thing();
```

### Namespace and Function Imports

The PHP language allows for functionality to be imported and aliased from external namespaces via the `use` operator in order to avoid writing longer, fully qualified names. It is important to be clear when referencing external functionality so that it is obvious where various functionality is defined. To better distinguish the use of external functionality, the following guidelines should be observed:

* Importing functions explicitly with `use function` should be avoided.
* Aliasing functions explicitly with `use function ... as` should also be avoided.
* Parent namespaces are preferred for importing functions.
* Aliasing namespaces is acceptable, as long as the renaming is kept close to the original. For example `use SS\Block\Helpers as BlockHelpers`

The above are not hard rules, but serve as a general guideline so dependencies
can be easily tracked from within the code.

```php
// No:
use function SS\Some\thing;
thing();

// No:
use function SS\Some\thing as other_thing;
other_thing();

// Encouraged:
use SS\Some;
Some\thing();

// Allowed:
use SS\Some\Longer\Namespace as LongerNamespace;
LongerNamespace\thing();
```

## Yoda Conditions

Yoda conditions are confusing and solve the wrong problem. You have my permission to not use Yoda conditions.

## Anonymous Functions (Closures)

`function` should be followed with a space, as should `use`.

```php
// No:
function() {}
function( $x ) {}
function( $x ) use( $y ) {}
function( $x ) use ( $y ) {}

// Yes:
function () {}
function ( $x ) {}
function ( $x ) use ( $y ) {}
```

When passing an anonymous function as an argument, if it is the last parameter, you can compress the closing braces and parenthesis without a space:

```php
array_map( $list, function ( $item ) {
    return $item->title;
}); // Space not required between brace and parenthesis.
```

However, you cannot do this if it is not the last parameter.

```php
add_action( 'init', function () {
    call_func();
}, 10 ); // Space required after 10
```

(Note that this rule applies also to standard function calls; you can have `));` for nested function calls or `]);` for short arrays in a function call.)

Generally speaking, you should avoid using anonymous functions for actions and filters. These aren’t possible to unhook neatly, and with namespaces you don’t need to worry about the global namespace pollution of one-function-per-hook.

## Array Creation

When creating a new array, prefer the `[]` short-array syntax over the `array()` old-style syntax. This is more compact, and fits better with how JavaScript works. In addition, it can make it a little easier to read nested options arrays.

```php
// No:
get_posts( array(
    'author_in' => array( 1, 2, 3 ),
    'meta_query' => array(
        'relation' => 'AND',
        array(
            'key' => '_my_meta',
            'value' => '1'
        ),
    ),
) );

// Yes:
get_posts( [
    'author_in' => [ 1, 2, 3 ],
    'meta_query' => [
        'relation' => 'AND',
        [
            'key' => '_my_meta',
            'value' => '1'
        ],
    ],
] );
```

## Visibility (Public/Protected/Private)

Class methods and properties should always be marked with a visibility keyword, one of `public`, `protected` or `private`. Generally speaking, `private` should be avoided in favour of `protected`, as it doesn’t allow subclasses to access the method. In most cases, this is an unnecessary hindrance, and leads to subclasses simply copy-and-pasting the code when they need to override it.

```php
// No:
class MyClass {
    var $foo = 1;
    function __construct() {}
}

// Yes:
class MyClass {
    protected $foo = 1;
    public function __construct() {}
}
```

## Inline Documentation

Generally, follow the [WordPress rules on inline documentation](https://make.wordpress.org/core/handbook/best-practices/inline-documentation-standards/php/).

An exception is made for the description. Descriptions should be written in the imperative (as a command) rather than third-person indicative. This is the **opposite** of what WordPress coding standards recommend.

When conjugating, pretend you’re commanding someone to do something. (The imperative generally follows the same rules as second-person indicative.)

* No: “Displays the date.”
* Yes: “Display the date.”

This is an intentional stylistic choice and deviation from the WordPress coding standards. The WordPress standards produce an incomplete sentence, missing a subject, which can be confusing.

The short description should be able to replace the function name. Doing this for all your functions should produce a sentence that describes what you’re doing.

* No: “Queries the database. Formats the data for output.”
* Yes: “Query the database. Format the data.”

## PHP Type Declarations

Adding type declarations where possible is a good way to catch bugs early, and have less manual type checking code. Both internally hosted and VIP projects have at least PHP 7.0, so type declarations for arguments and return types (including scalar values) are supported. Always use the short name for scalar types such as `bool` and `int`.

### Argument Type Declarations

For functions that are called by WordPress internals, only use type declarations when you are absolutely sure an argument is of that type (or can safely be cast to that type) in all expected executions. If your function is not being called by internal WordPress functionality, it’s recommended to use type declarations wherever possible.

### Return Type Declarations

Use return type declarations when your function can only return one type. This excludes any functions that can return an instance of `WP_Error` or multiple scalar and/or compound types. You may use nullable return types when returning `null` along with one other type.

```php
// Yes:
add_filter( 'excerpt_length', 'modify_excerpt_length' );
function modify_excerpt_length( int $length ) : int {
    return $length < 20 ? $length : 20;
}

// No:
add_filter( 'rest_pre_dispatch', 'check_permission' );
function check_permission( WP_REST_Request $request ) : WP_REST_Request {
    if ( ! current_user_can( 'manage_users' ) ) {
        return new WP_Error( 'rest-forbidden', 'You can not manage users.' );
    }
    return $request;
}
```

## PHP for Templates

When writing PHP in theme templates, you should follow a few different rules.

### Do not use a namespace

Templated PHP code generally shouldn’t live in a namespace. You can still have `use` declarations to simplify class and function calls.

Using a namespace for template files muddies the water with template tags. A `get_content()` call could refer to the core function, or could refer to a function in the theme’s namespace. This can significantly complicate code review by making escaping unclear, as well as potentially being unclear to other developers. It’s also easily possible to accidentally break existing templates if you add new functions.

Your `functions.php` should be namespaced however, and you can `use` this namespace into the template.

## Inline statements

When using an inline statement, start and end the PHP tags on the same line as the statement, and drop the semicolon.

```php
// No:
<?php echo esc_html( $x ); ?>

// Yes:
<?php echo esc_html( $x ) ?>

// No:
<?php endforeach; ?>

// Yes:
<?php endforeach ?>
```

This only applies to single statements. For multiple statements, these should be expanded into a full multiline statement with semicolons.

```php
// No:
<?php $x++; echo esc_html( $x ) ?>

// Yes:
<?php
$x++;
echo esc_html( $x );
?>

// No:
<?php echo esc_html( sprintf(
    'Hi %s',
    $user->display_name
)) ?>

// Yes:
<?php
echo esc_html( sprintf(
    'Hi %s',
    $user->display_name
));
?>
```