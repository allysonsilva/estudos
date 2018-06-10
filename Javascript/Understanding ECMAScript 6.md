# Understanding ECMAScript 6

## Block Bindings

This chapter demonstrates why classic `var` declarations can be confusing, introduces block-level bindings in ECMAScript 6, and then offers some best practices for using them.

### Var Declarations and Hoisting

Variable declarations using `var` are treated as if they are at the top of the function (or global scope, if declared outside of a function) regardless of where the actual declaration occurs; this is called hoisting. For a demonstration of what *hoisting* does, consider the following function definition:

```js
function getValue(condition) {
    if (condition) {
        var value = "blue";
        // other code
        return value;
    } else {
        // value exists here with a value of undefined
        return null;
    }
    // value exists here with a value of undefined
}
```

Se você não estiver familiarizado com JavaScript, então deva esperar que a variável `value` seja criado apenas se a expressão de `condition` seja avaliada como verdadeira. Sendo que `value` é criado independente do resultado da condição.

Isso é como o motor JavaScript avalia a função `getValue`:

```js
function getValue(condition) {
    var value;
    if (condition) {
        value = "blue";
        // other code
        return value;
    } else {
        return null;
    }
}
```

A declaração de `value` é elevada ao topo, enquanto a inicialização permanece no mesmo local.

Para resolver esse problema de elevação da variável ao top da função, ECMAScript 6 introduz escopos de nível de blocos que faz com que uma variável tenha seu ciclo de vida de acordo com bloco.

### Block-Level Declarations

Block-level declarations are those that declare variables that are inaccessible outside of a given block scope. Block scopes, also called lexical scopes, are created:

1. Inside of a function
2. Inside of a block (indicated by the `{` and `}` characters)

*A variable declared with either `let` or `const` cannot be accessed until after the declaration*.

#### Let Declarations

The `let` declaration syntax is the same as the syntax for `var`. You can basically replace `var` with `let` to declare a variable, but limit the variable’s scope to only the current code block (there are a few other subtle differences discussed a bit later, as well). Since `let` declarations are not hoisted to the top of the enclosing block, you may want to always place `let` declarations first in the block, so that they are available to the entire block.

```js
function getValue(condition) {
    if (condition) {
        let value = "blue";
        // other code
        return value;
    } else {
        // value doesn't exist here
        return null;
    }
    // value doesn't exist here
}
```

A variável `value` declarado com `let` não está mais acessível fora do `if` de `condition`, pois esse é seu escopo (Delimitados por meio de `{}`).

#### No Redeclaration

If an identifier has already been defined in a scope, then using the identifier in a `let` declaration inside that scope causes an error to be thrown. For example:

```js
var count = 30;

// Syntax error
let count = 40;
```

On the other hand, no error is thrown if a `let` declaration creates a new variable with the same name as a variable in its containing scope, as demonstrated in the following code:

```js
var count = 30;
// Does not throw an error
if (condition) {
    let count = 40;
    // more code
}
```

This `let` declaration doesn’t throw an error because it creates a new variable called `count` within the if statement, instead of creating `count` in the surrounding block. Inside the `if` block, this new variable shadows the global `count`, preventing access to it until execution leaves the block.

#### Constant Declarations

You can also define variables in ECMAScript 6 with the `const` declaration syntax. Variables declared using `const` are considered *constants*, meaning their values cannot be changed once set. For this reason, every `const` variable must be initialized on declaration, as shown in this example:

```js
// Valid constant
const maxItems = 30;

// Syntax error: missing initialization
const name;
```

**Constants vs Let Declarations**

Constants, like `let` declarations, are block-level declarations. That means constants are no longer accessible once execution flows out of the block in which they were declared, and declarations are not hoisted.

```js
if (condition) {
    const maxItems = 5;
    // more code
}
// maxItems isn't accessible here
```

In another similarity to `let`, a `const` declaration throws an error when made with an identifier for an already-defined variable in the same scope. It doesn’t matter if that variable was declared using `var` (for global or function scope) or `let` (for block scope).

```js
var message = "Hello!";
let age = 25;

// Each of these would throw an error.
const message = "Goodbye!";
const age = 30;
```

Despite those similarities, there is one big difference between `let` and `const` to remember. Attempting to assign a `const` to a previously defined constant will throw an error, in both strict and non-strict modes.

```js
const maxItems = 5;

maxItems = 6;      // throws error
```

**Declaring Objects with Const**

A `const` declaration prevents modification of the binding and not of the value itself. That means `const` declarations for objects do not prevent modification of those objects.

```js
const person = {
    name: "Nicholas"
};
// works
person.name = "Greg";
// throws an error
person = {
    name: "Greg"
};
```

### Global Block Bindings

Another way in which `let` and `const` are different from `var` is in their global scope behavior. When `var` is used in the global scope, it creates a new global variable, which is a property on the global object (`window` in browsers). That means you can accidentally overwrite an existing global using `var`, such as:

```js
// in a browser
var RegExp = "Hello!";
console.log(window.RegExp);     // "Hello!"

var ncz = "Hi!";
console.log(window.ncz);        // "Hi!"
```

If you instead use `let` or `const` in the global scope, a new binding is created in the global scope but no property is added to the global object. That also means you cannot overwrite a global variable using `let` or `const`, you can only shadow it. Here’s an example:

```js
// in a browser
let RegExp = "Hello!";
console.log(RegExp);    // "Hello!"
console.log(window.RegExp === RegExp);  // false

const ncz = "Hi!";
console.log(ncz);   // "Hi!"
console.log("ncz" in window);   // false
```

### Emerging Best Practices for Block Bindings

While ECMAScript 6 was in development, there was widespread belief you should use `let` by default instead of `var` for variable declarations. For many JavaScript developers, `let` behaves exactly the way they thought `var` should have behaved, and so the direct replacement makes logical sense. In this case, you would use const for variables that needed modification protection.

However, as more developers migrated to ECMAScript 6, an alternate approach gained popularity: use `const` by default and only use `let` when you know a variable’s value needs to change. The rationale is that most variables should not change their value after initialization because unexpected value changes are a source of bugs. This idea has a significant amount of traction and is worth exploring in your code as you adopt ECMAScript 6.

## Strings and Regular Expressions

### Other String Changes

JavaScript strings have always lagged behind similar features of other languages. It was only in ECMAScript 5 that strings finally gained a `trim()` method, for example, and ECMAScript 6 continues extending JavaScript’s capacity to parse strings with new functionality.

#### Methods for Identifying Substrings

Developers have used the `indexOf()` method to identify strings inside other strings since JavaScript was first introduced. ECMAScript 6 includes the following three methods, which are designed to do just that:

* The `includes()` method returns true if the given text is found anywhere within the string. It returns false if not.
* The `startsWith()` method returns true if the given text is found at the beginning of the string. It returns false if not.
* The `endsWith()` method returns true if the given text is found at the end of the string. It returns false if not.

Each methods accept two arguments: the text to search for and an optional index from which to start the search. When the second argument is provided, `includes()` and `startsWith()` start the match from that index while `endsWith()` starts the match from the length of the string minus the second argument; when the second argument is omitted, `includes()` and `startsWith()` search from the beginning of the string, while `endsWith()` starts from the end. In effect, the second argument minimizes the amount of the string being searched. Here are some examples showing these three methods in action:

```js
var msg = "Hello world!";

console.log(msg.startsWith("Hello"));       // true
console.log(msg.endsWith("!"));             // true
console.log(msg.includes("o"));             // true

console.log(msg.startsWith("o"));           // false
console.log(msg.endsWith("world!"));        // true
console.log(msg.includes("x"));             // false

console.log(msg.startsWith("o", 4));        // true
console.log(msg.endsWith("o", 8));          // true
console.log(msg.includes("o", 8));          // false
```

#### The repeat() Method

ECMAScript 6 also adds a `repeat()` method to strings, which accepts the number of times to repeat the string as an argument. It returns a new string containing the original string repeated the specified number of times.

```js
console.log("x".repeat(3));         // "xxx"
console.log("hello".repeat(2));     // "hellohello"
console.log("abc".repeat(4));       // "abcabcabcabc"
```

### Template Literals

In reality, though, template literals are ECMAScript 6’s answer to the following features that JavaScript lacked all the way through ECMAScript 5:

* **Multiline strings** A formal concept of multiline strings.
* **Basic string formatting** The ability to substitute parts of the string for values contained in variables.
* **HTML escaping** The ability to transform a string such that it is safe to insert into HTML.

#### Basic Syntax

