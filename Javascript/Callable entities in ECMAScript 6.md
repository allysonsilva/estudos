# Callable entities in ECMAScript 6

This chapter gives advice on how to properly use entities you can call (via function calls, method calls, etc.) in ES6.

----------

## Overview

In ES5, a single construct, the (traditional) function, played three roles:

- Real (non-method) function
- Method
- Constructor

In ES6, there is more specialization. The three duties are now handled as follows. As far as function definitions and class definitions are concerned, a definition is either a declaration or an expression.

- Real (non-method) function:
    - Arrow functions (only have an expression form)
    - Traditional functions (created via function definitions)
    - Generator functions (created via generator function definitions)
- Method:
    - Methods (created by method definitions in object literals and class definitions)
    - Generator methods (created by generator method definitions in object literals and class definitions)
- Constructor:
    - Classes (created via class definitions)

Especially for callbacks, arrow functions are handy, because they don’t shadow the `this` of the surrounding scope.

For longer callbacks and stand-alone functions, traditional functions can be OK. Some APIs use `this` as an implicit parameter. In that case, you have no choice but to use traditional functions.

Note that I distinguish:

- The entities: e.g. traditional functions
- The syntax that creates the entities: e.g. function definitions

Even though their behaviors differ (as explained later), all of these entities are functions. For example:

```js
typeof (() => {}) // arrow function
// 'function'
typeof function* () {} // generator function
// 'function'
typeof class {} // class
// 'function'
```

## Ways of calling in ES6

Some calls can be made anywhere, others are restricted to specific locations.

### Calls that can be made anywhere

Three kinds of calls can be made anywhere in ES6:

- Function calls: `func(3, 1)`
- Method calls: `obj.method('abc')`
- Constructor calls: `new Constr(8)`

### Calls via `super` are restricted to specific locations

Two kinds of calls can be made via the `super` keyword; their use is restricted to specific locations:

- Super-method calls: `super.method('abc')`

    Only available within method definitions inside either object literals or derived class definitions.

- Super-constructor calls: `super(8)`

    Only available inside the special method `constructor()` inside a derived class definition.

### Non-method functions versus methods

The difference between non-method functions and methods is becoming more pronounced in ECMAScript 6. There are now special entities for both and things that only they can do:

- Arrow functions are made for non-method functions. They pick up `this` from their surrounding scopes (“lexical `this`”).
- Method definitions are made for methods. They provide support for `super`, to refer to super-properties and to make super-method calls.

## Recommendations for using callable entities

This section gives tips for using callable entities: When it’s best to use which entity; etc.

### Prefer arrow functions as callbacks

As callbacks, arrow functions have two advantages over traditional functions:

- `this` is lexical and therefore safer to use.
- Their syntax is more compact. That matters especially in functional programming, where there are many higher-order functions and methods (functions and methods whose parameters are functions).

For callbacks that span multiple lines, I find traditional function expressions acceptable, too. But you have to be careful with `this`.

#### Problem: `this` as an implicit parameter

Alas, some JavaScript APIs use `this` as an implicit argument for their callbacks, which prevents you from using arrow functions. For example: The `this` in line B is an implicit argument of the function in line A.

```js
beforeEach(function () { // (A)
    this.addMatchers({ // (B)
        toBeInRange: function (start, end) {
            ···
        }
    });
});
```

This pattern is less explicit and prevents you from using arrow functions.

#### Solution 1: change the API

This is easy to fix, but requires the API to change:

```js
beforeEach(api => {
    api.addMatchers({
        toBeInRange(start, end) {
            ···
        }
    });
});
```

We have turned the API from an implicit parameter `this` into an explicit parameter `api`. I like this kind of explicitness.

#### Solution 2: access the value of `this` in some other way

In some APIs, there are alternate ways to get to the value of `this`. For example, the following code uses `this`.

```js
var $button = $('#myButton');
$button.on('click', function () {
    this.classList.toggle('clicked');
});
```

But the target of the event can also be accessed via `event.target`:

```js
var $button = $('#myButton');
$button.on('click', event => {
    event.target.classList.toggle('clicked');
});
```

### Prefer function declarations as stand-alone functions

As stand-alone functions (versus callbacks), I prefer function declarations:

```js
function foo(arg1, arg2) {
    ···
}
```

The benefits are:

- Subjectively, I find they look nicer. In this case, the verbose keyword `function`is an advantage – you want the construct to stand out.
- They look like generator function declarations, leading to more visual consistency of the code.

