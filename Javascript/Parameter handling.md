# Parameter handling

## Visão geral

O gerenciamento de parâmetros foi significativamente atualizado no *ECMAScript 6*. Ele agora suporta parâmetros com valores padrão, rest parameters e desestruturação.

Additionally, the spread operator helps with function/method/constructor calls and Array literals.

### Default parameter values

A *default parameter value* é especificado para um parâmetro através de um sinal de igual (`=`). Se um chamador não fornecer um valor para o parâmetro, o valor padrão é usado. No exemplo a seguir, o valor padrão do parâmetro `y` é `0`:

```js
function func(x, y = 0) {
    return [x, y];
}

func(1, 2); // [1, 2]
func(1); // [1, 0]
func(); // [undefined, 0]
```

### Rest parameters

Se você prefixar um nome de parâmetro com rest operator (`...`), esse parâmetro recebe todos os parâmetros restantes através de uma matriz:

```js
function format(pattern, ...params) {
    return {pattern, params};
}

format(1, 2, 3); // { pattern: 1, params: [ 2, 3 ] }
format(); // { pattern: undefined, params: [] }
```

### Named parameters via destructuring

You can simulate named parameters if you destructure with an object pattern in the parameter list:

```js
function selectEntries({ start=0, end=-1, step=1 } = {}) { // (A)
    // The object pattern is an abbreviation of:
    // { start: start=0, end: end=-1, step: step=1 }
    // Use the variables `start`, `end` and `step` here
    ···
}

selectEntries({ start: 10, end: 30, step: 2 });
selectEntries({ step: 3 });
selectEntries({});
selectEntries();
```

The `= {}` in line `A` enables you to call `selectEntries()` without paramters.

### Spread operator (`...`)

In function and constructor calls, ***the spread operator turns iterable values into arguments***:

```js
Math.max(-1, 5, 11, 3)          // 11
Math.max(...[-1, 5, 11, 3])     // 11
Math.max(-1, ...[-1, 5, 11], 3) // 11
```

In Array literals, the spread operator turns iterable values into Array elements:

```js
[1, ...[2,3], 4]    // [1, 2, 3, 4]
```

## Parameter handling as destructuring

The ES6 way of handling parameters is equivalent to destructuring the actual parameters via the formal parameters. That is, the following function call:

```js
function func(«FORMAL_PARAMETERS») {
    «CODE»
}

func(«ACTUAL_PARAMETERS»);
```

é aproximadamente equivalente a:

```js
{
    let [«FORMAL_PARAMETERS»] = [«ACTUAL_PARAMETERS»];
    {
        «CODE»
    }
}
```

Exemplo - a seguinte chamada de função:

```js
function logSum(x = 0, y = 0) {
    console.log(x + y);
}

logSum(7, 8);
```

torna-se:

```js
{
    let [x = 0, y = 0] = [7, 8];
    {
        console.log(x + y);
    }
}
```

## Parameter default values

O *ECMAScript 6* permite especificar valores padrão para os parâmetros:

```js
function f(x, y = 0) {
    return [x, y];
}
```

Omitting the second parameter triggers the default value:

```js
f(1)    // [1, 0]
f()     // [undefined, 0]
```

Watch out – `undefined` triggers the default value, too:

```js
f(undefined, undefined)   // [undefined, 0]
```

O valor padrão é calculado sob demanda, somente quando é realmente necessário:

```js
const log = console.log.bind(console);

function g(x = log('x'), y = log('y')) {
    return 'DONE';
}

g()     // x // y // 'DONE'
g(1)    // y // 'DONE'
g(1, 2) // 'DONE'
```

### Referring to other parameters in default values

Within a parameter default value, you can refer to any variable, including other parameters:

```js
function foo(x = 3, y = x) {
}

foo();     // x=3; y=3
foo(7);    // x=7; y=7
foo(7, 2); // x=7; y=2
```

However, order matters. Parameters are declared from left to right. “Inside” a default value, you get a `ReferenceError` if you access a parameter that hasn’t been declared, yet:

```js
function bar(x = y, y = 4) {}
bar(3); // OK
bar();  // ReferenceError: y is not defined
```

### Referring to “inner” variables in default values

Default values exist in their own scope, which is between the “outer” scope surrounding the function and the “inner” scope of the function body. Therefore, you can’t access “inner” variables from the default values:

```js
const x = 'outer';

function foo(a = x) {
    const x = 'inner';
    console.log(a); // outer
}
```

If there were no outer `x` in the previous example, the default value `x` would produce a `ReferenceError` (if triggered).