At their simplest, template literals act like regular strings delimited by backticks (`) instead of double or single quotes. For example, consider the following:

```js
let message = `Hello world!`;

console.log(message);               // "Hello world!"
console.log(typeof message);        // "string"
console.log(message.length);        // 12
```

```js
let message = `\`Hello\` world!`;

console.log(message);               // "`Hello` world!"
console.log(typeof message);        // "string"
console.log(message.length);        // 14
```

#### Multiline Strings

JavaScript developers have wanted a way to create multiline strings since the first version of the language. But when using double or single quotes, strings must be completely contained on a single line.

**Pre-ECMAScript 6 Workarounds**

Thanks to a long-standing syntax bug, JavaScript does have a workaround. You can create multiline strings if there’s a backslash (`\`) before a newline.

```js
var message = "Multiline \
string";

console.log(message);       // "Multiline string"
```

The `message` string has no newlines present when printed to the console because the backslash is treated as a continuation rather than a newline. In order to show a newline in output, you’d need to manually include it:

```js
var message = "Multiline \n\
string";

console.log(message);       // "Multiline
                            //  string"
```

Other pre-ECMAScript 6 attempts to create multiline strings usually relied on arrays or string concatenation, such as:

```js
var message = [
    "Multiline ",
    "string"
].join("\n");

let message = "Multiline \n" +
    "string";
```

**Multiline Strings the Easy Way**

ECMAScript 6’s template literals make multiline strings easy because there’s no special syntax. Just include a newline where you want, and it shows up in the result.

```js
let message = `Multiline
string`;

console.log(message);           // "Multiline
                                //  string"
console.log(message.length);    // 16
```

All whitespace inside the backticks is part of the string, so be careful with indentation.

```js
let message = `Multiline
               string`;

console.log(message);           // "Multiline
                                //                 string"
console.log(message.length);    // 31
```

In this code, all whitespace before the second line of the template literal is considered part of the string itself. If making the text line up with proper indentation is important to you, then consider leaving nothing on the first line of a multiline template literal and then indenting after that, as follows:

```js
let html = `
<div>
    <h1>Title</h1>
</div>`.trim();
```

#### Making Substitutions

At this point, template literals may look like fancier versions of normal JavaScript strings. The real difference between the two lies in template literal *substitutions*. Substitutions allow you to embed any valid JavaScript expression inside a template literal and output the result as part of the string.

Substitutions are delimited by an opening `${` and a closing `}` that can have any JavaScript expression inside. The simplest substitutions let you embed local variables directly into a resulting string, like this:

```js
let name = "Nicholas",
    message = `Hello, ${name}.`;

console.log(message);       // "Hello, Nicholas."
```

```js
let count = 10,
    price = 0.25,
    message = `${count} items cost $${(count * price).toFixed(2)}.`;

console.log(message);       // "10 items cost $2.50."
```

Template literals are also JavaScript expressions, which means you can place a template literal inside of another template literal, as in this example:

```js
let name = "Nicholas",
    message = `Hello, ${
`my name is ${ name }`
    }.`;

console.log(message);        // "Hello, my name is Nicholas."
```

## Functions

Functions are an important part of any programming language, and prior to ECMAScript 6, JavaScript functions hadn’t changed much since the language was created. This left a backlog of problems and nuanced behavior that made making mistakes easy and often required more code just to achieve very basic behaviors.

ECMAScript 6 functions make a big leap forward, taking into account years of complaints and requests from JavaScript developers. The result is a number of incremental improvements on top of ECMAScript 5 functions that make programming in JavaScript less error-prone and more powerful.

### Functions with Default Parameter Values

Functions in JavaScript are unique in that they allow any number of parameters to be passed, regardless of the number of parameters declared in the function definition. This allows you to define functions that can handle different numbers of parameters, often by just filling in default values when parameters aren’t provided. This section covers how default parameters work both in and prior to ECMAScript 6, along with some important information on the `arguments` object, using expressions as parameters, and another TDZ.

#### Simulating Default Parameter Values in ECMAScript 5

In ECMAScript 5 and earlier, you would likely use the following pattern to create a function with default parameters values:

```js
function makeRequest(url, timeout, callback) {
    timeout = timeout || 2000;
    callback = callback || function() {};
    // the rest of the function
}
```

In that case, a safer alternative is to check the type of the argument using `typeof`, as in this example:

```js
function makeRequest(url, timeout, callback) {
    timeout = (typeof timeout !== "undefined") ? timeout : 2000;
    callback = (typeof callback !== "undefined") ? callback : function() {};
    // the rest of the function
}
```

While this approach is safer, it still requires a lot of extra code for a very basic operation. Popular JavaScript libraries are filled with similar patterns, as this represents a common pattern.

#### Default Parameter Values in ECMAScript 6

ECMAScript 6 makes it easier to provide default values for parameters by providing initializations that are used when the parameter isn’t formally passed.

```js
function makeRequest(url, timeout = 2000, callback = function() {}) {
    // the rest of the function
}
```

When `makeRequest()` is called with all three parameters, the defaults are not used.

```js
// uses default timeout and callback
makeRequest("/foo");

// uses default callback
makeRequest("/foo", 500);

// doesn't use defaults
makeRequest("/foo", 500, function(body) {
    doSomething(body);
});
```

It’s possible to specify default values for any arguments, including those that appear before arguments without default values in the function declaration.

In this case, the default value for `timeout` will only be used if there is no second argument passed in or if the second argument is explicitly passed in as `undefined`, as in this example:

```js
// uses default timeout
makeRequest("/foo", undefined, function(body) {
    doSomething(body);
});

// uses default timeout
makeRequest("/foo");

// doesn't use default timeout
makeRequest("/foo", null, function(body) {
    doSomething(body);
});
```

In the case of default parameter values, a value of `null` is considered to be valid, meaning that in the third call to `makeRequest()`, the default value for `timeout` will not be used.

### Working with Unnamed Parameters

So far, the examples in this chapter have only covered parameters that have been named in the function definition. However, JavaScript functions don’t limit the number of parameters that can be passed to the number of named parameters defined. You can always pass fewer or more parameters than formally specified. Default parameter values make it clear when a function can accept fewer parameters, and ECMAScript 6 sought to make the problem of passing more parameters than defined better as well.

#### Unnamed Parameters in ECMAScript 5

Early on, JavaScript provided the `arguments` object as a way to inspect all function parameters that are passed without necessarily defining each parameter individually. While inspecting `arguments` works fine in most cases, this object can be a little cumbersome to work with. For example, examine this code, which inspects the `arguments` object:

```js
function pick(object) {
    let result = Object.create(null);

    // start at the second parameter
    for (let i = 1, len = arguments.length; i < len; i++) {
        result[arguments[i]] = object[arguments[i]];
    }

    return result;
}

let book = {
    title: "Understanding ECMAScript 6",
    author: "Nicholas C. Zakas",
    year: 2015
};

let bookData = pick(book, "author", "year");

console.log(bookData.author);   // "Nicholas C. Zakas"
console.log(bookData.year);     // 2015
```

ECMAScript 6 introduces **rest parameters** to help with these issues.

#### Rest Parameters

A *rest parameter* is indicated by three dots (`...`) preceding a named parameter. That named parameter becomes an `Array` containing the rest of the parameters passed to the function, which is where the name “rest” parameters originates.

```js
function pick(object, ...keys) {
    let result = Object.create(null);

    for (let i = 0, len = keys.length; i < len; i++) {
        result[keys[i]] = object[keys[i]];
    }

    return result;
}
```

**Rest Parameter Restrictions**

There are two restrictions on rest parameters. The first restriction is that there can be only one rest parameter, and the rest parameter must be last.

```js
// Syntax error: Can't have a named parameter after rest parameters
function pick(object, ...keys, last) {
    let result = Object.create(null);

    for (let i = 0, len = keys.length; i < len; i++) {
        result[keys[i]] = object[keys[i]];
    }

    return result;
}
```

The second restriction is that rest parameters cannot be used in an object literal setter. That means this code would also cause a syntax error:

```js
let object = {
    // Syntax error: Can't use rest param in setter
    set name(...value) {
        // do something
    }
};
```

This restriction exists because object literal setters are restricted to a single argument. Rest parameters are, by definition, an infinite number of arguments, so they’re not allowed in this context.

**How Rest Parameters Affect the arguments Object**

Rest parameters were designed to replace `arguments` in ECMAScript. Originally, ECMAScript 4 did away with `arguments` and added rest parameters to allow an unlimited number of `arguments` to be passed to functions. ECMAScript 4 never came into being, but this idea was kept around and reintroduced in ECMAScript 6, despite `arguments` not being removed from the language.

The `arguments` object works together with rest parameters by reflecting the arguments that were passed to the function when called, as illustrated in this program:

```js
function checkArgs(...args) {
    console.log(args.length);
    console.log(arguments.length);
    console.log(args[0], arguments[0]);
    console.log(args[1], arguments[1]);
}

checkArgs("a", "b");
```

The call to `checkArgs()` outputs:

```
2
2
a a
b b
```

The `arguments` object always correctly reflects the parameters that were passed into a function regardless of rest parameter usage.

### The Spread Operator

Closely related to rest parameters is the spread operator. While rest parameters allow you to specify that multiple independent arguments should be combined into an array, the spread operator allows you to specify an array that should be split and have its items passed in as separate arguments to a function.

```js
let value1 = 25,
    value2 = 50;

console.log(Math.max(value1, value2));      // 50
```

```js
let values = [25, 50, 75, 100]

console.log(Math.max.apply(Math, values));  // 100
```

This solution works, but using apply() in this manner is a bit confusing. It actually seems to obfuscate the true meaning of the code with additional syntax.

The ECMAScript 6 spread operator makes this case very simple. Instead of calling apply(), you can pass the array to Math.max() directly and prefix it with the same `...` pattern used with rest parameters. The JavaScript engine then splits the array into individual arguments and passes them in, like this:

```js
let values = [25, 50, 75, 100]

// equivalent to
// console.log(Math.max(25, 50, 75, 100));
console.log(Math.max(...values));           // 100

let smallNumbers = [1, 2, 3],
    bigNumbers = [100, 101, 102],
    allNumbers = [0, ...smallNumbers, ...bigNumbers];

console.log(allNumbers.length);     // 7
console.log(allNumbers);    // [0, 1, 2, 3, 100, 101, 102]
```

The spread operator for argument passing makes using arrays for function arguments much easier. You’ll likely find it to be a suitable replacement for the `apply()` method in most circumstances.

In addition to the uses you’ve seen for default and rest parameters so far, in ECMAScript 6, you can also apply both parameter types to JavaScript’s `Function` constructor.

### Clarifying the Dual Purpose of Functions

In ECMAScript 5 and earlier, functions serve the dual purpose of being callable with or without `new`. When used with `new`, the `this` value inside a function is a new object and that new object is returned.

```js
function Person(name) {
    this.name = name;
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");

console.log(person);        // "[Object object]"
console.log(notAPerson);    // "undefined"
```

JavaScript has two different internal-only methods for functions: `[[Call]]` and `[[Construct]].` When a function is called without new, the `[[Call]]` method is executed, which executes the body of the function as it appears in the code. When a function is called with new, that’s when the `[[Construct]]` method is called. The `[[Construct]]` method is responsible for creating a new object, called the new target, and then executing the function body with this set to the new target. Functions that have a `[[Construct]]` method are called constructors.

#### Determining How a Function was Called in ECMAScript 5

The most popular way to determine if a function was called with `new` (and hence, with constructor) in ECMAScript 5 is to use `instanceof`.

```js
function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");  // throws error
```

Here, the `this` value is checked to see if it’s an instance of the constructor, and if so, execution continues as normal. If `this` isn’t an instance of `Person`, then an error is thrown. This works because the `[[Construct]]` method creates a new instance of `Person` and assigns it to `this`. Unfortunately, this approach is not completely reliable because `this` can be an instance of `Person` without using `new`, as in this example:

```js
function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // works!
```

The call to `Person.call()` passes the `person` variable as the first argument, which means `this` is set to `person` inside of the `Person` function. To the function, there’s no way to distinguish this from being called with `new`.

#### The new.target MetaProperty

To solve this problem, ECMAScript 6 introduces the `new.target` *metaproperty*. A metaproperty is a property of a non-object that provides additional information related to its target (such as `new`). When a function’s `[[Construct]]` method is called, `new.target` is filled with the target of the `new` operator. That target is typically the constructor of the newly created object instance that will become `this` inside the function body. If `[[Call]]` is executed, then `new.target` is `undefined`.

This new metaproperty allows you to safely detect if a function is called with new by checking whether `new.target` is defined as follows:

```js
function Person(name) {
    if (typeof new.target !== "undefined") {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // error!
```

By using `new.target` instead of `this instanceof Person`, the `Person` constructor is now correctly throwing an error when used without `new`.

You can also check that `new.target` was called with a specific constructor.

```js
function Person(name) {
    if (new.target === Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

function AnotherPerson(name) {
    Person.call(this, name);
}

var person = new Person("Nicholas");
var anotherPerson = new AnotherPerson("Nicholas");  // error!
```

In this code, `new.target` must be `Person` in order to work correctly. When `new AnotherPerson("Nicholas")` is called, the subsequent call to `Person.call(this, name)` will throw an error because `new.target` is `undefined` inside of the `Person` constructor (it was called without `new`).

### Block-Level Functions

In an attempt to rein in this incompatible behavior, ECMAScript 5 strict mode introduced an error whenever a function declaration was used inside of a block in this way:

```js
"use strict";

if (true) {
    // Throws a syntax error in ES5, not so in ES6
    function doSomething() {
        // ...
    }
}
```

In ECMAScript 5, this code throws a syntax error. In ECMAScript 6, the `doSomething()` function is considered a block-level declaration and can be accessed and called within the same block in which it was defined.

```js
"use strict";

if (true) {
    console.log(typeof doSomething);        // "function"

    function doSomething() {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);            // "undefined"
```

**Block level functions are hoisted to the top of the block in which they are defined**

#### Deciding When to Use Block-Level Functions

Block level functions are similar to `let` function expressions in that the function definition is removed once execution flows out of the block in which it’s defined. The key difference is that block level functions are hoisted to the top of the containing block. Function expressions that use `let` are not hoisted, as this example illustrates:

```js
"use strict";

if (true) {

    console.log(typeof doSomething);        // throws error

    let doSomething = function () {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);
```

#### Block-Level Functions in Nonstrict Mode

ECMAScript 6 also allows block-level functions in nonstrict mode, but the behavior is slightly different. Instead of hoisting these declarations to the top of the block, they are hoisted all the way to the containing function or global environment.

```js
// ECMAScript 6 behavior
if (true) {

    console.log(typeof doSomething);        // "function"

    function doSomething() {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);            // "function"
```

Allowing block-level functions improves your ability to declare functions in JavaScript, but ECMAScript 6 also introduced a completely new way to declare functions.

#### Arrow Functions

One of the most interesting new parts of ECMAScript 6 is the arrow function. Arrow functions are, as the name suggests, functions defined with a new syntax that uses an “arrow” (`=>`). But arrow functions behave differently than traditional JavaScript functions in a number of important ways:

* **No this, super, arguments, and new.target bindings** - The value of `this`, `super`, `arguments`, and `new.target` inside of the function is by the closest containing nonarrow function.

* **Cannot be called with new** - Arrow functions do not have a `[[Construct]]` method and therefore cannot be used as constructors. Arrow functions throw an error when used with `new`.

* **No prototype** - since you can’t use `new` on an arrow function, there’s no need for a prototype. The prototype property of an arrow function doesn’t exist.

* **Can’t change this** - The value of `this` inside of the function can’t be changed. It remains the same throughout the entire lifecycle of the function.

* **No arguments object** - Since arrow functions have no `arguments` binding, you must rely on named and rest parameters to access function arguments.

* **No duplicate named parameters** - arrow functions cannot have duplicate named parameters in strict or nonstrict mode, as opposed to nonarrow functions that cannot have duplicate named parameters only in strict mode.

##### Arrow Function Syntax

The syntax for arrow functions comes in many flavors depending upon what you’re trying to accomplish. All variations begin with function arguments, followed by the arrow, followed by the body of the function. Both the arguments and the body can take different forms depending on usage. For example, the following arrow function takes a single argument and simply returns it:

```js
var reflect = value => value;

// effectively equivalent to:

var reflect = function(value) {
    return value;
};
```

When there is only one argument for an arrow function, that one argument can be used directly without any further syntax. The arrow comes next and the expression to the right of the arrow is evaluated and returned. Even though there is no explicit `return` statement, this arrow function will return the first argument that is passed in.

If you are passing in more than one argument, then you must include parentheses around those arguments, like this:

```js
var sum = (num1, num2) => num1 + num2;

// effectively equivalent to:

var sum = function(num1, num2) {
    return num1 + num2;
};
```

If there are no arguments to the function, then you must include an empty set of parentheses in the declaration, as follows:

```js
var getName = () => "Nicholas";

// effectively equivalent to:

var getName = function() {
    return "Nicholas";
};
```

When you want to provide a more traditional function body, perhaps consisting of more than one expression, then you need to wrap the function body in braces and explicitly define a return value, as in this version of `sum()`:

```js
var sum = (num1, num2) => {
    return num1 + num2;
};

// effectively equivalent to:

var sum = function(num1, num2) {
    return num1 + num2;
};
```

If you want to create a function that does nothing, then you need to include curly braces, like this:

```js
var doNothing = () => {};

// effectively equivalent to:

var doNothing = function() {};
```

Curly braces are used to denote the function’s body, which works just fine in the cases you’ve seen so far. But an arrow function that wants to return an object literal outside of a function body must wrap the literal in parentheses. For example:

```js
var getTempItem = id => ({ id: id, name: "Temp" });

// effectively equivalent to:

var getTempItem = function(id) {

    return {
        id: id,
        name: "Temp"
    };
};
```

Wrapping the object literal in parentheses signals that the braces are an object literal instead of the function body.

##### Creating Immediately-Invoked Function Expressions

```js
let person = function(name) {

    return {
        getName: function() {
            return name;
        }
    };

}("Nicholas");

console.log(person.getName());      // "Nicholas"
```

In this code, the IIFE is used to create an object with a `getName()` method. The method uses the name argument as the return value, effectively making `name` a private member of the returned object.

You can accomplish the same thing using arrow functions, so long as you wrap the arrow function in parentheses:

```js
let person = ((name) => {

    return {
        getName: function() {
            return name;
        }
    };

})("Nicholas");

console.log(person.getName());      // "Nicholas"
```

Note that the parentheses are only around the arrow function definition, and not around `("Nicholas")`. This is different from a formal function, where the parentheses can be placed outside of the passed-in parameters as well as just around the function definition.

##### No this Binding

One of the most common areas of error in JavaScript is the binding of `this` inside of functions. Since the value of `this` can change inside a single function depending on the context in which the function is called, it’s possible to mistakenly affect one object when you meant to affect another. Consider the following example:

```js
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", (function(event) {
            this.doSomething(event.type);     // no error
        }).bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

Arrow functions have no `this` binding, which means the value of `this` inside an arrow function can only be determined by looking up the scope chain. If the arrow function is contained within a nonarrow function, `this` will be the same as the containing function; otherwise, `this` is equivalent to the value of `this` in the global scope. Here’s one way you could write this code using an arrow function:

```js
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
                event => this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

The event handler in this example is an arrow function that calls `this.doSomething()`. The value of this is the same as it is within `init()`, so this version of the code works similarly to the one using `bind(this)`. Even though the `doSomething()` method doesn’t return a value, it’s still the only statement executed in the function body, and so there is no need to include braces.

Arrow functions are designed to be “throwaway” functions, and so cannot be used to define new types; this is evident from the missing `prototype` property, which regular functions have. If you try to use the `new` operator with an arrow function, you’ll get an error, as in this example:

```js
var MyType = () => {},
    object = new MyType();  // error - you can't use arrow functions with 'new'
```

Also, since the `this` value is determined by the containing function in which the arrow function is defined, you cannot change the value of `this` using `call()`, `apply()`, or `bind()`.

Also like other functions, you can still use `call()`, `apply()`, and `bind()` on arrow functions, although the this-binding of the function will not be affected. Here are some examples:

```js
var sum = (num1, num2) => num1 + num2;

console.log(sum.call(null, 1, 2));      // 3
console.log(sum.apply(null, [1, 2]));   // 3

var boundSum = sum.bind(null, 1, 2);

console.log(boundSum());                // 3
```

## Expanded Object Functionality

ECMAScript 6 focuses heavily on improving the utility of objects, which makes sense because nearly every value in JavaScript is some type of object. Additionally, the number of objects used in an average JavaScript program continues to increase as the complexity of JavaScript applications increases, meaning that programs are creating more objects all the time. With more objects comes the necessity to use them more effectively.

### Object Literal Syntax Extensions

The object literal is one of the most popular patterns in JavaScript. JSON is built upon its syntax, and it’s in nearly every JavaScript file on the Internet. The object literal is so popular because it’s a succinct syntax for creating objects that otherwise would take several lines of code. Luckily for developers, ECMAScript 6 makes object literals more powerful and even more succinct by extending the syntax in several ways.

#### Property Initializer Shorthand

In ECMAScript 5 and earlier, object literals were simply collections of name-value pairs. That meant there could be some duplication when property values are initialized. For example:

```js
function createPerson(name, age) {
    return {
        name: name,
        age: age
    };
}
```

In ECMAScript 6, you can eliminate the duplication that exists around property names and local variables by using the *property initializer* shorthand. When an object property name is the same as the local variable name, you can simply include the name without a colon and value.

```js
function createPerson(name, age) {
    return {
        name,
        age
    };
}
```

#### Concise Methods

ECMAScript 6 also improves the syntax for assigning methods to object literals. In ECMAScript 5 and earlier, you must specify a name and then the full function definition to add a method to an object, as follows:

```js
var person = {
    name: "Nicholas",
    sayName: function() {
        console.log(this.name);
    }
};
```

In ECMAScript 6, the syntax is made more concise by eliminating the colon and the `function` keyword. That means you can rewrite the previous example like this:

```js
var person = {
    name: "Nicholas",
    sayName() {
        console.log(this.name);
    }
};
```

#### Computed Property Names

ECMAScript 5 and earlier could compute property names on object instances when those properties were set with square brackets instead of dot notation. The square brackets allow you to specify property names using variables and string literals that may contain characters that would cause a syntax error if used in an identifier. Here’s an example:

```js
var person = {},
    lastName = "last name";

person["first name"] = "Nicholas";
person[lastName] = "Zakas";

console.log(person["first name"]);      // "Nicholas"
console.log(person[lastName]);          // "Zakas"
```

Additionally, you can use string literals directly as property names in object literals, like this:

```js
var person = {
    "first name": "Nicholas"
};

console.log(person["first name"]);      // "Nicholas"
```

In ECMAScript 6, computed property names are part of the object literal syntax, and they use the same square bracket notation that has been used to reference computed property names in object instances. For example:

```js
var lastName = "last name";

var person = {
    "first name": "Nicholas",
    [lastName]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person[lastName]);          // "Zakas"
```

The square brackets inside the object literal indicate that the property name is computed, so its contents are evaluated as a string. That means you can also include expressions such as:

```js
var suffix = " name";

var person = {
    ["first" + suffix]: "Nicholas",
    ["last" + suffix]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person["last name"]);       // "Zakas"
```

### New Methods

One of the design goals of ECMAScript beginning with ECMAScript 5 was to avoid creating new global functions or methods on `Object.prototype`, and instead try to find objects on which new methods should be available. As a result, the `Object` global has received an increasing number of methods when no other objects are more appropriate. ECMAScript 6 introduces a couple new methods on the `Object` global that are designed to make certain tasks easier.

#### The `Object.is()` Method

When you want to compare two values in JavaScript, you’re probably used to using either the equals operator (`==`) or the identically equals operator (`===`). Many developers prefer the latter, to avoid type coercion during comparison. But even the identically equals operator isn’t entirely accurate.

ECMAScript 6 introduces the `Object.is()` method to make up for the remaining quirks of the identically equals operator. This method accepts two arguments and returns `true` if the values are equivalent. Two values are considered equivalent when they are of the same type and have the same value.

```js
console.log(+0 == -0);              // true
console.log(+0 === -0);             // true
console.log(Object.is(+0, -0));     // false

console.log(NaN == NaN);            // false
console.log(NaN === NaN);           // false
console.log(Object.is(NaN, NaN));   // true

console.log(5 == 5);                // true
console.log(5 == "5");              // true
console.log(5 === 5);               // true
console.log(5 === "5");             // false
console.log(Object.is(5, 5));       // true
console.log(Object.is(5, "5"));     // false
```

In many cases, `Object.is()` works the same as the `===` operator. Choose whether to use `Object.is()` instead of `==` or `===` based on how those special cases affect your code.

#### The `Object.assign()` Method

*Mixins* are among the most popular patterns for object composition in JavaScript. In a mixin, one object receives properties and methods from another object. Many JavaScript libraries have a mixin method similar to this:

```js
function mixin(receiver, supplier) {
    Object.keys(supplier).forEach(function(key) {
        receiver[key] = supplier[key];
    });

    return receiver;
}
```

```js
var receiver = {};

Object.assign(receiver,
    {
        type: "js",
        name: "file.js"
    },
    {
        type: "css"
    }
);

console.log(receiver.type);     // "css"
console.log(receiver.name);     // "file.js"
```

### Duplicate Object Literal Properties

ECMAScript 5 strict mode introduced a check for duplicate object literal properties that would throw an error if a duplicate was found. For example, this code was problematic:

```js
"use strict";

var person = {
    name: "Nicholas",
    name: "Greg"        // syntax error in ES5 strict mode
};
```

But in ECMAScript 6, the duplicate property check was removed. Both strict and nonstrict mode code no longer check for duplicate properties. Instead, the last property of the given name becomes the property’s actual value, as shown here:

```js
"use strict";

var person = {
    name: "Nicholas",
    name: "Greg"        // no error in ES6 strict mode
};

console.log(person.name);       // "Greg"
```

### Own Property Enumeration Order

ECMAScript 5 didn’t define the enumeration order of object properties, as it left this up to the JavaScript engine vendors. However, ECMAScript 6 strictly defines the order in which own properties must be returned when they are enumerated. This affects how properties are returned using `Object.getOwnPropertyNames()` and `Reflect.ownKeys`. It also affects the order in which properties are processed by `Object.assign()`.

The basic order for own property enumeration is:

1. All numeric keys in ascending order
2. All string keys in the order in which they were added to the object
3. All symbol keys in the order in which they were added to the object

```js
var obj = {
    a: 1,
    0: 1,
    c: 1,
    2: 1,
    b: 1,
    1: 1
};

obj.d = 1;

console.log(Object.getOwnPropertyNames(obj).join(""));     // "012acbd"
```

### More Powerful Prototypes

Prototypes are the foundation of inheritance in JavaScript, and ECMAScript 6 continues to make prototypes more powerful. Early versions of JavaScript severely limited what could be done with prototypes. However, as the language matured and developers became more familiar with how prototypes work, it became clear that developers wanted more control over prototypes and easier ways to work with them. As a result, ECMAScript 6 introduced some improvements to prototypes.

#### Changing an Object’s Prototype

Normally, the prototype of an object is specified when the object is created, via either a constructor or the `Object.create()` method. The idea that an object’s prototype remains unchanged after instantiation was one of the biggest assumptions in JavaScript programming through ECMAScript 5. ECMAScript 5 did add the `Object.getPrototypeOf()` method for retrieving the prototype of any given object, but it still lacked a standard way to change an object’s prototype after instantiation.

ECMAScript 6 changes that assumption by adding the `Object.setPrototypeOf()` method, which allows you to change the prototype of any given object. The `Object.setPrototypeOf()` method accepts two arguments: the object whose prototype should change and the object that should become the first argument’s prototype. For example:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

let dog = {
    getGreeting() {
        return "Woof";
    }
};

// prototype is person
let friend = Object.create(person);
console.log(friend.getGreeting());                      // "Hello"
console.log(Object.getPrototypeOf(friend) === person);  // true

// set prototype to dog
Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting());                      // "Woof"
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

The actual value of an object’s prototype is stored in an internal-only property called `[[Prototype]]`. The `Object.getPrototypeOf()` method returns the value stored in `[[Prototype]`] and `Object.setPrototypeOf()` changes the value stored in `[[Prototype]]`. However, these aren’t the only ways to work with the value of `[[Prototype]]`.

#### Easy Prototype Access with Super References

As previously mentioned, prototypes are very important for JavaScript and a lot of work went into making them easier to use in ECMAScript 6. Another improvement is the introduction of `super` references, which make accessing functionality on an object’s prototype easier. For example, to override a method on an object instance such that it also calls the prototype method of the same name, you’d do the following:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

let dog = {
    getGreeting() {
        return "Woof";
    }
};

let friend = {
    getGreeting() {
        return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    }
};

// set prototype to person
Object.setPrototypeOf(friend, person);
console.log(friend.getGreeting());                      // "Hello, hi!"
console.log(Object.getPrototypeOf(friend) === person);  // true

// set prototype to dog
Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting());                      // "Woof, hi!"
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

Remembering to use `Object.getPrototypeOf()` and `.call(this)` to call a method on the prototype is a bit involved, so ECMAScript 6 introduced `super`. At its simplest, `super` is a pointer to the current object’s prototype, effectively the `Object.getPrototypeOf(this)` value. Knowing that, you can simplify the `getGreeting()` method as follows:

```js
let friend = {
    getGreeting() {
        // in the previous example, this is the same as:
        // Object.getPrototypeOf(this).getGreeting.call(this)
        return super.getGreeting() + ", hi!";
    }
};
```

The call to `super.getGreeting()` is the same as `Object.getPrototypeOf(this).getGreeting.call(this)` in this context. Similarly, you can call any method on an object’s prototype by using a super reference, so long as it’s inside a concise method. Attempting to use `super` outside of concise methods results in a syntax error, as in this example:

```js
let friend = {
    getGreeting: function() {
        // syntax error
        return super.getGreeting() + ", hi!";
    }
};
```

This example uses a named property with a function, and the call to `super.getGreeting()` results in a syntax error because super is invalid in this context.

The `super` reference is really powerful when you have multiple levels of inheritance, because in that case, `Object.getPrototypeOf()` no longer works in all circumstances. For example:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    getGreeting() {
        return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    }
};
Object.setPrototypeOf(friend, person);


// prototype is friend
let relative = Object.create(friend);

console.log(person.getGreeting());                  // "Hello"
console.log(friend.getGreeting());                  // "Hello, hi!"
console.log(relative.getGreeting());                // error!
```

The call to `Object.getPrototypeOf()` results in an error when `relative.getGreeting()` is called. That’s because `this` is `relative`, and the prototype of `relative` is the `friend` object. When `friend.getGreeting().call()` is called with `relative` as `this`, the process starts over again and continues to call recursively until a stack overflow error occurs.

That problem is difficult to solve in ECMAScript 5, but with ECMAScript 6 and `super`, it’s easy:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    getGreeting() {
        return super.getGreeting() + ", hi!";
    }
};
Object.setPrototypeOf(friend, person);

// prototype is friend
let relative = Object.create(friend);

console.log(person.getGreeting());                  // "Hello"
console.log(friend.getGreeting());                  // "Hello, hi!"
console.log(relative.getGreeting());                // "Hello, hi!"
```

### A Formal Method Definition

Prior to ECMAScript 6, the concept of a “method” wasn’t formally defined. Methods were just object properties that contained functions instead of data. ECMAScript 6 formally defines a method as a function that has an internal `[[HomeObject]]` property containing the object to which the method belongs. Consider the following:

```js
let person = {

    // method
    getGreeting() {
        return "Hello";
    }
};

// not a method
function shareGreeting() {
    return "Hi!";
}
```

Any reference to `super` uses the `[[HomeObject]]` to determine what to do. The first step is to call `Object.getPrototypeOf()` on the `[[HomeObject]]` to retrieve a reference to the prototype. Then, the prototype is searched for a function with the same name. Last, the `this` binding is set and the method is called. Here’s an example:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    getGreeting() {
        return super.getGreeting() + ", hi!";
    }
};
Object.setPrototypeOf(friend, person);

console.log(friend.getGreeting());  // "Hello, hi!"
```

Calling `friend.getGreeting()` returns a string, which combines the value from `person.getGreeting()` with `", hi!"`. The `[[HomeObject]]` of `friend.getGreeting()` is `friend`, and the prototype of `friend` is `person`, so `super.getGreeting()` is equivalent to `person.getGreeting.call(this)`.

## Destructuring for Easier Data Access

Object and array literals are two of the most frequently used notations in JavaScript, and thanks to the popular JSON data format, they’ve become a particularly important part of the language. It’s quite common to define objects and arrays, and then systematically pull out relevant pieces of information from those structures. ECMAScript 6 simplifies this task by adding destructuring, which is the process of breaking a data structure down into smaller parts. This chapter shows you how to harness destructuring for both objects and arrays.

### Why is Destructuring Useful?

In ECMAScript 5 and earlier, the need to fetch information from objects and arrays could lead to a lot of code that looks the same, just to get certain data into local variables.

```js
let options = {
        repeat: true,
        save: false
    };

// extract data from the object
let repeat = options.repeat,
    save = options.save;
```

That’s why ECMAScript 6 adds destructuring for both objects and arrays. When you break a data structure into smaller parts, getting the information you need out of it becomes much easier. Many languages implement destructuring with a minimal amount of syntax to make the process simpler to use. The ECMAScript 6 implementation actually makes use of syntax you’re already familiar with: the syntax for object and array literals.

### Object Destructuring

Object destructuring syntax uses an object literal on the left side of an assignment operation.

```js
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name } = node;

console.log(type);      // "Identifier"
console.log(name);      // "foo"
```

**Don’t Forget the Initializer**

When using destructuring to declare variables using `var`, `let`, or `const`, you must supply an initializer (the value after the equals sign). The following lines of code will all throw syntax errors due to a missing initializer:

```js
// syntax error!
var { type, name };

// syntax error!
let { type, name };

// syntax error!
const { type, name };
```

While `const` always requires an initializer, even when using nondestructured variables, `var` and `let` only require initializers when using destructuring.

**Destructuring Assignment**

The object destructuring examples so far have used variable declarations. However, it’s also possible to use destructuring in assignments. For instance, you may decide to change the values of variables after they are defined.

```js
let node = {
        type: "Identifier",
        name: "foo"
    },
    type = "Literal",
    name = 5;

// assign different values using destructuring
({ type, name } = node);

console.log(type);      // "Identifier"
console.log(name);      // "foo"
```

A destructuring assignment expression evaluates to the right side of the expression (after the `=`). That means you can use a destructuring assignment expression anywhere a value is expected. For instance, passing a value to a function:

```js
let node = {
        type: "Identifier",
        name: "foo"
    },
    type = "Literal",
    name = 5;

function outputInfo(value) {
    console.log(value === node);        // true
}

outputInfo({ type, name } = node);

console.log(type);      // "Identifier"
console.log(name);      // "foo"
```

**Default Values**

When you use a destructuring assignment statement, if you specify a local variable with a property name that doesn’t exist on the object, then that local variable is assigned a value of `undefined`.

```js
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name, value } = node;

console.log(type);      // "Identifier"
console.log(name);      // "foo"
console.log(value);     // undefined
```

You can optionally define a default value to use when a specified property doesn’t exist. To do so, insert an equals sign (`=`) after the property name and specify the default value, like this:

```js
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name, value = true } = node;

console.log(type);      // "Identifier"
console.log(name);      // "foo"
console.log(value);     // true
```

**Assigning to Different Local Variable Names**

ECMAScript 6 has an extended syntax that allows you to assign to a local variable with a different name, and that syntax looks like the object literal nonshorthand property initializer syntax. Here’s an example:

```js
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type: localType, name: localName } = node;

console.log(localType);     // "Identifier"
console.log(localName);     // "foo"
```

You can add default values when using a different variable name, as well. The equals sign and default value are still placed after the local variable name. For example:

```js
let node = {
        type: "Identifier"
    };

let { type: localType, name: localName = "bar" } = node;

console.log(localType);     // "Identifier"
console.log(localName);     // "bar"
```

**Nested Object Destructuring**

By using a syntax similar to object literals, you can navigate into a nested object structure to retrieve just the information you want. Here’s an example:

```js
let node = {
        type: "Identifier",
        name: "foo",
        loc: {
            start: {
                line: 1,
                column: 1
            },
            end: {
                line: 1,
                column: 4
            }
        }
    };

let { loc: { start }} = node;

console.log(start.line);        // 1
console.log(start.column);      // 1
```

You can go one step further and use a different name for the local variable as well:

```js
let node = {
        type: "Identifier",
        name: "foo",
        loc: {
            start: {
                line: 1,
                column: 1
            },
            end: {
                line: 1,
                column: 4
            }
        }
    };

// extract node.loc.start
let { loc: { start: localStart }} = node;

console.log(localStart.line);   // 1
console.log(localStart.column); // 1
```

Destructuring patterns can be nested to an arbitrary level of depth, with all capabilities available at each level.

Object destructuring is very powerful and has a lot of options, but array destructuring offers some unique capabilities that allow you to extract information from arrays.

### Array Destructuring

Array destructuring syntax is very similar to object destructuring; it just uses array literal syntax instead of object literal syntax. The destructuring operates on positions within an array, rather than the named properties that are available in objects. For example:

```js
let colors = [ "red", "green", "blue" ];

let [ firstColor, secondColor ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

You can also omit items in the destructuring pattern and only provide variable names for the items you’re interested in. If, for example, you just want the third value of an array, you don’t need to supply variable names for the first and second items. Here’s how that works:

```js
let colors = [ "red", "green", "blue" ];

let [ , , thirdColor ] = colors;

console.log(thirdColor);        // "blue"
```

*Similar to object destructuring, you must always provide an initializer when using array destructuring with `var`, `let`, or `const`*.

**Destructuring Assignment**

You can use array destructuring in the context of an assignment, but unlike object destructuring, there is no need to wrap the expression in parentheses. For example:

```js
let colors = [ "red", "green", "blue" ],
    firstColor = "black",
    secondColor = "purple";

[ firstColor, secondColor ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

The destructured assignment in this code works in a similar manner to the last array destructuring example. The only difference is that `firstColor` and `secondColor` have already been defined. Most of the time, that’s probably all you’ll need to know about array destructuring assignment, but there’s a little bit more to it that you will probably find useful.

Array destructuring assignment has a very unique use case that makes it easier to swap the values of two variables. Value swapping is a common operation in sorting algorithms, and the ECMAScript 5 way of swapping variables involves a third, temporary variable, as in this example:

```js
// Swapping variables in ECMAScript 5
let a = 1,
    b = 2,
    tmp;

tmp = a;
a = b;
b = tmp;

console.log(a);     // 2
console.log(b);     // 1
```

The intermediate variable tmp is necessary in order to swap the values of a and b. Using array destructuring assignment, however, there’s no need for that extra variable. Here’s how you can swap variables in ECMAScript 6:

```js
// Swapping variables in ECMAScript 6
let a = 1,
    b = 2;

[ a, b ] = [ b, a ];

console.log(a);     // 2
console.log(b);     // 1
```

*Like object destructuring assignment, an error is thrown when the right side of an array destructured assignment expression evaluates to `null` or `undefined`*.

**Default Values**

Array destructuring assignment allows you to specify a default value for any position in the array, too. The default value is used when the property at the given position either doesn’t exist or has the value `undefined`.

```js
let colors = [ "red" ];

let [ firstColor, secondColor = "green" ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

**Nested Destructuring**

You can destructure nested arrays in a manner similar to destructuring nested objects. By inserting another array pattern into the overall pattern, the destructuring will descend into a nested array, like this:

```js
let colors = [ "red", [ "green", "lightgreen" ], "blue" ];

// later

let [ firstColor, [ secondColor ] ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

**Rest Items**

Introduced rest parameters for functions, and array destructuring has a similar concept called *rest items*. Rest items use the `...` syntax to assign the remaining items in an array to a particular variable. Here’s an example:

```js
let colors = [ "red", "green", "blue" ];

let [ firstColor, ...restColors ] = colors;

console.log(firstColor);        // "red"
console.log(restColors.length); // 2
console.log(restColors[0]);     // "green"
console.log(restColors[1]);     // "blue"
```

```js
// cloning an array in ECMAScript 6
let colors = [ "red", "green", "blue" ];
let [ ...clonedColors ] = colors;

console.log(clonedColors);      //"[red,green,blue]"
```

*Rest items must be the last entry in the destructured array and cannot be followed by a comma. Including a comma after rest items is a syntax error*.

### Mixed Destructuring

Object and array destructuring can be used together to create more complex expressions. In doing so, you are able to extract just the pieces of information you want from any mixture of objects and arrays. For example:

```js
let node = {
        type: "Identifier",
        name: "foo",
        loc: {
            start: {
                line: 1,
                column: 1
            },
            end: {
                line: 1,
                column: 4
            }
        },
        range: [0, 3]
    };

let {
    loc: { start },
    range: [ startIndex ]
} = node;

console.log(start.line);        // 1
console.log(start.column);      // 1
console.log(startIndex);        // 0
```

### Destructured Parameters

Destructuring has one more particularly helpful use case, and that is when passing function arguments. When a JavaScript function takes a large number of optional parameters, one common pattern is to create an `options` object whose properties specify the additional parameters, like this:

```js
// properties on options represent additional parameters
function setCookie(name, value, options) {

    options = options || {};

    let secure = options.secure,
        path = options.path,
        domain = options.domain,
        expires = options.expires;

    // code to set the cookie
}

// third argument maps to options
setCookie("type", "js", {
    secure: true,
    expires: 60000
});
```

Destructured parameters offer an alternative that makes it clearer what arguments a function expects. A destructured parameter uses an object or array destructuring pattern in place of a named parameter. To see this in action, look at this rewritten version of the `setCookie()` function from the last example:

```js
function setCookie(name, value, { secure, path, domain, expires }) {
    // code to set the cookie
}

setCookie("type", "js", {
    secure: true,
    expires: 60000
});
```

> Destructured parameters have all of the capabilities of destructuring that you’ve learned so far in this chapter. You can use default values, mix object and array patterns, and use variable names that differ from the properties you’re reading from.

#### Destructured Parameters are Required

```js
// Error!
setCookie("type", "js");
```

The third argument is missing, and so it evaluates to `undefined` as expected. This causes an error because destructured parameters are really just a shorthand for destructured declaration. When the `setCookie()` function is called, the JavaScript engine actually does this:

```js
function setCookie(name, value, options) {
    let { secure, path, domain, expires } = options;
    // code to set the cookie
}
```

Since destructuring throws an error when the right side expression evaluates to `null` or `undefined`, the same is true when the third argument isn’t passed to the `setCookie()` function.

If you want the destructured parameter to be required, then this behavior isn’t all that troubling. But if you want the destructured parameter to be optional, you can work around this behavior by providing a default value for the destructured parameter, like this:

```js
function setCookie(name, value, { secure, path, domain, expires } = {}) {
    // ...
}
```

#### Default Values for Destructured Parameters

You can specify destructured default values for destructured parameters just as you would in destructured assignment. Just add the equals sign after the parameter and specify the default value. For example:

```js
function setCookie(name, value,
    {
        secure = false,
        path = "/",
        domain = "example.com",
        expires = new Date(Date.now() + 360000000)
    } = {}
) {
    // ...
}
```

Each property in the destructured parameter has a default value in this code, so you can avoid checking to see if a given property has been included in order to use the correct value. Also, the entire destructured parameter has a default value of an empty object, making the parameter optional. This does make the function declaration look a bit more complicated than usual, but that’s a small price to pay for ensuring each argument has a usable value.

## Symbols and Symbol Properties

Symbols are a primitive type introduced in ECMAScript 6, joining the existing primitive types: strings, numbers, booleans, `null`, and `undefined`. Symbols began as a way to create private object members, a feature JavaScript developers wanted for a long time. Before symbols, any property with a string name was easy to access regardless of the obscurity of the name, and the “private names” feature was meant to let developers create non-string property names. That way, normal techniques for detecting these private names wouldn’t work.

The private names proposal eventually evolved into ECMAScript 6 symbols, and this chapter will teach you how to use symbols effectively. While the implementation details remained the same (that is, they added non-string values for property names), the goal of privacy was dropped. Instead, symbol properties are categorized separately from other object properties.

### Creating Symbols

Symbols are unique among JavaScript primitives in that they don’t have a literal form, like `true`for booleans or `42` for numbers. You can create a symbol by using the global `Symbol` function, as in this example:

```js
let firstName = Symbol();
let person = {};

person[firstName] = "Allyson";
console.log(person[firstName]);     // "Allyson"
```

Here, the symbol `firstName` is created and used to assign a new property on the `person` object. That symbol must be used each time you want to access that same property. Naming the symbol variable appropriately is a good idea, so you can easily tell what the symbol represents.

*Because symbols are primitive values, calling `new Symbol()` throws an error when called. You can create an instance of `Symbol` via `new Object(yourSymbol)` as well, but it’s unclear when this capability would be useful.*

The `Symbol` function also accepts an optional argument that is the description of the symbol. The description itself cannot be used to access the property, but is used for debugging purposes. For example:

```js
let firstName = Symbol("first name");
let person = {};

person[firstName] = "Allyson";

console.log("first name" in person);        // false
console.log(person[firstName]);             // "Allyson"
console.log(firstName);                     // "Symbol(first name)"
```

A symbol’s description is stored internally in the `[[Description]]` property. This property is read whenever the symbol’s `toString()` method is called either explicitly or implicitly. The `firstName` symbol’s `toString()` method is called implictly by `console.log()` in this example, so the description gets printed to the log. It is not otherwise possible to access `[[Description]]`directly from code. I recommended always providing a description to make both reading and debugging symbols easier.

#### Identifying Symbols

Since symbols are primitive values, you can use the `typeof` operator to determine if a variable contains a symbol. ECMAScript 6 extends `typeof` to return `"symbol"` when used on a symbol. For example:

```js
let symbol = Symbol("test symbol");
console.log(typeof symbol);         // "symbol"
```

While there are other indirect ways of determining whether a variable is a symbol, the `typeof` operator is the most accurate and preferred technique.

### Using Symbols

You can use symbols anywhere you’d use a computed property name. You’ve already seen bracket notation used with symbols in this chapter, but you can use symbols in computed object literal property names as well as with `Object.defineProperty()` and `Object.defineProperties()`calls, such as:

```js
let firstName = Symbol("first name");

// use a computed object literal property
let person = {
    [firstName]: "Allyson"
};

// make the property read only
Object.defineProperty(person, firstName, { writable: false });

let lastName = Symbol("last name");

Object.defineProperties(person, {
    [lastName]: {
        value: "Silva",
        writable: false
    }
});

console.log(person[firstName]);     // "Allyson"
console.log(person[lastName]);      // "Silva"
```

This example first uses a computed object literal property to create the `firstName` symbol property. The following line then sets the property to be read-only. Later, a read-only `lastName`symbol property is created using the `Object.defineProperties()` method. A computed object literal property is used once again, but this time, it’s part of the second argument to the `Object.defineProperties()` call.

While symbols can be used in any place that computed property names are allowed, you’ll need to have a system for sharing these symbols between different pieces of code in order to use them effectively.

### Sharing Symbols

You may find that you want different parts of your code to use the same symbols. For example, suppose you have two different object types in your application that should use the same symbol property to represent a unique identifier. Keeping track of symbols across files or large codebases can be difficult and error-prone. That’s why ECMAScript 6 provides a global symbol registry that you can access at any point in time.

When you want to create a symbol to be shared, use the `Symbol.for()` method instead of calling the `Symbol()` method. The `Symbol.for()` method accepts a single parameter, which is a string identifier for the symbol you want to create. That parameter is also used as the symbol’s description. For example:

```js
let uid = Symbol.for("uid");
let object = {};

object[uid] = "12345";

console.log(object[uid]);       // "12345"
console.log(uid);               // "Symbol(uid)"
```

The `Symbol.for()` method first searches the global symbol registry to see if a symbol with the key `"uid"` exists. If so, the method returns the existing symbol. If no such symbol exists, then a new symbol is created and registered to the global symbol registry using the specified key. The new symbol is then returned. That means subsequent calls to `Symbol.for()` using the same key will return the same symbol, as follows:

```js
let uid = Symbol.for("uid");
let object = {
    [uid]: "12345"
};

console.log(object[uid]);       // "12345"
console.log(uid);               // "Symbol(uid)"

let uid2 = Symbol.for("uid");

console.log(uid === uid2);      // true
console.log(object[uid2]);      // "12345"
console.log(uid2);              // "Symbol(uid)"
```

In this example, `uid` and `uid2` contain the same symbol and so they can be used interchangeably. The first call to `Symbol.for()` creates the symbol, and the second call retrieves the symbol from the global symbol registry.

Another unique aspect of shared symbols is that you can retrieve the key associated with a symbol in the global symbol registry by calling the `Symbol.keyFor()` method. For example:

```js
let uid = Symbol.for("uid");
console.log(Symbol.keyFor(uid));    // "uid"

let uid2 = Symbol.for("uid");
console.log(Symbol.keyFor(uid2));   // "uid"

let uid3 = Symbol("uid");
console.log(Symbol.keyFor(uid3));   // undefined
```

Notice that both `uid` and `uid2` return the `"uid"` key. The symbol `uid3` doesn’t exist in the global symbol registry, so it has no key associated with it and `Symbol.keyFor()` returns `undefined`.

*The global symbol registry is a shared environment, just like the global scope. That means you can’t make assumptions about what is or is not already present in that environment. Use namespacing of symbol keys to reduce the likelihood of naming collisions when using third-party components. For example, jQuery code might use `"jquery."` to prefix all keys, for keys like `"jquery.element"` or similar.*

### Symbol Coercion

Type coercion is a significant part of JavaScript, and there’s a lot of flexibility in the language’s ability to coerce one data type into another. Symbols, however, are quite inflexible when it comes to coercion because other types lack a logical equivalent to a symbol. Specifically, symbols cannot be coerced into strings or numbers so that they cannot accidentally be used as properties that would otherwise be expected to behave as symbols.

The examples in this chapter have used `console.log()` to indicate the output for symbols, and that works because `console.log()` calls `String()` on symbols to create useful output. You can use `String()` directly to get the same result. For instance:

```js
let uid = Symbol.for("uid"),
    desc = String(uid);

console.log(desc);              // "Symbol(uid)"
```

The `String()` function calls `uid.toString()` and the symbol’s string description is returned. If you try to concatenate the symbol directly with a string, however, an error will be thrown:

```js
let uid = Symbol.for("uid"),
    desc = uid + "";            // error!
```

Concatenating `uid` with an empty string requires that `uid` first be coerced into a string. An error is thrown when the coercion is detected, preventing its use in this manner.

Similarly, you cannot coerce a symbol to a number. All mathematical operators cause an error when applied to a symbol. For example:

```js
let uid = Symbol.for("uid"),
    sum = uid / 1;            // error!
```

This example attempts to divide the symbol by 1, which causes an error. Errors are thrown regardless of the mathematical operator used (logical operators do not throw an error because all symbols are considered equivalent to `true`, just like any other non-empty value in JavaScript).

### Retrieving Symbol Properties

The `Object.keys()` and `Object.getOwnPropertyNames()` methods can retrieve all property names in an object. The former method returns all enumerable property names, and the latter returns all properties regardless of enumerability. Neither method returns symbol properties, however, to preserve their ECMAScript 5 functionality. Instead, the `Object.getOwnPropertySymbols()` method was added in ECMAScript 6 to allow you to retrieve property symbols from an object.

The return value of `Object.getOwnPropertySymbols()` is an array of own property symbols. For example:

```js
let uid = Symbol.for("uid");
let object = {
    [uid]: "12345"
};

let symbols = Object.getOwnPropertySymbols(object);

console.log(symbols.length);        // 1
console.log(symbols[0]);            // "Symbol(uid)"
console.log(object[symbols[0]]);    // "12345"
```

In this code, `object` has a single symbol property called `uid`. The array returned from `Object.getOwnPropertySymbols()` is an array containing just that symbol.

All objects start with zero own symbol properties, but objects can inherit symbol properties from their prototypes. ECMAScript 6 predefines several such properties, implemented using what are called well-known symbols.

### Exposing Internal Operations with Well-Known Symbols

A central theme for ECMAScript 5 was exposing and defining some of the “magic” parts of JavaScript, the parts that developers couldn’t emulate at the time. ECMAScript 6 carries on that tradition by exposing even more of the previously internal logic of the language, primarily by using symbol prototype properties to define the basic behavior of certain objects.

ECMAScript 6 has predefined symbols called  _well-known symbols_  that represent common behaviors in JavaScript that were previously considered internal-only operations. Each well-known symbol is represented by a property on the `Symbol` object, such as `Symbol.create`.

The well-known symbols are:

- `Symbol.hasInstance` - A method used by `instanceof` to determine an object’s inheritance.
- `Symbol.isConcatSpreadable` - A Boolean value indicating that `Array.prototype.concat()`should flatten the collection’s elements if the collection is passed as a parameter to `Array.prototype.concat()`.
- `Symbol.iterator` - A method that returns an iterator.
- `Symbol.match` - A method used by `String.prototype.match()` to compare strings.
- `Symbol.replace` - A method used by `String.prototype.replace()` to replace substrings.
- `Symbol.search` - A method used by `String.prototype.search()` to locate substrings.
- `Symbol.species` - The constructor for making derived objects.
- `Symbol.split` - A method used by `String.prototype.split()` to split up strings.
- `Symbol.toPrimitive` - A method that returns a primitive value representation of an object.
- `Symbol.toStringTag` - A string used by `Object.prototype.toString()` to create an object description.
- `Symbol.unscopables` - An object whose properties are the names of object properties that should not be included in a `with` statement.

Some commonly used well-known symbols are discussed in the following sections, while others are discussed throughout the rest of the book to keep them in the correct context.

*Overwriting a method defined with a well-known symbol changes an ordinary object to an exotic object because this changes some internal default behavior. There is no practical impact to your code as a result, it just changes the way the specification describes the object.*

#### The Symbol.hasInstance Property

Every function has a `Symbol.hasInstance` method that determines whether or not a given object is an instance of that function. The method is defined on `Function.prototype` so that all functions inherit the default behavior for the `instanceof` property and the method is nonwritable and nonconfigurable as well as nonenumerable, to ensure it doesn’t get overwritten by mistake.

The `Symbol.hasInstance` method accepts a single argument: the value to check. It returns true if the value passed is an instance of the function. To understand how `Symbol.hasInstance` works, consider the following code:

```js
obj instanceof Array;
```

This code is equivalent to:

```js
Array[Symbol.hasInstance](obj);
```

ECMAScript 6 essentially redefined the `instanceof` operator as shorthand syntax for this method call. And now that there’s a method call involved, you can actually change how `instanceof`works.

For instance, suppose you want to define a function that claims no object as an instance. You can do so by hardcoding the return value of `Symbol.hasInstance` to `false`, such as:

```js
function MyObject() {
    // ...
}

Object.defineProperty(MyObject, Symbol.hasInstance, {
    value: function(v) {
        return false;
    }
});

let obj = new MyObject();

console.log(obj instanceof MyObject);       // false
```

You must use `Object.defineProperty()` to overwrite a nonwritable property, so this example uses that method to overwrite the `Symbol.hasInstance` method with a new function. The new function always returns `false`, so even though `obj` is actually an instance of the `MyObject`class, the `instanceof` operator returns `false` after the `Object.defineProperty()` call.

Of course, you can also inspect the value and decide whether or not a value should be considered an instance based on any arbitrary condition. For instance, maybe numbers with values between 1 and 100 are to be considered instances of a special number type. To achieve that behavior, you might write code like this:

```js
function SpecialNumber() {
    // empty
}

Object.defineProperty(SpecialNumber, Symbol.hasInstance, {
    value: function(v) {
        return (v instanceof Number) && (v >=1 && v <= 100);
    }
});

let two = new Number(2),
    zero = new Number(0);

console.log(two instanceof SpecialNumber);    // true
console.log(zero instanceof SpecialNumber);   // false
```

This code defines a `Symbol.hasInstance` method that returns `true` if the value is an instance of `Number` and also has a value between 1 and 100. Thus, `SpecialNumber` will claim `two` as an instance even though there is no directly defined relationship between the `SpecialNumber`function and the `two` variable. Note that the left operand to `instanceof` must be an object to trigger the `Symbol.hasInstance` call, as nonobjects cause `instanceof` to simply return `false`all the time.

*You can also overwrite the default `Symbol.hasInstance` property for all builtin functions such as the `Date` and `Error` functions. This isn’t recommended, however, as the effects on your code can be unexpected and confusing. It’s a good idea to only overwrite `Symbol.hasInstance` on your own functions and only when necessary.*

#### The Symbol.isConcatSpreadable Symbol

JavaScript arrays have a `concat()` method designed to concatenate two arrays together. Here’s how that method is used:

```js
let colors1 = [ "red", "green" ],
    colors2 = colors1.concat([ "blue", "black" ]);

console.log(colors2.length);    // 4
console.log(colors2);           // ["red","green","blue","black"]
```

This code concatenates a new array to the end of `colors1` and creates `colors2`, a single array with all items from both arrays. However, the `concat()` method can also accept nonarray arguments and, in that case, those arguments are simply added to the end of the array. For example:

```js
let colors1 = [ "red", "green" ],
    colors2 = colors1.concat([ "blue", "black" ], "brown");

console.log(colors2.length);    // 5
console.log(colors2);           // ["red","green","blue","black","brown"]
```

Here, the extra argument `"brown"` is passed to `concat()` and it becomes the fifth item in the `colors2` array. Why is an array argument treated differently than a string argument? The JavaScript specification says that arrays are automatically split into their individual items and all other types are not. Prior to ECMAScript 6, there was no way to adjust this behavior.

The `Symbol.isConcatSpreadable` property is a boolean value indicating that an object has a `length` property and numeric keys, and that its numeric property values should be added individually to the result of a `concat()` call. Unlike other well-known symbols, this symbol property doesn’t appear on any standard objects by default. Instead, the symbol is available as a way to augment how `concat()` works on certain types of objects, effectively short-circuiting the default behavior. You can define any type to behave like arrays do in a `concat()` call, like this:

```js
let collection = {
    0: "Hello",
    1: "world",
    length: 2,
    [Symbol.isConcatSpreadable]: true
};

let messages = [ "Hi" ].concat(collection);

console.log(messages.length);    // 3
console.log(messages);           // ["Hi","Hello","world"]
```

The `collection` object in this example is set up to look like an array: it has a `length` property and two numeric keys. The `Symbol.isConcatSpreadable` property is set to `true` to indicate that the property values should be added as individual items to an array. When `collection` is passed to the `concat()` method, the resulting array has `"Hello"` and `"world"` as separate items after the `"Hi"` element.

You can also set `Symbol.isConcatSpreadable` to `false` on array subclasses to prevent items from being separated by `concat()` calls.

#### The Symbol.match, Symbol.replace, Symbol.search, and Symbol.split Symbols

Strings and regular expressions have always had a close relationship in JavaScript. The string type, in particular, has several methods that accept regular expressions as arguments:

- `match(regex)` - Determines whether the given string matches a regular expression
- `replace(regex, replacement)` - Replaces regular expression matches with a `replacement`
- `search(regex)` - Locates a regular expression match inside the string
- `split(regex)` - Splits a string into an array on a regular expression match

Prior to ECMAScript 6, the way these methods interacted with regular expressions was hidden from developers, leaving no way to mimic regular expressions using developer-defined objects. ECMAScript 6 defines four symbols that correspond to these four methods, effectively outsourcing the native behavior to the `RegExp` builtin object.

The `Symbol.match`, `Symbol.replace`, `Symbol.search`, and `Symbol.split` symbols represent methods on the regular expression argument that should be called on the first argument to the `match()` method, the `replace()` method, the `search()` method, and the `split()` method, respectively. The four symbol properties are defined on `RegExp.prototype` as the default implementation that the string methods should use.

Knowing this, you can create an object to use with the string methods in a way that is similar to regular expressions. To do, you can use the following symbol functions in code:

- `Symbol.match` - A function that accepts a string argument and returns an array of matches, or `null` if no match is found.
- `Symbol.replace` - A function that accepts a string argument and a replacement string, and returns a string.
- `Symbol.search` - A function that accepts a string argument and returns the numeric index of the match, or -1 if no match is found.
- `Symbol.split` - A function that accepts a string argument and returns an array containing pieces of the string split on the match.

The ability to define these properties on an object allows you to create objects that implement pattern matching without regular expressions and use them in methods that expect regular expressions. Here’s an example that shows these symbols in action:

```js
// effectively equivalent to /^.{10}$/
let hasLengthOf10 = {
    [Symbol.match]: function(value) {
        return value.length === 10 ? [value] : null;
    },
    [Symbol.replace]: function(value, replacement) {
        return value.length === 10 ? replacement : value;
    },
    [Symbol.search]: function(value) {
        return value.length === 10 ? 0 : -1;
    },
    [Symbol.split]: function(value) {
        return value.length === 10 ? ["", ""] : [value];
    }
};

let message1 = "Hello world",   // 11 characters
    message2 = "Hello John";    // 10 characters


let match1 = message1.match(hasLengthOf10),
    match2 = message2.match(hasLengthOf10);

console.log(match1);            // null
console.log(match2);            // ["Hello John"]

let replace1 = message1.replace(hasLengthOf10, "Howdy!"),
    replace2 = message2.replace(hasLengthOf10, "Howdy!");

console.log(replace1);          // "Hello world"
console.log(replace2);          // "Howdy!"

let search1 = message1.search(hasLengthOf10),
    search2 = message2.search(hasLengthOf10);

console.log(search1);           // -1
console.log(search2);           // 0

let split1 = message1.split(hasLengthOf10),
    split2 = message2.split(hasLengthOf10);

console.log(split1);            // ["Hello world"]
console.log(split2);            // ["", ""]
```

The `hasLengthOf10` object is intended to work like a regular expression that matches whenever the string length is exactly 10. Each of the four methods on `hasLengthOf10` is implemented using the appropriate symbol, and then the corresponding methods on two strings are called. The first string, `message1`, has 11 characters and so it will not match; the second string, `message2`, has 10 characters and so it will match. Despite not being a regular expression, `hasLengthOf10` is passed to each string method and used correctly due to the additional methods.

While this is a simple example, the ability to perform more complex matches than are currently possible with regular expressions opens up a lot of possibilities for custom pattern matchers.

#### The Symbol.toPrimitive Method

JavaScript frequently attempts to convert objects into primitive values implicitly when certain operations are applied. For instance, when you compare a string to an object using the double equals (`==`) operator, the object is converted into a primitive value before comparing. Exactly what primitive value should be used was previously an internal operation, but ECMAScript 6 exposes that value (making it changeable) through the `Symbol.toPrimitive` method.

The `Symbol.toPrimitive` method is defined on the prototype of each standard type and prescribes what should happen when the object is converted into a primitive. When a primitive conversion is needed, `Symbol.toPrimitive` is called with a single argument, referred to as `hint`in the specification. The `hint` argument is one of three string values. If `hint` is `"number"` then `Symbol.toPrimitive` should return a number. If `hint` is `"string"` then a string should be returned, and if it’s `"default"` then the operation has no preference as to the type.

For most standard objects, number mode has the following behaviors, in order by priority:

1. Call the `valueOf()` method, and if the result is a primitive value, return it.
2. Otherwise, call the `toString()` method, and if the result is a primitive value, return it.
3. Otherwise, throw an error.

Similarly, for most standard objects, the behaviors of string mode have the following priority:

1. Call the `toString()` method, and if the result is a primitive value, return it.
2. Otherwise, call the `valueOf()` method, and if the result is a primitive value, return it.
3. Otherwise, throw an error.

In many cases, standard objects treat default mode as equivalent to number mode (except for `Date`, which treats default mode as equivalent to string mode). By defining an `Symbol.toPrimitive` method, you can override these default coercion behaviors.

*Default mode is only used for the `==` operator, the `+` operator, and when passing a single argument to the `Date` constructor. Most operations use string or number mode.*

To override the default conversion behaviors, use `Symbol.toPrimitive` and assign a function as its value. For example:

```js
function Temperature(degrees) {
    this.degrees = degrees;
}

Temperature.prototype[Symbol.toPrimitive] = function(hint) {

    switch (hint) {
        case "string":
            return this.degrees + "\u00b0"; // degrees symbol

        case "number":
            return this.degrees;

        case "default":
            return this.degrees + " degrees";
    }
};

let freezing = new Temperature(32);

console.log(freezing + "!");            // "32 degrees!"
console.log(freezing / 2);              // 16
console.log(String(freezing));          // "32째"
```

This script defines a `Temperature` constructor and overrides the default `Symbol.toPrimitive`method on the prototype. A different value is returned depending on whether the `hint`argument indicates string, number, or default mode (the `hint` argument is filled in by the JavaScript engine). In string mode, the `Symbol.toPrimitive` method returns the temperature with the Unicode degrees symbol. In number mode, it returns just the numeric value, and in default mode, it appends the word “degrees” after the number.

Each of the log statements triggers a different `hint` argument value. The `+` operator triggers default mode by setting `hint` to `"default"`, the `/` operator triggers number mode by setting `hint` to `"number"`, and the `String()` function triggers string mode by setting `hint` to `"string"`. Returning different values for all three modes is possible, it’s much more common to set the default mode to be the same as string or number mode.

#### The Symbol.toStringTag Symbol

One of the most interesting problems in JavaScript has been the availability of multiple global execution environments. This occurs in web browsers when a page includes an iframe, as the page and the iframe each have their own execution environments. In most cases, this isn’t a problem, as data can be passed back and forth between the environments with little cause for concern. The problem arises when trying to identify what type of object you’re dealing with after the object has been passed between different objects.

The canonical example of this issue is passing an array from an iframe into the containing page or vice-versa. In ECMAScript 6 terminology, the iframe and the containing page each represent a different  _realm_  which is an execution environment for JavaScript. Each realm has its own global scope with its own copy of global objects. In whichever realm the array is created, it is definitely an array. When it’s passed to a different realm, however, an `instanceof Array` call returns `false` because the array was created with a constructor from a different realm and `Array`represents the constructor in the current realm.

##### A Workaround for the Identification Problem

Faced with this problem, developers soon found a good way to identify arrays. They discovered that by calling the standard `toString()` method on the object, a predictable string was always returned. Thus, many JavaScript libraries began including a function like this:

```js
function isArray(value) {
    return Object.prototype.toString.call(value) === "[object Array]";
}

console.log(isArray([]));   // true
```

This may look a bit roundabout, but it worked quite well for identifying arrays in all browsers. The `toString()` method on arrays isn’t useful for identifying an object because it returns a string representation of the items the object contains. But the `toString()` method on `Object.prototype` had a quirk: it included internally-defined name called `[[Class]]` in the returned result. Developers could use this method on an object to retrieve what the JavaScript environment thought the object’s data type was.

Developers quickly realized that since there was no way to change this behavior, it was possible to use the same approach to distinguish between native objects and those created by developers. The most important case of this was the ECMAScript 5 `JSON` object.

Prior to ECMAScript 5, many developers used Douglas Crockford’s  _json2.js_, which creates a global `JSON` object. As browsers started to implement the `JSON` global object, figuring out whether the global `JSON` was provided by the JavaScript environment itself or through some other library became necessary. Using the same technique I showed with the `isArray()` function, many developers created functions like this:

```js
function supportsNativeJSON() {
    return typeof JSON !== "undefined" &&
        Object.prototype.toString.call(JSON) === "[object JSON]";
}
```

The same characteristic of `Object.prototype` that allowed developers to identify arrays across iframe boundaries also provided a way to tell if `JSON` was the native `JSON` object or not. A non-native `JSON` object would return `[object Object]` while the native version returned `[object JSON]` instead. This approach became the de facto standard for identifying native objects.

##### The ECMAScript 6 Answer

ECMAScript 6 redefines this behavior through the `Symbol.toStringTag` symbol. This symbol represents a property on each object that defines what value should be produced when `Object.prototype.toString.call()` is called on it. For an array, the value that function returns is explained by storing `"Array"` in the `Symbol.toStringTag` property.

Likewise, you can define the `Symbol.toStringTag` value for your own objects:

```js
function Person(name) {
    this.name = name;
}

Person.prototype[Symbol.toStringTag] = "Person";

let me = new Person("Nicholas");

console.log(me.toString());                         // "[object Person]"
console.log(Object.prototype.toString.call(me));    // "[object Person]"
```

In this example, a `Symbol.toStringTag` property is defined on `Person.prototype` to provide the default behavior for creating a string representation. Since `Person.prototype` inherits the `Object.prototype.toString()` method, the value returned from `Symbol.toStringTag` is also used when calling the `me.toString()` method. However, you can still define your own `toString()` method that provides a different behavior without affecting the use of the `Object.prototype.toString.call()` method. Here’s how that might look:

```js
function Person(name) {
    this.name = name;
}

Person.prototype[Symbol.toStringTag] = "Person";

Person.prototype.toString = function() {
    return this.name;
};

let me = new Person("Allyson");

console.log(me.toString());                         // "Allyson"
console.log(Object.prototype.toString.call(me));    // "[object Person]"
```

This code defines `Person.prototype.toString()` to return the value of the `name` property. Since `Person` instances no longer inherit the `Object.prototype.toString()` method, calling `me.toString()` exhibits a different behavior.

*All objects inherit `Symbol.toStringTag` from `Object.prototype` unless otherwise specified. The string `"Object"` is the default property value.*

There is no restriction on which values can be used for `Symbol.toStringTag` on developer-defined objects. For example, nothing prevents you from using `"Array"` as the value of the `Symbol.toStringTag` property, such as:

```js
function Person(name) {
    this.name = name;
}

Person.prototype[Symbol.toStringTag] = "Array";

Person.prototype.toString = function() {
    return this.name;
};

let me = new Person("Allyson");

console.log(me.toString());                         // "Allyson"
console.log(Object.prototype.toString.call(me));    // "[object Array]"
```

The result of calling `Object.prototype.toString()` is `"[object Array]"` in this code, which is the same result you’d get from an actual array. This highlights the fact that `Object.prototype.toString()` is no longer a completely reliable way of identifying an object’s type.

Changing the string tag for native objects is also possible. Just assign to `Symbol.toStringTag` on the object’s prototype, like this:

```js
Array.prototype[Symbol.toStringTag] = "Magic";

let values = [];

console.log(Object.prototype.toString.call(values));    // "[object Magic]"
```

Even though `Symbol.toStringTag` is overwritten for arrays in this example, the call to `Object.prototype.toString()` results in `"[object Magic]"` instead. While I recommended not changing built-in objects in this way, there’s nothing in the language that forbids doing so.

#### The Symbol.unscopables Symbol

The `with` statement is one of the most controversial parts of JavaScript. Originally designed to avoid repetitive typing, the `with` statement later became roundly criticized for making code harder to understand and for negative performance implications as well as being error-prone.

As a result, the `with` statement is not allowed in strict mode; that restriction also affects classes and modules, which are strict mode by default and have no opt-out.

While future code will undoubtedly not use the `with` statement, ECMAScript 6 still supports `with` in nonstrict mode for backwards compatibility and, as such, had to find ways to allow code that does use `with` to continue to work properly.

To understand the complexity of this task, consider the following code:

```js
let values = [1, 2, 3],
    colors = ["red", "green", "blue"],
    color = "black";

with(colors) {
    push(color);
    push(...values);
}

console.log(colors);    // ["red", "green", "blue", "black", 1, 2, 3]
```

In this example, the two calls to `push()` inside the `with` statement are equivalent to `colors.push()` because the `with` statement added `push` as a local binding. The `color`reference refers to the variable created outside the `with` statement, as does the `values`reference.

But ECMAScript 6 added a `values` method to arrays. That would mean in an ECMAScript 6 environment, the `values` reference inside the `with` statement should refer not to the local variable `values`, but to the array’s `values` method, which would break the code. This is why the `Symbol.unscopables`symbol exists.

The `Symbol.unscopables` symbol is used on `Array.prototype` to indicate which properties shouldn’t create bindings inside of a `with` statement. When present, `Symbol.unscopables` is an object whose keys are the identifiers to omit from `with` statement bindings and whose values are `true` to enforce the block. Here’s the default `Symbol.unscopables` property for arrays:

```js
// built into ECMAScript 6 by default
Array.prototype[Symbol.unscopables] = Object.assign(Object.create(null), {
    copyWithin: true,
    entries: true,
    fill: true,
    find: true,
    findIndex: true,
    keys: true,
    values: true
});
```

The `Symbol.unscopables` object has a `null` prototype, which is created by the `Object.create(null)` call, and contains all of the new array methods in ECMAScript 6. Bindings for these methods are not created inside a `with` statement, allowing old code to continue working without any problem.

In general, you shouldn’t need to define `Symbol.unscopables` for your objects unless you use the `with` statement and are making changes to an existing object in your code base.

### Summary

Symbols are a new type of primitive value in JavaScript and are used to create properties that can’t be accessed without referencing the symbol.

While not truly private, these properties are harder to accidentally change or overwrite and are therefore suitable for functionality that needs a level of protection from developers.

You can provide descriptions for symbols that allow for easier identification of symbol values. There is a global symbol registry that allows you to use shared symbols in different parts of code by using the same description. In this way, the same symbol can be used for the same reason in multiple places.

Methods like `Object.keys()` or `Object.getOwnPropertyNames()` don’t return symbols, so a new method called `Object.getOwnPropertySymbols()` was added in ECMAScript 6 to allow retrieval of symbol properties. You can still make changes to symbol properties by calling the `Object.defineProperty()` and `Object.defineProperties()` methods.

Well-known symbols define previously internal-only functionality for standard objects and use globally-available symbol constants, such as the `Symbol.hasInstance` property. These symbols use the prefix `Symbol.` in the specification and allow developers to modify standard object behavior in a variety of ways.

## Sets and Maps

JavaScript only had one type of collection, represented by the Array type, for most of its history (though some may argue all non-array objects are just collections of key-value pairs, their intended use was, originally quite different from arrays). Arrays are used in JavaScript just like arrays in other languages, but the lack of other collection options meant arrays were often used as queues and stacks, as well. Since arrays only use numeric indices, developers used non-array objects whenever a non-numeric index was necessary. That technique led to custom implementations of sets and maps using non-array objects.

A set is a list of values that cannot contain duplicates. You typically don’t access individual items in a set like you would items in an array; instead, it’s much more common to just check a set to see if a value is present. A map is a collection of keys that correspond to specific values. As such, each item in a map stores two pieces of data, and values are retrieved by specifying the key to read from. Maps are frequently used as caches, for storing data to be quickly retrieved later. While ECMAScript 5 didn’t formally have sets and maps, developers worked around this limitation using non-array objects, too.

ECMAScript 6 added sets and maps to JavaScript, and this chapter discusses everything you need to know about these two collection types.

### Sets and Maps in ECMAScript 5

In ECMAScript 5, developers mimicked sets and maps by using object properties, like this:

```js
let set = Object.create(null);

set.foo = true;

// checking for existence
if (set.foo) {
    // do something
}
```

The only real difference between an object used as a set and an object used as a map is the value being stored. For instance, this example uses an object as a map:

```js
let map = Object.create(null);

map.foo = "bar";

// retrieving a value
let value = map.foo;

console.log(value);         // "bar"
```

### Problems with Workarounds

While using objects as sets and maps works okay in simple situations, the approach can get more complicated once you run into the limitations of object properties. For example, since all object properties must be strings, you must be certain no two keys evaluate to the same string. Consider the following:

```js
let map = Object.create(null);

map[5] = "foo";

console.log(map["5"]);      // "foo"
```

This example assigns the string value `"foo"` to a numeric key of `5`. Internally, that numeric value is converted to a string, so `map["5"]` and `map[5]` actually reference the same property. That internal conversion can cause problems when you want to use both numbers and strings as keys. Another problem arises when using objects as keys, like this:

```js
let map = Object.create(null),
    key1 = {},
    key2 = {};

map[key1] = "foo";

console.log(map[key2]);     // "foo"
```

Here, `map[key2]` and `map[key1]` reference the same value. The objects `key1` and `key2` are converted to strings because object properties must be strings. Since `"[object Object]"` is the default string representation for objects, both `key1` and `key2` are converted to that string. This can cause errors that may not be obvious because it’s logical to assume that different object keys would, in fact, be different.

The conversion to the default string representation makes it difficult to use objects as keys. (The same problem exists when trying to use an object as a set.)

These are difficult problems to identify and debug when they occur in large applications, which is a prime reason that ECMAScript 6 adds both sets and maps to the language.

*JavaScript has the in operator that returns true if a property exists in an object without reading the value of the object. However, the in operator also searches the prototype of an object, which makes it only safe to use when an object has a null prototype. Even so, many developers still incorrectly use code as in the last example rather than using in.*

### Sets in ECMAScript 6

ECMAScript 6 adds a Set type that is an ordered list of values without duplicates. Sets allow fast access to the data they contain, adding a more efficient manner of tracking discrete values.

#### Creating Sets and Adding Items

Sets are created using `new Set()` and items are added to a set by calling the `add()` method. You can see how many items are in a set by checking the `size` property:

```js
let set = new Set();
set.add(5);
set.add("5");

console.log(set.size);    // 2
```

Sets do not coerce values to determine whether they are the same. That means a set can contain both the number `5` and the string `"5"` as two separate items.

```js
let set = new Set(),
    key1 = {},
    key2 = {};

set.add(key1);
set.add(key2);

console.log(set.size);    // 2
```

Because `key1` and `key2` are not converted to strings, they count as two unique items in the set. (Remember, if they were converted to strings, they would both be equal to `"[Object object]"`.)

If the `add()` method is called more than once with the same value, all calls after the first one are effectively ignored:

```js
let set = new Set();
set.add(5);
set.add("5");
set.add(5);     // duplicate - this is ignored

console.log(set.size);    // 2
```

You can initialize a set using an array, and the `Set` constructor will ensure that only unique values are used. For instance:

```js
let set = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
console.log(set.size);    // 5
```

You can test which values are in a set using the `has()` method, like this:

```js
let set = new Set();
set.add(5);
set.add("5");

console.log(set.has(5));    // true
console.log(set.has(6));    // false
```

#### Removing Values

It’s also possible to remove values from a set. You can remove single value by using the `delete()` method, or you can remove all values from the set by calling the `clear()` method.

```js
let set = new Set();
set.add(5);
set.add("5");

console.log(set.has(5));    // true

set.delete(5);

console.log(set.has(5));    // false
console.log(set.size);      // 1

set.clear();

console.log(set.has("5"));  // false
console.log(set.size);      // 0
```

#### The forEach() Method for Sets

Using `forEach()` is basically the same for a set as it is for an array. Here’s some code that shows the method at work:

```js
let set = new Set([1, 2]);

set.forEach(function(value, key, ownerSet) {
    console.log(key + " " + value);
    console.log(ownerSet === set);
});
```

This code iterates over each item in the set and outputs the values passed to the `forEach()` callback function. Each time the callback function executes, `key` and `value` are the same, and `ownerSet` is always equal to `set`. This code outputs:

```
1 1
true
2 2
true
```

Also the same as arrays, you can pass a this value as the second argument to `forEach()` if you need to use `this` in your callback function:

```js
let set = new Set([1, 2]);

let processor = {
    output(value) {
        console.log(value);
    },
    process(dataSet) {
        dataSet.forEach(function(value) {
            this.output(value);
        }, this);
    }
};

processor.process(set);
```

You can also use an arrow function to get the same effect without passing the second argument, like this:

```js
let set = new Set([1, 2]);

let processor = {
    output(value) {
        console.log(value);
    },
    process(dataSet) {
        dataSet.forEach((value) => this.output(value));
    }
};

processor.process(set);
```

#### Converting a Set to an Array

It’s easy to convert an array into a set because you can pass the array to the Set constructor. It’s also easy to convert a set back into an array using the spread operator. You can also use the spread operator(`...`) to work on iterable objects, such as sets, to convert them into arrays. For example:

```js
let set = new Set([1, 2, 3, 3, 3, 4, 5]),
    array = [...set];

console.log(array);             // [1,2,3,4,5]
```

This approach is useful when you already have an array and want to create an array without duplicates. For example:

```js
function eliminateDuplicates(items) {
    return [...new Set(items)];
}

let numbers = [1, 2, 3, 3, 3, 4, 5],
    noDuplicates = eliminateDuplicates(numbers);

console.log(noDuplicates);      // [1,2,3,4,5]
```

#### Weak Sets

The `Set` type could alternately be called a strong set, because of the way it stores object references. An object stored in an instance of Set is effectively the same as storing that object inside a variable. As long as a reference to that `Set` instance exists, the object cannot be garbage collected to free memory. For example:

```js
let set = new Set(),
    key = {};

set.add(key);
console.log(set.size);      // 1

// eliminate original reference
key = null;

console.log(set.size);      // 1

// get the original reference back
key = [...set][0];
```

To alleviate such issues, ECMAScript 6 also includes *weak* sets, which only store weak object references and cannot store primitive values. A *weak reference* to an object does not prevent garbage collection if it is the only remaining reference.

**Creating a Weak Set**

Weak sets are created using the `WeakSet` constructor and have an `add()` method, a `has()` method, and a `delete()` method. Here’s an example that uses all three:

```js
let set = new WeakSet(),
    key = {};

// add the object to the set
set.add(key);

console.log(set.has(key));      // true

set.delete(key);

console.log(set.has(key));      // false
```

Using a weak set is a lot like using a regular set. You can add, remove, and check for references in the weak set. You can also seed a weak set with values by passing an iterable to the constructor:

```js
let key1 = {},
    key2 = {},
    set = new WeakSet([key1, key2]);

console.log(set.has(key1));     // true
console.log(set.has(key2));     // true
```

**Key Differences Between Set Types**

The biggest difference between weak sets and regular sets is the weak reference held to the object value. Here’s an example that demonstrates that difference:

```js
let set = new WeakSet(),
    key = {};

// add the object to the set
set.add(key);

console.log(set.has(key));      // true

// remove the last strong reference to key, also removes from weak set
key = null;
```

After this code executes, the reference to `key` in the weak set is no longer accessible. It is not possible to verify its removal because you would need one reference to that object to pass to the `has()` method. This can make testing weak sets a little confusing, but you can trust that the reference has been properly removed by the JavaScript engine.

These examples show that weak sets share some characteristics with regular sets, but there are some key differences. Those are:

1. In a `WeakSet` instance, the `add()` method throws an error when passed a non-object (`has()` and `delete()` always return `false` for non-object arguments).
2. Weak sets are not iterables and therefore cannot be used in a `for-of` loop.
3. Weak sets do not expose any iterators (such as the `keys()` and `values()` methods), so there is no way to programmatically determine the contents of a weak set.
4. Weak sets do not have a `forEach()` method.
5. Weak sets do not have a `size` property.

The seemingly limited functionality of weak sets is necessary in order to properly handle memory. In general, if you only need to track object references, then you should use a weak set instead of a regular set.

Sets give you a new way to handle lists of values, but they aren’t useful when you need to associate additional information with those values. That’s why ECMAScript 6 also adds maps.

### Maps in ECMAScript 6

The ECMAScript 6 `Map` type is an ordered list of key-value pairs, where both the key and the value can have any type. Keys equivalence is determined by using the same approach as `Set` objects, so you can have both a key of `5` and a key of `"5"` because they are different types. This is quite different from using object properties as keys, as object properties always coerce values into strings.

You can add items to maps by calling the `set()` method and passing it a key and the value to associate with the key. You can later retrieve a value by passing the key to the `get()` method. For example:

```js
let map = new Map();
map.set("title", "Understanding ES6");
map.set("year", 2016);

console.log(map.get("title"));      // "Understanding ES6"
console.log(map.get("year"));       // 2016
```

If either key didn’t exist in the map, then `get()` would have returned the special value `undefined` instead of a value.

You can also use objects as keys, which isn’t possible when using object properties to create a map in the old workaround approach. Here’s an example:

```js
let map = new Map(),
    key1 = {},
    key2 = {};

map.set(key1, 5);
map.set(key2, 42);

console.log(map.get(key1));         // 5
console.log(map.get(key2));         // 42
```

#### Map Methods

Maps share several methods with sets. That is intentional, and it allows you to interact with maps and sets in similar ways. These three methods are available on both maps and sets:

* `has(key)` - Determines if the given key exists in the map
* `delete(key)` - Removes the key and its associated value from the map
* `clear()` - Removes all keys and values from the map

Maps also have a `size` property that indicates how many key-value pairs it contains. This code uses all three methods and `size` in different ways:

```js
let map = new Map();
map.set("name", "Nicholas");
map.set("age", 25);

console.log(map.size);          // 2

console.log(map.has("name"));   // true
console.log(map.get("name"));   // "Nicholas"

console.log(map.has("age"));    // true
console.log(map.get("age"));    // 25

map.delete("name");
console.log(map.has("name"));   // false
console.log(map.get("name"));   // undefined
console.log(map.size);          // 1

map.clear();
console.log(map.has("name"));   // false
console.log(map.get("name"));   // undefined
console.log(map.has("age"));    // false
console.log(map.get("age"));    // undefined
console.log(map.size);          // 0
```

The `clear()` method is a fast way to remove a lot of data from a map, but there’s also a way to add a lot of data to a map at one time.

#### Map Initialization

Also similar to sets, you can initialize a map with data by passing an array to the `Map` constructor. Each item in the array must itself be an array where the first item is the key and the second is that key’s corresponding value. The entire map, therefore, is an array of these two-item arrays, for example:

```js
let map = new Map([["name", "Nicholas"], ["age", 25]]);

console.log(map.has("name"));   // true
console.log(map.get("name"));   // "Nicholas"
console.log(map.has("age"));    // true
console.log(map.get("age"));    // 25
console.log(map.size);          // 2
```

#### The forEach Method on Maps

The `forEach()` method for maps is similar to `forEach()` for sets and arrays, in that it accepts a callback function that receives three arguments:

1. The value from the next position in the map
2. The key for that value
3. The map from which the value is read

```js
let map = new Map([ ["name", "Nicholas"], ["age", 25]]);

map.forEach(function(value, key, ownerMap) {
    console.log(key + " " + value);
    console.log(ownerMap === map);
});
```

The `forEach()` callback function outputs the information that is passed to it. The `value` and `key` are output directly, and `ownerMap` is compared to `map` to show that the values are equivalent. This outputs:

```
name Nicholas
true
age 25
true
```

The callback passed to `forEach()` receives each key-value pair in the order in which the pairs were inserted into the map. This behavior differs slightly from calling `forEach()` on arrays, where the callback receives each item in order of numeric index.

*You can also provide a second argument to `forEach()` to specify the `this` value inside the callback function. A call like that behaves the same as the set version of the `forEach()` method.*

#### Weak Maps

Weak maps are to maps what weak sets are to sets: they’re a way to store weak object references. In weak maps, every key must be an object (an error is thrown if you try to use a non-object key), and those object references are held weakly so they don’t interfere with garbage collection. When there are no references to a weak map key outside a weak map, the key-value pair is removed from the weak map.

**Using Weak Maps**

The ECMAScript 6 `WeakMap` type is an unordered list of key-value pairs, where a key must be a non-null object and a value can be of any type. The interface for `WeakMap` is very similar to that of `Map` in that `set()` and `get()` are used to add and retrieve data, respectively:

```js
let map = new WeakMap(),
    element = document.querySelector(".element");

map.set(element, "Original");

let value = map.get(element);
console.log(value);             // "Original"

// remove the element
element.parentNode.removeChild(element);
element = null;

// the weak map is empty at this point
```

Similar to weak sets, there is no way to verify that a weak map is empty, because it doesn’t have a `size` property. Because there are no remaining references to the key, you can’t retrieve the value by calling the `get()` method, either. The weak map has cut off access to the value for that key, and when the garbage collector runs, the memory occupied by the value will be freed.

**Weak Map Initialization**

To initialize a weak map, pass an array of arrays to the `WeakMap` constructor. Just like initializing a regular map, each array inside the containing array should have two items, where the first item is the non-null object key and the second item is the value (any data type). For example:

```js
let key1 = {},
    key2 = {},
    map = new WeakMap([[key1, "Hello"], [key2, 42]]);

console.log(map.has(key1));     // true
console.log(map.get(key1));     // "Hello"
console.log(map.has(key2));     // true
console.log(map.get(key2));     // 42
```

**Weak Map Methods**

Weak maps have only two additional methods available to interact with key-value pairs. There is a `has()` method to determine if a given key exists in the map and a `delete()` method to remove a specific key-value pair. There is no `clear()` method because that would require enumerating keys, and like weak sets, that isn’t possible with weak maps. This example uses both the `has()` and `delete()` methods:

```js
let map = new WeakMap(),
    element = document.querySelector(".element");

map.set(element, "Original");

console.log(map.has(element));   // true
console.log(map.get(element));   // "Original"

map.delete(element);
console.log(map.has(element));   // false
console.log(map.get(element));   // undefined
```

**Private Object Data**

One practical use of weak maps is to store data that is private to object instances. All object properties are public in ECMAScript 6, and so you need to use some creativity to make data accessible to objects, but not accessible to everything. Consider the following example:

```js
function Person(name) {
    this._name = name;
}

Person.prototype.getName = function() {
    return this._name;
};
```

In ECMAScript 5, it’s possible to get close to having truly private data, by creating an object using a pattern such as this:

```js
var Person = (function() {

    var privateData = {},
        privateId = 0;

    function Person(name) {
        Object.defineProperty(this, "_id", { value: privateId++ });

        privateData[this._id] = {
            name: name
        };
    }

    Person.prototype.getName = function() {
        return privateData[this._id].name;
    };

    return Person;
}());
```

The big problem with this approach is that the data in `privateData` never disappears because there is no way to know when an object instance is destroyed; the `privateData` object will always contain extra data. This problem can be solved by using a weak map instead, as follows:

```js
let Person = (function() {

    let privateData = new WeakMap();

    function Person(name) {
        privateData.set(this, { name: name });
    }

    Person.prototype.getName = function() {
        return privateData.get(this).name;
    };

    return Person;
}());
```

This version of the `Person` example uses a weak map for the private data instead of an object. Because the `Person` object instance itself can be used as a key, there’s no need to keep track of a separate ID. When the `Person` constructor is called, a new entry is made into the weak map with a key of `this` and a value of an object containing private information. In this case, that value is an object containing only `name`. The `getName()` function retrieves that private information by passing `this` to the `privateData.get()` method, which fetches the value object and accesses the `name` property. This technique keeps the private information private, and destroys that information whenever an object instance associated with it is destroyed.

**Weak Map Uses and Limitations**

When deciding whether to use a weak map or a regular map, the primary decision to consider is whether you want to use only object keys. Anytime you’re going to use only object keys, then the best choice is a weak map. That will allow you to optimize memory usage and avoid memory leaks by ensuring that extra data isn’t kept around after it’s no longer accessible.

Keep in mind that weak maps give you very little visibility into their contents, so you can’t use the `forEach()` method, the `size` property, or the `clear()` method to manage the items. If you need some inspection capabilities, then regular maps are a better choice. Just be sure to keep an eye on memory usage.

Of course, if you only want to use non-object keys, then regular maps are your only choice.




## Iterators and Generators

Many programming languages have shifted from iterating over data with `for` loops, which require initializing variables to track position in a collection, to using iterator objects that programmatically return the next item in a collection. Iterators make working with collections of data easier, and ECMAScript 6 adds iterators to JavaScript. When coupled with new array methods and new types of collections (such as sets and maps), iterators are key for efficient data processing, and you will find them in many parts of the language. There’s a new `for-of` loop that works with iterators, the spread (`...`) operator uses iterators, and iterators even make asynchronous programming easier.

This chapter covers the many uses of iterators, but first, it’s important to understand the history behind why iterators were added to JavaScript.

### The Loop Problem

If you’ve ever programmed in JavaScript, you’ve probably written code that looks like this:

```js
var colors = ["red", "green", "blue"];

for (var i = 0, len = colors.length; i < len; i++) {
    console.log(colors[i]);
}
```

This standard `for` loop tracks the index into the `colors` array with the `i` variable. The value of `i` increments each time the loop executes if `i` isn’t larger than the length of the array (stored in `len`).

While this loop is fairly straightforward, loops grow in complexity when you nest them and need to keep track of multiple variables. Additional complexity can lead to errors, and the boilerplate nature of the `for` loop lends itself to more errors as similar code is written in multiple places. Iterators are meant to solve that problem.

### What are Iterators?

Iterators are just objects with a specific interface designed for iteration. All iterator objects have a `next()` method that returns a result object. The result object has two properties: `value`, which is the next value, and `done`, which is a boolean that’s `true` when there are no more values to return. The iterator keeps an internal pointer to a location within a collection of values and with each call to the `next()` method, it returns the next appropriate value.

If you call `next()` after the last value has been returned, the method returns `done` as `true` and `value` contains the  _return value_  for the iterator. That return value is not part of the data set, but rather a final piece of related data, or `undefined` if no such data exists. An iterator’s return value is similar to a function’s return value in that it’s a final way to pass information to the caller.

With that in mind, creating an iterator using ECMAScript 5 is fairly straightforward:

```js
function createIterator(items) {

    var i = 0;

    return {
        next: function() {

            var done = (i >= items.length);
            var value = !done ? items[i++] : undefined;

            return {
                done: done,
                value: value
            };

        }
    };
}

var iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

The `createIterator()` function returns an object with a `next()` method. Each time the method is called, the next value in the `items` array is returned as `value`. When `i` is 3, `done` becomes `true` and the ternary conditional operator that sets `value` evaluates to `undefined`. These two results fulfill the special last case for iterators in ECMAScript 6, where `next()` is called on an iterator after the last piece of data has been used.

As this example shows, writing iterators that behave according to the rules laid out in ECMAScript 6 is a bit complex.

Fortunately, ECMAScript 6 also provides generators, which make creating iterator objects much simpler.

### What Are Generators?

A  _generator_  is a function that returns an iterator. Generator functions are indicated by a star character (`*`) after the `function` keyword and use the new `yield` keyword. It doesn’t matter if the star is directly next to `function` or if there’s some whitespace between it and the `*`character, as in this example:

```js
// generator
function *createIterator() {
    yield 1;
    yield 2;
    yield 3;
}

// generators are called like regular functions but return an iterator
let iterator = createIterator();

console.log(iterator.next().value);     // 1
console.log(iterator.next().value);     // 2
console.log(iterator.next().value);     // 3
```

The `*` before `createIterator()` makes this function a generator. The `yield` keyword, also new to ECMAScript 6, specifies values the resulting iterator should return when `next()` is called, in the order they should be returned. The iterator generated in this example has three different values to return on successive calls to the `next()` method: first `1`, then `2`, and finally `3`. A generator gets called like any other function, as shown when `iterator` is created.

Perhaps the most interesting aspect of generator functions is that they stop execution after each `yield` statement. For instance, after `yield 1` executes in this code, the function doesn’t execute anything else until the iterator’s `next()` method is called. At that point, `yield 2` executes. This ability to stop execution in the middle of a function is extremely powerful and leads to some interesting uses of generator functions (discussed in the “Advanced Iterator Functionality” section).

The `yield` keyword can be used with any value or expression, so you can write generator functions that add items to iterators without just listing the items one by one. For example, here’s one way you could use `yield` inside a `for` loop:

```js
function *createIterator(items) {
    for (let i = 0; i < items.length; i++) {
        yield items[i];
    }
}

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

This example passes an array called `items` to the `createIterator()` generator function. Inside the function, a `for` loop yields the elements from the array into the iterator as the loop progresses. Each time `yield` is encountered, the loop stops, and each time `next()` is called on `iterator`, the loop picks up with the next `yield` statement.

Generator functions are an important feature of ECMAScript 6, and since they are just functions, they can be used in all the same places. The rest of this section focuses on other useful ways to write generators.

The `yield` keyword can only be used inside of generators. Use of `yield`anywhere else is a syntax error, including functions that are inside of generators, such as:

```js
function *createIterator(items) {

    items.forEach(function(item) {

        // syntax error
        yield item + 1;
    });
}
```

Even though `yield` is technically inside of `createIterator()`, this code is a syntax error because `yield` cannot cross function boundaries. In this way, `yield` is similar to `return`, in that a nested function cannot return a value for its containing function.

#### Generator Function Expressions

You can use function expressions to create generators by just including a star (`*`) character between the `function` keyword and the opening parenthesis. For example:

```js
let createIterator = function *(items) {
    for (let i = 0; i < items.length; i++) {
        yield items[i];
    }
};

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this code, `createIterator()` is a generator function expression instead of a function declaration. The asterisk goes between the `function` keyword and the opening parentheses because the function expression is anonymous. Otherwise, this example is the same as the previous version of the `createIterator()` function, which also used a `for` loop.

Creating an arrow function that is also a generator is not possible.

#### Generator Object Methods

Because generators are just functions, they can be added to objects, too. For example, you can make a generator in an ECMAScript 5-style object literal with a function expression:

```js
var o = {

    createIterator: function *(items) {
        for (let i = 0; i < items.length; i++) {
            yield items[i];
        }
    }
};

let iterator = o.createIterator([1, 2, 3]);
```

You can also use the ECMAScript 6 method shorthand by prepending the method name with a star (`*`):

```js
var o = {

    *createIterator(items) {
        for (let i = 0; i < items.length; i++) {
            yield items[i];
        }
    }
};

let iterator = o.createIterator([1, 2, 3]);
```

These examples are functionally equivalent to the example in the “Generator Function Expressions” section; they just use different syntax. In the shorthand version, because the `createIterator()` method is defined with no `function` keyword, the star is placed immediately before the method name, though you can leave whitespace between the star and the method name.

### Iterables and for-of

Closely related to iterators, an  _iterable_  is an object with a `Symbol.iterator` property. The well-known `Symbol.iterator` symbol specifies a function that returns an iterator for the given object. All collection objects (arrays, sets, and maps) and strings are iterables in ECMAScript 6 and so they have a default iterator specified. Iterables are designed to be used with a new addition to ECMAScript: the `for-of` loop.

All iterators created by generators are also iterables, as generators assign the `Symbol.iterator` property by default.

At the beginning of this chapter, I mentioned the problem of tracking an index inside a `for` loop. Iterators are the first part of the solution to that problem. The `for-of` loop is the second part: it removes the need to track an index into a collection entirely, leaving you free to focus on working with the contents of the collection.

A `for-of` loop calls `next()` on an iterable each time the loop executes and stores the `value`from the result object in a variable. The loop continues this process until the returned object’s `done` property is `true`. Here’s an example:

```js
let values = [1, 2, 3];

for (let num of values) {
    console.log(num);
}
```

This code outputs the following:

```
1
2
3
```

This `for-of` loop first calls the `Symbol.iterator` method on the `values` array to retrieve an iterator. (The call to `Symbol.iterator` happens behind the scenes in the JavaScript engine itself.) Then `iterator.next()` is called, and the `value` property on the iterator’s result object is read into `num`. The `num` variable is first 1, then 2, and finally 3. When `done` on the result object is `true`, the loop exits, so `num` is never assigned the value of `undefined`.

If you are simply iterating over values in an array or collection, then it’s a good idea to use a `for-of` loop instead of a `for` loop. The `for-of` loop is generally less error-prone because there are fewer conditions to keep track of. Save the traditional `for` loop for more complex control conditions.

The `for-of` statement will throw an error when used on, a non-iterable object, `null`, or `undefined`.

#### Accessing the Default Iterator

You can use `Symbol.iterator` to access the default iterator for an object, like this:

```js
let values = [1, 2, 3];
let iterator = values[Symbol.iterator]();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

This code gets the default iterator for `values` and uses that to iterate over the items in the array. This is the same process that happens behind-the-scenes when using a `for-of` loop.

Since `Symbol.iterator` specifies the default iterator, you can use it to detect whether an object is iterable as follows:

```js
function isIterable(object) {
    return typeof object[Symbol.iterator] === "function";
}

console.log(isIterable([1, 2, 3]));     // true
console.log(isIterable("Hello"));       // true
console.log(isIterable(new Map()));     // true
console.log(isIterable(new Set()));     // true
console.log(isIterable(new WeakMap())); // false
console.log(isIterable(new WeakSet())); // false
```

The `isIterable()` function simply checks to see if a default iterator exists on the object and is a function. The `for-of` loop does a similar check before executing.

So far, the examples in this section have shown ways to use `Symbol.iterator` with built-in iterable types, but you can also use the `Symbol.iterator` property to create your own iterables.

#### Creating Iterables

Developer-defined objects are not iterable by default, but you can make them iterable by creating a `Symbol.iterator` property containing a generator. For example:

```js
let collection = {
    items: [],
    *[Symbol.iterator]() {
        for (let item of this.items) {
            yield item;
        }
    }
};

collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for (let x of collection) {
    console.log(x);
}

```

This code outputs the following:

```
1
2
3
```

First, the example defines a default iterator for an object called `collection`. The default iterator is created by the `Symbol.iterator` method, which is a generator (note the star still comes before the name). The generator then uses a `for-of` loop to iterate over the values in `this.items` and uses `yield` to return each one. Instead of manually iterating to define values for the default iterator of `collection` to return, the `collection` object relies on the default iterator of `this.items` to do the work.

“Delegating Generators” later in this chapter describes a different approach to using the iterator of another object.

Now you’ve seen some uses for the default array iterator, but there are many more iterators built in to ECMAScript 6 to make working with collections of data easy.

### Built-in Iterators

Iterators are an important part of ECMAScript 6, and as such, you don’t need to create your own iterators for many built-in types; the language includes them by default. You only need to create iterators when the built-in iterators don’t serve your purpose, which will most frequently be when defining your own objects or classes. Otherwise, you can rely on built-in iterators to do your work. Perhaps the most common iterators to use are those that work on collections.

#### Collection Iterators

ECMAScript 6 has three types of collection objects: arrays, maps, and sets. All three have the following built-in iterators to help you navigate their content:

- `entries()` - Returns an iterator whose values are a key-value pair
- `values()` - Returns an iterator whose values are the values of the collection
- `keys()` - Returns an iterator whose values are the keys contained in the collection

You can retrieve an iterator for a collection by calling one of these methods.

##### The entries() Iterator

The `entries()` iterator returns a two-item array each time `next()` is called. The two-item array represents the key and value for each item in the collection. For arrays, the first item is the numeric index; for sets, the first item is also the value (since values double as keys in sets); for maps, the first item is the key.

Here are some examples that use this iterator:

```js
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

for (let entry of colors.entries()) {
    console.log(entry);
}

for (let entry of tracking.entries()) {
    console.log(entry);
}

for (let entry of data.entries()) {
    console.log(entry);
}
```

The `console.log()` calls give the following output:

```js
[0, "red"]
[1, "green"]
[2, "blue"]
[1234, 1234]
[5678, 5678]
[9012, 9012]
["title", "Understanding ECMAScript 6"]
["format", "ebook"]
```

This code uses the `entries()` method on each type of collection to retrieve an iterator, and it uses `for-of` loops to iterate the items. The console output shows how the keys and values are returned in pairs for each object.

##### The values() Iterator

The `values()` iterator simply returns values as they are stored in the collection. For example:

```js
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

for (let value of colors.values()) {
    console.log(value);
}

for (let value of tracking.values()) {
    console.log(value);
}

for (let value of data.values()) {
    console.log(value);
}
```

This code outputs the following:

```
"red"
"green"
"blue"
1234
5678
9012
"Understanding ECMAScript 6"
"ebook"
```

Calling the `values()` iterator, as in this example, returns the exact data contained in each collection without any information about that data’s location in the collection.

##### The keys() Iterator

The `keys()` iterator returns each key present in a collection. For arrays, it only returns numeric keys, never other own properties of the array. For sets, the keys are the same as the values, and so `keys()` and `values()` return the same iterator. For maps, the `keys()` iterator returns each unique key. Here’s an example that demonstrates all three:

```js
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

for (let key of colors.keys()) {
    console.log(key);
}

for (let key of tracking.keys()) {
    console.log(key);
}

for (let key of data.keys()) {
    console.log(key);
}
```

This example outputs the following:

```
0
1
2
1234
5678
9012
"title"
"format"
```

The `keys()` iterator fetches each key in `colors`, `tracking`, and `data`, and those keys are printed from inside the three `for-of` loops. For the array object, only numeric indices are printed, which would still happen even if you added named properties to the array. This is different from the way the `for-in` loop works with arrays, because the `for-in` loop iterates over properties rather than just the numeric indices.

##### Default Iterators for Collection Types

Each collection type also has a default iterator that is used by `for-of` whenever an iterator isn’t explicitly specified. The `values()` method is the default iterator for arrays and sets, while the `entries()` method is the default iterator for maps. These defaults make using collection objects in `for-of` loops a little easier. For instance, consider this example:

```js
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "print");

// same as using colors.values()
for (let value of colors) {
    console.log(value);
}

// same as using tracking.values()
for (let num of tracking) {
    console.log(num);
}

// same as using data.entries()
for (let entry of data) {
    console.log(entry);
}
```

No iterator is specified, so the default iterator functions will be used. The default iterators for arrays, sets, and maps are designed to reflect how these objects are initialized, so this code outputs the following:

```
"red"
"green"
"blue"
1234
5678
9012
["title", "Understanding ECMAScript 6"]
["format", "print"]
```

Arrays and sets return their values by default, while maps return the same array format that can be passed into the `Map` constructor. Weak sets and weak maps, on the other hand, do not have built-in iterators. Managing weak references means there’s no way to know exactly how many values are in these collections, which also means there’s no way to iterate over them.

#### Destructuring and for-of Loops

The behavior of the default iterator for maps is also helpful when used in `for-of`loops with destructuring, as in this example:

```js
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

// same as using data.entries()
for (let [key, value] of data) {
    console.log(key + "=" + value);
}
```

The `for-of` loop in this code uses a destructured array to assign `key` and `value` for each entry in the map. In this way, you can easily work with keys and values at the same time without needing to access a two-item array or going back to the map to fetch either the key or the value. Using a destructured array for maps makes the `for-of`loop equally useful for maps as it is for sets and arrays.

#### String Iterators

JavaScript strings have slowly become more like arrays since ECMAScript 5 was released. For example, ECMAScript 5 formalized bracket notation for accessing characters in strings (that is, using `text[0]` to get the first character, and so on). But bracket notation works on code units rather than characters, so it cannot be used to access double-byte characters correctly, as this example demonstrates:

```js
var message = "A ð ®· B";

for (let i=0; i < message.length; i++) {
    console.log(message[i]);
}
```

This code uses bracket notation and the `length` property to iterate over and print a string containing a Unicode character. The output is a bit unexpected:

```
A
(blank)
(blank)
(blank)
(blank)
B
```

Since the double-byte character is treated as two separate code units, there are four empty lines between `A` and `B` in the output.

Fortunately, ECMAScript 6 aims to fully support Unicode, and the default string iterator is an attempt to solve the string iteration problem. As such, the default iterator for strings works on characters rather than code units. Changing this example to use the default string iterator with a `for-of` loop results in more appropriate output. Here’s the tweaked code:

```js
var message = "A ð ®· B";

for (let c of message) {
    console.log(c);
}
```

This outputs the following:

```
A
(blank)
ð ®·
(blank)
B
```

This result is more in line with what you’d expect when working with characters: the loop successfully prints the Unicode character, as well as all the rest.

#### NodeList Iterators

The Document Object Model (DOM) has a `NodeList` type that represents a collection of elements in a document. For those who write JavaScript to run in web browsers, understanding the difference between `NodeList` objects and arrays has always been a bit difficult. Both `NodeList`objects and arrays use the `length` property to indicate the number of items, and both use bracket notation to access individual items. Internally, however, a `NodeList` and an array behave quite differently, which has led to a lot of confusion.

With the addition of default iterators in ECMAScript 6, the DOM definition of `NodeList` (included in the HTML specification rather than ECMAScript 6 itself) includes a default iterator that behaves in the same manner as the array default iterator. That means you can use `NodeList` in a `for-of`loop or any other place that uses an object’s default iterator. For example:

```js
var divs = document.getElementsByTagName("div");

for (let div of divs) {
    console.log(div.id);
}
```

This code calls `getElementsByTagName()` to retrieve a `NodeList` that represents all of the `<div>`elements in the `document` object. The `for-of` loop then iterates over each element and outputs the element IDs, effectively making the code the same as it would be for a standard array.

### The Spread Operator and Non-Array Iterables

Recall that the spread operator (`...`) can be used to convert a set into an array. For example:

```js
let set = new Set([1, 2, 3, 3, 3, 4, 5]),
    array = [...set];

console.log(array);             // [1,2,3,4,5]
```

This code uses the spread operator inside an array literal to fill in that array with the values from `set`. The spread operator works on all iterables and uses the default iterator to determine which values to include. All values are read from the iterator and inserted into the array in the order in which values were returned from the iterator. This example works because sets are iterables, but it can work equally well on any iterable. Here’s another example:

```js
let map = new Map([ ["name", "Nicholas"], ["age", 25]]),
    array = [...map];

console.log(array);         // [ ["name", "Nicholas"], ["age", 25]]
```

Here, the spread operator converts `map` into an array of arrays. Since the default iterator for maps returns key-value pairs, the resulting array looks like the array that was passed during the `new Map()` call.

You can use the spread operator in an array literal as many times as you want, and you can use it wherever you want to insert multiple items from an iterable. Those items will just appear in order in the new array at the location of the spread operator. For example:

```js
let smallNumbers = [1, 2, 3],
    bigNumbers = [100, 101, 102],
    allNumbers = [0, ...smallNumbers, ...bigNumbers];

console.log(allNumbers.length);     // 7
console.log(allNumbers);    // [0, 1, 2, 3, 100, 101, 102]
```

The spread operator is used to create `allNumbers` from the values in `smallNumbers` and `bigNumbers`. The values are placed in `allNumbers` in the same order the arrays are added when `allNumbers` is created: `0` is first, followed by the values from `smallNumbers`, followed by the values from `bigNumbers`. The original arrays are unchanged, though, as their values have just been copied into `allNumbers`.

Since the spread operator can be used on any iterable, it’s the easiest way to convert an iterable into an array. You can convert strings into arrays of characters (not code units) and `NodeList`objects in the browser into arrays of nodes.

Now that you understand the basics of how iterators work, including `for-of` and the spread operator, it’s time to look at some more complex uses of iterators.

### Advanced Iterator Functionality

You can accomplish a lot with the basic functionality of iterators and the convenience of creating them using generators. However, iterators are much more powerful when used for tasks other than simply iterating over a collection of values. During the development of ECMAScript 6, a lot of unique ideas and patterns emerged that encouraged the creators to add more functionality. Some of those additions are subtle, but when used together, can accomplish some interesting interactions.

#### Passing Arguments to Iterators

Throughout this chapter, examples have shown iterators passing values out via the `next()`method or by using `yield` in a generator. But you can also pass arguments to the iterator through the `next()` method. When an argument is passed to the `next()` method, that argument becomes the value of the `yield` statement inside a generator. This capability is important for more advanced functionality such as asynchronous programming. Here’s a basic example:

```js
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;       // 4 + 2
    yield second + 3;                   // 5 + 3
}

let iterator = createIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next(4));          // "{ value: 6, done: false }"
console.log(iterator.next(5));          // "{ value: 8, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

The first call to `next()` is a special case where any argument passed to it is lost. Since arguments passed to `next()` become the values returned by `yield`, an argument from the first call to `next()` could only replace the first yield statement in the generator function if it could be accessed before that `yield` statement. That’s not possible, so there’s no reason to pass an argument the first time `next()` is called.

On the second call to `next()`, the value `4` is passed as the argument. The `4` ends up assigned to the variable `first` inside the generator function. In a `yield` statement including an assignment, the right side of the expression is evaluated on the first call to `next()` and the left side is evaluated on the second call to `next()` before the function continues executing. Since the second call to `next()` passes in `4`, that value is assigned to `first` and then execution continues.

The second `yield` uses the result of the first `yield` and adds two, which means it returns a value of six. When `next()` is called a third time, the value `5` is passed as an argument. That value is assigned to the variable `second` and then used in the third `yield` statement to return `8`.

It’s a bit easier to think about what’s happening by considering which code is executing each time execution continues inside the generator function. Figure 8-1 uses colors to show the code being executed before yielding.

![Code execution inside a generator](./Images/Code%20execution%20inside%20a%20generator.png)

The color yellow represents the first call to `next()` and all the code executed inside of the generator as a result. The color aqua represents the call to `next(4)` and the code that is executed with that call. The color purple represents the call to `next(5)` and the code that is executed as a result. The tricky part is how the code on the right side of each expression executes and stops before the left side is executed. This makes debugging complicated generators a bit more involved than debugging regular functions.

So far, you’ve seen that `yield` can act like `return` when a value is passed to the `next()`method. However, that’s not the only execution trick you can do inside a generator. You can also cause iterators throw an error.

#### Throwing Errors in Iterators

It’s possible to pass not just data into iterators but also error conditions. Iterators can choose to implement a `throw()` method that instructs the iterator to throw an error when it resumes. This is an important capability for asynchronous programming, but also for flexibility inside generators, where you want to be able to mimic both return values and thrown errors (the two ways of exiting a function). You can pass an error object to `throw()` that should be thrown when the iterator continues processing. For example:

```js
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;       // yield 4 + 2, then throw
    yield second + 3;                   // never is executed
}

let iterator = createIterator();

console.log(iterator.next());                   // "{ value: 1, done: false }"
console.log(iterator.next(4));                  // "{ value: 6, done: false }"
console.log(iterator.throw(new Error("Boom"))); // error thrown from generator
```

In this example, the first two `yield` expressions are evaluated as normal, but when `throw()` is called, an error is thrown before `let second` is evaluated. This effectively halts code execution similar to directly throwing an error. The only difference is the location in which the error is thrown. Figure 8-2 shows which code is executed at each step.

![Code execution inside a generator](./Images/Throwing%20an%20error%20inside%20a%20generator.png)

In this figure, the color red represents the code executed when `throw()` is called, and the red star shows approximately when the error is thrown inside the generator. The first two `yield`statements are executed, and when `throw()` is called, an error is thrown before any other code executes.

Knowing this, you can catch such errors inside the generator using a `try-catch` block:

```js
function *createIterator() {
    let first = yield 1;
    let second;

    try {
        second = yield first + 2;       // yield 4 + 2, then throw
    } catch (ex) {
        second = 6;                     // on error, assign a different value
    }
    yield second + 3;
}

let iterator = createIterator();

console.log(iterator.next());                   // "{ value: 1, done: false }"
console.log(iterator.next(4));                  // "{ value: 6, done: false }"
console.log(iterator.throw(new Error("Boom"))); // "{ value: 9, done: false }"
console.log(iterator.next());                   // "{ value: undefined, done:\
 true }"
```

In this example, a `try-catch` block is wrapped around the second `yield` statement. While this `yield` executes without error, the error is thrown before any value can be assigned to `second`, so the `catch` block assigns it a value of six. Execution then flows to the next `yield` and returns nine.

Notice that something interesting happened: the `throw()` method returned a result object just like the `next()` method. Because the error was caught inside the generator, code execution continued on to the next `yield` and returned the next value, `9`.

It helps to think of `next()` and `throw()` as both being instructions to the iterator. The `next()`method instructs the iterator to continue executing (possibly with a given value) and `throw()`instructs the iterator to continue executing by throwing an error. What happens after that point depends on the code inside the generator.

The `next()` and `throw()` methods control execution inside an iterator when using `yield`, but you can also use the `return` statement. But `return` works a bit differently than it does in regular functions, as you will see in the next section.

#### Generator Return Statements

Since generators are functions, you can use the `return` statement both to exit early and specify a return value for the last call to the `next()` method. In most examples in this chapter, the last call to `next()` on an iterator returns `undefined`, but you can specify an alternate value by using `return` as you would in any other function. In a generator, `return` indicates that all processing is done, so the `done` property is set to `true` and the value, if provided, becomes the `value` field. Here’s an example that simply exits early using `return`:

```js
function *createIterator() {
    yield 1;
    return;
    yield 2;
    yield 3;
}

let iterator = createIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this code, the generator has a `yield` statement followed by a `return` statement. The `return`indicates that there are no more values to come, and so the rest of the `yield` statements will not execute (they are unreachable).

You can also specify a return value that will end up in the `value` field of the returned object. For example:

```js
function *createIterator() {
    yield 1;
    return 42;
}

let iterator = createIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 42, done: true }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

Here, the value `42` is returned in the `value` field on the second call to the `next()` method (which is the first time that `done` is `true`). The third call to `next()` returns an object whose `value` property is once again `undefined`. Any value you specify with `return` is only available on the returned object one time before the `value` field is reset to `undefined`.

The spread operator and `for-of` ignore any value specified by a `return`statement. As soon as they see `done` is `true`, they stop without reading the `value`. Iterator return values are helpful, however, when delegating generators.

#### Delegating Generators

In some cases, combining the values from two iterators into one is useful. Generators can delegate to other iterators using a special form of `yield` with a star (`*`) character. As with generator definitions, where the star appears doesn’t matter, as long as the star falls between the `yield`keyword and the generator function name. Here’s an example:

```js
function *createNumberIterator() {
    yield 1;
    yield 2;
}

function *createColorIterator() {
    yield "red";
    yield "green";
}

function *createCombinedIterator() {
    yield *createNumberIterator();
    yield *createColorIterator();
    yield true;
}

var iterator = createCombinedIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: "red", done: false }"
console.log(iterator.next());           // "{ value: "green", done: false }"
console.log(iterator.next());           // "{ value: true, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this example, the `createCombinedIterator()` generator delegates first to the iterator returned from `createNumberIterator()` and then to the iterator returned from `createColorIterator()`. The iterator returned from `createCombinedIterator()` appears, from the outside, to be one consistent iterator that has produced all of the values. Each call to `next()` is delegated to the appropriate iterator until the iterators created by `createNumberIterator()` and `createColorIterator()` are empty. Then the final `yield` is executed to return `true`.

Generator delegation also lets you make further use of generator return values. This is the easiest way to access such returned values and can be quite useful in performing complex tasks. For example:

```js
function *createNumberIterator() {
    yield 1;
    yield 2;
    return 3;
}

function *createRepeatingIterator(count) {
    for (let i=0; i < count; i++) {
        yield "repeat";
    }
}

function *createCombinedIterator() {
    let result = yield *createNumberIterator();
    yield *createRepeatingIterator(result);
}

var iterator = createCombinedIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

Here, the `createCombinedIterator()` generator delegates to `createNumberIterator()` and assigns the return value to `result`. Since `createNumberIterator()` contains `return 3`, the returned value is `3`. The `result` variable is then passed to `createRepeatingIterator()` as an argument indicating how many times to yield the same string (in this case, three times).

Notice that the value `3` was never output from any call to the `next()` method. Right now, it exists solely inside the `createCombinedIterator()` generator. But you can output that value as well by adding another `yield` statement, such as:

```js
function *createNumberIterator() {
    yield 1;
    yield 2;
    return 3;
}

function *createRepeatingIterator(count) {
    for (let i=0; i < count; i++) {
        yield "repeat";
    }
}

function *createCombinedIterator() {
    let result = yield *createNumberIterator();
    yield result;
    yield *createRepeatingIterator(result);
}

var iterator = createCombinedIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this code, the extra `yield` statement explicitly outputs the returned value from the `createNumberIterator()` generator.

Generator delegation using the return value is a very powerful paradigm that allows for some very interesting possibilities, especially when used in conjunction with asynchronous operations.

You can use `yield *` directly on strings (such as `yield * "hello"`) and the string’s default iterator will be used.

### Asynchronous Task Running

A lot of the excitement around generators is directly related to asynchronous programming. Asynchronous programming in JavaScript is a double-edged sword: simple tasks are easy to do asynchronously, while complex tasks become an errand in code organization. Since generators allow you to effectively pause code in the middle of execution, they open up a lot of possibilities related to asynchronous processing.

The traditional way to perform asynchronous operations is to call a function that has a callback. For example, consider reading a file from the disk in Node.js:

```js
let fs = require("fs");

fs.readFile("config.json", function(err, contents) {
    if (err) {
        throw err;
    }

    doSomethingWith(contents);
    console.log("Done");
});
```

The `fs.readFile()` method is called with the filename to read and a callback function. When the operation is finished, the callback function is called. The callback checks to see if there’s an error, and if not, processes the returned `contents`. This works well when you have a small, finite number of asynchronous tasks to complete, but gets complicated when you need to nest callbacks or otherwise sequence a series of asynchronous tasks. This is where generators and `yield` are helpful.

#### A Simple Task Runner

Because `yield` stops execution and waits for the `next()` method to be called before starting again, you can implement asynchronous calls without managing callbacks. To start, you need a function that can call a generator and start the iterator, such as this:

```js
function run(taskDef) {

    // create the iterator, make available elsewhere
    let task = taskDef();

    // start the task
    let result = task.next();

    // recursive function to keep calling next()
    function step() {

        // if there's more to do
        if (!result.done) {
            result = task.next();
            step();
        }
    }

    // start the process
    step();

}
```

The `run()` function accepts a task definition (a generator function) as an argument. It calls the generator to create an iterator and stores the iterator in `task`. The `task` variable is outside the function so it can be accessed by other functions; I will explain why later in this section. The first call to `next()` begins the iterator and the result is stored for later use. The `step()` function checks to see if `result.done` is false and, if so, calls `next()` before recursively calling itself. Each call to `next()` stores the return value in `result`, which is always overwritten to contain the latest information. The initial call to `step()` starts the process of looking at the `result.done`variable to see whether there’s more to do.

With this implementation of `run()`, you can run a generator containing multiple `yield`statements, such as:

```js
run(function*() {
    console.log(1);
    yield;
    console.log(2);
    yield;
    console.log(3);
});
```

This example just outputs three numbers to the console, which simply shows that all calls to `next()` are being made. However, just yielding a couple of times isn’t very useful. The next step is to pass values into and out of the iterator.

#### Task Running With Data

The easiest way to pass data through the task runner is to pass the value specified by `yield` into the next call to the `next()` method. To do so, you need only pass `result.value`, as in this code:

```js
function run(taskDef) {

    // create the iterator, make available elsewhere
    let task = taskDef();

    // start the task
    let result = task.next();

    // recursive function to keep calling next()
    function step() {

        // if there's more to do
        if (!result.done) {
            result = task.next(result.value);
            step();
        }
    }

    // start the process
    step();

}
```

Now that `result.value` is passed to `next()` as an argument, it’s possible to pass data between `yield` calls, like this:

```js
run(function*() {
    let value = yield 1;
    console.log(value);         // 1

    value = yield value + 3;
    console.log(value);         // 4
});
```

This example outputs two values to the console: 1 and 4. The value 1 comes from `yield 1`, as the 1 is passed right back into the `value` variable. The 4 is calculated by adding 3 to `value` and passing that result back to `value`. Now that data is flowing between calls to `yield`, you just need one small change to allow asynchronous calls.

#### Asynchronous Task Runner

The previous example passed static data back and forth between `yield` calls, but waiting for an asynchronous process is slightly different. The task runner needs to know about callbacks and how to use them. And since `yield` expressions pass their values into the task runner, that means any function call must return a value that somehow indicates the call is an asynchronous operation that the task runner should wait for.

Here’s one way you might signal that a value is an asynchronous operation:

```js
function fetchData() {
    return function(callback) {
        callback(null, "Hi!");
    };
}
```

For the purposes of this example, any function meant to be called by the task runner will return a function that executes a callback. The `fetchData()` function returns a function that accepts a callback function as an argument. When the returned function is called, it executes the callback function with a single piece of data (the `"Hi!"` string). The `callback` argument needs to come from the task runner to ensure executing the callback correctly interacts with the underlying iterator. While the `fetchData()` function is synchronous, you can easily extend it to be asynchronous by calling the callback with a slight delay, such as:

```js
function fetchData() {
    return function(callback) {
        setTimeout(function() {
            callback(null, "Hi!");
        }, 50);
    };
}
```

This version of `fetchData()` introduces a 50ms delay before calling the callback, demonstrating that this pattern works equally well for synchronous and asynchronous code. You just have to make sure each function that wants to be called using `yield` follows the same pattern.

With a good understanding of how a function can signal that it’s an asynchronous process, you can modify the task runner to take that fact into account. Anytime `result.value` is a function, the task runner will execute it instead of just passing that value to the `next()` method. Here’s the updated code:

```js
function run(taskDef) {

    // create the iterator, make available elsewhere
    let task = taskDef();

    // start the task
    let result = task.next();

    // recursive function to keep calling next()
    function step() {

        // if there's more to do
        if (!result.done) {
            if (typeof result.value === "function") {
                result.value(function(err, data) {
                    if (err) {
                        result = task.throw(err);
                        return;
                    }

                    result = task.next(data);
                    step();
                });
            } else {
                result = task.next(result.value);
                step();
            }

        }
    }

    // start the process
    step();

}
```

When `result.value` is a function (checked with the `===` operator), it is called with a callback function. That callback function follows the Node.js convention of passing any possible error as the first argument (`err`) and the result as the second argument. If `err` is present, then that means an error occurred and `task.throw()` is called with the error object instead of `task.next()` so an error is thrown at the correct location. If there is no error, then `data` is passed into `task.next()` and the result is stored. Then, `step()` is called to continue the process. When `result.value` is not a function, it is directly passed to the `next()` method.

This new version of the task runner is ready for all asynchronous tasks. To read data from a file in Node.js, you need to create a wrapper around `fs.readFile()` that returns a function similar to the `fetchData()` function from the beginning of this section. For example:

```js
let fs = require("fs");

function readFile(filename) {
    return function(callback) {
        fs.readFile(filename, callback);
    };
}
```

The `readFile()` method accepts a single argument, the filename, and returns a function that calls a callback. The callback is passed directly to the `fs.readFile()` method, which will execute the callback upon completion. You can then run this task using `yield` as follows:

```js
run(function*() {
    let contents = yield readFile("config.json");
    doSomethingWith(contents);
    console.log("Done");
});
```

This example is performing the asynchronous `readFile()` operation without making any callbacks visible in the main code. Aside from `yield`, the code looks the same as synchronous code. As long as the functions performing asynchronous operations all conform to the same interface, you can write logic that reads like synchronous code.

Of course, there are downsides to the pattern used in these examples–namely that you can’t always be sure a function that returns a function is asynchronous. For now, though, it’s only important that you understand the theory behind the task running. Using promises offers more powerful ways of scheduling asynchronous tasks, and chapter covers this topic further.

### Summary

Iterators are an important part of ECMAScript 6 and are at the root of several key language elements. On the surface, iterators provide a simple way to return a sequence of values using a simple API. However, there are far more complex ways to use iterators in ECMAScript 6.

The `Symbol.iterator` symbol is used to define default iterators for objects. Both built-in objects and developer-defined objects can use this symbol to provide a method that returns an iterator. When `Symbol.iterator` is provided on an object, the object is considered an iterable.

The `for-of` loop uses iterables to return a series of values in a loop. Using `for-of` is easier than iterating with a traditional `for` loop because you no longer need to track values and control when the loop ends. The `for-of` loop automatically reads all values from the iterator until there are no more, and then it exits.

To make `for-of` easier to use, many values in ECMAScript 6 have default iterators. All the collection types–that is, arrays, maps, and sets–have iterators designed to make their contents easy to access. Strings also have a default iterator, which makes iterating over the characters of the string (rather than the code units) easy.

The spread operator works with any iterable and makes converting iterables into arrays easy, too. The conversion works by reading values from an iterator and inserting them individually into an array.

A generator is a special function that automatically creates an iterator when called. Generator definitions are indicated by a star (`*`) character and use of the `yield` keyword to indicate which value to return for each successive call to the `next()` method.

Generator delegation encourages good encapsulation of iterator behavior by letting you reuse existing generators in new generators. You can use an existing generator inside another generator by calling `yield *` instead of `yield`. This process allows you to create an iterator that returns values from multiple iterators.

Perhaps the most interesting and exciting aspect of generators and iterators is the possibility of creating cleaner-looking asynchronous code. Instead of needing to use callbacks everywhere, you can set up code that looks synchronous but in fact uses `yield` to wait for asynchronous operations to complete.

## Introducing JavaScript Classes

Unlike most formal object-oriented programming languages, JavaScript didn’t support classes and classical inheritance as the primary way of defining similar and related objects when it was created. This left many developers confused, and from pre-ECMAScript 1 all the way through ECMAScript 5, many libraries created utilities to make JavaScript look like it supports classes. While some JavaScript developers do feel strongly that the language doesn’t need classes, the number of libraries created specifically for this purpose led to the inclusion of classes in ECMAScript 6.

### Class-Like Structures in ECMAScript 5

In ECMAScript 5 and earlier, JavaScript had no classes. The closest equivalent to a class was creating a constructor and then assigning methods to the constructor’s prototype, an approach typically called creating a custom type. For example:

```js
function PersonType(name) {
    this.name = name;
}

PersonType.prototype.sayName = function() {
    console.log(this.name);
};

let person = new PersonType("Allyson");
person.sayName();   // outputs "Allyson"

console.log(person instanceof PersonType);  // true
console.log(person instanceof Object);      // true
```

### Class Declarations

The simplest class form in ECMAScript 6 is the class declaration, which looks similar to classes in other languages.

#### A Basic Class Declaration

Class declarations begin with the `class` keyword followed by the name of the class. The rest of the syntax looks similar to concise methods in object literals, without requiring commas between them. For example, here’s a simple class declaration:

```js
class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
}

let person = new PersonClass("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"
```

The class declaration `PersonClass` behaves quite similarly to `PersonType` from the previous example. But instead of defining a function as the constructor, class declarations allow you to define the constructor directly inside the class with the special `constructor` method name. Since class methods use the concise syntax, there’s no need to use the `function` keyword. All other method names have no special meaning, so you can add as many methods as you want.

Interestingly, class declarations are just syntactic sugar on top of the existing custom type declarations. The `PersonClass` declaration actually creates a function that has the behavior of the `constructor` method, which is why `typeof PersonClass` gives `"function"` as the result. The `sayName()` method also ends up as a method on `PersonClass.prototype` in this example, similar to the relationship between `sayName()` and `PersonType.prototype` in the previous example. These similarities allow you to mix custom types and classes without worrying too much about which you’re using.

#### Why to Use the Class Syntax

Despite the similarities between classes and custom types, there are some important differences to keep in mind:

1. Class declarations, unlike function declarations, are not hoisted. Class declarations act like `let` declarations and so exist in the temporal dead zone until execution reaches the declaration.
2. All code inside of class declarations runs in strict mode automatically. There’s no way to opt-out of strict mode inside of classes.
3. All methods are non-enumerable. This is a significant change from custom types, where you need to use `Object.defineProperty()` to make a method non-enumerable.
4. All methods lack an internal `[[Construct]]` method and will throw an error if you try to call them with `new`.
5. Calling the class constructor without `new` throws an error.
6. Attempting to overwrite the class name within a class method throws an error.

With all of this in mind, the `PersonClass` declaration from the previous example is directly equivalent to the following code, which doesn’t use the class syntax:

```js
// direct equivalent of PersonClass
let PersonType2 = (function() {

    "use strict";

    const PersonType2 = function(name) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.name = name;
    }

    Object.defineProperty(PersonType2.prototype, "sayName", {
        value: function() {

            // make sure the method wasn't called with new
            if (typeof new.target !== "undefined") {
                throw new Error("Method cannot be called with new.");
            }

            console.log(this.name);
        },
        enumerable: false,
        writable: true,
        configurable: true
    });

    return PersonType2;
}());
```

This example shows that while it’s possible to do everything classes do without using the new syntax, the class syntax simplifies all of the functionality significantly.

### Class Expressions

Classes and functions are similar in that they have two forms: declarations and expressions. Function and class declarations begin with an appropriate keyword (`function` or `class`, respectively) followed by an identifier. Functions have an expression form that doesn’t require an identifier after `function`, and similarly, classes have an expression form that doesn’t require an identifier after `class`. These *class expressions* are designed to be used in variable declarations or passed into functions as arguments.

#### A Basic Class Expression

Here’s the class expression equivalent of the previous `PersonClass` examples, followed by some code that uses it:

```js
let PersonClass = class {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

let person = new PersonClass("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"
```

Whether you use class declarations or class expressions is mostly a matter of style. Unlike function declarations and function expressions, both class declarations and class expressions are not hoisted, and so the choice has little bearing on the runtime behavior of the code.

#### Named Class Expressions

The previous section used an anonymous class expression in the example, but just like function expressions, you can also name class expressions. To do so, include an identifier after the `class` keyword like this:

```js
let PersonClass = class PersonClass2 {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

console.log(typeof PersonClass);        // "function"
console.log(typeof PersonClass2);       // "undefined"
```

In this example, the class expression is named `PersonClass2`. The `PersonClass2` identifier exists only within the class definition so that it can be used inside the class methods (such as the `sayName()` method in this example). Outside the class, `typeof PersonClass2` is `"undefined"` because no `PersonClass2` binding exists there. To understand why this is, look at an equivalent declaration that doesn’t use classes:

```js
// direct equivalent of PersonClass named class expression
let PersonClass = (function() {

    "use strict";

    const PersonClass2 = function(name) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.name = name;
    }

    Object.defineProperty(PersonClass2.prototype, "sayName", {
        value: function() {

            // make sure the method wasn't called with new
            if (typeof new.target !== "undefined") {
                throw new Error("Method cannot be called with new.");
            }

            console.log(this.name);
        },
        enumerable: false,
        writable: true,
        configurable: true
    });

    return PersonClass2;
}());
```

Creating a named class expression slightly changes what’s happening in the JavaScript engine. For class declarations, the outer binding (defined with `let`) has the same name as the inner binding (defined with `const`). A named class expression uses its name in the `const` definition, so `PersonClass2` is defined for use only inside the class.

### Classes as First-Class Citizens

In programming, something is said to be a *first-class* citizen when it can be used as a value, meaning it can be passed into a function, returned from a function, and assigned to a variable. JavaScript functions are first-class citizens (sometimes they’re just called first class functions), and that’s part of what makes JavaScript unique.

ECMAScript 6 continues this tradition by making classes first-class citizens as well. That allows classes to be used in a lot of different ways. For example, they can be passed into functions as arguments:

```js
function createObject(classDef) {
    return new classDef();
}

let obj = createObject(class {
    sayHi() {
        console.log("Hi!");
    }
});

obj.sayHi();        // "Hi!"
```

In this example, the `createObject()` function is called with an anonymous class expression as an argument, creates an instance of that class with `new`, and returns the instance. The variable `obj` then stores the returned instance.

Another interesting use of class expressions is creating singletons by immediately invoking the class constructor. To do so, you must use `new` with a class expression and include parentheses at the end. For example:

```js
let person = new class {
    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(this.name);
    }
}("Nicholas");

person.sayName();       // "Nicholas"
```

Here, an anonymous class expression is created and then executed immediately. This pattern allows you to use the class syntax for creating singletons without leaving a class reference available for inspection. (Remember that `PersonClass` only creates a binding inside of the class, not outside.) The parentheses at the end of the class expression are the indicator that you’re calling a function while also allowing you to pass in an argument.

### Accessor Properties

While own properties should be created inside class constructors, classes allow you to define accessor properties on the prototype. To create a getter, use the keyword `get` followed by a space, followed by an identifier; to create a setter, do the same using the keyword `set`. For example:

```js
class CustomHTMLElement {

    constructor(element) {
        this.element = element;
    }

    get html() {
        return this.element.innerHTML;
    }

    set html(value) {
        this.element.innerHTML = value;
    }
}

var descriptor = Object.getOwnPropertyDescriptor(CustomHTMLElement.prototype, "html");

console.log("get" in descriptor);   // true
console.log("set" in descriptor);   // true
console.log(descriptor.enumerable); // false
```

In this code, the `CustomHTMLElement` class is made as a wrapper around an existing DOM element. It has both a getter and setter for `html` that delegates to the `innerHTML` method on the element itself. This accessor property is created on the `CustomHTMLElement.prototype` and, just like any other method would be, is created as non-enumerable. The equivalent non-class representation is:

```js
// direct equivalent to previous example
let CustomHTMLElement = (function() {
    "use strict";
    const CustomHTMLElement = function(element) {
        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.element = element;
    }

    Object.defineProperty(CustomHTMLElement.prototype, "html", {
        enumerable: false,
        configurable: true,
        get: function() {
            return this.element.innerHTML;
        },
        set: function(value) {
            this.element.innerHTML = value;
        }
    });

    return CustomHTMLElement;
}());
```

### Computed Member Names

The similarities between object literals and classes aren’t quite over yet. Class methods and accessor properties can also have computed names. Instead of using an identifier, use square brackets around an expression, which is the same syntax used for object literal computed names. For example:

```js
let methodName = "sayName";

class PersonClass {

    constructor(name) {
        this.name = name;
    }

    [methodName]() {
        console.log(this.name);
    }
}

let me = new PersonClass("Nicholas");
me.sayName();           // "Nicholas"
```

Accessor properties can use computed names in the same way, like this:

```js
let propertyName = "html";

class CustomHTMLElement {

    constructor(element) {
        this.element = element;
    }

    get [propertyName]() {
        return this.element.innerHTML;
    }

    set [propertyName](value) {
        this.element.innerHTML = value;
    }
}
```

You’ve seen that there are a lot of similarities between classes and object literals, with methods, accessor properties, and computed names. There’s just one more similarity to cover: generators.

### Generator Methods

### Static Members

Adding additional methods directly onto constructors to simulate static members is another common pattern in ECMAScript 5 and earlier. For example:

```js
function PersonType(name) {
    this.name = name;
}

// static method
PersonType.create = function(name) {
    return new PersonType(name);
};

// instance method
PersonType.prototype.sayName = function() {
    console.log(this.name);
};

var person = PersonType.create("Nicholas");
```

ECMAScript 6 classes simplify the creation of static members by using the formal `static` annotation before the method or accessor property name. For instance, here’s the class equivalent of the last example:

```js
class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }

    // equivalent of PersonType.create
    static create(name) {
        return new PersonClass(name);
    }
}

let person = PersonClass.create("Nicholas");
```

*Static members are not accessible from instances. You must always access static members from the class directly.*

### Inheritance with Derived Classes

Prior to ECMAScript 6, implementing inheritance with custom types was an extensive process. Proper inheritance required multiple steps. For instance, consider this example:

```js
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

function Square(length) {
    Rectangle.call(this, length, length);
}

Square.prototype = Object.create(Rectangle.prototype, {
    constructor: {
        value:Square,
        enumerable: true,
        writable: true,
        configurable: true
    }
});

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true
```

Classes make inheritance easier to implement by using the familiar `extends` keyword to specify the function from which the class should inherit. The prototypes are automatically adjusted, and you can access the base class constructor by calling the `super()` method. Here’s the ECMAScript 6 equivalent of the previous example:

```js
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }
}

class Square extends Rectangle {
    constructor(length) {

        // same as Rectangle.call(this, length, length)
        super(length, length);
    }
}

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true
```

Classes that inherit from other classes are referred to as *derived classes*. Derived classes require you to use `super()` if you specify a constructor; if you don’t, an error will occur. If you choose not to use a constructor, then `super()` is automatically called for you with all arguments upon creating a new instance of the class. For instance, the following two classes are identical:

```js
class Square extends Rectangle {
    // no constructor
}

// Is equivalent to

class Square extends Rectangle {
    constructor(...args) {
        super(...args);
    }
}
```

The second class in this example shows the equivalent of the default constructor for all derived classes. All of the arguments are passed, in order, to the base class constructor. In this case, the functionality isn’t quite correct because the `Square` constructor needs only one argument, and so it’s best to manually define the constructor.

There are a few things to keep in mind when using super():

1. You can only use `super()` in a derived class. If you try to use it in a non-derived class (a class that doesn’t use `extends`) or a function, it will throw an error.
2. You must call `super()` before accessing `this` in the constructor. Since `super()` is responsible for initializing `this`, attempting to access `this` before calling `super()` results in an error.
3. The only way to avoid calling `super()` is to return an object from the class constructor.

#### Shadowing Class Methods

The methods on derived classes always shadow methods of the same name on the base class. For instance, you can add `getArea()` to `Square` to redefine that functionality:

```js
class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }

    // override and shadow Rectangle.prototype.getArea()
    getArea() {
        return this.length * this.length;
    }
}
```

Since `getArea()` is defined as part of `Square`, the `Rectangle.prototype.getArea()` method will no longer be called by any instances of `Square`. Of course, you can always decide to call the base class version of the method by using the `super.getArea()` method, like this:

```js
class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }

    // override, shadow, and call Rectangle.prototype.getArea()
    getArea() {
        return super.getArea();
    }
}
```

#### Inherited Static Members

If a base class has static members, then those static members are also available on the derived class. Inheritance works like that in other languages, but this is a new concept for JavaScript. Here’s an example:

```js
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }

    static create(length, width) {
        return new Rectangle(length, width);
    }
}

class Square extends Rectangle {
    constructor(length) {

        // same as Rectangle.call(this, length, length)
        super(length, length);
    }
}

var rect = Square.create(3, 4);

console.log(rect instanceof Rectangle);     // true
console.log(rect.getArea());                // 12
console.log(rect instanceof Square);        // false
```

In this code, a new static `create()` method is added to the Rectangle class. Through inheritance, that method is available as `Square.create()` and behaves in the same manner as the `Rectangle.create()` method.

#### Derived Classes from Expressions

Perhaps the most powerful aspect of derived classes in ECMAScript 6 is the ability to derive a class from an expression. You can use `extends` with any expression as long as the expression resolves to a function with `[[Construct]]` and a prototype. For instance:

```js
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }
}

var x = new Square(3);
console.log(x.getArea());               // 9
console.log(x instanceof Rectangle);    // true
```

Accepting any type of expression after `extends` offers powerful possibilities, such as dynamically determining what to inherit from. For example:

```js
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

function getBase() {
    return Rectangle;
}

class Square extends getBase() {
    constructor(length) {
        super(length, length);
    }
}

var x = new Square(3);
console.log(x.getArea());               // 9
console.log(x instanceof Rectangle);    // true
```

#### Inheriting from Built-ins

For almost as long as JavaScript arrays have existed, developers have wanted to create their own special array types through inheritance. In ECMAScript 5 and earlier, this wasn’t possible. Attempting to use classical inheritance didn’t result in functioning code. For example:

```js
// built-in array behavior
var colors = [];
colors[0] = "red";
console.log(colors.length);         // 1

colors.length = 0;
console.log(colors[0]);             // undefined

// trying to inherit from array in ES5

function MyArray() {
    Array.apply(this, arguments);
}

MyArray.prototype = Object.create(Array.prototype, {
    constructor: {
        value: MyArray,
        writable: true,
        configurable: true,
        enumerable: true
    }
});

var colors = new MyArray();
colors[0] = "red";
console.log(colors.length);         // 0

colors.length = 0;
console.log(colors[0]);             // "red"
```

In ECMAScript 5 classical inheritance, the value of `this` is first created by the derived type (for example, `MyArray`), and then the base type constructor (like the `Array.apply()` method) is called. That means `this` starts out as an instance of `MyArray` and then is decorated with additional properties from `Array`.

In ECMAScript 6 class-based inheritance, the value of `this` is first created by the base (`Array`) and then modified by the derived class constructor (`MyArray`). The result is that `this` starts with all the built-in functionality of the base and correctly receives all functionality related to it.

The following example shows a class-based special array in action:

```js
class MyArray extends Array {
    // empty
}

var colors = new MyArray();
colors[0] = "red";
console.log(colors.length);         // 1

colors.length = 0;
console.log(colors[0]);             // undefined
```

`MyArray` inherits directly from `Array` and therefore works like `Array`. Interacting with numeric properties updates the `length` property, and manipulating the `length` property updates the numeric properties. That means you can both properly inherit from `Array` to create your own derived array classes and inherit from other built-ins as well. With all this added functionality, ECMAScript 6 and derived classes have effectively removed the last special case of inheriting from built-ins, but that case is still worth exploring.

### Using new.target in Class Constructors

You learned about `new.target` and how its value changes depending on how a function is called. You can also use `new.target` in class constructors to determine how the class is being invoked. In the simple case, `new.target` is equal to the constructor function for the class, as in this example:

```js
class Rectangle {
    constructor(length, width) {
        console.log(new.target === Rectangle);
        this.length = length;
        this.width = width;
    }
}

// new.target is Rectangle
var obj = new Rectangle(3, 4);      // outputs true
```

This code shows that `new.target` is equivalent to `Rectangle` when new `Rectangle(3, 4)` is called. Class constructors can’t be called without `new`, so the `new.target` property is always defined inside of class constructors. But the value may not always be the same. Consider this code:

```js
class Rectangle {
    constructor(length, width) {
        console.log(new.target === Rectangle);
        this.length = length;
        this.width = width;
    }
}

class Square extends Rectangle {
    constructor(length) {
        super(length, length)
    }
}

// new.target is Square
var obj = new Square(3);      // outputs false
```

`Square` is calling the `Rectangle` constructor, so `new.target` is equal to `Square` when the `Rectangle` constructor is called. This is important because it gives each constructor the ability to alter its behavior based on how it’s being called. For instance, you can create an abstract base class (one that can’t be instantiated directly) by using `new.target` as follows:

```js
// abstract base class
class Shape {
    constructor() {
        if (new.target === Shape) {
            throw new Error("This class cannot be instantiated directly.")
        }
    }
}

class Rectangle extends Shape {
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }
}

var x = new Shape();                // throws error

var y = new Rectangle(3, 4);        // no error
console.log(y instanceof Shape);    // true
```

*Since classes can’t be called without `new`, the `new.target` property is never `undefined` inside of a class constructor*.

## Improved Array Capabilities

## Promises and Asynchronous Programming

Promises are another option for asynchronous programming, and they work like futures and deferreds do in other languages. A promise specifies some code to be executed later (as with events and callbacks) and also explicitly indicates whether the code succeeded or failed at its job. You can chain promises together based on success or failure in ways that make your code easier to understand and debug.

### Asynchronous Programming Background

JavaScript engines are built on the concept of a single-threaded event loop. Single-threaded means that only one piece of code is ever executed at a time. Contrast this with languages like Java or C++, where threads can allow multiple different pieces of code to execute at the same time. Maintaining and protecting state when multiple pieces of code can access and change that state is a difficult problem and a frequent source of bugs in thread-based software.

JavaScript engines can only execute one piece of code at a time, so they need to keep track of code that is meant to run. That code is kept in a job queue. Whenever a piece of code is ready to be executed, it is added to the job queue. When the JavaScript engine is finished executing code, the event loop executes the next job in the queue. The event loop is a process inside the JavaScript engine that monitors code execution and manages the job queue. Keep in mind that as a queue, job execution runs from the first job in the queue to the last.

### Promise Basics

A promise is a placeholder for the result of an asynchronous operation. Instead of subscribing to an event or passing a callback to a function, the function can return a promise, like this:

```js
// readFile promises to complete at some point in the future
let promise = readFile("example.txt");
```

In this code, `readFile()` doesn’t actually start reading the file immediately; that will happen later. Instead, the function returns a promise object representing the asynchronous read operation so you can work with it in the future. Exactly when you’ll be able to work with that result depends entirely on how the promise’s lifecycle plays out.

#### The Promise Lifecycle

Each promise goes through a short lifecycle starting in the *pending* state, which indicates that the asynchronous operation hasn’t completed yet. A pending promise is considered *unsettled*. The promise in the last example is in the pending state as soon as the `readFile()` function returns it. Once the asynchronous operation completes, the promise is considered *settled* and enters one of two possible states:

1. *Fulfilled*: The promise’s asynchronous operation has completed successfully.
2. *Rejected*: The promise’s asynchronous operation didn’t complete successfully due to either an error or some other cause.

An internal `[[PromiseState]]` property is set to `"pending"`, `"fulfilled"`, or `"rejected"` to reflect the promise’s state. This property isn’t exposed on promise objects, so you can’t determine which state the promise is in programmatically. But you can take a specific action when a promise changes state by using the `then()` method.

The `then()` method is present on all promises and takes two arguments. The first argument is a function to call when the promise is fulfilled. Any additional data related to the asynchronous operation is passed to this fulfillment function. The second argument is a function to call when the promise is rejected. Similar to the fulfillment function, the rejection function is passed any additional data related to the rejection.

*Any object that implements the `then()` method in this way is called a thenable. All promises are thenables, but not all thenables are promises*.

Both arguments to `then()` are optional, so you can listen for any combination of fulfillment and rejection. For example, consider this set of `then()` calls:

```js
let promise = readFile("example.txt");

promise.then(function(contents) {
    // fulfillment
    console.log(contents);
}, function(err) {
    // rejection
    console.error(err.message);
});

promise.then(function(contents) {
    // fulfillment
    console.log(contents);
});

promise.then(null, function(err) {
    // rejection
    console.error(err.message);
});
```

All three `then()` calls operate on the same promise. The first call listens for both fulfillment and rejection. The second only listens for fulfillment; errors won’t be reported. The third just listens for rejection and doesn’t report success.

Promises also have a `catch()` method that behaves the same as `then()` when only a rejection handler is passed. For example, the following `catch()` and `then()` calls are functionally equivalent:

```js
promise.catch(function(err) {
    // rejection
    console.error(err.message);
});

// is the same as:

promise.then(null, function(err) {
    // rejection
    console.error(err.message);
});
```

The intent behind `then()` and `catch()` is for you to use them in combination to properly handle the result of asynchronous operations. This system is better than events and callbacks because it makes whether the operation succeeded or failed completely clear. (Events tend not to fire when there’s an error, and in callbacks you must always remember to check the error argument.) Just know that if you don’t attach a rejection handler to a promise, all failures will happen silently. Always attach a rejection handler, even if the handler just logs the failure.

A fulfillment or rejection handler will still be executed even if it is added to the job queue after the promise is already settled. This allows you to add new fulfillment and rejection handlers at any time and guarantee that they will be called. For example:

```js
let promise = readFile("example.txt");

// original fulfillment handler
promise.then(function(contents) {
    console.log(contents);

    // now add another
    promise.then(function(contents) {
        console.log(contents);
    });
});
```

In this code, the fulfillment handler adds another fulfillment handler to the same promise. The promise is already fulfilled at this point, so the new fulfillment handler is added to the job queue and called when ready. Rejection handlers work the same way.

#### Creating Unsettled Promises

New promises are created using the `Promise` constructor. This constructor accepts a single argument: a function called the *executor*, which contains the code to initialize the promise. The executor is passed two functions named `resolve()` and `reject()` as arguments. The `resolve()` function is called when the executor has finished successfully to signal that the promise is ready to be resolved, while the `reject()` function indicates that the executor has failed.

Here’s an example that uses a promise in Node.js to implement the `readFile()` function from earlier in this chapter:

```js
// Node.js example

let fs = require("fs");

function readFile(filename) {
    return new Promise(function(resolve, reject) {

        // trigger the asynchronous operation
        fs.readFile(filename, { encoding: "utf8" }, function(err, contents) {

            // check for errors
            if (err) {
                reject(err);
                return;
            }

            // the read succeeded
            resolve(contents);

        });
    });
}

let promise = readFile("example.txt");

// listen for both fulfillment and rejection
promise.then(function(contents) {
    // fulfillment
    console.log(contents);
}, function(err) {
    // rejection
    console.error(err.message);
});
```

In this example, the native Node.js `fs.readFile()` asynchronous call is wrapped in a promise. The executor either passes the error object to the `reject()` function or passes the file contents to the `resolve()` function.

The promise executor executes immediately, before anything that appears after it in the source code. For instance:

```js
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

console.log("Hi!");
```

The output for this code is:

```
Promise
Hi!
```

Calling `resolve()` triggers an asynchronous operation. Functions passed to `then()` and `catch()` are executed asynchronously, as these are also added to the job queue. Here’s an example:

```js
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

promise.then(function() {
    console.log("Resolved.");
});

console.log("Hi!");
```

The output for this example is:

```
Promise
Hi!
Resolved
```

Note that even though the call to `then()` appears before the `console.log("Hi!")` line, it doesn’t actually execute until later (unlike the executor). That’s because fulfillment and rejection handlers are always added to the end of the job queue after the executor has completed.

#### Creating Settled Promises

The `Promise` constructor is the best way to create unsettled promises due to the dynamic nature of what the promise executor does. But if you want a promise to represent just a single known value, then it doesn’t make sense to schedule a job that simply passes a value to the `resolve()` function. Instead, there are two methods that create settled promises given a specific value.

**Using Promise.resolve()**

The `Promise.resolve()` method accepts a single argument and returns a promise in the fulfilled state. That means no job scheduling occurs, and you need to add one or more fulfillment handlers to the promise to retrieve the value.

```js
let promise = Promise.resolve(42);

promise.then(function(value) {
    console.log(value);         // 42
});
```

**Using Promise.reject()**

You can also create rejected promises by using the `Promise.reject()` method. This works like `Promise.resolve()` except the created promise is in the rejected state, as follows:

```js
let promise = Promise.reject(42);

promise.catch(function(value) {
    console.log(value);         // 42
});
```

**Non-Promise Thenables**

Both `Promise.resolve()` and `Promise.reject()` also accept non-promise thenables as arguments. When passed a non-promise thenable, these methods create a new promise that is called after the `then()` function.

A non-promise thenable is created when an object has a `then()` method that accepts a `resolve` and a `reject` argument, like this:

```js
let thenable = {
    then: function(resolve, reject) {
        resolve(42);
    }
};
```

The `thenable` object in this example has no characteristics associated with a promise other than the `then()` method. You can call `Promise.resolve()` to convert `thenable` into a fulfilled promise:

```js
let thenable = {
    then: function(resolve, reject) {
        resolve(42);
    }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
    console.log(value);     // 42
});
```

The same process can be used with `Promise.resolve()` to create a `rejected` promise from a thenable:

```js
let thenable = {
    then: function(resolve, reject) {
        reject(42);
    }
};

let p1 = Promise.resolve(thenable);
p1.catch(function(value) {
    console.log(value);     // 42
});
```

`Promise.resolve()` and `Promise.reject()` work like this to allow you to easily work with non-promise thenables. A lot of libraries used thenables prior to promises being introduced in ECMAScript 6, so the ability to convert thenables into formal promises is important for backwards-compatibility with previously existing libraries. When you’re unsure if an object is a promise, passing the object through `Promise.resolve()` or `Promise.reject()` (depending on your anticipated result) is the best way to find out because promises just pass through unchanged.

#### Executor Errors

If an error is thrown inside an executor, then the promise’s rejection handler is called. For example:

```js
let promise = new Promise(function(resolve, reject) {
    throw new Error("Explosion!");
});

promise.catch(function(error) {
    console.log(error.message);     // "Explosion!"
});
```

In this code, the executor intentionally throws an error. There is an implicit `try-catch` inside every executor such that the error is caught and then passed to the rejection handler. The previous example is equivalent to:

```js
let promise = new Promise(function(resolve, reject) {
    try {
        throw new Error("Explosion!");
    } catch (ex) {
        reject(ex);
    }
});

promise.catch(function(error) {
    console.log(error.message);     // "Explosion!"
});
```

The executor handles catching any thrown errors to simplify this common use case, but an error thrown in the executor is only reported when a rejection handler is present. Otherwise, the error is suppressed. This became a problem for developers early on in the use of promises, and JavaScript environments address it by providing hooks for catching rejected promises.

### Global Promise Rejection Handling

### Chaining Promises

Each call to `then()` or `catch()` actually creates and returns another promise. This second promise is resolved only once the first has been fulfilled or rejected. Consider this example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);
}).then(function() {
    console.log("Finished");
});
```

The code outputs:

```js
42
Finished
```

The call to `p1.then()` returns a second promise on which `then()` is called. The second `then()` fulfillment handler is only called after the first promise has been resolved. If you unchain this example, it looks like this:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = p1.then(function(value) {
    console.log(value);
})

p2.then(function() {
    console.log("Finished");
});
```

In this unchained version of the code, the result of `p1.then()` is stored in `p2`, and then `p2.then()` is called to add the final fulfillment handler. As you might have guessed, the call to `p2.then()` also returns a promise. This example just doesn’t use that promise.

#### Catching Errors

Promise chaining allows you to catch errors that may occur in a fulfillment or rejection handler from a previous promise. For example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    throw new Error("Boom!");
}).catch(function(error) {
    console.log(error.message);     // "Boom!"
});
```

In this code, the fulfillment handler for `p1` throws an error. The chained call to the `catch()` method, which is on a second promise, is able to receive that error through its rejection handler. The same is true if a rejection handler throws an error:

```js
let p1 = new Promise(function(resolve, reject) {
    throw new Error("Explosion!");
});

p1.catch(function(error) {
    console.log(error.message);     // "Explosion!"
    throw new Error("Boom!");
}).catch(function(error) {
    console.log(error.message);     // "Boom!"
});
```

#### Returning Values in Promise Chains

Another important aspect of promise chains is the ability to pass data from one promise to the next. You’ve already seen that a value passed to the `resolve()` handler inside an executor is passed to the fulfillment handler for that promise. You can continue passing data along a chain by specifying a return value from the fulfillment handler. For example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);         // "42"
    return value + 1;
}).then(function(value) {
    console.log(value);         // "43"
});
```

The fulfillment handler for `p1` returns `value + 1` when executed. Since `value` is 42 (from the executor), the fulfillment handler returns 43. That value is then passed to the fulfillment handler of the second promise, which outputs it to the console.

You could do the same thing with the rejection handler. When a rejection handler is called, it may return a value. If it does, that value is used to fulfill the next promise in the chain, like this:

```js
let p1 = new Promise(function(resolve, reject) {
    reject(42);
});

p1.catch(function(value) {
    // first fulfillment handler
    console.log(value);         // "42"
    return value + 1;
}).then(function(value) {
    // second fulfillment handler
    console.log(value);         // "43"
});
```

#### Returning Promises in Promise Chains

Returning primitive values from fulfillment and rejection handlers allows passing of data between promises, but what if you return an object? If the object is a promise, then there’s an extra step that’s taken to determine how to proceed. Consider the following example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

p1.then(function(value) {
    // first fulfillment handler
    console.log(value);     // 42
    return p2;
}).then(function(value) {
    // second fulfillment handler
    console.log(value);     // 43
});
```

The important thing to recognize about this pattern is that the second fulfillment handler is not added to `p2`, but rather to a third promise. The second fulfillment handler is therefore attached to that third promise, making the previous example equivalent to this:

```js
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = p1.then(function(value) {
    // first fulfillment handler
    console.log(value);     // 42
    return p2;
});

p3.then(function(value) {
    // second fulfillment handler
    console.log(value);     // 43
});
```

Here, it’s clear that the second fulfillment handler is attached to `p3` rather than `p2`. This is a subtle but important distinction, as the second fulfillment handler will not be called if `p2` is rejected. For instance:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

p1.then(function(value) {
    // first fulfillment handler
    console.log(value);     // 42
    return p2;
}).then(function(value) {
    // second fulfillment handler
    console.log(value);     // never called
});
```

In this example, the second fulfillment handler is never called because `p2` is rejected. You could, however, attach a rejection handler instead:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

p1.then(function(value) {
    // first fulfillment handler
    console.log(value);     // 42
    return p2;
}).catch(function(value) {
    // rejection handler
    console.log(value);     // 43
});
```

Here, the rejection handler is called as a result of `p2` being rejected. The rejected value 43 from `p2` is passed into that rejection handler.

Returning thenables from fulfillment or rejection handlers doesn’t change when the promise executors are executed. The first defined promise will run its executor first, then the second promise executor will run, and so on. Returning thenables simply allows you to define additional responses to the promise results. You defer the execution of fulfillment handlers by creating a new promise within a fulfillment handler. For example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);     // 42

    // create a new promise
    let p2 = new Promise(function(resolve, reject) {
        resolve(43);
    });

    return p2
}).then(function(value) {
    console.log(value);     // 43
});
```

In this example, a new promise is created within the fulfillment handler for `p1`. That means the second fulfillment handler won’t execute until after `p2` is fulfilled. This pattern is useful when you want to wait until a previous promise has been settled before triggering another promise.

### Responding to Multiple Promises

Up to this point, each example in this chapter has dealt with responding to one promise at a time. Sometimes, however, you’ll want to monitor the progress of multiple promises in order to determine the next action. ECMAScript 6 provides two methods that monitor multiple promises: `Promise.all()` and `Promise.race()`.

#### The Promise.all() Method

The `Promise.all()` method accepts a single argument, which is an iterable (such as an array) of promises to monitor, and returns a promise that is resolved only when every promise in the iterable is resolved. The returned promise is fulfilled when every promise in the iterable is fulfilled, as in this example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.then(function(value) {
    console.log(Array.isArray(value));  // true
    console.log(value[0]);              // 42
    console.log(value[1]);              // 43
    console.log(value[2]);              // 44
});
```

If any promise passed to `Promise.all()` is rejected, the returned promise is immediately rejected without waiting for the other promises to complete:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.catch(function(value) {
    console.log(Array.isArray(value))   // false
    console.log(value);                 // 43
});
```

The rejection handler always receives a single value rather than an array, and the value is the rejection value from the promise that was rejected. In this case, the rejection handler is passed 43 to reflect the rejection from `p2`.

#### The Promise.race() Method

The `Promise.race()` method provides a slightly different take on monitoring multiple promises. This method also accepts an iterable of promises to monitor and returns a promise, but the returned promise is settled as soon as the first promise is settled. Instead of waiting for all promises to be fulfilled like the `Promise.all()` method, the `Promise.race()` method returns an appropriate promise as soon as any promise in the array is fulfilled. For example:

```js
let p1 = Promise.resolve(42);

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.race([p1, p2, p3]);

p4.then(function(value) {
    console.log(value);     // 42
});
```

In this code, `p1` is created as a fulfilled promise while the others schedule jobs. The fulfillment handler for `p4` is then called with the value of 42 and ignores the other promises. The promises passed to `Promise.race()` are truly in a race to see which is settled first. If the first promise to settle is fulfilled, then the returned promise is fulfilled; if the first promise to settle is rejected, then the returned promise is rejected. Here’s an example with a rejection:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = Promise.reject(43);

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.race([p1, p2, p3]);

p4.catch(function(value) {
    console.log(value);     // 43
});
```

Here, `p4` is rejected because `p2` is already in the rejected state when `Promise.race()` is called. Even though `p1` and `p3` are fulfilled, those results are ignored because they occur after `p2` is rejected.

### Inheriting from Promises

Just like other built-in types, you can use a promise as the base for a derived class. This allows you to define your own variation of promises to extend what built-in promises can do. Suppose, for instance, you’d like to create a promise that can use methods named `success()` and `failure()` in addition to the usual `then()` and `catch()` methods. You could create that promise type as follows:

```js
class MyPromise extends Promise {
    // use default constructor
    success(resolve, reject) {
        return this.then(resolve, reject);
    }

    failure(reject) {
        return this.catch(reject);
    }
}

let promise = new MyPromise(function(resolve, reject) {
    resolve(42);
});

promise.success(function(value) {
    console.log(value);             // 42
}).failure(function(value) {
    console.log(value);
});
```

In this example, `MyPromise` is derived from Promise and has two additional methods. The `success()` method mimics `resolve()` and `failure()` mimics the `reject()` method.

Each added method uses `this` to call the method it mimics. The derived promise functions the same as a built-in promise, except now you can call `success()` and `failure()` if you want.

Since static methods are inherited, the `MyPromise.resolve()` method, the `MyPromise.reject()` method, the `MyPromise.race()` method, and the `MyPromise.all()` method are also present on derived promises. The last two methods behave the same as the built-in methods, but the first two are slightly different.

Both `MyPromise.resolve()` and `MyPromise.reject()` will return an instance of `MyPromise` regardless of the value passed because those methods use the `Symbol.species` property to determine the type of promise to return. If a built-in promise is passed to either method, the promise will be resolved or rejected, and the method will return a new `MyPromise` so you can assign fulfillment and rejection handlers. For example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = MyPromise.resolve(p1);
p2.success(function(value) {
    console.log(value);         // 42
});

console.log(p2 instanceof MyPromise);   // true
```

Here, `p1` is a built-in promise that is passed to the `MyPromise.resolve()` method. The result, `p2`, is an instance of `MyPromise` where the resolved value from `p1` is passed into the fulfillment handler.

If an instance of `MyPromise` is passed to the `MyPromise.resolve()` or `MyPromise.reject()` methods, it will just be returned directly without being resolved. In all other ways these two methods behave the same as `Promise.resolve()` and `Promise.reject()`.




## Proxies and the Reflection API

ECMAScript 5 and ECMAScript 6 were both developed with demystifying JavaScript functionality in mind. For example, JavaScript environments contained nonenumerable and nonwritable object properties before ECMAScript 5, but developers couldn’t define their own nonenumerable or nonwritable properties. ECMAScript 5 included the `Object.defineProperty()` method to allow developers to do what JavaScript engines could do already.

ECMAScript 6 gives developers further access to JavaScript engine capabilities previously available only to built-in objects. The language exposes the inner workings of objects through  _proxies_, which are wrappers that can intercept and alter low-level operations of the JavaScript engine. This chapter starts by describing the problem that proxies are meant to address in detail, and then discusses how you can create and use proxies effectively.

### The Array Problem

The JavaScript array object behaves in ways that developers couldn’t mimic in their own objects before ECMASCript 6. An array’s `length` property is affected when you assign values to specific array items, and you can modify array items by modifying the `length` property. For example:

```js
let colors = ["red", "green", "blue"];

console.log(colors.length);         // 3

colors[3] = "black";

console.log(colors.length);         // 4
console.log(colors[3]);             // "black"

colors.length = 2;

console.log(colors.length);         // 2
console.log(colors[3]);             // undefined
console.log(colors[2]);             // undefined
console.log(colors[1]);             // "green"
```

The `colors` array starts with three items. Assigning `"black"` to `colors[3]` automatically increments the `length` property to `4`. Setting the `length` property to `2` removes the last two items in the array, leaving only the first two items. Nothing in ECMAScript 5 allows developers to achieve this behavior, but proxies change that.

*This nonstandard behavior is why arrays are considered exotic objects in ECMAScript 6.*

### What are Proxies and Reflection?

You can create a proxy to use in place of another object (called the  _target_) by calling `new Proxy()`. The proxy  _virtualizes_  the target so that the proxy and the target appear to be the same object to functionality using the proxy.

Proxies allow you to intercept low-level object operations on the target that are otherwise internal to the JavaScript engine. These low-level operations are intercepted using a  _trap_, which is a function that responds to a specific operation.

The reflection API, represented by the `Reflect` object, is a collection of methods that provide the default behavior for the same low-level operations that proxies can override. There is a `Reflect`method for every proxy trap. Those methods have the same name and are passed the same arguments as their respective proxy traps. Table 11-1 summarizes this behavior.

Table: Proxy traps in JavaScript

| **Proxy Trap**           | **Overrides the Behavior Of**                                                 | **Default Behavior**                |
|--------------------------|-------------------------------------------------------------------------------|-------------------------------------|
| `get`                      | Reading a property value                                                    | `Reflect.get()`                      |
| `set`                      | Writing to a property                                                       | `Reflect.set()`                      |
| `has`                      | The `in` operator                                                           | `Reflect.has()`                      |
| `deleteProperty`           | The `delete` operator                                                       | `Reflect.deleteProperty()`           |
| `getPrototypeOf`           | `Object.getPrototypeOf()`                                                   | `Reflect.getPrototypeOf()`           |
| `setPrototypeOf`           | `Object.setPrototypeOf()`                                                   | `Reflect.setPrototypeOf()`           |
| `isExtensible`             | `Object.isExtensible()`                                                     | `Reflect.isExtensible()`             |
| `preventExtensions`        | `Object.preventExtensions()`                                                | `Reflect.preventExtensions()`        |
| `getOwnPropertyDescriptor` | `Object.getOwnPropertyDescriptor()`                                         | `Reflect.getOwnPropertyDescriptor()` |
| `defineProperty`            | `Object.defineProperty()`                                                    | `Reflect.defineProperty`              |
| `ownKeys`                  | `Object.keys`, `Object.getOwnPropertyNames()`, `Object.getOwnPropertySymbols()` | `Reflect.ownKeys()`              |
| `apply`                    | Calling a function                                                        | `Reflect.apply()`                      |
| `construct`                | Calling a function with `new`                                             | `Reflect.construct()`                  |

Each trap overrides some built-in behavior of JavaScript objects, allowing you to intercept and modify the behavior. If you still need to use the built-in behavior, then you can use the corresponding reflection API method. The relationship between proxies and the reflection API becomes clear when you start creating proxies, so it’s best to dive in and look at some examples.

*The original ECMAScript 6 specification had an additional trap called `enumerate` that was designed to alter how `for-in` and `Object.keys()`enumerated properties on an object. However, the `enumerate` trap was removed in ECMAScript 7 (also called ECMAScript 2016) as difficulties were discovered during implementation. The `enumerate` trap no longer exists in any JavaScript environment and is therefore not covered in this chapter.*

### Creating a Simple Proxy

When you use the `Proxy` constructor to make a proxy, you’ll pass it two arguments: the target and a handler. A  _handler_  is an object that defines one or more traps. The proxy uses the default behavior for all operations except when traps are defined for that operation. To create a simple forwarding proxy, you can use a handler without any traps:

```js
let target = {};

let proxy = new Proxy(target, {});

proxy.name = "proxy";
console.log(proxy.name);        // "proxy"
console.log(target.name);       // "proxy"

target.name = "target";
console.log(proxy.name);        // "target"
console.log(target.name);       // "target"
```

In this example, `proxy` forwards all operations directly to `target`. When `"proxy"` is assigned to the `proxy.name` property, `name` is created on `target`. The proxy itself is not storing this property; it’s simply forwarding the operation to `target`. Similarly, the values of `proxy.name`and `target.name` are the same because they are both references to `target.name`. That also means setting `target.name` to a new value causes `proxy.name` to reflect the same change. Of course, proxies without traps aren’t very interesting, so what happens when you define a trap?

### Validating Properties Using the `set` Trap

Suppose you want to create an object whose property values must be numbers. That means every new property added to the object must be validated, and an error must be thrown if the value isn’t a number. To accomplish this, you could define a `set` trap that overrides the default behavior of setting a value. The `set` trap receives four arguments:

1. `trapTarget` - the object that will receive the property (the proxy’s target)
2. `key` - the property key (string or symbol) to write to
3. `value` - the value being written to the property
4. `receiver` - the object on which the operation took place (usually the proxy)

`Reflect.set()` is the `set` trap’s corresponding reflection method, and it’s the default behavior for this operation. The `Reflect.set()` method accepts the same four arguments as the `set`proxy trap, making the method easy to use inside of the trap. The trap should return `true` if the property was set or `false` if not. (The `Reflect.set()` method returns the correct value based on whether the operation succeeded.)

To validate the values of properties, you’d use the `set` trap and inspect the `value` that is passed in. Here’s an example:

```js
let target = {
    name: "target"
};

let proxy = new Proxy(target, {
    set(trapTarget, key, value, receiver) {

        // ignore existing properties so as not to affect them
        if (!trapTarget.hasOwnProperty(key)) {
            if (isNaN(value)) {
                throw new TypeError("Property must be a number.");
            }
        }

        // add the property
        return Reflect.set(trapTarget, key, value, receiver);
    }
});

// adding a new property
proxy.count = 1;
console.log(proxy.count);       // 1
console.log(target.count);      // 1

// you can assign to name because it exists on target already
proxy.name = "proxy";
console.log(proxy.name);        // "proxy"
console.log(target.name);       // "proxy"

// throws an error
proxy.anotherName = "proxy";
```

This code defines a proxy trap that validates the value of any new property added to `target`. When `proxy.count = 1` is executed, the `set` trap is called. The `trapTarget` value is equal to `target`, `key` is `"count"`, `value` is `1`, and `receiver` (not used in this example) is `proxy`. There is no existing property named `count` in `target`, so the proxy validates `value` by passing it to `isNaN()`. If the result is `NaN`, then the property value is not numeric and an error is thrown. Since this code sets `count` to `1`, the proxy calls `Reflect.set()` with the same four arguments that were passed to the trap to add the new property.

When `proxy.name` is assigned a string, the operation completes successfully. Since `target`already has a `name` property, that property is omitted from the validation check by calling the `trapTarget.hasOwnProperty()` method. This ensures that previously-existing non-numeric property values are still supported.

When `proxy.anotherName` is assigned a string, however, an error is thrown. The `anotherName`property doesn’t exist on the target, so its value needs to be validated. During validation, the error is thrown because `"proxy"` isn’t a numeric value.

Where the `set` proxy trap lets you intercept when properties are being written to, the `get`proxy trap lets you intercept when properties are being read.

### Object Shape Validation Using the `get` Trap

One of the interesting, and sometimes confusing, aspects of JavaScript is that reading nonexistent properties doesn’t throw an error. Instead, the value `undefined` is used for the property value, as in this example:

```js
let target = {};

console.log(target.name);       // undefined
```

In most other languages, attempting to read `target.name` throws an error because the property doesn’t exist. But JavaScript just uses `undefined` for the value of the `target.name` property. If you’ve ever worked on a large code base, you’ve probably seen how this behavior can cause significant problems, especially when there’s a typo in the property name. Proxies can help you save yourself from this problem by having object shape validation.

An  _object shape_  is the collection of properties and methods available on the object. JavaScript engines use object shapes to optimize code, often creating classes to represent the objects. If you can safely assume an object will always have the same properties and methods it began with (a behavior you can enforce with the `Object.preventExtensions()` method, the `Object.seal()`method, or the `Object.freeze()` method), then throwing an error on attempts to access nonexistent properties can be helpful. Proxies make object shape validation easy.

Since property validation only has to happen when a property is read, you’d use the `get` trap. The `get` trap is called when a property is read, even if that property doesn’t exist on the object, and it takes three arguments:

1. `trapTarget` - the object from which the property is read (the proxy’s target)
2. `key` - the property key (a string or symbol) to read
3. `receiver` - the object on which the operation took place (usually the proxy)

These arguments mirror the `set` trap’s arguments, with one noticeable difference. There’s no `value` argument here because `get` traps don’t write values. The `Reflect.get()` method accepts the same three arguments as the `get` trap and returns the property’s default value.

You can use the `get` trap and `Reflect.get()` to throw an error when a property doesn’t exist on the target, as follows:

```js
let proxy = new Proxy({}, {
        get(trapTarget, key, receiver) {
            if (!(key in receiver)) {
                throw new TypeError("Property " + key + " doesn't exist.");
            }

            return Reflect.get(trapTarget, key, receiver);
        }
    });

// adding a property still works
proxy.name = "proxy";
console.log(proxy.name);            // "proxy"

// nonexistent properties throw an error
console.log(proxy.nme);             // throws error
```

In this example, the `get` trap intercepts property read operations. The `in` operator is used to determine if the property already exists on the `receiver`. The `receiver` is used with `in` instead of `trapTarget` in case `receiver` is a proxy with a `has` trap, a type I’ll cover in the next section. Using `trapTarget` in this case would sidestep the `has` trap and potentially give you the wrong result. An error is thrown if the property doesn’t exist, and otherwise, the default behavior is used.

This code allows new properties like `proxy.name` to be added, written to, and read from with no problems. The last line contains a typo: `proxy.nme` should probably be `proxy.name` instead. This throws an error because `nme` doesn’t exist as a property.

### Hiding Property Existence Using the `has` Trap

The `in` operator determines whether a property exists on a given object and returns `true` if there is either an own property or a prototype property matching the name or symbol. For example:

```js
let target = {
    value: 42;
}

console.log("value" in target);     // true
console.log("toString" in target);  // true
```

Both `value` and `toString` exist on `object`, so in both cases the `in` operator returns `true`. The `value` property is an own property while `toString` is a prototype property (inherited from `Object`). Proxies allow you to intercept this operation and return a different value for `in` with the `has` trap.

The `has` trap is called whenever the `in` operator is used. When called, two arguments are passed to the `has` trap:

1. `trapTarget` - the object the property is read from (the proxy’s target)
2. `key` - the property key (string or symbol) to check

The `Reflect.has()` method accepts these same arguments and returns the default response for the `in` operator. Using the `has` trap and `Reflect.has()` allows you to alter the behavior of `in`for some properties while falling back to default behavior for others. For instance, suppose you just want to hide the `value` property. You can do so like this:

```js
let target = {
    name: "target",
    value: 42
};

let proxy = new Proxy(target, {
    has(trapTarget, key) {

        if (key === "value") {
            return false;
        } else {
            return Reflect.has(trapTarget, key);
        }
    }
});

console.log("value" in proxy);      // false
console.log("name" in proxy);       // true
console.log("toString" in proxy);   // true
```

The `has` trap for `proxy` checks to see if `key` is `"value"` returns `false` if so. Otherwise, the default behavior is used via a call to the `Reflect.has()` method. As a result, the `in` operator returns `false` for the `value` property even though `value` actually exists on the target. The other properties, `name` and `toString`, correctly return `true` when used with the `in` operator.

### Preventing Property Deletion with the `deleteProperty` Trap

The `delete` operator removes a property from an object and returns `true` when successful and `false` when unsuccessful. In strict mode, `delete` throws an error when you attempt to delete a nonconfigurable property; in nonstrict mode, `delete` simply returns `false`. Here’s an example:

```js
let target = {
    name: "target",
    value: 42
};

Object.defineProperty(target, "name", { configurable: false });

console.log("value" in target);     // true

let result1 = delete target.value;
console.log(result1);               // true

console.log("value" in target);     // false

// Note: The following line throws an error in strict mode
let result2 = delete target.name;
console.log(result2);               // false

console.log("name" in target);      // true
```

The `value` property is deleted using the `delete` operator and, as a result, the `in` operator returns `false` in the third `console.log()` call. The nonconfigurable `name` property can’t be deleted so the `delete` operator simply returns `false` (if this code is run in strict mode, an error is thrown instead). You can alter this behavior by using the `deleteProperty` trap in a proxy.

The `deleteProperty` trap is called whenever the `delete` operator is used on an object property. The trap is passed two arguments:

1. `trapTarget` - the object from which the property should be deleted (the proxy’s target)
2. `key` - the property key (string or symbol) to delete

The `Reflect.deleteProperty()` method provides the default implementation of the `deleteProperty` trap and accepts the same two arguments. You can combine `Reflect.deleteProperty()` and the `deleteProperty` trap to change how the `delete` operator behaves. For instance, you could ensure that the `value` property can’t be deleted:

```js
let target = {
    name: "target",
    value: 42
};

let proxy = new Proxy(target, {
    deleteProperty(trapTarget, key) {

        if (key === "value") {
            return false;
        } else {
            return Reflect.deleteProperty(trapTarget, key);
        }
    }
});

// Attempt to delete proxy.value

console.log("value" in proxy);      // true

let result1 = delete proxy.value;
console.log(result1);               // false

console.log("value" in proxy);      // true

// Attempt to delete proxy.name

console.log("name" in proxy);       // true

let result2 = delete proxy.name;
console.log(result2);               // true

console.log("name" in proxy);       // false
```

This code is very similar to the `has` trap example in that the `deleteProperty` trap checks to see if the `key` is `"value"` and returns `false` if so. Otherwise, the default behavior is used by calling the `Reflect.deleteProperty()` method. The `value` property can’t be deleted through `proxy`because the operation is trapped, but the `name` property is deleted as expected. This approach is especially useful when you want to protect properties from deletion without throwing an error in strict mode.

### Prototype Proxy Traps

Chapter introduced the `Object.setPrototypeOf()` method that ECMAScript 6 added to complement the ECMAScript 5 `Object.getPrototypeOf()` method. Proxies allow you to intercept execution of both methods through the `setPrototypeOf` and `getPrototypeOf` traps. In both cases, the method on `Object` calls the trap of the corresponding name on the proxy, allowing you to alter the methods’ behavior.

Since there are two traps associated with prototype proxies, there’s a set of methods associated with each type of trap. The `setPrototypeOf` trap receives these arguments:

1. `trapTarget` - the object for which the prototype should be set (the proxy’s target)
2. `proto` - the object to use for as the prototype

These are the same arguments passed to the `Object.setPrototypeOf()` and `Reflect.setPrototypeOf()` methods. The `getPrototypeOf` trap, on the other hand, only receives the `trapTarget` argument, which is the argument passed to the `Object.getPrototypeOf()` and `Reflect.getPrototypeOf()` methods.

#### How Prototype Proxy Traps Work

There are some restrictions on these traps. First, the `getPrototypeOf` trap must return an object or `null`, and any other return value results in a runtime error. The return value check ensures that `Object.getPrototypeOf()` will always return an expected value. Similarly, the return value of the `setPrototypeOf` trap must be `false` if the operation doesn’t succeed. When `setPrototypeOf` returns `false`, `Object.setPrototypeOf()` throws an error. If `setPrototypeOf` returns any value other than `false`, then `Object.setPrototypeOf()` assumes the operation succeeded.

The following example hides the prototype of the proxy by always returning `null` and also doesn’t allow the prototype to be changed:

```js
let target = {};
let proxy = new Proxy(target, {
    getPrototypeOf(trapTarget) {
        return null;
    },
    setPrototypeOf(trapTarget, proto) {
        return false;
    }
});

let targetProto = Object.getPrototypeOf(target);
let proxyProto = Object.getPrototypeOf(proxy);

console.log(targetProto === Object.prototype);      // true
console.log(proxyProto === Object.prototype);       // false
console.log(proxyProto);                            // null

// succeeds
Object.setPrototypeOf(target, {});

// throws error
Object.setPrototypeOf(proxy, {});
```

This code emphasizes the difference between the behavior of `target` and `proxy`. While `Object.getPrototypeOf()` returns a value for `target`, it returns `null` for `proxy` because the `getPrototypeOf` trap is called. Similarly, `Object.setPrototypeOf()` succeeds when used on `target` but throws an error when used on `proxy` due to the `setPrototypeOf` trap.

If you want to use the default behavior for these two traps, you can use the corresponding methods on `Reflect`. For instance, this code implements the default behavior for the `getPrototypeOf` and `setPrototypeOf` traps:

```js
let target = {};
let proxy = new Proxy(target, {
    getPrototypeOf(trapTarget) {
        return Reflect.getPrototypeOf(trapTarget);
    },
    setPrototypeOf(trapTarget, proto) {
        return Reflect.setPrototypeOf(trapTarget, proto);
    }
});

let targetProto = Object.getPrototypeOf(target);
let proxyProto = Object.getPrototypeOf(proxy);

console.log(targetProto === Object.prototype);      // true
console.log(proxyProto === Object.prototype);       // true

// succeeds
Object.setPrototypeOf(target, {});

// also succeeds
Object.setPrototypeOf(proxy, {});
```

In this example, you can use `target` and `proxy` interchangeably and get the same results because the `getPrototypeOf` and `setPrototypeOf` traps are just passing through to use the default implementation. It’s important that this example use the `Reflect.getPrototypeOf()` and `Reflect.setPrototypeOf()` methods rather than the methods of the same name on `Object` due to some important differences.

#### Why Two Sets of Methods?

The confusing aspect of `Reflect.getPrototypeOf()` and `Reflect.setPrototypeOf()` is that they look suspiciously similar to the `Object.getPrototypeOf()` and `Object.setPrototypeOf()`methods. While both sets of methods perform similar operations, there are some distinct differences between the two.

To begin, `Object.getPrototypeOf()` and `Object.setPrototypeOf()` are higher-level operations that were created for developer use from the start. The `Reflect.getPrototypeOf()` and `Reflect.setPrototypeOf()` methods are lower-level operations that give developers access to the previously internal-only `[[GetPrototypeOf]]` and `[[SetPrototypeOf]]` operations. The `Reflect.getPrototypeOf()` method is the wrapper for the internal `[[GetPrototypeOf]]`operation (with some input validation). The `Reflect.setPrototypeOf()` method and `[[SetPrototypeOf]]` have the same relationship. The corresponding methods on `Object` also call `[[GetPrototypeOf]]` and `[[SetPrototypeOf]]` but perform a few steps before the call and inspect the return value to determine how to behave.

The `Reflect.getPrototypeOf()` method throws an error if its argument is not an object, while `Object.getPrototypeOf()` first coerces the value into an object before performing the operation. If you were to pass a number into each method, you’d get a different result:

```js
let result1 = Object.getPrototypeOf(1);
console.log(result1 === Number.prototype);  // true

// throws an error
Reflect.getPrototypeOf(1);
```

The `Object.getPrototypeOf()` method allows you retrieve a prototype for the number `1`because it first coerces the value into a `Number` object and then returns `Number.prototype`. The `Reflect.getPrototypeOf()` method doesn’t coerce the value, and since `1` isn’t an object, it throws an error.

The `Reflect.setPrototypeOf()` method also has a few more differences from the `Object.setPrototypeOf()` method. First, `Reflect.setPrototypeOf()` returns a boolean value indicating whether the operation was successful. A `true` value is returned for success, and `false` is returned for failure. If `Object.setPrototypeOf()` fails, it throws an error.

As the first example under “How Prototype Proxy Traps Work” showed, when the `setPrototypeOf` proxy trap returns `false`, it causes `Object.setPrototypeOf()` to throw an error. The `Object.setPrototypeOf()` method returns the first argument as its value and therefore isn’t suitable for implementing the default behavior of the `setPrototypeOf` proxy trap. The following code demonstrates these differences:

```js
let target1 = {};
let result1 = Object.setPrototypeOf(target1, {});
console.log(result1 === target1);                   // true

let target2 = {};
let result2 = Reflect.setPrototypeOf(target2, {});
console.log(result2 === target2);                   // false
console.log(result2);                               // true
```

In this example, `Object.setPrototypeOf()` returns `target1` as its value, but `Reflect.setPrototypeOf()` returns `true`. This subtle difference is very important. You’ll see more seemingly duplicate methods on `Object` and `Reflect`, but always be sure to use the method on `Reflect` inside any proxy traps.

Both sets of methods will call the `getPrototypeOf` and `setPrototypeOf` proxy traps when used on a proxy.

### Object Extensibility Traps

ECMAScript 5 added object extensibility modification through the `Object.preventExtensions()`and `Object.isExtensible()` methods, and ECMAScript 6 allows proxies to intercept those method calls to the underlying objects through the `preventExtensions` and `isExtensible` traps. Both traps receive a single argument called `trapTarget` that is the object on which the method was called. The `isExtensible` trap must return a boolean value indicating whether the object is extensible while the `preventExtensions` trap must return a boolean value indicating if the operation succeeded.

There are also `Reflect.preventExtensions()` and `Reflect.isExtensible()` methods to implement the default behavior. Both return boolean values, so they can be used directly in their corresponding traps.

#### Two Basic Examples

To see object extensibility traps in action, consider the following code, which implements the default behavior for the `isExtensible` and `preventExtensions` traps:

```js
let target = {};
let proxy = new Proxy(target, {
    isExtensible(trapTarget) {
        return Reflect.isExtensible(trapTarget);
    },
    preventExtensions(trapTarget) {
        return Reflect.preventExtensions(trapTarget);
    }
});


console.log(Object.isExtensible(target));       // true
console.log(Object.isExtensible(proxy));        // true

Object.preventExtensions(proxy);

console.log(Object.isExtensible(target));       // false
console.log(Object.isExtensible(proxy));        // false
```

This example shows that both `Object.preventExtensions()` and `Object.isExtensible()`correctly pass through from `proxy` to `target`. You can, of course, also change the behavior. For example, if you don’t want to allow `Object.preventExtensions()` to succeed on your proxy, you could return `false` from the `preventExtensions` trap:

```js
let target = {};
let proxy = new Proxy(target, {
    isExtensible(trapTarget) {
        return Reflect.isExtensible(trapTarget);
    },
    preventExtensions(trapTarget) {
        return false
    }
});


console.log(Object.isExtensible(target));       // true
console.log(Object.isExtensible(proxy));        // true

Object.preventExtensions(proxy);

console.log(Object.isExtensible(target));       // true
console.log(Object.isExtensible(proxy));        // true
```

Here, the call to `Object.preventExtensions(proxy)` is effectively ignored because the `preventExtensions` trap returns `false`. The operation isn’t forwarded to the underlying `target`, so `Object.isExtensible()` returns `true`.

#### Duplicate Extensibility Methods

You may have noticed that, once again, there are seemingly duplicate methods on `Object` and `Reflect`. In this case, they’re more similar than not. The methods `Object.isExtensible()` and `Reflect.isExtensible()` are similar except when passed a non-object value. In that case, `Object.isExtensible()` always returns `false` while `Reflect.isExtensible()` throws an error. Here’s an example of that behavior:

```js
let result1 = Object.isExtensible(2);
console.log(result1);                       // false

// throws error
let result2 = Reflect.isExtensible(2);
```

This restriction is similar to the difference between the `Object.getPrototypeOf()` and `Reflect.getPrototypeOf()` methods, as the method with lower-level functionality has stricter error checks than its higher-level counterpart.

The `Object.preventExtensions()` and `Reflect.preventExtensions()` methods are also very similar. The `Object.preventExtensions()` method always returns the value that was passed to it as an argument even if the value isn’t an object. The `Reflect.preventExtensions()` method, on the other hand, throws an error if the argument isn’t an object; if the argument is an object, then `Reflect.preventExtensions()` returns `true` when the operation succeeds or `false` if not. For example:

```js
let result1 = Object.preventExtensions(2);
console.log(result1);                               // 2

let target = {};
let result2 = Reflect.preventExtensions(target);
console.log(result2);                               // true

// throws error
let result3 = Reflect.preventExtensions(2);
```

Here, `Object.preventExtensions()` passes through the value `2` as its return value even though `2` isn’t an object. The `Reflect.preventExtensions()` method returns `true` when an object is passed to it and throws an error when `2` is passed to it.

### Property Descriptor Traps

One of the most important features of ECMAScript 5 was the ability to define property attributes using the `Object.defineProperty()` method. In previous versions of JavaScript, there was no way to define an accessor property, make a property read-only, or make a property nonenumerable. All of these are possible with the `Object.defineProperty()` method, and you can retrieve those attributes with the `Object.getOwnPropertyDescriptor()` method.

Proxies let you intercept calls to `Object.defineProperty()` and `Object.getOwnPropertyDescriptor()` using the `defineProperty` and `getOwnPropertyDescriptor` traps, respectively. The `defineProperty` trap receives the following arguments:

1. `trapTarget` - the object on which the property should be defined (the proxy’s target)
2. `key` - the string or symbol for the property
3. `descriptor` - the descriptor object for the property

The `defineProperty` trap requires you to return `true` if the operation is successful and `false`if not. The `getOwnPropertyDescriptor` traps receives only `trapTarget` and `key`, and you are expected to return the descriptor. The corresponding `Reflect.defineProperty()` and `Reflect.getOwnPropertyDescriptor()` methods accept the same arguments as their proxy trap counterparts. Here’s an example that just implements the default behavior for each trap:

```js
let proxy = new Proxy({}, {
    defineProperty(trapTarget, key, descriptor) {
        return Reflect.defineProperty(trapTarget, key, descriptor);
    },
    getOwnPropertyDescriptor(trapTarget, key) {
        return Reflect.getOwnPropertyDescriptor(trapTarget, key);
    }
});


Object.defineProperty(proxy, "name", {
    value: "proxy"
});

console.log(proxy.name);            // "proxy"

let descriptor = Object.getOwnPropertyDescriptor(proxy, "name");

console.log(descriptor.value);      // "proxy"
```

This code defines a property called `"name"` on the proxy with the `Object.defineProperty()`method. The property descriptor for that property is then retrieved by the `Object.getOwnPropertyDescriptor()` method.

#### Blocking Object.defineProperty()

The `defineProperty` trap requires you to return a boolean value to indicate whether the operation was successful. When `true` is returned, `Object.defineProperty()` succeeds as usual; when `false` is returned, `Object.defineProperty()` throws an error. You can use this functionality to restrict the kinds of properties that the `Object.defineProperty()` method can define. For instance, if you want to prevent symbol properties from being defined, you could check that the key is a string and return `false` if not, like this:

```js
let proxy = new Proxy({}, {
    defineProperty(trapTarget, key, descriptor) {

        if (typeof key === "symbol") {
            return false;
        }

        return Reflect.defineProperty(trapTarget, key, descriptor);
    }
});


Object.defineProperty(proxy, "name", {
    value: "proxy"
});

console.log(proxy.name);                    // "proxy"

let nameSymbol = Symbol("name");

// throws error
Object.defineProperty(proxy, nameSymbol, {
    value: "proxy"
});
```

The `defineProperty` proxy trap returns `false` when `key` is a symbol and otherwise proceeds with the default behavior. When `Object.defineProperty()` is called with `"name"` as the key, the method succeeds because the key is a string. When `Object.defineProperty()` is called with `nameSymbol`, it throws an error because the `defineProperty` trap returns `false`.

You can also have `Object.defineProperty()` silently fail by returning `true`and not calling the `Reflect.defineProperty()` method. That will suppress the error while not actually defining the property.

#### Descriptor Object Restrictions

To ensure consistent behavior when using the `Object.defineProperty()` and `Object.getOwnPropertyDescriptor()` methods, descriptor objects passed to the `defineProperty` trap are normalized. Objects returned from `getOwnPropertyDescriptor` trap are always validated for the same reason.

No matter what object is passed as the third argument to the `Object.defineProperty()` method, only the properties `enumerable`, `configurable`, `value`, `writable`, `get`, and `set` will be on the descriptor object passed to the `defineProperty` trap. For example:

```js
let proxy = new Proxy({}, {
    defineProperty(trapTarget, key, descriptor) {
        console.log(descriptor.value);              // "proxy"
        console.log(descriptor.name);               // undefined

        return Reflect.defineProperty(trapTarget, key, descriptor);
    }
});


Object.defineProperty(proxy, "name", {
    value: "proxy",
    name: "custom"
});
```

Here, `Object.defineProperty()` is called with a nonstandard `name` property on the third argument. When the `defineProperty` trap is called, the `descriptor` object doesn’t have a `name`property but does have a `value` property. That’s because `descriptor` isn’t a reference to the actual third argument passed to the `Object.defineProperty()` method, but rather a new object that contains only the allowable properties. The `Reflect.defineProperty()` method also ignores any nonstandard properties on the descriptor.

The `getOwnPropertyDescriptor` trap has a slightly different restriction that requires the return value to be `null`, `undefined`, or an object. If an object is returned, only `enumerable`, `configurable`, `value`, `writable`, `get`, and `set` are allowed as own properties of the object. An error is thrown if you return an object with an own property that isn’t allowed, as this code shows:

```js
let proxy = new Proxy({}, {
    getOwnPropertyDescriptor(trapTarget, key) {
        return {
            name: "proxy"
        };
    }
});

// throws error
let descriptor = Object.getOwnPropertyDescriptor(proxy, "name");
```

The property `name` isn’t allowable on property descriptors, so when `Object.getOwnPropertyDescriptor()` is called, the `getOwnPropertyDescriptor` return value triggers an error. This restriction ensures that the value returned by `Object.getOwnPropertyDescriptor()` always has a reliable structure regardless of use on proxies.

#### Duplicate Descriptor Methods

Once again, ECMAScript 6 has some confusingly similar methods, as the `Object.defineProperty()` and `Object.getOwnPropertyDescriptor()` methods appear to do the same thing as the `Reflect.defineProperty()` and `Reflect.getOwnPropertyDescriptor()`methods, respectively. Like other method pairs discussed earlier in this chapter, these have some subtle but important differences.

##### defineProperty() Methods

The `Object.defineProperty()` and `Reflect.defineProperty()` methods are exactly the same except for their return values. The `Object.defineProperty()` method returns the first argument, while `Reflect.defineProperty()` returns `true` if the operation succeeded and `false` if not. For example:

```js
let target = {};

let result1 = Object.defineProperty(target, "name", { value: "target "});

console.log(target === result1);        // true

let result2 = Reflect.defineProperty(target, "name", { value: "reflect" });

console.log(result2);                   // true
```

When `Object.defineProperty()` is called on `target`, the return value is `target`. When `Reflect.defineProperty()` is called on `target`, the return value is `true`, indicating that the operation succeeded. Since the `defineProperty` proxy trap requires a boolean value to be returned, it’s better to use `Reflect.defineProperty()` to implement the default behavior when necessary.

##### getOwnPropertyDescriptor() Methods

The `Object.getOwnPropertyDescriptor()` method coerces its first argument into an object when a primitive value is passed and then continues the operation. On the other hand, the `Reflect.getOwnPropertyDescriptor()` method throws an error if the first argument is a primitive value. Here’s an example showing both:

```js
let descriptor1 = Object.getOwnPropertyDescriptor(2, "name");
console.log(descriptor1);       // undefined

// throws an error
let descriptor2 = Reflect.getOwnPropertyDescriptor(2, "name");
```

The `Object.getOwnPropertyDescriptor()` method returns `undefined` because it coerces `2` into an object, and that object has no `name` property. This is the standard behavior of the method when a property with the given name isn’t found on an object. When `Reflect.getOwnPropertyDescriptor()` is called, however, an error is thrown immediately because that method doesn’t accept primitive values for the first argument.

### The `ownKeys` Trap

The `ownKeys` proxy trap intercepts the internal method `[[OwnPropertyKeys]]` and allows you to override that behavior by returning an array of values. This array is used in four methods: the `Object.keys()` method, the `Object.getOwnPropertyNames()` method, the `Object.getOwnPropertySymbols()` method, and the `Object.assign()` method. (The `Object.assign()` method uses the array to determine which properties to copy.)

The default behavior for the `ownKeys` trap is implemented by the `Reflect.ownKeys()` method and returns an array of all own property keys, including both strings and symbols. The `Object.getOwnProperyNames()` method and the `Object.keys()` method filter symbols out of the array and returns the result while `Object.getOwnPropertySymbols()` filters the strings out of the array and returns the result. The `Object.assign()` method uses the array with both strings and symbols.

The `ownKeys` trap receives a single argument, the target, and must always return an array or array-like object; otherwise, an error is thrown. You can use the `ownKeys` trap to, for example, filter out certain property keys that you don’t want used when the `Object.keys()`, the `Object.getOwnPropertyNames()` method, the `Object.getOwnPropertySymbols()` method, or the `Object.assign()` method is used. Suppose you don’t want to include any property names that begin with an underscore character, a common notation in JavaScript indicating that a field is private. You can use the `ownKeys` trap to filter out those keys as follows:

```js
let proxy = new Proxy({}, {
    ownKeys(trapTarget) {
        return Reflect.ownKeys(trapTarget).filter(key => {
            return typeof key !== "string" || key[0] !== "_";
        });
    }
});

let nameSymbol = Symbol("name");

proxy.name = "proxy";
proxy._name = "private";
proxy[nameSymbol] = "symbol";

let names = Object.getOwnPropertyNames(proxy),
    keys = Object.keys(proxy);
    symbols = Object.getOwnPropertySymbols(proxy);

console.log(names.length);      // 1
console.log(names[0]);          // "name"

console.log(keys.length);      // 1
console.log(keys[0]);          // "name"

console.log(symbols.length);    // 1
console.log(symbols[0]);        // "Symbol(name)"
```

This example uses an `ownKeys` trap that first calls `Reflect.ownKeys()` to get the default list of keys for the target. Then, the `filter()` method is used to filter out keys that are strings and begin with an underscore character. Then, three properties are added to the `proxy` object: `name`, `_name`, and `nameSymbol`. When `Object.getOwnPropertyNames()` and `Object.keys()` is called on `proxy`, only the `name` property is returned. Similarly, only `nameSymbol` is returned when `Object.getOwnPropertySymbols()` is called on `proxy`. The `_name` property doesn’t appear in either result because it is filtered out.

The `ownKeys` trap also affects the `for-in` loop, which calls the trap to determine which keys to use inside of the loop.

### Function Proxies with the `apply` and `construct` Traps

Of all the proxy traps, only `apply` and `construct` require the proxy target to be a function. Recall that functions have two internal methods called `[[Call]]` and `[[Construct]]` that are executed when a function is called without and with the `new` operator, respectively. The `apply` and `construct` traps correspond to and let you override those internal methods. When a function is called without `new`, the `apply` trap receives, and `Reflect.apply()`expects, the following arguments:

1. `trapTarget` - the function being executed (the proxy’s target)
2. `thisArg` - the value of `this` inside of the function during the call
3. `argumentsList` - an array of arguments passed to the function

The `construct` trap, which is called when the function is executed using `new`, receives the following arguments:

1. `trapTarget` - the function being executed (the proxy’s target)
2. `argumentsList` - an array of arguments passed to the function

The `Reflect.construct()` method also accepts these two arguments and has an optional third argument called `newTarget`. When given, the `newTarget` argument specifies the value of `new.target` inside of the function.

Together, the `apply` and `construct` traps completely control the behavior of any proxy target function. To mimic the default behavior of a function, you can do this:

```js
let target = function() { return 42 },
    proxy = new Proxy(target, {
        apply: function(trapTarget, thisArg, argumentList) {
            return Reflect.apply(trapTarget, thisArg, argumentList);
        },
        construct: function(trapTarget, argumentList) {
            return Reflect.construct(trapTarget, argumentList);
        }
    });

// a proxy with a function as its target looks like a function
console.log(typeof proxy);                  // "function"

console.log(proxy());                       // 42

var instance = new proxy();
console.log(instance instanceof proxy);     // true
console.log(instance instanceof target);    // true
```

This example has a function that returns the number 42. The proxy for that function uses the `apply` and `construct` traps to delegate those behaviors to the `Reflect.apply()` and `Reflect.construct()` methods, respectively. The end result is that the proxy function works exactly like the target function, including identifying itself as a function when `typeof` is used. The proxy is called without `new` to return 42 and then is called with `new` to create an object called `instance`. The `instance` object is considered an instance of both `proxy` and `target`because `instanceof` uses the prototype chain to determine this information. Prototype chain lookup is not affected by this proxy, which is why `proxy` and `target` appear to have the same prototype to the JavaScript engine.

#### Validating Function Parameters

The `apply` and `construct` traps open up a lot of possibilities for altering the way a function is executed. For instance, suppose you want to validate that all arguments are of a specific type. You can check the arguments in the `apply` trap:

```js
// adds together all arguments
function sum(...values) {
    return values.reduce((previous, current) => previous + current, 0);
}

let sumProxy = new Proxy(sum, {
        apply: function(trapTarget, thisArg, argumentList) {

            argumentList.forEach((arg) => {
                if (typeof arg !== "number") {
                    throw new TypeError("All arguments must be numbers.");
                }
            });

            return Reflect.apply(trapTarget, thisArg, argumentList);
        },
        construct: function(trapTarget, argumentList) {
            throw new TypeError("This function can't be called with new.");
        }
    });

console.log(sumProxy(1, 2, 3, 4));          // 10

// throws error
console.log(sumProxy(1, "2", 3, 4));

// also throws error
let result = new sumProxy();
```

This example uses the `apply` trap to ensure that all arguments are numbers. The `sum()` function adds up all of the arguments that are passed. If a non-number value is passed, the function will still attempt the operation, which can cause unexpected results. By wrapping `sum()` inside the `sumProxy()` proxy, this code intercepts function calls and ensures that each argument is a number before allowing the call to proceed. To be safe, the code also uses the `construct` trap to ensure that the function can’t be called with `new`.

You can also do the opposite, ensuring that a function must be called with `new` and validating its arguments to be numbers:

```js
function Numbers(...values) {
    this.values = values;
}

let NumbersProxy = new Proxy(Numbers, {

        apply: function(trapTarget, thisArg, argumentList) {
            throw new TypeError("This function must be called with new.");
        },

        construct: function(trapTarget, argumentList) {
            argumentList.forEach((arg) => {
                if (typeof arg !== "number") {
                    throw new TypeError("All arguments must be numbers.");
                }
            });

            return Reflect.construct(trapTarget, argumentList);
        }
    });

let instance = new NumbersProxy(1, 2, 3, 4);
console.log(instance.values);               // [1,2,3,4]

// throws error
NumbersProxy(1, 2, 3, 4);
```

Here, the `apply` trap throws an error while the `construct` trap uses the `Reflect.construct()`method to validate input and return a new instance. Of course, you can accomplish the same thing without proxies using `new.target` instead.

#### Calling Constructors Without new

Chapter 3 introduced the `new.target` metaproperty. To review, `new.target` is a reference to the function on which `new` is called, meaning that you can tell if a function was called using `new` or not by checking the value of `new.target` like this:

```js
function Numbers(...values) {

    if (typeof new.target === "undefined") {
        throw new TypeError("This function must be called with new.");
    }

    this.values = values;
}

let instance = new Numbers(1, 2, 3, 4);
console.log(instance.values);               // [1,2,3,4]

// throws error
Numbers(1, 2, 3, 4);
```

This example throws an error when `Numbers` is called without using `new`, which is similar to the example in the “Validating Function Parameters” section but doesn’t use a proxy. Writing code like this is much simpler than using a proxy and is preferable if your only goal is to prevent calling the function without `new`. But sometimes you aren’t in control of the function whose behavior needs to be modified. In that case, using a proxy makes sense.

Suppose the `Numbers` function is defined in code you can’t modify. You know that the code relies on `new.target` and want to avoid that check while still calling the function. The behavior when using `new` is already set, so you can just use the `apply` trap:

```js
function Numbers(...values) {

    if (typeof new.target === "undefined") {
        throw new TypeError("This function must be called with new.");
    }

    this.values = values;
}


let NumbersProxy = new Proxy(Numbers, {
        apply: function(trapTarget, thisArg, argumentsList) {
            return Reflect.construct(trapTarget, argumentsList);
        }
    });


let instance = NumbersProxy(1, 2, 3, 4);
console.log(instance.values);               // [1,2,3,4]
```

The `NumbersProxy` function allows you to call `Numbers` without using `new` and have it behave as if `new` were used. To do so, the `apply` trap calls `Reflect.construct()` with the arguments passed into `apply`. The `new.target` inside of `Numbers` is equal to `Numbers` itself, and no error is thrown. While this is a simple example of modifying `new.target`, you can also do so more directly.

#### Overriding Abstract Base Class Constructors

You can go one step further and specify the third argument to `Reflect.construct()` as the specific value to assign to `new.target`. This is useful when a function is checking `new.target`against a known value, such as when creating an abstract base class constructor. In an abstract base class constructor, `new.target` is expected to be something other than the class constructor itself, as in this example:

```js
class AbstractNumbers {

    constructor(...values) {
        if (new.target === AbstractNumbers) {
            throw new TypeError("This function must be inherited from.");
        }

        this.values = values;
    }
}

class Numbers extends AbstractNumbers {}

let instance = new Numbers(1, 2, 3, 4);
console.log(instance.values);           // [1,2,3,4]

// throws error
new AbstractNumbers(1, 2, 3, 4);
```

When `new AbstractNumbers()` is called, `new.target` is equal to `AbstractNumbers` and an error is thrown. Calling `new Numbers()` still works because `new.target` is equal to `Numbers`. You can bypass this restriction by manually assigning `new.target` with a proxy:

```js
class AbstractNumbers {

    constructor(...values) {
        if (new.target === AbstractNumbers) {
            throw new TypeError("This function must be inherited from.");
        }

        this.values = values;
    }
}

let AbstractNumbersProxy = new Proxy(AbstractNumbers, {
        construct: function(trapTarget, argumentList) {
            return Reflect.construct(trapTarget, argumentList, function() {});
        }
    });


let instance = new AbstractNumbersProxy(1, 2, 3, 4);
console.log(instance.values);               // [1,2,3,4]
```

The `AbstractNumbersProxy` uses the `construct` trap to intercept the call to the `new AbstractNumbersProxy()` method. Then, the `Reflect.construct()` method is called with arguments from the trap and adds an empty function as the third argument. That empty function is used as the value of `new.target` inside of the constructor. Because `new.target` is not equal to `AbstractNumbers`, no error is thrown and the constructor executes completely.

#### Callable Class Constructors

Chapter 9 explained that class constructors must always be called with `new`. That happens because the internal `[[Call]]` method for class constructors is specified to throw an error. But proxies can intercept calls to the `[[Call]]` method, meaning you can effectively create callable class constructors by using a proxy. For instance, if you want a class constructor to work without using `new`, you can use the `apply` trap to create a new instance. Here’s some sample code:

```js
class Person {
    constructor(name) {
        this.name = name;
    }
}

let PersonProxy = new Proxy(Person, {
        apply: function(trapTarget, thisArg, argumentList) {
            return new trapTarget(...argumentList);
        }
    });


let me = PersonProxy("Nicholas");
console.log(me.name);                   // "Nicholas"
console.log(me instanceof Person);      // true
console.log(me instanceof PersonProxy); // true
```

The `PersonProxy` object is a proxy of the `Person` class constructor. Class constructors are just functions, so they behave like functions when used in proxies. The `apply` trap overrides the default behavior and instead returns a new instance of `trapTarget` that’s equal to `Person`. (I used `trapTarget` in this example to show that you don’t need to manually specify the class.) The `argumentList` is passed to `trapTarget` using the spread operator to pass each argument separately. Calling `PersonProxy()` without using `new` returns an instance of `Person`; if you attempt to call `Person()` without `new`, the constructor will still throw an error. Creating callable class constructors is something that is only possible using proxies.

### Revocable Proxies

Normally, a proxy can’t be unbound from its target once the proxy has been created. All of the examples to this point in this chapter have used nonrevocable proxies. But there may be situations when you want to revoke a proxy so that it can no longer be used. You’ll find it most helpful to revoke proxies when you want to provide an object through an API for security purposes and maintain the ability to cut off access to some functionality at any point in time.

You can create revocable proxies with the `Proxy.revocable()` method, which takes the same arguments as the `Proxy` constructor–a target object and the proxy handler. The return value is an object with the following properties:

1. `proxy` - the proxy object that can be revoked
2. `revoke` - the function to call to revoke the proxy

When the `revoke()` function is called, no further operations can be performed through the `proxy`. Any attempt to interact with the proxy object in a way that would trigger a proxy trap throws an error. For example:

```js
let target = {
    name: "target"
};

let { proxy, revoke } = Proxy.revocable(target, {});

console.log(proxy.name);        // "target"

revoke();

// throws error
console.log(proxy.name);
```

This example creates a revocable proxy. It uses destructuring to assign the `proxy` and `revoke`variables to the properties of the same name on the object returned by the `Proxy.revocable()`method. After that, the `proxy` object can be used just like a nonrevocable proxy object, so `proxy.name` returns `"target"` because it passes through to `target.name`. Once the `revoke()`function is called, however, `proxy` no longer functions. Attempting to access `proxy.name` throws an error, as will any other operation that would trigger a trap on `proxy`.

### Solving the Array Problem

At the beginning of this chapter, I explained how developers couldn’t mimic the behavior of an array accurately in JavaScript prior to ECMAScript 6. Proxies and the reflection API allow you to create an object that behaves in the same manner as the built-in `Array` type when properties are added and removed. To refresh your memory, here’s an example showing the behavior that proxies help to mimick:

```js
let colors = ["red", "green", "blue"];

console.log(colors.length);         // 3

colors[3] = "black";

console.log(colors.length);         // 4
console.log(colors[3]);             // "black"

colors.length = 2;

console.log(colors.length);         // 2
console.log(colors[3]);             // undefined
console.log(colors[2]);             // undefined
console.log(colors[1]);             // "green"
```

There are two particularly important behaviors to notice in this example:

1.  The `length` property is increased to 4 when `colors[3]` is assigned a value.
2.  The last two items in the array are deleted when the `length` property is set to 2.

These two behaviors are the only ones that need to be mimicked to accurately recreate how built-in arrays work. The next few sections describe how to make an object that correctly mimics them.

#### Detecting Array Indices

Keep in mind that assigning to an integer property key is a special case for arrays, as those are treated differently from non-integer keys. The ECMAScript 6 specification gives these instructions on how to determine if a property key is an array index:

> A String property name `P` is an array index if and only if `ToString(ToUint32(P))` is equal to `P` and `ToUint32(P)` is not equal to 232-1.

This operation can be implemented in JavaScript as follows:

```js
function toUint32(value) {
    return Math.floor(Math.abs(Number(value))) % Math.pow(2, 32);
}

function isArrayIndex(key) {
    let numericKey = toUint32(key);
    return String(numericKey) == key && numericKey < (Math.pow(2, 32) - 1);
}
```

The `toUint32()` function converts a given value into an unsigned 32-bit integer using an algorithm described in the specification. The `isArrayIndex()` function first converts the key into a uint32 and then performs the comparisons to determine if the key is an array index or not. With these utility functions available, you can start to implement an object that will mimic a built-in array.

#### Increasing length when Adding New Elements

You might have noticed that both array behaviors I described rely on the assignment of a property. That means you really only need to use the `set` proxy trap to accomplish both behaviors. To get started, here’s an example that implements the first of the two behaviors by incrementing the `length` property when an array index larger than `length - 1` is used:

```js
function toUint32(value) {
    return Math.floor(Math.abs(Number(value))) % Math.pow(2, 32);
}

function isArrayIndex(key) {
    let numericKey = toUint32(key);
    return String(numericKey) == key && numericKey < (Math.pow(2, 32) - 1);
}

function createMyArray(length=0) {
    return new Proxy({ length }, {
        set(trapTarget, key, value) {

            let currentLength = Reflect.get(trapTarget, "length");

            // the special case
            if (isArrayIndex(key)) {
                let numericKey = Number(key);

                if (numericKey >= currentLength) {
                    Reflect.set(trapTarget, "length", numericKey + 1);
                }
            }

            // always do this regardless of key type
            return Reflect.set(trapTarget, key, value);
        }
    });
}

let colors = createMyArray(3);
console.log(colors.length);         // 3

colors[0] = "red";
colors[1] = "green";
colors[2] = "blue";

console.log(colors.length);         // 3

colors[3] = "black";

console.log(colors.length);         // 4
console.log(colors[3]);             // "black"
```

This example uses the `set` proxy trap to intercept the setting of an array index. If the key is an array index, then it is converted into a number because keys are always passed as strings. Next, if that numeric value is greater than or equal to the current `length` property, then the `length`property is updated to be one more than the numeric key (setting an item in position 3 means the `length` must be 4). After that, the default behavior for setting a property is used via `Reflect.set()`, since you do want the property to receive the value as specified.

The initial custom array is created by calling `createMyArray()` with a `length` of 3 and the values for those three items are added immediately afterward. The `length` property correctly remains 3 until the value `"black"` is assigned to position 3. At that point, `length` is set to 4.

With the first behavior working, it’s time to move on to the second.

#### Deleting Elements on Reducing length

The first array behavior to mimic is used only when an array index is greater than or equal to the `length` property. The second behavior does the opposite and removes array items when the `length` property is set to a smaller value than it previously contained. That involves not only changing the `length` property, but also deleting all items that might otherwise exist. For instance, if an array with a `length` of 4 has `length` set to 2, the items in positions 2 and 3 are deleted. You can accomplish this inside the `set` proxy trap alongside the first behavior. Here’s the previous example again, with an updated `createMyArray` method:

```js
function toUint32(value) {
    return Math.floor(Math.abs(Number(value))) % Math.pow(2, 32);
}

function isArrayIndex(key) {
    let numericKey = toUint32(key);
    return String(numericKey) == key && numericKey < (Math.pow(2, 32) - 1);
}

function createMyArray(length=0) {
    return new Proxy({ length }, {
        set(trapTarget, key, value) {

            let currentLength = Reflect.get(trapTarget, "length");

            // the special case
            if (isArrayIndex(key)) {
                let numericKey = Number(key);

                if (numericKey >= currentLength) {
                    Reflect.set(trapTarget, "length", numericKey + 1);
                }
            } else if (key === "length") {

                if (value < currentLength) {
                    for (let index = currentLength - 1; index >= value; index\
--) {
                        Reflect.deleteProperty(trapTarget, index);
                    }
                }

            }

            // always do this regardless of key type
            return Reflect.set(trapTarget, key, value);
        }
    });
}

let colors = createMyArray(3);
console.log(colors.length);         // 3

colors[0] = "red";
colors[1] = "green";
colors[2] = "blue";
colors[3] = "black";

console.log(colors.length);         // 4

colors.length = 2;

console.log(colors.length);         // 2
console.log(colors[3]);             // undefined
console.log(colors[2]);             // undefined
console.log(colors[1]);             // "green"
console.log(colors[0]);             // "red"
```

The `set` proxy trap in this code checks to see if `key` is `"length"` in order to adjust the rest of the object correctly. When that happens, the current length is first retrieved using `Reflect.get()` and compared against the new value. If the new value is less than the current length, then a `for` loop deletes all properties on the target that should no longer be available. The `for` loop goes backward from the current array length (`currentLength`) and deletes each property until it reaches the new array length (`value`).

This example adds four colors to `colors` and then sets the `length` property to 2. That effectively removes the items in positions 2 and 3, so they now return `undefined` when you attempt to access them. The `length` property is correctly set to 2 and the items in positions 0 and 1 are still accessible.

With both behaviors implemented, you can easily create an object that mimics the behavior of built-in arrays. But doing so with a function isn’t as desirable as creating a class to encapsulate this behavior, so the next step is to implement this functionality as a class.

#### Implementing the MyArray Class

The simplest way to create a class that uses a proxy is to define the class as usual and then return a proxy from the constructor. That way, the object returned when a class is instantiated will be the proxy instead of the instance. (The instance is the value of `this` inside the constructor.) The instance becomes the target of the proxy and the proxy is returned as if it were the instance. The instance will be completely private and you won’t be able to access it directly, though you’ll be able to access it indirectly through the proxy.

Here’s a simple example of returning a proxy from a class constructor:

```js
class Thing {
    constructor() {
        return new Proxy(this, {});
    }
}

let myThing = new Thing();
console.log(myThing instanceof Thing);      // true
```

In this example, the class `Thing` returns a proxy from its constructor. The proxy target is `this`and the proxy is returned from the constructor. That means `myThing` is actually a proxy even though it was created by calling the `Thing` constructor. Because proxies pass through their behavior to their targets, `myThing` is still considered an instance of `Thing`, making the proxy completely transparent to anyone using the `Thing` class.

With that in mind, creating a custom array class using a proxy in relatively straightforward. The code is mostly the same as the code in the “Deleting Elements on Reducing Length” section. The same proxy code is used, but this time, it’s inside a class constructor. Here’s the complete example:

```js
function toUint32(value) {
    return Math.floor(Math.abs(Number(value))) % Math.pow(2, 32);
}

function isArrayIndex(key) {
    let numericKey = toUint32(key);
    return String(numericKey) == key && numericKey < (Math.pow(2, 32) - 1);
}

class MyArray {
    constructor(length=0) {
        this.length = length;

        return new Proxy(this, {
            set(trapTarget, key, value) {

                let currentLength = Reflect.get(trapTarget, "length");

                // the special case
                if (isArrayIndex(key)) {
                    let numericKey = Number(key);

                    if (numericKey >= currentLength) {
                        Reflect.set(trapTarget, "length", numericKey + 1);
                    }
                } else if (key === "length") {

                    if (value < currentLength) {
                        for (let index = currentLength - 1; index >= value; i\
ndex--) {
                            Reflect.deleteProperty(trapTarget, index);
                        }
                    }

                }

                // always do this regardless of key type
                return Reflect.set(trapTarget, key, value);
            }
        });

    }
}


let colors = new MyArray(3);
console.log(colors instanceof MyArray);     // true

console.log(colors.length);         // 3

colors[0] = "red";
colors[1] = "green";
colors[2] = "blue";
colors[3] = "black";

console.log(colors.length);         // 4

colors.length = 2;

console.log(colors.length);         // 2
console.log(colors[3]);             // undefined
console.log(colors[2]);             // undefined
console.log(colors[1]);             // "green"
console.log(colors[0]);             // "red"
```

This code creates a `MyArray` class that returns a proxy from its constructor. The `length`property is added in the constructor (initialized to either the value that is passed in or to a default value of 0) and then a proxy is created and returned. This gives the `colors` variable the appearance of being just an instance of `MyArray` and implements both of the key array behaviors.

Although returning a proxy from a class constructor is easy, it does mean that a new proxy is created for every instance. There is, however, a way to have all instances share one proxy: you can use the proxy as a prototype.

### Using a Proxy as a Prototype

Proxies can be used as prototypes, but doing so is a bit more involved than the previous examples in this chapter. When a proxy is a prototype, the proxy traps are only called when the default operation would normally continue on to the prototype, which does limit a proxy’s capabilities as a prototype. Consider this example:

```js
let target = {};
let newTarget = Object.create(new Proxy(target, {

    // never called
    defineProperty(trapTarget, name, descriptor) {

        // would cause an error if called
        return false;
    }
}));

Object.defineProperty(newTarget, "name", {
    value: "newTarget"
});

console.log(newTarget.name);                    // "newTarget"
console.log(newTarget.hasOwnProperty("name"));  // true
```

The `newTarget` object is created with a proxy as the prototype. Making `target` the proxy target effectively makes `target` the prototype of `newTarget` because the proxy is transparent. Now, proxy traps will only be called if an operation on `newTarget` would pass the operation through to happen on `target`.

The `Object.defineProperty()` method is called on `newTarget` to create an own property called `name`. Defining a property on an object isn’t an operation that normally continues to the object’s prototype, so the `defineProperty` trap on the proxy is never called and the `name` property is added to `newTarget` as an own property.

While proxies are severely limited when used as prototypes, there are a few traps that are still useful.

#### Using the `get` Trap on a Prototype

When the internal `[[Get]]` method is called to read a property, the operation looks for own properties first. If an own property with the given name isn’t found, then the operation continues to the prototype and looks for a property there. The process continues until there are no further prototypes to check.

Thanks to that process, if you set up a `get` proxy trap, the trap will be called on a prototype whenever an own property of the given name doesn’t exist. You can use the `get` trap to prevent unexpected behavior when accessing properties that you can’t guarantee will exist. Just create an object that throws an error whenever you try to access a property that doesn’t exist:

```js
let target = {};
let thing = Object.create(new Proxy(target, {
    get(trapTarget, key, receiver) {
        throw new ReferenceError(`${key} doesn't exist`);
    }
}));