There is one caveat: Normally, you don’t need `this` in stand-alone functions. If you use it, you want to access the `this` of the surrounding scope (e.g. a method which contains the stand-alone function). Alas, function declarations don’t let you do that – they have their own `this`, which shadows the `this` of the surrounding scope. Therefore, you may want to let a linter warn you about `this` in function declarations.

Another option for stand-alone functions is assigning arrow functions to variables. Problems with `this` are avoided, because it is lexical.

```js
const foo = (arg1, arg2) => {
    ···
};
```

### Prefer method definitions for methods

Method definitions are the only way to create methods that use `super`. They are the obvious choice in object literals and classes (where they are the only way to define methods), but what about adding a method to an existing object? For example:

```js
MyClass.prototype.foo = function (arg1, arg2) {
    ···
};
```

The following is a quick way to do the same thing in ES6 (caveat: `Object.assign()`doesn’t move methods with `super` properly).

```js
Object.assign(MyClass.prototype, {
    foo(arg1, arg2) {
        ···
    }
});
```

### Methods versus callbacks

Usually, function-valued properties should be created via method definitions. However, occasionally, arrow functions are the better choice. The following two subsections explain what to use when: the former approach is better for objects with methods, the latter approach is better for objects with callbacks.

#### An object whose properties are methods

Create function-valued properties via method definitions if those properties are really methods. That’s the case if the property values are closely related to the object (`obj`in the following example) and their sibling methods, not to the surrounding scope (`surroundingMethod()` in the example).

With a method definition, the `this` of a property value is the _receiver_ of the method call (e.g. `obj` if the method call is `obj.m(···)`).

