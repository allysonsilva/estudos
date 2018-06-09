# Variables and scoping

## Overview

*ES6* provides two new ways of declaring variables: `let` and `const`, which mostly replace the *ES5* way of declaring variables, `var`.

####  Valores avaliados como falso:

* `false`
* `undefined`
* `null`
* `0`
* `NaN`
* the empty string (`""`)

### De `var` para `const/let`

No *ES5*, você declara variáveis ​​via `var`. Tais variáveis ​​são ***escopo de função***. O comportamento de `var` é ocasionalmente confuso.

Veja o exemplo:

```js
var x = 3;

function func(randomize) {
    if (randomize) {
        var x = Math.random();  // (A) scope: whole function
        return x;
    }
    return x;   // accesses the x from line A
}

func(false);    // undefined
```

De modo que `func()` retorna `undefined` pode ser imprevisto. Você pode ver por que se você reescrever o código para que ele reflita mais de perto o que realmente está acontecendo:

```js
var x = 3;

function func(randomize) {
    var x;
    if (randomize) {
        x = Math.random();
        return x;
    }

    return x;
}

func(false); // undefined
```

Em *ES6*, você poderá declarar variáveis via `let` e `const`. Tais variáveis são ***escopo de bloco***, their scopes are the innermost enclosing blocks. `let` é aproximadamente uma versão de escopo de bloco de `var`. `const` funciona como `let`, mas cria variáveis cujos valores não podem ser alterados.

`let` and `const` behave more strictly and throw more exceptions (e.g. when you access their variables inside their scope before they are declared). Block-scoping helps with keeping the effects of code fragments more local. And it’s more mainstream than function-scoping, which eases moving between JavaScript and other programming languages.

If you replace `var` with `let` in the initial version, you get different behavior:

```js
let x = 3;

function func(randomize) {
    if (randomize) {
        let x = Math.random();
        return x;
    }

    return x;
}

func(false); // 3
```

That means that you can’t blindly replace `var` with `let` or `const` in existing code; you have to be careful during refactoring.

*My advice is*:

* Prefer `const`. You can use it for all variables whose values never change.
* Otherwise, use `let` – for variables whose values do change.
* Avoid `var`.

### let

`let` funciona de forma semelhante a `var`, mas a variável declara é ***block-scoped***, ela só existe dentro do bloco atual. `var` é *function-scoped*.

No código a seguir, você pode ver que a variável declarada `let tmp` só existe dentro do bloco que começa na linha `(A)`:

```js
function order(x, y) {
    if (x > y) { // (A)
        let tmp = x;
        x = y;
        y = tmp;
    }

    console.log(tmp===x); // ReferenceError: tmp is not defined
    return [x, y];
}
```

```js
function func() {
    if (true) {
        let tmp = 123;
    }
    console.log(tmp); // ReferenceError: tmp is not defined
}
```

In contrast, var-declared variables are function-scoped:

```js
function func() {
    if (true) {
        var tmp = 123;
    }

    console.log(tmp); // 123
}
```

Block scoping means that you can shadow variables within a function:

```js
function func() {
    let foo = 5;

    if (···) {
        let foo = 10; // shadows outer `foo`
        console.log(foo); // 10
    }

    console.log(foo); // 5
}
```

### const

`const` funciona como `let`, mas a variável que você declara deve ser inicializada imediatamente, com um valor que não pode ser alterado posteriormente.

```js
const foo; // SyntaxError: missing = in const declaration

const bar = 123;
bar = 456; // TypeError: `bar` is read-only
```

Since `for-of` creates one binding (storage space for a variable) per loop iteration, it is OK to `const`-declare the loop variable:

```js
for (const x of ['a', 'b']) {
    console.log(x);
}

// Output:
// a
// b
```

## Block scoping via `let` and `const`

Ambos `let` e `const` criar variáveis que são de block-scoped(escopo de bloco) - eles só existem dentro do bloco limitado pelas chaves`{}`. O código a seguir demonstra que a variável `const tmp` só existe dentro do bloco da instrução `if`:

```js
function func() {
    if (true) {
        const tmp = 123;
    }

    console.log(tmp); // ReferenceError: tmp is not defined
}
```

Em contraste, as declaradas de variáveis `var` são function-scoped(escopo de função):

```js
function func() {
    if (true) {
        var tmp = 123;
    }

    console.log(tmp); // 123
}
```

Block scoping means that you can shadow variables within a function:

```js
function func() {
    const foo = 5;

    if (···) {
        const foo = 10; // shadows outer `foo`
        console.log(foo); // 10
    }

    console.log(foo); // 5
}
```

## `const` creates immutable variables

Variáveis criadas por meio de `let` são mutáveis:

```js
let foo = 'abc';
foo = 'def';
console.log(foo); // def
```

Constantes, variáveis criadas por `const`, são imutáveis - não é possível atribuir valores diferentes para eles depois de criada:

```js
const foo = 'abc';
foo = 'def'; // TypeError
```

### Pitfall: `const` does not make the value immutable

`const` significa apenas que uma variável sempre tem o mesmo valor, mas isso não significa que o valor em si é ou se torna imutável. Por exemplo, `obj` é uma constante, mas o valor que ele aponta é mutável - podemos adicionar uma propriedade a ele:

```js
const obj = {};
obj.prop = 123;
console.log(obj.prop); // 123
```

Não podemos, no entanto, atribuir um valor diferente para `obj`:

```js
obj = {}; // TypeError
```

If you want the value of `obj` to be immutable, you have to take care of it, yourself.

For example, by *freezing* it:

```js
const obj = Object.freeze({});
obj.prop = 123; // TypeError
```

#### Pitfall: `Object.freeze()` is shallow

Tenha em mente que `Object.freeze()` é superficial, ele só congela as propriedades de seu argumento, não os objetos armazenados em suas propriedades.

```js
const obj = Object.freeze({ foo: {} });
obj.bar = 123   // TypeError: Can't add property bar, object is not extensible
obj.foo = {}    // TypeError: Cannot assign to read only property 'foo' of #<Object>
```

But the object `obj.foo` is not.

```js
obj.foo.qux = 'abc';
obj.foo.qux // 'abc'
```

### const in loop bodies

Once a `const` variable has been created, it can’t be changed. But that doesn’t mean that you can’t re-enter its scope and start fresh, with a new value. For example, via a loop:

```js
function logArgs(...args) {
    for (const [index, elem] of args.entries()) {   // (A)
        const message = index + '. ' + elem;        // (B)
        console.log(message);
    }
}

logArgs('Hello', 'everyone');

// Output:
// 0. Hello
// 1. everyone
```

Há duas declarações `const` neste código, um na linha `(A)` e um na linha `(B)`. E durante cada iteração do loop, suas constantes têm valores diferentes.

## The temporal dead zone

A variable declared by `let` or `const` has a so-called temporal dead zone (**TDZ**): When entering its scope, it can’t be accessed (got or set) until execution reaches the declaration. Let’s compare the life cycles of `var`-declared variables (which don’t have TDZs) and `let`-declared variables (which have TDZs).

### The life cycle of `var`-declared variables

Variáveis `var` ​​não têm zonas mortas temporais. Seu ciclo de vida compreende os passos seguintes:

* When the scope (its surrounding function) of a `var` variable is entered, storage space (a *binding*) is created for it. The variable is immediately initialized, by setting it to `undefined`.
* When the execution within the scope reaches the declaration, the variable is set to the value specified by the *initializer* (an assignment) – if there is one. If there isn’t, the value of the variable remains `undefined`.

### The life cycle of `let`-declared variables

Variables declared via `let` have temporal dead zones and their life cycle looks like this:

* When the scope (its surrounding block) of a `let` variable is entered, storage space (a *binding*) is created for it. The variable remains uninitialized.
* Getting or setting an uninitialized variable causes a `ReferenceError`.
* When the execution within the scope reaches the declaration, the variable is set to the value specified by the *initializer* (an assignment) – if there is one. If there isn’t then the value of the variable is set to `undefined`.

`const` variables work similarly to `let` variables, but they must have an initializer (i.e., be set to a value immediately) and can’t be changed.

### Exemplos

Within a *TDZ*, an exception is thrown if a variable is got or set:

```js
let tmp = true;

if (true) { // enter new scope, TDZ starts
    // Uninitialized binding for `tmp` is created
    console.log(tmp); // ReferenceError

    let tmp; // TDZ ends, `tmp` is initialized with `undefined`
    console.log(tmp); // undefined

    tmp = 123;
    console.log(tmp); // 123
}

console.log(tmp); // true
```

If there is an initializer then the *TDZ* ends after the initializer was evaluated and the result was assigned to the variable:

```js
let foo = console.log(foo); // ReferenceError
```

The following code demonstrates that the dead zone is really *temporal* (based on time) and not spatial (based on location):

```js
if (true) { // enter new scope, TDZ starts
    const func = function () {
        console.log(myVar); // OK!
    };

    // Here we are within the TDZ and
    // accessing `myVar` would cause a `ReferenceError`

    let myVar = 3; // TDZ ends
    func(); // called outside TDZ
}
```

### `typeof` throws a `ReferenceError` for a variable in the TDZ

Se você acessar uma variável na temporal dead zone via `typeof`, você obtém uma exceção:

```js
if (true) {
    console.log(typeof foo);                      // ReferenceError (TDZ)
    console.log(typeof aVariableThatDoesntExist); // 'undefined'
    let foo;
}
```

Why? The rationale is as follows: foo is not undeclared, it is uninitialized. You should be aware of its existence, but aren’t. Therefore, being warned seems desirable.

### Why is there a temporal dead zone?

Há várias razões pelas quais `const` e `let` tem temporal dead zones:

* To catch programming errors: Being able to access a variable before its declaration is strange. If you do so, it is normally by accident and you should be warned about it.
* For `const`: Making `const` work properly is difficult. Quoting Allen Wirfs-Brock: “TDZs … provide a rational semantics for `const`. There was significant technical discussion of that topic and TDZs emerged as the best solution.” `let` also has a temporal dead zone so that switching between `let` and `const` doesn’t change behavior in unexpected ways.
* Future-proofing for guards: JavaScript may eventually have guards, a mechanism for enforcing at runtime that a variable has the correct value (think runtime type check). If the value of a variable is `undefined` before its declaration then that value may be in conflict with the guarantee given by its guard.

## Parameters as variables

### Parameters versus local variables

If you `let`-declare a variable that has the same name as a parameter, you get a static (load-time) error:

```js
function func(arg) {
    let arg; // static error: duplicate declaration of `arg`
}
```

Doing the same inside a block shadows the parameter:

```js
function func(arg) {
    {
        let arg; // shadows parameter `arg`
    }
}
```

In contrast, `var`-declaring a variable that has the same name as a parameter does nothing, just like re-declaring a `var` variable within the same scope does nothing.

```js
function func(arg) {
    var arg; // does nothing
}

function func(arg) {
    {
        // We are still in same `var` scope as `arg`
        var arg; // does nothing
    }
}
```

### Parameter default values and the temporal dead zone

If parameters have default values, they are treated like a sequence of `let` statements and are subject to temporal dead zones:

```js
// OK: `y` accesses `x` after it has been declared
function foo(x=1, y=x) {
    return [x, y];
}

foo(); // [1,1]
```

```js
// Exception: `x` tries to access `y` within TDZ
function bar(x=y, y=2) {
    return [x, y];
}

bar(); // ReferenceError
```

### Parameter default values don’t see the scope of the body

The scope of parameter default values is separate from the scope of the body (the former surrounds the latter). That means that methods or functions defined “inside” parameter default values don’t see the local variables of the body:

```js
const foo = 'outer';

function bar(func = x => foo) {
    const foo = 'inner';
    console.log(func()); // outer
}

bar();
```

## Function declarations and class declarations

Declarações de função ...

* are block-scoped, like `let`.
* create properties in the global object (while in global scope), like `var`.
* are hoisted: independently of where a function declaration is mentioned in its scope, it is always created at the beginning of the scope.

O código a seguir demonstra o *hoisting* de declarações de função:

```js
{ // Enter a new scope
    console.log(foo()); // OK, due to hoisting

    function foo() {
        return 'hello';
    }
}
```

Declarações de classe ...

* are block-scoped.
* don’t create properties on the global object.
* are not hoisted.

Classes not being hoisted may be surprising, because, under the hood, they create functions. The rationale for this behavior is that the values of their `extends` clauses are defined via expressions and those expressions have to be executed at the appropriate times.

```js
{ // Enter a new scope

    const identity = x => x;

    // Here we are in the temporal dead zone of `MyClass`
    const inst = new MyClass(); // ReferenceError

    // Note the expression in the `extends` clause
    class MyClass extends identity(Object) {
    }
}
```

## Coding style: `const` versus `let` versus `var`

Eu recomendo usar sempre qualquer um `let` ou `const`:

1. Prefer `const`. You can use it whenever a variable never changes its value. In other words: the variable should never be the left-hand side of an assignment or the operand of ++ or --. Changing an object that a `const` variable refers to is allowed:

```js
const foo = {};
foo.prop = 123; // OK
```

You can even use `const` in a `for-of` loop, because one (immutable) binding is created per loop iteration:

```js
for (const x of ['a', 'b']) {
    console.log(x);
}

// Output:
// a
// b
```

Inside the body of the `for-of` loop, `x` can’t be changed.

2. Otherwise, use `let` – when the initial value of a variable changes later on.

```js
let counter = 0; // initial value
counter++; // change

let obj = {}; // initial value
obj = { foo: 123 }; // change
```

3. Evite `var`.

If you follow these rules, `var` will only appear in legacy code, as a signal that careful refactoring is required.

`var` does one thing that `let` and `const` don’t: variables declared via it become properties of the global object. However, that’s generally not a good thing. You can achieve the same effect by assigning to `window` (in browsers) or `global` (in Node.js).