thing.name = "thing";

console.log(thing.name);        // "thing"

// throw an error
let unknown = thing.unknown;
```

In this code, the `thing` object is created with a proxy as its prototype. The `get` trap throws an error when called to indicate that the given key doesn’t exist on the `thing` object. When `thing.name` is read, the operation never calls the `get` trap on the prototype because the property exists on `thing`. The `get` trap is called only when the `thing.unknown` property, which doesn’t exist, is accessed.

When the last line executes, `unknown` isn’t an own property of `thing`, so the operation continues to the prototype. The `get` trap then throws an error. This type of behavior can be very useful in JavaScript, where unknown properties silently return `undefined` instead of throwing an error (as happens in other languages).

It’s important to understand that in this example, `trapTarget` and `receiver` are different objects. When a proxy is used as a prototype, the `trapTarget` is the prototype object itself while the `receiver` is the instance object. In this case, that means `trapTarget` is equal to `target` and `receiver` is equal to `thing`. That allows you access both to the original target of the proxy and the object on which the operation is meant to take place.

#### Using the `set` Trap on a Prototype

The internal `[[Set]]` method also checks for own properties and then continues to the prototype if needed. When you assign a value to an object property, the value is assigned to the own property with the same name if it exists. If no own property with the given name exists, then the operation continues to the prototype. The tricky part is that even though the assignment operation continues to the prototype, assigning a value to that property will create a property on the instance (not the prototype) by default, regardless of whether a property of that name exists on the prototype.

To get a better idea of when the `set` trap will be called on a prototype and when it won’t, consider the following example showing the default behavior:

```js
let target = {};
let thing = Object.create(new Proxy(target, {
    set(trapTarget, key, value, receiver) {
        return Reflect.set(trapTarget, key, value, receiver);
    }
}));

