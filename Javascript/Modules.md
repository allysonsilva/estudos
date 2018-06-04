# Modules

## Visão geral

JavaScript has had modules for a long time. However, they were implemented via libraries, not built into the language. ES6 is the first time that JavaScript has built-in modules.

ES6 modules are stored in files. There is exactly one module per file and one file per module. You have two ways of exporting things from a module. These two ways can be mixed, but it is usually better to use them separately.

### Multiple named exports

There can be multiple *named exports*:

```js
//------ lib.js ------
export const sqrt = Math.sqrt;

export function square(x) {
    return x * x;
}

export function diag(x, y) {
    return sqrt(square(x) + square(y));
}

//------ main.js ------
import { square, diag } from 'lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
```

You can also import the complete module:

```js
//------ main.js ------
import * as lib from 'lib';
console.log(lib.square(11)); // 121
console.log(lib.diag(4, 3)); // 5
```

### Single default export

There can be a single `default` export. For example, a function:

```js
//------ myFunc.js ------
export default function () { ··· } // no semicolon!

//------ main1.js ------
import myFunc from 'myFunc';
myFunc();
```

Ou uma classe:

```js
//------ MyClass.js ------
export default class { ··· } // no semicolon!

//------ main2.js ------
import MyClass from 'MyClass';
const inst = new MyClass();
```

Note that there is no semicolon at the end if you default-export a function or a class (which are anonymous declarations).

## Modules in JavaScript

Even though JavaScript never had built-in modules, the community has converged on a simple style of modules, which is supported by libraries in ES5 and earlier. This style has also been adopted by ES6:

* Each module is a piece of code that is executed once it is loaded.
* In that code, there may be declarations (variable declarations, function declarations, etc.).
    * By default, these declarations stay local to the module.
    * You can mark some of them as `exports`, then other modules can import them.
* A module can import things from other modules. It refers to those modules via module specifiers, strings that are either:
    * Relative paths (`'../model/user'`): these paths are interpreted relatively to the location of the importing module. The file extension `.js` can usually be omitted.
    * Absolute paths (`'/lib/js/helpers'`): point directly to the file of the module to be imported.
    * Names (`'util'`): What modules names refer to has to be configured.
* *Modules are* ***singletons***. Even if a module is imported multiple times, only a single “instance” of it exists.

This approach to modules avoids global variables, the only things that are global are module specifiers.

### ECMAScript 5 module systems

It is impressive how well *ES5* module systems work without explicit support from the language. The two most important (and unfortunately incompatible) standards are:

* *CommonJS* Modules: The dominant implementation of this standard is in Node.js (Node.js modules have a few features that go beyond CommonJS). Characteristics:
    * Compact syntax
    * Designed for synchronous loading and servers
* Asynchronous Module Definition (*AMD*): The most popular implementation of this standard is *RequireJS*. Characteristics:
    * Slightly more complicated syntax, enabling AMD to work without eval() (or a compilation step)
    * Designed for asynchronous loading and browsers

### ECMAScript 6 modules

The goal for *ECMAScript 6* modules was to create a format that both users of *CommonJS* and of *AMD* are happy with:

* Similarly to *CommonJS*, they have a compact syntax, a preference for single exports and support for cyclic dependencies.
* Similarly to *AMD*, they have direct support for asynchronous loading and configurable module loading.

Being built into the language allows *ES6* modules to go beyond *CommonJS* and *AMD* (details are explained later):

* Their syntax is even more compact than CommonJS’s.
* Their structure can be statically analyzed (for static checking, optimization, etc.).
* Their support for cyclic dependencies is better than CommonJS’s.

The ES6 module standard has two parts:

* Declarative syntax (for importing and exporting)
* Programmatic loader API: to configure how modules are loaded and to conditionally load modules

## The basics of ES6 modules

There are two kinds of exports: *named exports* (several per module) and *default exports* (one per module). As explained later, it is possible use both at the same time, but usually best to keep them separate.

### Named exports (several per module)

A module can export multiple things by prefixing its declarations with the keyword `export`. These exports are distinguished by their names and are called named exports.

```js
//------ lib.js ------
export const sqrt = Math.sqrt;

export function square(x) {
    return x * x;
}

export function diag(x, y) {
    return sqrt(square(x) + square(y));
}

//------ main.js ------
import { square, diag } from 'lib';
console.log(square(11));    // 121
console.log(diag(4, 3));    // 5
```

