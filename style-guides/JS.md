# JavaScript

Follow the regular [WordPress JavaScript style guide](https://make.wordpress.org/core/handbook/best-practices/coding-standards/javascript/), with the additions and modifications listed here.

Some of the more basic style rules such as tab indentation may also be enforced automatically within your editor by using an `.editorconfig` [file](http://editorconfig.org/).

## Modern JavaScript

Modern JavaScript syntax (commonly “ES6” or “ESNext”) should be used wherever it makes sense, and compiled to ES5 syntax for browser compatibility using [babel](https://babeljs.io/). This syntax is usually much more readable than the older ES5 alternatives. New features includes classes, arrow functions, and module import/exports, among other useful syntax additions.

Selected resources:

* [ES6 for Everyone](https://es6.io/) from Wes Bos (Paid video course)
* [Eloquent JavaScript](https://eloquentjavascript.net/) by Marijn Haverbeke (free online book)
* [Exploring JS](http://exploringjs.com/) by Axel Rauschmayer (free online book)
* [ES6 Interactive Guide](http://stack.formidable.com/es6-interactive-guide/) by Formidable Labs
* [Comprehensive Overview of ES6 Features](http://es6-features.org/)

### Classes

ES6 includes a class syntax which should be used instead of ES5-style constructor functions and prototype methods.

For example, in ES5 you would add methods to a function’s prototype:

```js
var MyClass = function () {
    this.something = 0;
};
MyClass.prototype.add = function () {
    this.something++;
};
```

In ES6, this is now written:

```js
class MyClass {
    constructor() {
        this.something = 0;
    }

    add() {
        this.something++;
    }
}
```

Methods should have one empty line between them (i.e. `}\n\n`).

### Arrow functions

Arrow functions are shorter forms of closures/anonymous functions that automatically bind to the existing context.

For example, in ES5:

```js
window.setTimeout( this.update.bind( this ) );
// or:
window.setTimeout( ( function () {
    this.update();
} ).bind( this ) );
```

In ES6, this is instead written:

```js
window.setTimeout( () => {
    this.update();
});
```

This may be shortened to a one-liner by dropping the braces:

```js
window.setTimeout( () => this.update() );
```

Note however that arrow functions implicitly return the result of any brace-free statement, so this example does behave slightly differently than the prior code block. If you want to avoid returning a value, do not drop the braces.

This same implicit return behavior is quite useful when writing short callback functions in a functional programming style. As an example, these array manipuation snippets are equivalent:

```js
// With function bodies
var sorted = items
    .map( ( item ) => {
        return item.id;
    } )
    .sort( ( id ) => {
        return id;
    } );

// One-line arrow function with implicit return
var sorted = items.map( item => item.id ).sort( id => id );
```

Because of the brace syntax used to write an arrow function, to return an object value from an arrow function you must either use the braces syntax to create a function block or else wrap the object in parentheses before returning.

```js
const returnObj = ( inputs ) => {
    return {
        val: inputs.a,
        otherVal: inputs.b,
    };
};

const returnObjImplicitly = ( inputs ) => ( {
    val: inputs.a,
    otherVal: inputs.b,
} );
```

### Avoid using .bind(this)

As the body of a short arrow function will always share context (the value of this) with its containing scope, use short arrow functions instead of calling `func.bind(this)`. This tends to be easier to understand.

```js
// Avoid:
function callback() {
    this.handle();
}
something( callback.bind( this ) );

// Prefer:
something( () => {
    this.handle();
} );
```

`.bind()` may be used if the function should be bound to something other than `this`.

For partial application (pre-binding a subset of function arguments), arrow functions are again preferred:

```js
// Avoid:
var newHandle = this.handle.bind( this, arg1, arg2 );

// Prefer:
var newHandle = ( ...args ) => this.handle( arg1, arg2, ...args );
```

### Modules

Browser JavaScript should be split up into modules using ES6 module syntax.

```js
import MyClass, { namedExport } from './MyClass';

export default class SubClass extends MyClass {
    myMethod() {
        /* ... */
    }
}
```

In addition to or instead of the default export in the example above, a module may also provide one or more named exports:

```js
export function fetchPosts( args = {} ) { /* ... */ }

export function fetchSinglePost( id, args = {} ) { /* ... */ }

export function fetchPages( args = {} ) { /* ... */ }
```

Files exporting a class (whether a React component or otherwise) should only export one single class, and should provide it using export default. When using higher-order components, it may also be useful to expose the original, unwrapped class as a named export for testing. Use named exports sparingly, however: if your class file exports utility functions, they are often better placed in a separate utility file.

You may also find it useful to consistently declare default exports at the bottom of their containing module file.

### Variable declaration

`const` & `let` should be used in place of `var` when declaring variables.

`const` should be used in most cases. Note that `const` means that a variable may not be re-assigned later, not that it is immutable. Complex types like Objects and Arrays may be declared with `const` then later mutated, and only re-assignment will cause an error:

```js
const primitiveValue = 'forty-two';
const obj = {};

// These would error:
primitiveValue = 42;
obj = function() {};

// This will not error:
obj.property = 42;
```

`let` should be used only if you will need to re-assign the variable’s value.

```js
// Avoid:

for ( var i = 0; i < 5; i++ ) {
    window.setTimeout( () => console.log( i ) );
}
//=> 5 5 5 5 5

let obj = [ 'only assigned once' ];

var str = 'default value';
if ( condition ) {
    str = 'other value maybe assigned later';
}

// Prefer:

for ( let i = 0; i < 5; i++ ) {
    window.setTimeout( () => console.log( i ) );
}
//=> 0 1 2 3 4

const obj = [ 'only assigned once' ];

let str = 'default value';
if ( condition ) {
    str = 'other value maybe assigned later';
}
```

Note that `const` and `let` are block scoped, meaning that their declarations are limited to the surrounding block (paired curly brackets), not just to their containing function. In particular, a `let` variable will not travel upwards from a conditional block such as an `if`:

```js
// Avoid:

if ( condition ) {
    let x = 1;
}
x; // undefined variable "x"

// Prefer:

if ( condition ) {
    const x = 1;
    doSomethingToX( x );
}

// Use "let" if x is needed outside the block:
let x;
if ( condition ) {
    x = 1;
}
```

Variable declarations should generally be just-in-time, typically when you initialise it to a value. If you cannot declare the variable when you initialise it, the declaration should be as close as practicable:

```js
// Avoid:

function () {
    let x;

    something();

    if ( condition ) {
        x = 1;
    }
}

// Prefer:

function () {
    something();

    let x;
    if ( condition ) {
        x = 1;
    }
}
```

Finally, note that `let` and `const` are not subject to [variable hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting). A variable declared with `let` or `const` may not be referenced until after it is declared.

### Destructuring Assignment

Destructuring assignment lets you concisely pull multiple values out of an object:

```js
// Avoid:

const TraditionalAssignment = props => {
    const content = props.content;
    const title = props.title;
    const id = props.id;
    // ...
};

// Prefer:

const DestructuredAssignment = props => {
    const { content, title, id } = props;
    // ...
};
```

Destructuring is also available for function parameters, although we recommend only using this when dealing with a small number of properties. These are equivalent:

```js
const TraditionalParameters = ( props ) => {
    const { title, content } = props;
    // ...
};
const DestructuredParameters = ( { title, content } ) => {
    // ...
}
```

Note: it is possible to nest destructuring assignment syntax. Do not do this; it is very hard to read.

### Spread operator

The `...` spread operator can simplify several operations in JavaScript, and should be used in place of more verbose syntax. It can take a little while to learn the different ways the spread operator is used, but once you are familiar with it the code is much more obvious.

```js
// Avoid:

const newObject = Object.assign( {}, oldObject, {
    overridden: 'property',
} );

// Prefer:

// Object rest spread operator
const newObject = {
    ...oldObject,
    overridden: 'property',
};

// Bonus: object rest spread within argument destructuring
const MyComponent = ( { title, ...props } ) => (
    <div>
        <h2>{ title.content.rendered }</h2>
        { renderEverythingButTheTitle( props ) }
    </div>
);
```

```js
// Avoid:

const nodes = Array.prototype.slice.call( document.querySelectorAll( 'p' ) );

// Prefer:

// Iterable spread operator
const nodes = [ ...document.querySelectorAll( 'p' ) ];
```

```js
// ES5:

const arr = [ some, values ];
const largerArr = [ newValue ].concat( arr );
// [ newValue, some, values ];

// ES6:

const arr = [ some, values ];
const largerArr = [ newValue, ...arr ];
// [ newValue, some, values ];
```

## Other Object Enhancements

In addition to the spread operator, object declarations now support computed property names:

```js
// Avoid:
const obj = {};
obj[ someDynamicKeyName ] = 'value';

// Prefer:
const objWithComputedPropertyName = {
    [ someDynamicKeyName ]: 'value',
};
```

A shorthand property syntax is also available to handle the common situation where an object key is assigned a variable with the same identifier:

```js
const title = getUserInput( 'title' );
const content = getUserInput( 'content' );

// ES5:

return {
    title: title,
    content: content,
};

// ES6:

return {
    title,
    content,
};
```

## Style

### Trailing commas

Multi-line array and object declarations should always have a trailing comma after each item. This cleans up the diff for future changes.

Single-line declarations do not need trailing commas.

### Semicolons

Semicolons should end every statement in JS.

```js
const obj0 = { key1: 'value1', key2: 'value2' };

const DestructuredAssignment = props => {
    const { content, title, id } = props;
    // ...
};
```

### Do not align object properties

```js
{
    object: 'values',
    need: 'not',
    be: 'aligned',
    in: 'js',
}
```

### Multi-property objects should place properties on separate lines

```js
// Avoid:
const obj0 = { key1: 'value1', key2: 'value2' };

// Prefer:

// Single-property objects may span one or multiple lines.
const obj1 = {
    key: 'value',
};
const obj2 = { key: 'value' };

// Multi-property objects should always span multiple lines.
const obj3 = {
    title: 'A Good Title',
    meta: {
        key: 'Rating',
        value: 10,
    },
};
```

### Avoid Yoda conditions, you must

Yoda conditions are confusing and solve the wrong problem. You have my permission to not use Yoda conditions.

### Prefer functional programming over imperative iteration

Rather than using loops, use functional programming concepts like .map, .filter, etc.

## Libraries

### Avoid Underscore

Most functionality in Underscore.js is available natively in the browser, and can be compiled by Webpack/Babel into ES5-compatible calls. Khan’s style guide includes a [guide to replacing calls](https://github.com/Khan/style-guides/blob/master/style/javascript.md#dont-use-underscore).

If you do need a specific method from Underscore, consider using [Lodash](https://lodash.com/) instead. Lodash allows importing individual functions, providing only the functionality you need:

```js
import uniqueId from 'lodash/uniqueId';
```