console.log(thing.hasOwnProperty("name"));      // false

// triggers the `set` proxy trap
thing.name = "thing";

console.log(thing.name);                        // "thing"
console.log(thing.hasOwnProperty("name"));      // true

// does not trigger the `set` proxy trap
thing.name = "boo";

console.log(thing.name);                        // "boo"
```

In this example, `target` starts with no own properties. The `thing` object has a proxy as its prototype that defines a `set` trap to catch the creation of any new properties. When `thing.name`is assigned `"thing"` as its value, the `set` proxy trap is called because `thing` doesn’t have an own property called `name`. Inside the `set` trap, `trapTarget` is equal to `target` and `receiver`is equal to `thing`. The operation should ultimately create a new property on `thing`, and fortunately `Reflect.set()` implements this default behavior for you if you pass in `receiver` as the fourth argument.

Once the `name` property is created on `thing`, setting `thing.name` to a different value will no longer call the `set` proxy trap. At that point, `name` is an own property so the `[[Set]]` operation never continues on to the prototype.

#### Using the `has` Trap on a Prototype

Recall that the `has` trap intercepts the use of the `in` operator on objects. The `in` operator searches first for an object’s own property with the given name. If an own property with that name doesn’t exist, the operation continues to the prototype. If there’s no own property on the prototype, then the search continues through the prototype chain until the own property is found or there are no more prototypes to search.

The `has` trap is therefore only called when the search reaches the proxy object in the prototype chain. When using a proxy as a prototype, that only happens when there’s no own property of the given name. For example:

```js
let target = {};
let thing = Object.create(new Proxy(target, {
    has(trapTarget, key) {
        return Reflect.has(trapTarget, key);
    }
}));