There are other ways to specify named exports (which are explained later), but I find this one quite convenient: simply write your code as if there were no outside world, then label everything that you want to export with a keyword.

If you want to, you can also import the whole module and refer to its named exports via property notation:

```js
//------ main.js ------
import * as lib from 'lib';

console.log(lib.square(11)); // 121
console.log(lib.diag(4, 3)); // 5
```

**The same code in CommonJS syntax**: For a while, I tried several clever strategies to be less redundant with my module exports in Node.js. Now I prefer the following simple but slightly verbose style that is reminiscent of the revealing module pattern:

```js
//------ lib.js ------
var sqrt = Math.sqrt;

function square(x) {
    return x * x;
}

function diag(x, y) {
    return sqrt(square(x) + square(y));
}

module.exports = {
    sqrt: sqrt,
    square: square,
    diag: diag,
};

//------ main.js ------
var square = require('lib').square;
var diag = require('lib').diag;

console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
```

### Default exports (one per module)

Modules that only export single values are very popular in the Node.js community. But they are also common in frontend development where you often have classes for models and components, with one class per module. An ES6 module can pick a *default export*, the main exported value. Default exports are especially easy to import.

The following ECMAScript 6 module “is” a single function:

```js
//------ myFunc.js ------
export default function () {} // no semicolon!

//------ main1.js ------
import myFunc from 'myFunc';
myFunc();
```

An ECMAScript 6 module whose default export is a class looks as follows:

```js
//------ MyClass.js ------
export default class {} // no semicolon!

//------ main2.js ------
import MyClass from 'MyClass';
const inst = new MyClass();
```

There are two styles of default exports:

1. Labeling declarations
2. Default-exporting values directly

#### Default export style 1: labeling declarations

You can prefix any function declaration (or generator function declaration) or class declaration with the keywords `export default` to make it the default export:

```js
export default function foo() {}    // no semicolon!
export default class Bar {}         // no semicolon!
```

You can also omit the name in this case. That makes default exports the only place where JavaScript has anonymous function declarations and anonymous class declarations:

```js
export default function () {}   // no semicolon!
export default class {}         // no semicolon!
```

##### Why anonymous function declarations and not anonymous function expressions?

When you look at the previous two lines of code, you’d expect the operands of `export default` to be expressions. They are only declarations for reasons of consistency: operands can be named declarations, interpreting their anonymous versions as expressions would be confusing (even more so than introducing new kinds of declarations).

If you want the operands to be interpreted as expressions, you need to use parentheses:

```js
export default (function () {});
export default (class {});
```

#### Default export style 2: default-exporting values directly

The values are produced via expressions:

```js
export default 'abc';
export default foo();
export default /^xyz$/;
export default 5 * 7;
export default { no: false, yes: true };
```

Cada uma dessas exportações padrão tem a seguinte estrutura.

```js
export default «expression»;
```

Isso equivale a:

```js
const __default__ = «expression»;
export { __default__ as default }; // (A)
```

The statement in line `A` is an export clause (which is explained in a later section).

##### Why two default export styles?

The second default export style was introduced because variable declarations can’t be meaningfully turned into default exports if they declare multiple variables:

```js
export default const foo = 1, bar = 2, baz = 3; // not legal JavaScript!
```

Which one of the three variables `foo`, `bar` and `baz` would be the default export?

### Imports and exports must be at the top level

As explained in more detail later, the structure of ES6 modules is static, you can’t conditionally import or export things. That brings a variety of benefits.

This restriction is enforced syntactically by only allowing imports and exports at the top level of a module:

```js
if (Math.random()) {
    import 'foo'; // SyntaxError
}

// You can’t even nest `import` and `export`
// inside a simple block:
{
    import 'foo'; // SyntaxError
}
```

### Imports are hoisted

Module imports are *hoisted* (internally moved to the beginning of the current scope). Therefore, it doesn’t matter where you mention them in a module and the following code works without any problems:

```js
foo();

import { foo } from 'my_module';
```

### Imports are read-only views on exports

The imports of an *ES6* module are *read-only* views on the exported entities. That means that the connections to variables declared inside module bodies remain live, as demonstrated in the following code.

```js
//------ lib.js ------
export let counter = 3;

export function incCounter() {
    counter++;
}

//------ main.js ------
import { counter, incCounter } from './lib';

// The imported value `counter` is live
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```