This restriction is probably most surprising if default values are closures:

```js
const QUX = 2;

function bar(callback = () => QUX) { // returns 2
    const QUX = 3;
    callback();
}

bar(); // ReferenceError
```

## Rest parameters

Colocar o rest operator (`...`) na frente do último parâmetro formal significa que receberá todos os parâmetros restantes em uma matriz.

```js
function f(x, ...y) {
    ···
}

f('a', 'b', 'c');   // x = 'a'; y = ['b', 'c']
```

If there are no remaining parameters, the rest parameter will be set to the empty Array:

```js
f();    // x = undefined; y = []
```

> The *spread operator* (`...`) looks exactly like the *rest operator*, but is used inside function calls and Array literals (not inside destructuring patterns).

### No more arguments!

*Rest parameters* podem substituir completamente a infame variável especial do JavaScript `arguments`. Eles têm a vantagem de sempre serem Arrays:

```js
// ECMAScript 5: arguments
function logAllArguments() {
    for (var i = 0; i < arguments.length; i++) {
        console.log(arguments[i]);
    }
}

// ECMAScript 6: rest parameter
function logAllArguments(...args) {
    for (const arg of args) {
        console.log(arg);
    }
}
```

#### Combining destructuring and access to the destructured value

Uma característica interessante de `arguments` é que você pode ter parâmetros normais e uma matriz de todos os parâmetros ao mesmo tempo:

```js
function foo(x = 0, y = 0) {
    console.log('Arity: ' + arguments.length);
    ···
}
```

You can avoid `arguments` in such cases if you combine a rest parameter with Array destructuring. The resulting code is longer, but more explicit:

```js
function foo(...args) {
    let [x = 0, y = 0] = args;
    console.log('Arity: ' + args.length);
    ···
}
```

The same technique works for named parameters (options objects):

```js
function bar(options = {}) {
    let { namedParam1, namedParam2 } = options;
    ···
    if ('extra' in options) {
        ···
    }
}
```

## Simulating named parameters

When calling a function (or method) in a programming language, you must map the actual parameters (specified by the caller) to the formal parameters (of a function definition). There are two common ways to do so:

* *Positional parameters* are mapped by position. The first actual parameter is mapped to the first formal parameter, the second actual to the second formal, and so on: `selectEntries(3, 20, 2)`.

* *Named parameters* use names (labels) to perform the mapping. Formal parameters have labels. In a function call, these labels determine which value belongs to which formal parameter. It does not matter in which order named actual parameters appear, as long as they are labeled correctly. Simulating named parameters in JavaScript looks as follows. `selectEntries({ start: 3, end: 20, step: 2 })`.

Named parameters have two main benefits: they provide descriptions for arguments in function calls and they work well for optional parameters. I’ll first explain the benefits and then show you how to simulate named parameters in JavaScript via object literals.

### Named Parameters as Descriptions

As soon as a function has more than one parameter, you might get confused about what each parameter is used for. For example, let’s say you have a function, `selectEntries()`, that returns entries from a database. Given the function call:

```js
selectEntries(3, 20, 2);
```

### Optional Named Parameters

Optional positional parameters work well only if they are omitted at the end. Anywhere else, you have to insert placeholders such as `null` so that the remaining parameters have correct positions.

With optional named parameters, that is not an issue. You can easily omit any of them. Here are some examples:

```py
# Python syntax
selectEntries(step=2)
selectEntries(end=20, start=3)
selectEntries()
```

### Simulating Named Parameters in JavaScript

JavaScript does not have native support for named parameters, unlike Python and many other languages. But there is a reasonably elegant simulation: Each actual parameter is a property in an object literal whose result is passed as a single formal parameter to the callee. When you use this technique, an invocation of `selectEntries()` looks as follows.

```js
selectEntries({ start: 3, end: 20, step: 2 });
```

The function receives an object with the properties `start`, `end`, and `step`. You can omit any of them:

```js
selectEntries({ step: 2 });
selectEntries({ end: 20, start: 3 });
selectEntries();
```

No *ECMAScript 5*, você implementaria da `selectEntries()` seguinte maneira:

```js
function selectEntries(options) {
    options = options || {};
    var start = options.start || 0;
    var end = options.end || -1;
    var step = options.step || 1;
    ···
}
```

No *ECMAScript 6*, você pode usar *destructuring*, que se parece com isto:

```js
function selectEntries({ start = 0, end = -1, step = 1 }) {
    ···
}
```