// triggers the `has` proxy trap
console.log("name" in thing);                   // false

thing.name = "thing";

// does not trigger the `has` proxy trap
console.log("name" in thing);                   // true
```

This code creates a `has` proxy trap on the prototype of `thing`. The `has` trap isn’t passed a `receiver` object like the `get` and `set` traps are because searching the prototype happens automatically when the `in` operator is used. Instead, the `has` trap must operate only on `trapTarget`, which is equal to `target`. The first time the `in` operator is used in this example, the `has` trap is called because the property `name` doesn’t exist as an own property of `thing`. When `thing.name` is given a value and then the `in` operator is used again, the `has` trap isn’t called because the operation stops after finding the own property `name` on `thing`.

The prototype examples to this point have centered around objects created using the`Object.create()` method. But if you want to create a class that has a proxy as a prototype, the process is a bit more involved.

#### Proxies as Prototypes on Classes

Classes cannot be directly modified to use a proxy as a prototype because their `prototype`property is non-writable. You can, however, use a bit of misdirection to create a class that has a proxy as its prototype by using inheritance. To start, you need to create an ECMAScript 5-style type definition using a constructor function. You can then overwrite the prototype to be a proxy. Here’s an example:

```js
function NoSuchProperty() {
    // empty
}

NoSuchProperty.prototype = new Proxy({}, {
    get(trapTarget, key, receiver) {
        throw new ReferenceError(`${key} doesn't exist`);
    }
});