Imports as views have the following advantages:

* They enable cyclic dependencies, even for unqualified imports (as explained in the next section).
* Qualified and unqualified imports work the same way (they are both indirections).
* You can split code into multiple modules and it will continue to work (as long as you don’t try to change the values of imports).

## Importing and exporting in detail

### Importing styles

ECMAScript 6 provides several styles of importing:

* Default import:
    * `import localName from 'src/my_lib';`
* Namespace import: imports the module as an object (with one property per named export).
    * `import * as my_lib from 'src/my_lib';`
* Named imports:
    * `import { name1, name2 } from 'src/my_lib';`

You can rename named imports:

```js
// Renaming: import `name1` as `localName1`
import { name1 as localName1, name2 } from 'src/my_lib';

// Renaming: import the default export as `foo`
import { default as foo } from 'src/my_lib';
```

* Empty import: only loads the module, doesn’t import anything. The first such import in a program executes the body of the module.
    * `import 'src/my_lib';`

There are only two ways to combine these styles and the order in which they appear is fixed; the default export always comes first.

* Combining a default import with a namespace import:
    * `import theDefault, * as my_lib from 'src/my_lib';`
* Combining a default import with named imports:
    * `import theDefault, { name1, name2 } from 'src/my_lib';`

### Named exporting styles: inline versus clause

There are two ways in which you can export named things inside modules.

On one hand, you can mark declarations with the keyword export.

```js
export var myVar1 = ···;
export let myVar2 = ···;
export const MY_CONST = ···;

export function myFunc() {
    ···
}

export function* myGeneratorFunc() {
    ···
}

export class MyClass {
    ···
}
```

On the other hand, you can list everything you want to export at the end of the module (which is similar in style to the revealing module pattern).

```js
const MY_CONST = ···;
function myFunc() {
    ···
}

export { MY_CONST, myFunc };
```

You can also export things under different names:

```js
export { MY_CONST as FOO, myFunc };
```

### Re-exporting

Re-exporting means adding another module’s exports to those of the current module. You can either add all of the other module’s exports:

```js
export * from 'src/other_module';
```

Default exports are ignored by `export *`.

Or you can be more selective (optionally while renaming):

```js
export { foo, bar } from 'src/other_module';

// Renaming: export other_module’s foo as myFoo
export { foo as myFoo, bar } from 'src/other_module';
```

#### Making a re-export the default export

The following statement makes the default export of another module `foo` the `default` `export` of the current module:

```js
export { default } from 'foo';
```

The following statement makes the named export `myFunc` of module `foo` the default export of the current module:

```js
export { myFunc as default } from 'foo';
```

### All exporting styles

ECMAScript 6 provides several styles of exporting:

* Re-exporting:
    * Re-export everything (except for the default export):

    ```js
    export * from 'src/other_module';
    ```

    * Re-export via a clause:

    ```js
    export { foo as myFoo, bar } from 'src/other_module';

    export { default } from 'src/other_module';
    export { default as foo } from 'src/other_module';
    export { foo as default } from 'src/other_module';
    ```
* Named exporting via a clause:

```js
export { MY_CONST as FOO, myFunc };
export { foo as default };
```

* Inline named exports:

    * Variable declarations:

    ```js
    export var foo;
    export let foo;
    export const foo;
    ```

    * Function declarations:

    ```js
    export function myFunc() {}
    export function* myGenFunc() {}
    ```

    * Class declarations:

    ```js
    export class MyClass {}
    ```

* Default export:
    * Function declarations (can be anonymous here):

    ```js
    export default function myFunc() {}
    export default function () {}

    export default function* myGenFunc() {}
    export default function* () {}
    ```

    * Class declarations (can be anonymous here):

    ```js
    export default class MyClass {}
    export default class {}
    ```

    * Expressions: export values. Note the semicolons at the end.

    ```js
    export default foo;
    export default 'Hello world!';
    export default 3 * 7;
    export default (function () {});
    ```

### Having both named exports and a default export in a module

The following pattern is surprisingly common in JavaScript: A library is a single function, but additional services are provided via properties of that function. Examples include jQuery and Underscore.js. The following is a sketch of Underscore as a CommonJS module:

```js
//------ underscore.js ------
var _ = function (obj) {
    ···
};

var each = _.each = _.forEach =
    function (obj, iterator, context) {
        ···
    };

module.exports = _;

//------ main.js ------
var _ = require('underscore');
var each = _.each;
···
```