If you call `selectEntries()` with zero arguments, the destructuring fails, because you can’t match an object pattern against `undefined`. That can be fixed via a default value. In the following code, the object pattern is matched against `{}` if the first parameter is missing.

```js
function selectEntries({ start = 0, end = -1, step = 1 } = {}) {
    ···
}
```

You can also combine positional parameters with named parameters. It is customary for the latter to come last:

```js
someFunc(posArg1, { namedArg1: 7, namedArg2: true });
```

In principle, JavaScript engines could optimize this pattern so that no intermediate object is created, because both the object literals at the call sites and the object patterns in the function definitions are static.

## Examples of destructuring in parameter handling

### `forEach()` and destructuring

*destructuring the Arrays in an Array.*
```js
const items = [ ['foo', 3], ['bar', 9] ];
items.forEach(([word, count]) => {
    console.log(word+' '+count);
});
```

*destructuring the objects in an Array.*
```js
const items = [
    { word:'foo', count:3 },
    { word:'bar', count:9 },
];

items.forEach(({word, count}) => {
    console.log(word+' '+count);
});
```

### Transforming Maps

```js
const map0 = new Map([
    [1, 'a'],
    [2, 'b'],
    [3, 'c'],
]);

const map1 = new Map( // step 3
    [...map0] // step 1
    .map(([k, v]) => [k*2, '_'+v]) // step 2
);

// Resulting Map: {2 -> '_a', 4 -> '_b', 6 -> '_c'}
```

## The spread operator (`...`)

O *spread operator*(`...`) parece exatamente com o *rest operator*, mas é o contrário:

* ***Rest operator***: collects the remaining items of an iterable into an Array and is used for rest parameters and destructuring.
* ***Spread operator***: turns the items of an iterable into arguments of a function call or into elements of an Array.

### Spreading into function and method calls

`Math.max()` is a good example for demonstrating how the *spread operator* works in method calls. Math.max`(x1, x2, ···)` returns the argument whose value is greatest. It accepts an arbitrary number of arguments, but can’t be applied to Arrays. The spread operator fixes that:

```js
Math.max(-1, 5, 11, 3)      // 11
Math.max(...[-1, 5, 11, 3]) // 11
```

In contrast to the *rest operator*, you can use the *spread operator* anywhere in a sequence of parts:

```js
Math.max(-1, ...[-1, 5, 11], 3) // 11
```

Another example is JavaScript not having a way to destructively append the elements of one Array to another one. However, Arrays do have the method `push(x1, x2, ···)`, which appends all of its arguments to its receiver. The following code shows how you can use `push()` to append the elements of `arr2` to `arr1`.

```js
const arr1 = ['a', 'b'];
const arr2 = ['c', 'd'];

arr1.push(...arr2); // arr1 is now ['a', 'b', 'c', 'd']
```

### Spreading into constructors

Além das chamadas de função e método, o *spread operator* também funciona para chamadas de construtor:

```js
new Date(...[1912, 11, 24]) // Christmas Eve 1912
```

### Spreading into Arrays

The *spread operator* can also be used inside Array literals:

```js
[1, ...[2,3], 4]    // [1, 2, 3, 4]
```

That gives you a convenient way to concatenate Arrays:

```js
const x = ['a', 'b'];
const y = ['c'];
const z = ['d', 'e'];

const arr = [...x, ...y, ...z]; // ['a', 'b', 'c', 'd', 'e']
```

One advantage of the spread operator is that its operand can be any iterable value (in contrast to the Array method `concat()`, which does not support iteration).

#### Converting iterable or Array-like objects to Arrays

O *spread operator* permite que você converta qualquer valor iterável em uma matriz:

```js
const arr = [...someIterableObject];
```

Let’s convert a Set to an Array:

```js
const set = new Set([11, -1, 6]);
const arr = [...set];   // [11, -1, 6]
```

Seus próprios objetos iteráveis ​​podem ser convertidos em Arrays da mesma maneira:

```js
const obj = {
    * [Symbol.iterator]() {
        yield 'a';
        yield 'b';
        yield 'c';
    }
};

const arr = [...obj]; // ['a', 'b', 'c']
```

Note that, just like the `for-of` loop, the spread operator only works for iterable values. All built-in data structures are iterable: Arrays, Maps and Sets. All Array-like DOM data structures are also iterable.

Should you ever encounter something that is not *iterable*, but Array-like (indexed elements plus a property `length`), you can use `Array.from()` to convert it to an Array:

```js
const arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ECMAScript 5:
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ECMAScript 6:
const arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

// TypeError: Cannot spread non-iterable value
const arr3 = [...arrayLike];
```