let thing = new NoSuchProperty();

// throws error due to `get` proxy trap
let result = thing.name;
```

The `NoSuchProperty` function represents the base from which the class will inherit. There are no restrictions on the `prototype` property of functions, so you can overwrite it with a proxy. The `get` trap is used to throw an error when the property doesn’t exist. The `thing` object is created as an instance of `NoSuchProperty` and throws an error when the nonexistent `name` property is accessed.

The next step is to create a class that inherits from `NoSuchProperty`. You can simply use the `extends` syntax discussed in chapter to introduce the proxy into the class’ prototype chain, like this:

```js
function NoSuchProperty() {
    // empty
}

NoSuchProperty.prototype = new Proxy({}, {
    get(trapTarget, key, receiver) {
        throw new ReferenceError(`${key} doesn't exist`);
    }
});

class Square extends NoSuchProperty {
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }
}

let shape = new Square(2, 6);

let area1 = shape.length * shape.width;
console.log(area1);                         // 12

// throws an error because "wdth" doesn't exist
let area2 = shape.length * shape.wdth;
```

The `Square` class inherits from `NoSuchProperty` so the proxy is in the `Square` class’ prototype chain. The `shape` object is then created as a new instance of `Square` and has two own properties: `length` and `width`. Reading the values of those properties succeeds because the `get`proxy trap is never called. Only when a property that doesn’t exist on `shape` is accessed (`shape.wdth`, an obvious typo) does the `get` proxy trap trigger and throw an error.

That proves the proxy is in the prototype chain of `shape`, but it might not be obvious that the proxy is not the direct prototype of `shape`. In fact, the proxy is a couple of steps up the prototype chain from `shape`. You can see this more clearly by slightly altering the preceding example:

```js
function NoSuchProperty() {
    // empty
}