With ES6 glasses, the function _ is the default export, while each and forEach are named exports. As it turns out, you can actually have named exports and a default export at the same time. As an example, the previous CommonJS module, rewritten as an ES6 module, looks like this:

```js
//------ underscore.js ------
export default function (obj) {
    ···
}

export function each(obj, iterator, context) {
    ···
}

export { each as forEach };

//------ main.js ------
import _, { each } from 'underscore';
···
```

Note that the CommonJS version and the ECMAScript 6 version are only roughly similar. The latter has a flat structure, whereas the former is nested.

#### Recommendation: avoid mixing default exports and named exports

I generally recommend to keep the two kinds of exporting separate: per module, either only have a default export or only have named exports.

However, that is not a very strong recommendation; it occasionally may make sense to mix the two kinds. One example is a module that default-exports an entity. For unit tests, one could additionally make some of the internals available via named exports.

#### The default export is just another named export

The default export is actually just a named export with the special name `default`. That is, the following two statements are equivalent:

```js
import { default as foo } from 'lib';
import foo from 'lib';
```

Similarly, the following two modules have the same default export:

```js
//------ module1.js ------
export default function foo() {} // function declaration!

//------ module2.js ------
function foo() {}
export { foo as default };
```

#### default: OK as export name, but not as variable name

You can’t use reserved words (such as `default` and `new`) as variable names, but you can use them as names for exports (you can also use them as property names in ECMAScript 5). If you want to directly import such named exports, you have to rename them to proper variables names.

That means that `default` can only appear on the left-hand side of a renaming import:

```js
import { default as foo } from 'some_module';
```

And it can only appear on the right-hand side of a renaming export:

```js
export { foo as default };
```

In re-exporting, both sides of the `as` are export names:

```js
export { myFunc as default } from 'foo';
export { default as otherFunc } from 'foo';

// The following two statements are equivalent:
export { default } from 'foo';
export { default as default } from 'foo';
```

## Details: imports as views on exports

Imports work differently in CommonJS and ES6:

* In *CommonJS*, imports are copies of exported values.
* In *ES6*, imports are live read-only views on exported values.

The following sections explain what that means.

### In CommonJS, imports are copies of exported values

With CommonJS (Node.js) modules, things work in relatively familiar ways.

If you import a value into a variable, the value is copied twice: once when it is exported (line A) and once it is imported (line B).

```js
//------ lib.js ------
var counter = 3;

function incCounter() {
    counter++;
}

module.exports = {
    counter: counter, // (A)
    incCounter: incCounter,
};

//------ main1.js ------
var counter = require('./lib').counter; // (B)
var incCounter = require('./lib').incCounter;

// The imported value is a (disconnected) copy of a copy
console.log(counter); // 3
incCounter();
console.log(counter); // 3

// The imported value can be changed
counter++;
console.log(counter); // 4
```

If you access the value via the exports object, it is still copied once, on export:

```js
//------ main2.js ------
var lib = require('./lib');

// The imported value is a (disconnected) copy
console.log(lib.counter); // 3
lib.incCounter();
console.log(lib.counter); // 3

// The imported value can be changed
lib.counter++;
console.log(lib.counter); // 4
```

### In ES6, imports are live read-only views on exported values

In contrast to CommonJS, imports are views on exported values. In other words, every import is a live connection to the exported data. Imports are read-only:

* Unqualified imports (`import x from 'foo'`) are like `const`-declared variables.
* The properties of a module object foo (`import * as foo from 'foo'`) are like the properties of a frozen object.

The following code demonstrates how imports are like views:

```js
//------ lib.js ------
export let counter = 3;

export function incCounter() {
    counter++;
}

//------ main1.js ------
import { counter, incCounter } from './lib';

// The imported value `counter` is live
console.log(counter); // 3
incCounter();
console.log(counter); // 4

// The imported value can’t be changed
counter++; // TypeError
```

If you import the module object via the asterisk (*), you get the same results:

```js
//------ main2.js ------
import * as lib from './lib';

// The imported value `counter` is live
console.log(lib.counter); // 3
lib.incCounter();
console.log(lib.counter); // 4

// The imported value can’t be changed
lib.counter++; // TypeError
```

Note that while you can’t change the values of imports, you can change the objects that they are referring to. For example:

```js
//------ lib.js ------
export let obj = {};

//------ main.js ------
import { obj } from './lib';

obj.prop = 123; // OK
obj = {}; // TypeError
```

#### Why a new approach to importing?

Why introduce such a relatively complicated mechanism for importing that deviates from established practices?

* Cyclic dependencies: The main advantage is that it supports cyclic dependencies even for unqualified imports.
* Qualified and unqualified imports work the same. In CommonJS, they don’t: a qualified import provides direct access to a property of a module’s export object, an unqualified import is a copy of it.
* You can split code into multiple modules and it will continue to work (as long as you don’t try to change the values of imports).
* On the flip side, module folding, combining multiple modules into a single module becomes simpler, too.

### Implementing views

How do imports work as views of exports under the hood? Exports are managed via the data structure *export entry*. All export entries (except those for re-exports) have the following two names:

* Local name: is the name under which the export is stored inside the module.
* Export name: is the name that importing modules need to use to access the export.

After you have imported an entity, that entity is always accessed via a pointer that has the two components *module* and *local name*. In other words, that pointer refers to a binding (the storage space of a variable) inside a module.

Let’s examine the export names and local names created by various kinds of exporting. The following table (adapted from the ES6 spec) gives an overview, subsequent sections have more details.

Declaração	                Nome local	Nome da exportação
export {v};	                    'v'	        'v'
export {v as x};	            'v'	        'x'
export const v = 123;	        'v'	        'v'
export function f() {}	        'f'	        'f'
export default function f() {}	'f'	        'default'
export default function () {}	'*default*' 'default'
export default 123;	            '*default*' 'default'

#### Export clause

```js
function foo() {}
export { foo };
```

* Nome local: `foo`
* Nome da exportação: `foo`

```js
function foo() {}
export { foo as bar };
```

* Local name: `foo`
* Export name: `bar`

#### Inline exports

This is an inline export:

```js
export function foo() {}
```

It is equivalent to the following code:

```js
function foo() {}
export { foo };
```

Therefore, we have the following names:

* Local name: `foo`
* Export name: `foo`

#### Default exports

There are two kinds of default exports:

* Default exports of *hoistable declarations* (function declarations, generator function declarations) and class declarations are similar to normal inline exports in that named local entities are created and tagged.
* All other default exports are about exporting the results of expressions.

##### Default-exporting expressions

The following code default-exports the result of the expression `123`:

```js
export default 123;
```

It is equivalent to:

```js
const *default* = 123;  // *not* legal JavaScript
export { *default* as default };
```

If you default-export an expression, you get:

* Local name: `*default*`
* Export name: `default`

The local name was chosen so that it wouldn’t clash with any other local name.

Note that a default export still leads to a binding being created. But, due to `*default*` not being a legal identifier, you can’t access that binding from inside the module.

##### Default-exporting hoistable declarations and class declarations

The following code default-exports a function declaration:

```js
export default function foo() {}
```

It is equivalent to:

```js
function foo() {}
export { foo as default };
```

The names are:

* Local name: `foo`
* Export name: `default`

That means that you can change the value of the default export from within the module, by assigning a different value to `foo`.

(Only) for default exports, you can also omit the name of a function declaration:

```js
export default function () {}
```

That is equivalent to:

```js
function *default*() {} // *not* legal JavaScript
export { *default* as default };
```

The names are:

* Local name: `*default*`
* Export name: `default`

Default-exporting generator declarations and class declarations works similarly to default-exporting function declarations.

## Advantages of ECMAScript 6 modules

At first glance, having modules built into ECMAScript 6 may seem like a boring feature – after all, we already have several good module systems. But ECMAScript 6 modules have several new features:

* More compact syntax
* Static module structure (helping with dead code elimination, optimizations, static checking and more)
* Automatic support for cyclic dependencies

*ES6* modules will also – hopefully – end the fragmentation between the currently dominant standards CommonJS and AMD. Having a single, native standard for modules means:

* No more UMD (Universal Module Definition): UMD is a name for patterns that enable the same file to be used by several module systems (e.g. both *CommonJS* and *AMD*). Once *ES6* is the only module standard, UMD becomes obsolete.
* New browser APIs become modules instead of global variables or properties of `navigator`.
* No more objects-as-namespaces: Objects such as `Math` and `JSON` serve as namespaces for functions in ECMAScript 5. In the future, such functionality can be provided via modules.
