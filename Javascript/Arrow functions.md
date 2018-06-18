# Arrow functions

----------

## Overview

There are two benefits to arrow functions.

First, they are less verbose than traditional function expressions:

```js
const arr = [1, 2, 3];
const squares = arr.map(x => x * x);

// Traditional function expression:
const squares = arr.map(function (x) { return x * x });
```

Second, their `this` is picked up from surroundings (_lexical_). Therefore, you don’t need `bind()` or `that = this`, anymore.

```js
function UiComponent() {
    const button = document.getElementById('myButton');
    button.addEventListener('click', () => {
        console.log('CLICK');
        this.handleClick(); // lexical `this`
    });
}
```

The following variables are all lexical inside arrow functions:

- `arguments`
- `super`
- `this`
- `new.target`

## Traditional functions are bad non-method functions, due to `this`

In JavaScript, traditional functions can be used as:

1. Non-method functions
2. Methods
3. Constructors

These roles clash: Due to roles 2 and 3, functions always have their own `this`. But that prevents you from accessing the `this` of, e.g., a surrounding method from inside a callback (role 1).

You can see that in the following ES5 code:

```js
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) { // (A)
    'use strict';
    return arr.map(function (x) { // (B)
        // Doesn’t work:
        return this.prefix + x; // (C)
    });
};
```

In line C, we’d like to access `this.prefix`, but can’t, because the `this` of the function from line B shadows the `this` of the method from line A. In strict mode, `this` is `undefined` in non-method functions, which is why we get an error if we use `Prefixer`:

```js
var pre = new Prefixer('Hi ');
pre.prefixArray(['Joe', 'Alex'])
// TypeError: Cannot read property 'prefix' of undefined
```

There are three ways to work around this problem in ECMAScript 5.

### Solution 1: `that = this`

You can assign `this` to a variable that isn’t shadowed. That’s what’s done in line A, below:

```js
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    var that = this; // (A)
    return arr.map(function (x) {
        return that.prefix + x;
    });
};
```

Now `Prefixer` works as expected:

```js
var pre = new Prefixer('Hi ');
pre.prefixArray(['Joe', 'Alex'])
// [ 'Hi Joe', 'Hi Alex' ]
```

### Solution 2: specifying a value for `this`

A few Array methods have an extra parameter for specifying the value tha `this` should have when invoking the callback. That’s the last parameter in line A, below.

```js
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    return arr.map(function (x) {
        return this.prefix + x;
    }, this); // (A)
};
```

### Solution 3: `bind(this)`

You can use the method `bind()` to convert a function whose `this` is determined by how it is called (via `call()`, a function call, a method call, etc.) to a function whose `this` is always the same fixed value. That’s what we are doing in line A, below.

```js
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    return arr.map(function (x) {
        return this.prefix + x;
    }.bind(this)); // (A)
};
```

### ECMAScript 6 solution: arrow functions

Arrow functions work much like solution 3. However, it’s best to think of them as a new kind of functions that don’t lexically shadow `this`. That is, they are different from normal functions (you could even say that they do less). They are not normal functions plus binding.

With an arrow function, the code looks as follows.

```js
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    return arr.map((x) => {
        return this.prefix + x;
    });
};
```

To fully ES6-ify the code, you’d use a class and a more compact variant of arrow functions:

```js
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix;
    }
    prefixArray(arr) {
        return arr.map(x => this.prefix + x); // (A)
    }
}
```

In line A we save a few characters by tweaking two parts of the arrow function:

- If there is only one parameter and that parameter is an identifier then the parentheses can be omitted.
- An expression following the arrow leads to that expression being returned.

## Arrow function syntax

The “fat” arrow `=>` (as opposed to the thin arrow `->`) was chosen to be compatible with CoffeeScript, whose fat arrow functions are very similar.

Specifying parameters:

```js
    () => { ... } // no parameter
     x => { ... } // one parameter, an identifier
(x, y) => { ... } // several parameters
```

Specifying a body:

```js
x => { return x * x }  // block
x => x * x  // expression, equivalent to previous line
```

The statement block behaves like a normal function body. For example, you nee  `return` to give back a value. With an expression body, the expression is always implicitly returned.

Note how much an arrow function with an expression body can reduce verbosity. Compare:

```js
const squares = [1, 2, 3].map(function (x) { return x * x });
const squares = [1, 2, 3].map(x => x * x);
```

### Omitting parentheses around single parameters

Omitting the parentheses around the parameters is only possible if they consist of a single identifier:

```js
[1,2,3].map(x => 2 * x)
// [ 2, 4, 6 ]
```

As soon as there is anything else, you have to type the parentheses, even if there is only a single parameter. For example, you need parens if you destructure a single parameter:

```js
[[1,2], [3,4]].map(([a,b]) => a + b)
// [ 3, 7 ]
```

And you need parens if a single parameter has a default value (`undefined` triggers the default value!):

```js
[1, undefined, 3].map((x='yes') => x)
// [ 1, 'yes', 3 ]
```

## Lexical variables

### Propagating variable values: static versus dynamic

The following are two ways in which the values of variables can be propagated.

First, statically (lexically): Where a variable is accessible is determined by the structure of the program. Variables declared in a scope are accessible in all scopes nested inside it (unless shadowed). For example:

```js
const x = 123;

function foo(y) {
    return x; // value received statically
}
```

Second, dynamically: Variable values can be propagated via function calls. For example:

```js
function bar(arg) {
    return arg; // value received dynamically
}
```

### Variables that are lexical in arrow functions

The source of `this` is an important distinguishing aspect of arrow functions:

- Traditional functions have a _dynamic `this`_; its value is determined by how they are called.
- Arrow functions have a _lexical `this`_; its value is determined by the surrounding scope.

The [complete list](http://www.ecma-international.org/ecma-262/6.0/#sec-arrow-function-definitions-runtime-semantics-evaluation) of variables whose values are determined lexically is:

- `arguments`
- `super`
- `this`
- `new.target`

## Syntax pitfalls

There are a few syntax-related details that can sometimes trip you up.

### Arrow functions bind very loosely

If you view `=>` as an operator, you could say that it has a low precedence, that it binds loosely. That means that if it is in conflict with other operators, they usually win.

The reason for that is to allow an expression body to “stick together”:

```js
const f = x => (x % 2) === 0 ? x : 0;
```

In other words, we want `=>` to lose the fight against `===` and `?`. We want it to be interpreted as follows

```js
const f = x => ((x % 2) === 0 ? x : 0);
```

If `=>` won against both, it would look like this:

```js
const f = (x => (x % 2)) === 0 ? x : 0;
```

If `=>` lost against `===`, but won against `?`, it would look like this:

```js
const f = (x => ((x % 2) === 0)) ? x : 0;
```

As a consequence, you often have to wrap arrow functions in parentheses if they compete with other operators. For example:

```js
console.log(typeof () => {}); // SyntaxError
console.log(typeof (() => {})); // OK
```

On the flip side, you can use `typeof` as an expression body without putting it in parens:

```js
const f = x => typeof x;
```

### No line break after arrow function parameters

ES6 forbids a line break between the parameter definitions and the arrow of an arrow function:

```js
const func1 = (x, y) // SyntaxError
=> {
    return x + y;
};

const func2 = (x, y) => // OK
{
    return x + y;
};

const func3 = (x, y) => { // OK
    return x + y;
};


const func4 = (x, y) // SyntaxError
=> x + y;

const func5 = (x, y) => // OK
x + y;
```

Line breaks _inside_ parameter definitions are OK:

```js
const func6 = ( // OK
    x,
    y
) => {
    return x + y;
};
```

The rationale for this restriction is that it keeps the options open w.r.t. “headless” arrow functions in the future (you’d be able to omit the parentheses when defining an arrow function with zero parameters).

### You can’t use statements as expression bodies

#### Expressions versus statements

Quick review:

Expressions produce (are evaluated to) values. Examples:

```js
3 + 4
foo(7)
'abc'.length
```

Statements do things. Examples:

```js
while (true) { ··· }
return 123;
```

Most expressions can be used as statements, simply by mentioning them in statement positions:

```js
function bar() {
    3 + 4;
    foo(7);
    'abc'.length;
}
```

#### The bodies of arrow functions

If an expression is the body of an arrow function, you don’t need braces:

```js
asyncFunc.then(x => console.log(x));
```

However, statements have to be put in braces:

```js
asyncFunc.catch(x => { throw x });
```

### Returning object literals

Some parts of JavaScript’s syntax are ambiguous. Take, for example, the following code.

```js
{
    bar: 123
}
```

It could be:

- An object literal with a single property, `bar`.
- A block with the label `bar` and the expression statement `123`.

Given that the body of an arrow function can be either an expression or a statement, you have to put an object literal in parentheses if you want it to be an expression body:

```js
const f1 = x => ({ bar: 123 });
f1()
// { bar: 123 }
```

For comparison, this is an arrow function whose body is a block:

```js
const f2 = x => { bar: 123 };
f2()
// undefined
```

## Immediately-invoked arrow functions

Remember [Immediately Invoked Function Expressions (IIFEs)]? They look as follows and are used to simulate block-scoping and value-returning blocks in ECMAScript 5:

```js
(function () { // open IIFE
    // inside IIFE
})(); // close IIFE
```

You can save a few characters if you use an Immediately Invoked Arrow Function (IIAF):

```js
(() => {
    return 123
})();
```

### Semicolons

Similarly to IIFEs, you should terminate IIAFs with semicolons (or use [an equivalent measure], to avoid two consecutive IIAFs being interpreted as a function call (the first one as the function, the second one as the parameter).

### Parenthesizing arrow function with block bodies

Even if the IIAF has a block body, you must wrap it in parentheses, because it can’t be (directly) function-called. The reason for this syntactic constraint is consistency with arrow functions whose bodies are expressions (as explained next).

As a consequence, the parentheses must be around the arrow function. In contrast, you have a choice with IIFEs – you can either put the parentheses around the whole expression:

```js
(function () {
    ···
}());
```

Or just around the function expression:

```js
(function () {
    ···
})();
```

Given how arrow functions work, the latter way of parenthesizing should be preferred from now on.

### Parenthesizing arrow function with expression bodies

If you want to understand why you can’t invoke an arrow function by putting parentheses immediately after it, you have to examine how expression bodies work: parentheses after an expression body should be part of the expression, not an invocation of the whole arrow function. This has to do with arrow functions binding loosely.

Let’s look at an example:

```js
const value = () => foo();
```

This should be interpreted as:

```js
const value = () => (foo());
```

And not as:

```js
const value = (() => foo)();
```

**Further reading:** [A section in the chapter on callable entities] has more information on using IIFEs and IIAFs in ES6. Spoiler: you rarely need them, as ES6 often provides better alternatives.

## Arrow functions versus `bind()`

ES6 arrow functions are often a compelling alternative to `Function.prototype.bind()`.

### Extracting methods

If an extracted method is to work as a callback, you must specify a fixed `this`, otherwise it will be invoked as a function (and `this` will be `undefined` or the global object). For example:

```js
obj.on('anEvent', this.handleEvent.bind(this));
```

An alternative is to use an arrow function:

```js
obj.on('anEvent', event => this.handleEvent(event));
```

### `this` via parameters

The following code demonstrates a neat trick: For some methods, you don’t nee  `bind()` for a callback, because they let you specify the value of `this`, via an additional parameter. `filter()` is one such method:

```js
const as = new Set([1, 2, 3]);
const bs = new Set([3, 2, 4]);
const intersection = [...as].filter(bs.has, bs); // [2, 3]
```

However, this code is easier to understand if you use an arrow function:

```js
const as = new Set([1, 2, 3]);
const bs = new Set([3, 2, 4]);
const intersection = [...as].filter(a => bs.has(a)); // [2, 3]
```

### Partial evaluation

`bind()` enables you to do [_partial evaluation_](http://www.2ality.com/2011/09/currying-vs-part-eval.html), you can create new functions by filling in parameters of an existing function:

```js
function add(x, y) {
    return x + y;
}

const plus1 = add.bind(undefined, 1);
```

Again, I find an arrow function easier to understand:

```js
const plus1 = y => add(1, y);
```

## Arrow functions versus normal functions

An arrow function is different from a normal function in only two ways:

- The following constructs are lexical: `arguments`, `super`, `this` `new.target`
- It can’t be used as a constructor: Normal functions support `new` via the internal method `[[Construct]]` and the property `prototype`. Arrow functions have neither, which is why `new (() => {})` throws an error.

Apart from that, there are no observable differences between an arrow function and a normal function. For example, `typeof` and `instanceof` produce the same results:

```js
typeof (() => {})
// 'function'
() => {} instanceof Function
// true

typeof function () {}
// 'function'
function () {} instanceof Function
// true
```

## FAQ: arrow functions

### Why are there “fat” arrow functions (`=>`) in ES6, but no “thin” arrow functions (`->`)?

ECMAScript 6 has syntax for functions with a lexical `this`, so-called _arrow functions_. However, it does not have arrow syntax for functions with dynami  `this`. That omission was deliberate; method definitions cover most of the use cases for thin arrows. If you really need dynamic `this`, you can still use a traditional function expression.