// store a reference to the proxy that will be the prototype
let proxy = new Proxy({}, {
    get(trapTarget, key, receiver) {
        throw new ReferenceError(`${key} doesn't exist`);
    }
});

NoSuchProperty.prototype = proxy;

class Square extends NoSuchProperty {
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }
}

let shape = new Square(2, 6);

let shapeProto = Object.getPrototypeOf(shape);

console.log(shapeProto === proxy);                  // false

let secondLevelProto = Object.getPrototypeOf(shapeProto);

console.log(secondLevelProto === proxy);            // true
```

This version of the code stores the proxy in a variable called `proxy` so it’s easy to identify later. The prototype of `shape` is `Square.prototype`, which is not a proxy. But the prototype of `Square.prototype` is the proxy that was inherited from `NoSuchProperty`.

The inheritance adds another step in the prototype chain, and that matters because operations that might result in calling the `get` trap on `proxy` need to go through one extra step before getting there. If there’s a property on `Square.prototype`, then that will prevent the `get` proxy trap from being called, as in this example:

```js
function NoSuchProperty() {
    // empty
}

NoSuchProperty.prototype = new Proxy({}, {
    get(trapTarget, key, receiver) {
        throw new ReferenceError(`${key} doesn't exist`);
    }
});

class Square extends NoSuchProperty {
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }
}