For example, you can use [the WHATWG streams API](https://streams.spec.whatwg.org/) as follows:

```js
const surroundingObject = {
    surroundingMethod() {
        const obj = {
            data: 'abc',
            start(controller) {
                ···
                console.log(this.data); // abc (*)
                this.pull(); // (**)
                ···
            },
            pull() {
                ···
            },
            cancel() {
                ···
            },
        };
        const stream = new ReadableStream(obj);
    },
};
```

`obj` is an object whose properties `start`, `pull` and `cancel` are real methods. Accordingly, these methods can use `this` to access object-local state (line *) and to call each other (line **).

#### An object whose properties are callbacks

Create function-valued properties via arrow functions if the property values are callbacks. Such callbacks tend to be closely related to their surrounding scopes (`surroundingMethod()` in the following example), not to the objects they are stored in (`obj` in the example).

The `this` of an arrow function is the `this` of the surrounding scope (_lexical `this`_). Arrow functions make great callbacks, because that is the behavior you normally want for callbacks (real, non-method, functions). A callback shouldn’t have its own `this` that shadows the `this` of the surrounding scope.

If the properties `start`, `pull` and `cancel` are arrow functions then they pick up the `this` of `surroundingMethod()` (their surrounding scope):

```js
const surroundingObject = {
    surroundingData: 'xyz',
    surroundingMethod() {
        const obj = {
            start: controller => {
                ···
                console.log(this.surroundingData); // xyz (*)
                ···
            },

            pull: () => {
                ···
            },

            cancel: () => {
                ···
            },
        };
        const stream = new ReadableStream(obj);
    },
};

const stream = new ReadableStream();
```

If the output in line * surprises you then consider the following code:

```js
const obj = {
    foo: 123,
    bar() {
        const f = () => console.log(this.foo); // 123
        const o = {
            p: () => console.log(this.foo), // 123
        };
    },
}
```

Inside method `bar()`, the behavior of `f` should make immediate sense. The behavior of `o.p` is less obvious, but it is the same as `f`’s. Both arrow functions have the same surrounding lexical scope, `bar()`. The latter arrow function being surrounded by an object literal does not change that.

### Avoid IIFEs in ES6

This section gives tips for avoiding IIFEs in ES6.

#### Replace an IIFE with a block

In ES5, you had to use an IIFE if you wanted to keep a variable local:

```js
(function () {  // open IIFE
    var tmp = ···;
    ···
}());  // close IIFE

console.log(tmp); // ReferenceError
```

In ECMAScript 6, you can simply use a block and a `let` or `const` declaration:

```js
{  // open block
    let tmp = ···;
    ···
}  // close block

console.log(tmp); // ReferenceError
```

#### Replace an IIFE with a module

In ECMAScript 5 code that doesn’t use modules via libraries (such as RequireJS, browserify or webpack), the _revealing module pattern_ is popular, and based on an IIFE. Its advantage is that it clearly separates between what is public and what is private:

```js
var my_module = (function () {
    // Module-private variable:
    var countInvocations = 0;

    function myFunc(x) {
        countInvocations++;
        ···
    }

    // Exported by module:
    return {
        myFunc: myFunc
    };
}());
```

This module pattern produces a global variable and is used as follows:

```js
my_module.myFunc(33);
```

In ECMAScript 6, modules are built in, which is why the barrier to adopting them is low:

```js
// my_module.js

// Module-private variable:
let countInvocations = 0;

export function myFunc(x) {
    countInvocations++;
    ···
}
```

This module does not produce a global variable and is used as follows:

```js
import { myFunc } from 'my_module.js';

myFunc(33);
```

#### Immediately-invoked arrow functions

There is one use case where you still need an immediately-invoked function in ES6: Sometimes you only can produce a result via a sequence of statements, not via a single expression. If you want to inline those statements, you have to immediately invoke a function. In ES6, you can save a few characters via immediately-invoked arrow functions:

```js
const SENTENCE = 'How are you?';
const REVERSED_SENTENCE = (() => {
    // Iteration over the string gives us code points
    // (better for reversal than characters)
    const arr = [...SENTENCE];
    arr.reverse();
    return arr.join('');
})();
```

Note that you must parenthesize as shown (the parens are around the arrow function, not around the complete function call).

### Use classes as constructors

In ES5, constructor functions were the mainstream way of creating factories for objects (but there were also many other techniques, some arguably more elegant). In ES6, classes are the mainstream way of implementing constructor functions. Several frameworks support them as alternatives to their custom inheritance APIs.

## ES6 callable entities in detail

This section starts with a cheat sheet, before describing each ES6 callable entity in detail.

### Cheat sheet: callable entities

#### The behavior and structure of callable entities

Characteristics of the values produced by the entities:

|                      | **Func decl/Func expr** | **Arrow** | **Class** | **Method Function-callable** |
| -------------------- | ----------------------- | --------- | --------- | ---------------------------- |
| Function-callable    | ✔                       | ✔         | ✘         | ✔                            |
| Constructor-callable | ✔                       | ✘         | ✔         | ✘                            |
| Prototype            | `F.p`                   | `F.p`     | SC        | `F.p`                        |
| Property `prototype` | ✔                       | ✘         | ✔         | ✘                            |

Characteristics of the whole entities:

|                            | **Func decl** | **Func expr** | **Arrow** | **Class** | **Method** |
| -------------------------- | ------------- | ------------- | --------- | --------- | ---------- |
| Hoisted                    | ✔             |               |           | ✘         |            |
| Creates `window` prop. (1) | ✔             |               |           | ✘         |            |
| Inner name (2)             | ✘             | ✔             |           | ✔         | ✘          |

Characteristics of the bodies of the entities:

|              | **Func decl** | **Func expr** | **Arrow** | **Class (3)** | **Method** |
| ------------ | ------------- | ------------- | --------- | ------------- | ---------- |
| `this`       | ✔             | ✔             | lex       | ✔             | ✔          |
| `new.target` | ✔             | ✔             | lex       | ✔             | ✔          |
| `super.prop` | ✘             | ✘             | lex       | ✔             | ✔          |
| `super()`    | ✘             | ✘             | ✘         | ✔             | ✘          |

Legend – table cells:

- ✔ exists, allowed
- ✘ does not exist, not allowed
- Empty cell: not applicable, not relevant
- lex: lexical, inherited from surrounding lexical scope
- `F.p`: `Function.prototype`
- SC: superclass for derived classes, `Function.prototype` for base classes.

Legend – footnotes:

- (1) The rules for what declarations create properties on the global object.
- (2) The inner names of named function expressions and classes.
- (3) This column is about the body of the class constructor.

**What about generator functions and methods?** Those work like their non-generator counterparts, with two exceptions:

- Generator functions and methods have the prototype `(GeneratorFunction).prototype` (`(GeneratorFunction)` is an internal object, see diagram in Sect. “[Inheritance within the iteration API (including generators)]”).
- You can’t constructor-call generator functions.

#### The rules for `this`

|                                | **function call** | **Method call** | `new`       |
| ------------------------------ | ----------------- | --------------- | ----------- |
| Traditional function (strict)  | `undefined`       | receiver        | instance    |
| Traditional function (sloppy)  | `window`          | receiver        | instance    |
| Generator function (strict)    | `undefined`       | receiver        | `TypeError` |
| Generator function (sloppy)    | `window`          | receiver        | `TypeError` |
| Method (strict)                | `undefined`       | receiver        | `TypeError` |
| Method (sloppy)                | `window`          | receiver        | `TypeError` |
| Generator method (strict)      | `undefined`       | receiver        | `TypeError` |
| Generator method (sloppy)      | `window`          | receiver        | `TypeError` |
| Arrow function (strict&sloppy) | lexical           | lexical         | `TypeError` |
| Class (implicitly strict)      | `TypeError`       | `TypeError`     | SC protocol |

Legend – table cells:

- lexical: inherited from surrounding lexical scope
- SC (subclassing) protocol: A base class receives a new instance via `this`. A derived class gets its instance from its superclass.

### Traditional functions

These are the functions that you know from ES5. There are two ways to create them:

- Function expression:

  ```js
  const foo = function (x) { ··· };
  ```

- Function declaration:

  ```js
  function foo(x) { ··· }
  ```


Rules for `this`:

- Function calls: `this` is `undefined` in strict-mode functions and the global object in sloppy mode.
- Method calls: `this` is the receiver of the method call (or the first argument of `call`/`apply`).
- Constructor calls: `this` is the newly created instance.

### Generator functions

Their syntax is similar to traditional functions, but they have an extra asterisk:

- Generator function expression:

  ```js
  const foo = function* (x) { ··· };
  ```

- Generator function declaration:

  ```js
  function* foo(x) { ··· }
  ```

The rules for `this` are as follows. Note that `this` never refers to the generator object.

- Function/method calls: `this` is handled like it is with traditional functions. The results of such calls are generator objects.
- Constructor calls: You can’t constructor-call generator functions. A `TypeError` is thrown if you do.

### Method definitions

Method definitions can appear [inside object literals]:

```js
const obj = {
    add(x, y) {
        return x + y;
    }, // comma is required
    sub(x, y) {
        return x - y;
    }, // comma is optional
};
```

And [inside class definitions]:

```js
class AddSub {
    add(x, y) {
        return x + y;
    } // no comma
    sub(x, y) {
        return x - y;
    } // no comma
}
```

As you can see, you must separate method definitions in an object literal with commas, but there are no separators between them in a class definition. The former is necessary to keep the syntax consistent, especially with regard to getters and setters.

Method definitions are the only place where you can use `super` to refer to super-properties. Only method definitions that use `super` produce functions that have the internal property `[[HomeObject]]`, which is required for that feature.

Rules:

- Function calls: If you extract a method and call it as a function, it behaves like a traditional function.
- Method calls: work as with traditional functions, but additionally allow `super`.
- Constructor calls: throw a `TypeError`.

Inside class definitions, methods whose name is `constructor` are special, as explained later in this chapter.

### Generator method definitions

Their syntax is similar to method definitions, but they have an extra asterisk:

```js
const obj = {
    * generatorMethod(···) {
        ···
    },
};
class MyClass {
    * generatorMethod(···) {
        ···
    }
}
```

Rules:

- Calling a generator method returns a generator object.
- You can use `this` and `super` as you would in normal method definitions.

### Arrow functions

```js
const squares = [1,2,3].map(x => x * x);
```

The following variables are _lexical_ inside an arrow function (picked up from the surrounding scope):

- `arguments`
- `super`
- `this`
- `new.target`

Rules:

- Function calls: lexical `this` etc.
- Method calls: You can use arrow functions as methods, but their `this`continues to be lexical and does not refer to the receiver of a method call.
- Constructor calls: produce a `TypeError`.

### Classes

```js
// Base class: no `extends`
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return `(${this.x}, ${this.y})`;
    }
}

// This class is derived from `Point`
class ColorPoint extends Point {
    constructor(x, y, color) {
        super(x, y);
        this.color = color;
    }
    toString() {
        return super.toString() + ' in ' + this.color;
    }
}
```

The Method `constructor` is special, because it “becomes” the class. That is, classes are very similar to constructor functions:

```js
Point.prototype.constructor === Point
// true
```

Rules:

- Function/method calls: Classes can’t be called as functions or methods.
- Constructor calls: follow [a protocol that supports subclassing]. In a base class, an instance is created and `this` refers to it. A derived class receives its instance from its superclass, which is why it needs to call `super()` before it can access `this`.

## Dispatched and direct method calls in ES5 and ES6

There are two ways to call methods in JavaScript:

- Dynamic dispatch (`arr.slice(1)`): property `slice` is searched for in the prototype chain of `arr`. Its result is called with `this` set to `arr`.
- Direct call (`Array.prototype.slice.call(arr, 1)`): `slice` is called directly with `this` set to `arr` (the first argument of `call()`).

This section explains how these two work and why you will rarely call methods directly in ECMAScript 6. Before we get started, let’s refresh our knowledge of prototype chains.

### Background: prototype chains

Remember that each object in JavaScript is actually a chain of one or more objects. The first object inherits properties from the later objects. For example, the prototype chain of an Array `['a', 'b']` looks as follows:

1. The instance, holding the elements `'a'` and `'b'`
2. `Array.prototype`, the properties provided by the `Array` constructor
3. `Object.prototype`, the properties provided by the `Object` constructor
4. `null` (the end of the chain, so not really a member of it)

You can examine the chain via `Object.getPrototypeOf()`:

```js
var arr = ['a', 'b'];
var p = Object.getPrototypeOf;

p(arr) === Array.prototype
// true
p(p(arr)) === Object.prototype
// true
p(p(p(arr)))
// null
```

Properties in “earlier” objects override properties in “later” objects. For example, `Array.prototype` provides an Array-specific version of the `toString()` method, overriding `Object.prototype.toString()`.

```js
var arr = ['a', 'b'];
Object.getOwnPropertyNames(Array.prototype)
// [ 'toString', 'join', 'pop', ··· ]
arr.toString()
// 'a,b'
```

### Dispatched method calls

If you look at the method call `arr.toString()` you can see that it actually performs two steps:

1. Dispatch: In the prototype chain of `arr`, retrieve the value of the first property whose name is `toString`.
2. Call: Call the value and set the implicit parameter `this` to the _receiver_ `arr` of the method invocation.

You can make the two steps explicit by using the `call()` method of functions:

```js
var func = arr.toString; // dispatch
func.call(arr) // direct call, providing a value for `this`
// 'a,b'
```

### Direct method calls

There are two ways to make direct method calls in JavaScript:

- `Function.prototype.call(thisValue, arg0?, arg1?, ···)`
- `Function.prototype.apply(thisValue, argArray)`

Both method `call` and method `apply` are invoked on functions. They are different from normal function calls in that you specify a value for `this`. `call` provides the arguments of the method call via individual parameters, `apply` provides them via an Array.

With a dispatched method call, the receiver plays two roles: It is used to find the method and it is an implicit parameter. A problem with the first role is that a method must be in the prototype chain of an object if you want to invoke it. With a direct method call, the method can come from anywhere. That allows you to borrow a method from another object. For example, you can borrow `Object.prototype.toString` and thus apply the original, un-overridden implementation of `toString` to an Array `arr`:

```js
const arr = ['a','b','c'];
Object.prototype.toString.call(arr)
// '[object Array]'
```

The Array version of `toString()` produces a different result:

```js
arr.toString() // dispatched
// 'a,b,c'
Array.prototype.toString.call(arr); // direct
// 'a,b,c'
```

Methods that work with a variety of objects (not just with instances of “their” constructors) are called _generic_. _Speaking JavaScript_ has [a list] of all methods that are generic. The list includes most Array methods and all methods of `Object.prototype` (which have to work with all objects and are thus implicitly generic).

### Use cases for direct method calls

This section covers use cases for direct method calls. Each time, I’ll first describe the use case in ES5 and then how it changes with ES6 (where you’ll rarely need direct method calls).

#### ES5: Provide parameters to a method via an Array

Some functions accept multiple values, but only one value per parameter. What if you want to pass the values via an Array?

For example, `push()` lets you destructively append several values to an Array:

```js
var arr = ['a', 'b'];
arr.push('c', 'd')
// 4
arr
// [ 'a', 'b', 'c', 'd' ]
```

But you can’t destructively append a whole Array. You can work around that limitation by using `apply()`:

```js
var arr = ['a', 'b'];
Array.prototype.push.apply(arr, ['c', 'd'])
// 4
arr
// [ 'a', 'b', 'c', 'd' ]
```

Similarly, `Math.max()` and `Math.min()` only work for single values:

```js
Math.max(-1, 7, 2)
// 7
```

With `apply()`, you can use them for Arrays:

```js
Math.max.apply(null, [-1, 7, 2])
// 7
```

#### ES6: The spread operator (`...`) mostly replaces `apply()`

Making a direct method call via `apply()` only because you want to turn an Array into arguments is clumsy, which is why ECMAScript 6 has the spread operator (`...`) for this. It provides this functionality even in dispatched method calls.

```js
Math.max(...[-1, 7, 2])
// 7
```

Another example:

```js
const arr = ['a', 'b'];
arr.push(...['c', 'd'])
// 4
arr
// [ 'a', 'b', 'c', 'd' ]
```

As a bonus, spread also works with the `new` operator:

```js
new Date(...[2011, 11, 24])
// Sat Dec 24 2011 00:00:00 GMT+0100 (CET)
```

Note that `apply()` can’t be used with `new` – the above feat can only be achieved via [a complicated work-around] in ECMAScript 5.

#### ES5: Convert an Array-like object to an Array

Some objects in JavaScript are _Array-like_, they are almost Arrays, but don’t have any of the Array methods. Let’s look at two examples.

First, the special variable `arguments` of functions is Array-like. It has a `length` and indexed access to elements.

```js
var args = function () { return arguments }('a', 'b');
args.length
// 2
args[0]
// 'a'
```

But `arguments` isn’t an instance of `Array` and does not have the method `map()`.

```js
args instanceof Array
// false
args.map
// undefined
```

Second, the DOM method `document.querySelectorAll()` returns an instance of `NodeList`.

```js
document.querySelectorAll('a[href]') instanceof NodeList
// true
document.querySelectorAll('a[href]').map // no Array methods!
// undefined
```

Thus, for many complex operations, you need to convert Array-like objects to Arrays first. That is achieved via `Array.prototype.slice()`. This method copies the elements of its receiver into a new Array:

```js
var arr = ['a', 'b'];
arr.slice()
// [ 'a', 'b' ]
arr.slice() === arr
// false
```

If you call `slice()` directly, you can convert a `NodeList` to an Array:

```js
var domLinks = document.querySelectorAll('a[href]');
var links = Array.prototype.slice.call(domLinks);
links.map(function (link) {
    return link.href;
});
```

And you can convert `arguments` to an Array:

```js
function format(pattern) {
    // params start at arguments[1], skipping `pattern`
    var params = Array.prototype.slice.call(arguments, 1);
    return params;
}

console.log(format('a', 'b', 'c')); // ['b', 'c']
```

#### ES6: Array-like objects are less burdensome

On one hand, ECMAScript 6 has `Array.from()`, a simpler way of converting Array-like objects to Arrays:

```js
const domLinks = document.querySelectorAll('a[href]');
const links = Array.from(domLinks);
links.map(link => link.href);
```

On the other hand, you won’t need the Array-like `arguments`, because ECMAScript 6 has _rest parameters_ (declared via a triple dot):

```js
function format(pattern, ...params) {
    return params;
}

console.log(format('a', 'b', 'c')); // ['b', 'c']
```

#### ES5: Using `hasOwnProperty()` safely

`obj.hasOwnProperty('prop')` tells you whether `obj` has the _own_ (non-inherited) property `prop`.

```js
var obj = { prop: 123 };

obj.hasOwnProperty('prop')
// true

'toString' in obj // inherited
// true

obj.hasOwnProperty('toString') // own
// false
```

However, calling `hasOwnProperty` via dispatch can cease to work properly if `Object.prototype.hasOwnProperty` is overridden.

```js
var obj1 = { hasOwnProperty: 123 };
obj1.hasOwnProperty('toString')
// TypeError: Property 'hasOwnProperty' is not a function
```

`hasOwnProperty` may also be unavailable via dispatch if `Object.prototype` is not in the prototype chain of an object.

```js
var obj2 = Object.create(null);
obj2.hasOwnProperty('toString')
// TypeError: Object has no method 'hasOwnProperty'
```

In both cases, the solution is to make a direct call to `hasOwnProperty`:

```js
var obj1 = { hasOwnProperty: 123 };
Object.prototype.hasOwnProperty.call(obj1, 'hasOwnProperty')
// true

var obj2 = Object.create(null);
Object.prototype.hasOwnProperty.call(obj2, 'toString')
// false
```

#### ES6: Less need for `hasOwnProperty()`

`hasOwnProperty()` is mostly used to implement Maps via objects. Thankfully, ECMAScript 6 has a built-in `Map` data structure, which means that you’ll need `hasOwnProperty()` less.

### Abbreviations for `Object.prototype` and `Array.prototype`

You can access the methods of `Object.prototype` via an empty object literal (whose prototype is `Object.prototype`). For example, the following two direct method calls are equivalent:

```js
Object.prototype.hasOwnProperty.call(obj, 'propKey')
{}.hasOwnProperty.call(obj, 'propKey')
```

The same trick works for `Array.prototype`:

```js
Array.prototype.slice.call(arguments)
[].slice.call(arguments)
```

This pattern has become quite popular. It does not reflect the intention of the author as clearly as the longer version, but it’s much less verbose. [Speed-wise](http://jsperf.com/array-prototype-slice-call-vs-slice-call/17), there isn’t much of a difference between the two versions.

## The `name` property of functions

The `name` property of a function contains the function’s name:

```js
function foo() {}
foo.name
'foo'
```

This property is useful for debugging (its value shows up in stack traces) and some metaprogramming tasks (picking a function by name etc.).

Prior to ECMAScript 6, this property was already supported by most engines. With ES6, it becomes part of the language standard and is frequently filled in automatically.

### Constructs that provide names for functions

The following sections describe how `name` is set up automatically for various programming constructs.

#### Variable declarations and assignments

Functions pick up names if they are created via variable declarations:

```js
let func1 = function () {};
console.log(func1.name); // func1

const func2 = function () {};
console.log(func2.name); // func2

var func3 = function () {};
console.log(func3.name); // func3
```

But even with a normal assignment, `name` is set up properly:

```js
let func4;
func4 = function () {};
console.log(func4.name); // func4

var func5;
func5 = function () {};
console.log(func5.name); // func5
```

With regard to names, arrow functions are like anonymous function expressions:

```js
const func = () => {};
console.log(func.name); // func
```

From now on, whenever you see an anonymous function expression, you can assume that an arrow function works the same way.

#### Default values

If a function is a default value, it gets its name from its variable or parameter:

```js
let [func1 = function () {}] = [];
console.log(func1.name); // func1

let { f2: func2 = function () {} } = {};
console.log(func2.name); // func2

function g(func3 = function () {}) {
    return func3.name;
}
console.log(g()); // func3
```

#### Named function definitions

Function declarations and function expression are _function definitions_. This scenario has been supported for a long time: a function definition with a name passes it on to the `name` property.

For example, a function declaration:

```js
function foo() {}
console.log(foo.name); // foo
```

The name of a named function expression also sets up the `name` property.

```js
const bar = function baz() {};
console.log(bar.name); // baz
```

Because it comes first, the function expression’s name `baz` takes precedence over other names (e.g. the name `bar` provided via the variable declaration):

However, as in ES5, the name of a function expression is only a variable inside the function expression:

```js
const bar = function baz() {
    console.log(baz.name); // baz
};
bar();
console.log(baz); // ReferenceError
```

#### Methods in object literals

If a function is the value of a property, it gets its name from that property. It doesn’t matter if that happens via a method definition (line A), a traditional property definition (line B), a property definition with a computed property key (line C) or a property value shorthand (line D).

```js
function func() {}
let obj = {
    m1() {}, // (A)
    m2: function () {}, // (B)
    ['m' + '3']: function () {}, // (C)
    func, // (D)
};
console.log(obj.m1.name); // m1
console.log(obj.m2.name); // m2
console.log(obj.m3.name); // m3
console.log(obj.func.name); // func
```

The names of getters are prefixed with `'get'`, the names of setters are prefixed with `'set'`:

```js
let obj = {
    get foo() {},
    set bar(value) {},
};
let getter = Object.getOwnPropertyDescriptor(obj, 'foo').get;
console.log(getter.name); // 'get foo'

let setter = Object.getOwnPropertyDescriptor(obj, 'bar').set;
console.log(setter.name); // 'set bar'
```

#### Methods in class definitions

The naming of methods in class definitions is similar to object literals:

```js
class C {
    m1() {}
    ['m' + '2']() {} // computed property key

    static classMethod() {}
}
console.log(C.prototype.m1.name); // m1
console.log(new C().m1.name); // m1

console.log(C.prototype.m2.name); // m2

console.log(C.classMethod.name); // classMethod
```

Getters and setters again have the name prefixes `'get'` and `'set'`, respectively:

```js
class C {
    get foo() {}
    set bar(value) {}
}
let getter = Object.getOwnPropertyDescriptor(C.prototype, 'foo').get;
console.log(getter.name); // 'get foo'

let setter = Object.getOwnPropertyDescriptor(C.prototype, 'bar').set;
console.log(setter.name); // 'set bar'
```

#### Methods whose keys are symbols

In ES6, the key of a method can be a symbol. The `name` property of such a method is still a string:

- If the symbol has a description, the method’s name is the description in square brackets.
- Otherwise, the method’s name is the empty string (`''`).

```js
const key1 = Symbol('description');
const key2 = Symbol();

let obj = {
    [key1]() {},
    [key2]() {},
};
console.log(obj[key1].name); // '[description]'
console.log(obj[key2].name); // ''
```

#### Class definitions

Remember that class definitions create functions. Those functions also have their property `name` set up correctly:

```js
class Foo {}
console.log(Foo.name); // Foo

const Bar = class {};
console.log(Bar.name); // Bar
```

#### Default exports

All of the following statements set `name` to `'default'`:

```js
export default function () {}
export default (function () {});

export default class {}
export default (class {});

export default () => {};
```

#### Other programming constructs

- Generator functions and generator methods get their names the same way that normal functions and methods do.
- `new Function()` produces functions whose `name` is `'anonymous'`. [A webkit bug](https://bugs.webkit.org/show_bug.cgi?id=7726) describes why that is necessary on the web.
- `func.bind()` produces a function whose `name` is `'bound '+func.name`:

  ```js
  function foo(x) {
    return x
  }
  const bound = foo.bind(undefined, 123);
  console.log(bound.name); // 'bound foo'
  ```

### Caveats

#### Caveat: the name of a function is always assigned at creation

Function names are always assigned during creation and never changed later on. That is, JavaScript engines detect the previously mentioned patterns and create functions that start their lives with the correct names. The following code demonstrates that the name of the function created by `functionFactory()` is assigned in line A and not changed by the declaration in line B.

```js
function functionFactory() {
    return function () {}; // (A)
}
const foo = functionFactory(); // (B)
console.log(foo.name.length); // 0 (anonymous)
```

One could, in theory, check for each assignment whether the right-hand side evaluates to a function and whether that function doesn’t have a name, yet. But that would incur a significant performance penalty.

#### Caveat: minification

Function names are subject to minification, which means that they will usually change in minified code. Depending on what you want to do, you may have to manage function names via strings (which are not minified) or you may have to tell your minifier what names not to minify.

### Changing the names of functions

These are the attributes of property `name`:

```js
let func = function () {}
Object.getOwnPropertyDescriptor(func, 'name')
// { value: 'func',
//   writable: false,
//   enumerable: false,
//   configurable: true }
```

The property not being writable means that you can’t change its value via assignment:

```js
func.name = 'foo';
func.name
// 'func'
```

The property is, however, configurable, which means that you can change it by re-defining it:

```js
Object.defineProperty(func, 'name', {value: 'foo', configurable: true});
func.name
// 'foo'
```

If the property `name` already exists then you can omit the descriptor property `configurable`, because missing descriptor properties mean that the corresponding attributes are not changed.

If the property `name` does not exist yet then the descriptor property `configurable`ensures that `name` remains configurable (the default attribute values are all `false` or `undefined`).

### The function property `name` in the spec

- The spec operation [`SetFunctionName()`](http://www.ecma-international.org/ecma-262/6.0/#sec-setfunctionname)  sets up the property `name`. Search for its name in the spec to find out where that happens.
    - The third parameter of that operation specifies a name prefix. It is used for:
        - [Getters and setters](http://www.ecma-international.org/ecma-262/6.0/#sec-method-definitions-runtime-semantics-propertydefinitionevaluation)  (prefixes `'get'` and `'set'`)
        - [`Function.prototype.bind()`](http://www.ecma-international.org/ecma-262/6.0/#sec-function.prototype.bind)  (prefix `'bound'`)
- Anonymous function expressions not having a property `name` can be seen by looking at  [their runtime semantics](http://www.ecma-international.org/ecma-262/6.0/#sec-function-definitions-runtime-semantics-evaluation):
    - The names of named function expressions are set up via `SetFunctionName()`. That operation is not invoked for anonymous function expressions.
    - The names of function declarations are set up when entering a scope (they are hoisted!).
- [When an arrow function is created](http://www.ecma-international.org/ecma-262/6.0/#sec-arrow-function-definitions-runtime-semantics-evaluation), no name is set up, either (`SetFunctionName()` is not invoked).

## FAQ: callable entities

### How do I determine whether a function was invoked via `new`?

ES6 has a new protocol for subclassing. Part of that protocol is the _meta-property_ `new.target`, which refers to the first element in a chain of constructor calls (similar to `this` in a chain for supermethod calls). It is `undefined` if there is no constructor call. We can use that to enforce that a function must be invoked via `new` or that it must not be invoked via it. This is an example for the latter:

```js
function realFunction() {
    if (new.target !== undefined) {
        throw new Error('Can’t be invoked via `new`');
    }
    ···
}
```

In ES5, this was usually checked like this:

```js
function realFunction() {
    "use strict";
    if (this !== undefined) {
        throw new Error('Can’t be invoked via `new`');
    }
    ···
}
```