let shape = new Square(2, 6);

let area1 = shape.length * shape.width;
console.log(area1);                         // 12

let area2 = shape.getArea();
console.log(area2);                         // 12

// throws an error because "wdth" doesn't exist
let area3 = shape.length * shape.wdth;
```

Here, the `Square` class has a `getArea()` method. The `getArea()` method is automatically added to `Square.prototype` so when `shape.getArea()` is called, the search for the method `getArea()`starts on the `shape` instance and then proceeds to its prototype. Because `getArea()` is found on the prototype, the search stops and the proxy is never called. That is actually the behavior you want in this situation, as you wouldn’t want to incorrectly throw an error when `getArea()` was called.

Even though it takes a little bit of extra code to create a class with a proxy in its prototype chain, it can be worth the effort if you need such functionality.

### Summary

Prior to ECMAScript 6, certain objects (such as arrays) displayed nonstandard behavior that developers couldn’t replicate. Proxies change that. They let you define your own nonstandard behavior for several low-level JavaScript operations, so you can replicate all behaviors of built-in JavaScript objects through proxy traps. These traps are called behind the scenes when various operations take place, like a use of the `in` operator.

A reflection API was also introduced in ECMAScript 6 to allow developers to implement the default behavior for each proxy trap. Each proxy trap has a corresponding method of the same name on the `Reflect` object, another ECMAScript 6 addition. Using a combination of proxy traps and reflection API methods, it’s possible to filter some operations to behave differently only in certain conditions while defaulting to the built-in behavior.

Revocable proxies are a special proxies that can be effectively disabled by using a `revoke()`function. The `revoke()` function terminates all functionality on the proxy, so any attempt to interact with the proxy’s properties throws an error after `revoke()` is called. Revocable proxies are important for application security where third-party developers may need access to certain objects for a specified amount of time.

While using proxies directly is the most powerful use case, you can also use a proxy as the prototype for another object. In that case, you are severely limited in the number of proxy traps you can effectively use. Only the `get`, `set`, and `has` proxy traps will ever be called on a proxy when it’s used as a prototype, making the set of use cases much smaller.

## Encapsulating Code With Modules

JavaScript’s “shared everything” approach to loading code is one of the most error-prone and confusing aspects of the language. Other languages use concepts such as packages to define code scope, but before ECMAScript 6, everything defined in every JavaScript file of an application shared one global scope. As web applications became more complex and started using even more JavaScript code, that approach caused problems like naming collisions and security concerns. One goal of ECMAScript 6 was to solve the scope problem and bring some order to JavaScript applications. That’s where modules come in.

### What are Modules?

*Modules* are JavaScript files that are loaded in a different mode (as opposed to scripts, which are loaded in the original way JavaScript worked). This different mode is necessary because modules have very different semantics than scripts:

1. Module code automatically runs in strict mode, and there’s no way to opt-out of strict mode.
2. Variables created in the top level of a module aren’t automatically added to the shared global scope. They exist only within the top-level scope of the module.
3. The value of `this` in the top level of a module is undefined.
4. Modules don’t allow HTML-style comments within code (a leftover feature from JavaScript’s early browser days).
5. Modules must export anything that should be available to code outside of the module.
6. Modules may import bindings from other modules.

These differences may seem small at first glance, but they represent a significant change in how JavaScript code is loaded and evaluated, which I will discuss over the course of this chapter. The real power of modules is the ability to export and import only bindings you need, rather than everything in a file. A good understanding of exporting and importing is fundamental to understanding how modules differ from scripts.

### Basic Exporting

You can use the `export` keyword to expose parts of published code to other modules. In the simplest case, you can place `export` in front of any variable, function, or class declaration to export it from the module, like this:

```js
// export data
export var color = "red";
export let name = "Nicholas";
export const magicNumber = 7;

// export function
export function sum(num1, num2) {
    return num1 + num1;
}

// export class
export class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }
}

// this function is private to the module
function subtract(num1, num2) {
    return num1 - num2;
}

// define a function...
function multiply(num1, num2) {
    return num1 * num2;
}

// ...and then export it later
export { multiply };
```

There are a few things to notice in this example. First, apart from the `export` keyword, every declaration is exactly the same as it would be otherwise. Each exported function or class also has a name; that’s because exported function and class declarations require a name. You can’t export anonymous functions or classes using this syntax unless you use the `default` keyword (discussed in detail in the “Default Values in Modules” section).

Next, consider the `multiply()` function, which isn’t exported when it’s defined. That works because you need not always export a declaration: you can also export references. Finally, notice that this example doesn’t export the `subtract()` function. That function won’t be accessible from outside this module because any variables, functions, or classes that are not explicitly exported remain private to the module.

### Basic Importing

Once you have a module with exports, you can access the functionality in another module by using the `import` keyword. The two parts of an `import` statement are the identifiers you’re importing and the module from which those identifiers should be imported. This is the statement’s basic form:

```js
import { identifier1, identifier2 } from "./example.js";
```

The curly braces after `import` indicate the bindings to import from a given module. The keyword `from` indicates the module from which to import the given binding. The module is specified by a string representing the path to the module (called the *module specifier*). Browsers use the same path format you might pass to the `<script>` element, which means you must include a file extension. Node.js, on the other hand, follows its traditional convention of differentiating between local files and packages based on a filesystem prefix. For example, `example` would be a package and `./example.js` would be a local file.

When importing a binding from a module, the binding acts as if it were defined using `const`. That means you can’t define another variable with the same name (including importing another binding of the same name), use the identifier before the `import` statement, or change its value.

#### Importing a Single Binding

Suppose that the first example in the “Basic Exporting” section is in a module with the filename `example.js`. You can import and use bindings from that module in a number of ways. For instance, you can just import one identifier:

```js
// import just one
import { sum } from "./example.js";

console.log(sum(1, 2));     // 3
sum = 1;        // error
```

#### Importing Multiple Bindings

If you want to import multiple bindings from the example module, you can explicitly list them out as follows:

```js
// import multiple
import { sum, multiply, magicNumber } from "./example.js";
console.log(sum(1, magicNumber));   // 8
console.log(multiply(1, 2));        // 2
```

#### Importing All of a Module

There’s also a special case that allows you to import the entire module as a single object. All of the exports are then available on that object as properties. For example:

```js
// import everything
import * as example from "./example.js";
console.log(example.sum(1, example.magicNumber));   // 8
console.log(example.multiply(1, 2));    // 2
```

In this code, all exported bindings in `example.js` are loaded into an object called `example`. The named exports (the `sum()` function, the `multiple()` function, and `magicNumber`) are then accessible as properties on `example`. This import format is called a *namespace import* because the `example` object doesn’t exist inside of the `example.js` file and is instead created to be used as a namespace object for all of the exported members of `example.js`.

Keep in mind, however, that no matter how many times you use a module in `import` statements, the module will only be executed once. After the code to import the module executes, the instantiated module is kept in memory and reused whenever another `import` statement references it. Consider the following:

```js
import { sum } from "./example.js";
import { multiply } from "./example.js";
import { magicNumber } from "./example.js";
```

Even though there are three `import` statements in this module, `example.js` will only be executed once. If other modules in the same application were to import bindings from `example.js`, those modules would use the same module instance this code uses.

#### A Subtle Quirk of Imported Bindings

ECMAScript 6’s `import` statements create read-only bindings to variables, functions, and classes rather than simply referencing the original bindings like normal variables. Even though the module that imports the binding can’t change its value, the module that exports that identifier can. For example, suppose you want to use this module:

```js
export var name = "Nicholas";
export function setName(newName) {
    name = newName;
}
```

When you import those two bindings, the `setName()` function can change the value of name:

```js
import { name, setName } from "./example.js";

console.log(name);       // "Nicholas"
setName("Greg");
console.log(name);       // "Greg"

name = "Nicholas";       // error
```

The call to `setName("Greg")` goes back into the module from which `setName()` was exported and executes there, setting `name` to `"Greg"` instead. Note this change is automatically reflected on the imported name binding. That’s because `name` is the local name for the exported `name` identifier. The `name` used in the code above and the name used in the module being imported from aren’t the same.

### Renaming Exports and Imports

Sometimes, you may not want to use the original name of a variable, function, or class you’ve imported from a module. Fortunately, you can change the name of an export both during the export and during the import.

In the first case, suppose you have a function that you’d like to export with a different name. You can use the `as` keyword to specify the name that the function should be known as outside of the module:

```js
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as add };
```

Here, the `sum()` function (`sum` is the *local name*) is exported as `add()` (add is the *exported name*). That means when another module wants to import this function, it will have to use the name `add` instead:

```js
import { add } from "./example.js";
```

If the module importing the function wants to use a different name, it can also use `as`:

```js
import { add as sum } from "./example.js";
console.log(typeof add);            // "undefined"
console.log(sum(1, 2));             // 3
```

This code imports the `add()` function using the import name and renames it to `sum()` (the local name). That means there is no identifier named `add` in this module.

### Default Values in Modules

The module syntax is really optimized for exporting and importing default values from modules, as this pattern was quite common in other module systems, like CommonJS (another specification for using JavaScript outside the browser). The *default value* for a module is a single variable, function, or class as specified by the `default` keyword, and you can only set one default export per module. Using the `default` keyword with multiple exports is a syntax error.

#### Exporting Default Values

Here’s a simple example that uses the `default` keyword:

```js
export default function(num1, num2) {
    return num1 + num2;
}
```

This module exports a function as its default value. The `default` keyword indicates that this is a default export. The function doesn’t require a name because the module itself represents the function.

You can also specify an identifier as the default export by by placing it after `export default`, such as:

```js
function sum(num1, num2) {
    return num1 + num2;
}

export default sum;
```

A third way to specify an identifier as the default export is by using the renaming syntax as follows:

```js
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as default };
```

The identifier `default` has special meaning in a renaming export and indicates a value should be the default for the module. Because `default` is a keyword in JavaScript, it can’t be used for a variable, function, or class name (it can be used as a property name). So the use of `default` to rename an export is a special case to create a consistency with how non-default exports are defined. This syntax is useful if you want to use a single `export` statement to specify multiple exports, including the default, at once.

#### Importing Default Values

You can import a default value from a module using the following syntax:

```js
// import the default
import sum from "./example.js";

console.log(sum(1, 2));     // 3
```

This import statement imports the default from the module `example.js`. Note that no curly braces are used, unlike you’d see in a non-default import. The local name `sum` is used to represent whatever default function the module exports. This syntax is the cleanest, and the creators of ECMAScript 6 expect it to be the dominant form of import on the Web, allowing you to use an already-existing object.

For modules that export both a default and one or more non-default bindings, you can import all exported bindings with one statement. For instance, suppose you have this module

```js
export let color = "red";

export default function(num1, num2) {
    return num1 + num2;
}
```

You can import both `color` and the default function using the following `import` statement:

```js
import sum, { color } from "./example.js";

console.log(sum(1, 2));     // 3
console.log(color);         // "red"
```

The comma separates the default local name from the non-defaults, which are also surrounded by curly braces. Keep in mind that the default must come before the non-defaults in the `import` statement.

As with exporting defaults, you can import defauts with the renaming syntax, too:

```js
// equivalent to previous example
import { default as sum, color } from "example";

console.log(sum(1, 2));     // 3
console.log(color);         // "red"
```

In this code, the default export (`default`) is renamed to `sum` and the additional `color` export is also imported. This example is equivalent to the preceding example.

## Referências
https://github.com/nzakas/understandinges6/tree/master/manuscript
