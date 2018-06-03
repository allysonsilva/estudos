# Destructuring

## Visão geral (Overview)

Destructuring is a convenient way of extracting multiple values from data stored in (possibly nested) objects and Arrays. It can be used in locations that receive data (such as the left-hand side of an assignment). How to extract the values is specified via patterns (read on for examples).

### Object destructuring

Destructuring objects:

```js
const obj = { first: 'Allyson', last: 'Silva' };
const {first: f, last: l} = obj; // f = 'Allyson'; l = 'Silva'

// {prop} is short for {prop: prop}
const {first, last} = obj; // first = 'Allyson'; last = 'Silva'
```

A desestruturação ajuda com o processamento de valores de retorno:

```js
const obj = { foo: 123 };

const {writable, configurable} = Object.getOwnPropertyDescriptor(obj, 'foo');

console.log(writable, configurable); // true true
```

### Array destructuring

Array destructuring (works for all iterable values):

```js
const iterable = ['a', 'b'];
const [x, y] = iterable; // x = 'a'; y = 'b'
```

A desestruturação ajuda com o processamento de valores de retorno:

```js
const [all, year, month, day] = /^(\d\d\d\d)-(\d\d)-(\d\d)$/.exec('2999-12-31');
```

### Where can destructuring be used?

Destructuring can be used in the following locations (I’m showing Array patterns to demonstrate; object patterns work just as well):

```js
// Variable declarations:
const [x] = ['a'];
let [x] = ['a'];
var [x] = ['a'];

// Assignments:
[x] = ['a'];

// Parameter definitions:
function f([x]) { ··· }
f(['a']);
```

You can also destructure in a `for-of` loop:

```js
const arr = ['a', 'b'];

for (const [index, element] of arr.entries()) {
    console.log(index, element);
}

// Output:
// 0 a
// 1 b
```

## Background: Constructing data versus extracting data

To fully understand what destructuring is, let’s first examine its broader context.

JavaScript has operations for constructing data, one property at a time:

```js
const obj = {};
obj.first = 'Allyson';
obj.last = 'Silva';
```

A mesma sintaxe pode ser usada para extrair dados. Novamente, uma propriedade por vez:

```js
const f = obj.first;
const l = obj.last;
```

Além disso, há sintaxe para construir várias propriedades ao mesmo tempo, através de um objeto literal:

```js
const obj = { first: 'Allyson', last: 'Silva' };
```

Antes do *ES6*, não havia nenhum mecanismo correspondente para extrair dados. Isso é o que a desestruturação é - ele permite extrair várias propriedades de um objeto através de um padrão de objeto. Por exemplo, no lado esquerdo de uma assignment(atribuição):

```js
const { first: f, last: l } = obj;
```

Você também pode estruturar Arrays via padrões:

```js
const [x, y] = ['a', 'b']; // x = 'a'; y = 'b'
```

## Patterns for destructuring

The following two parties are involved in destructuring:

* **Destructuring source**: the data to be destructured. For example, the *right-hand* side of a destructuring assignment.
* **Destructuring target**: the pattern used for destructuring. For example, the *left-hand* side of a destructuring assignment.

The destructuring target is either one of three patterns:

* **Assignment target**. For example: `x`
    * An assignment target is usually a variable. But in destructuring assignment, you have more options, as I’ll explain later.
* **Object pattern**. For example: { `first`: `«pattern»`, `last`: `«pattern»` }
    * The parts of an object pattern are properties, the property values are again patterns (recursively).
* **Array pattern**. For example: [ `«pattern»`, `«pattern»` ]
The parts of an Array pattern are elements, the elements are again patterns (recursively).

That means that you can nest patterns, arbitrarily deeply:

```js
const obj = { a: [{ foo: 123, bar: 'abc' }, {}], b: true };
const { a: [{foo: f}] } = obj; // f = 123
```

### Pick what you need

Se você desestruturar um objeto, mencione apenas as propriedades que lhe interessa:

```js
const { x: x } = { x: 7, y: 3 }; // x = 7
```

If you destructure an Array, you can choose to only extract a prefix:

```js
const [x, y] = ['a', 'b', 'c']; // x='a'; y='b';
```

## How do patterns access the innards of values?

In an assignment `pattern = someValue`, how does the *pattern* access what’s inside `someValue`?

### Object patterns coerce values to objects

The object pattern coerces destructuring sources to objects before accessing properties. That means that it works with primitive values:

```js
const {length: len} = 'abc';   // len = 3
const {toString: s} = 123;      // s = Number.prototype.toString
```

#### Failing to object-destructure a value

The coercion to object is not performed via `Object()`, but via the internal operation `ToObject()`. The two operations handle `undefined` and `null` differently.

`Object()` converts primitive values to wrapper objects and leaves objects untouched:

```js
typeof Object('abc')    // 'object'

var obj = {};
Object(obj) === obj   // true
```

It also converts `undefined` and `null` to empty objects:

```js
Object(undefined) // {}
Object(null)    // {}
```

Em contraste, `ToObject()` lança um `TypeError` se ele se encontra `undefined` ou `null`. Portanto, as seguintes desestruturas falham, mesmo antes de desestruturação acessa quaisquer propriedades:

```js
const { prop: x } = undefined;  // TypeError
const { prop: y } = null;      // TypeError
```

Como conseqüência, você pode usar o padrão de objeto vazio `{}` para verificar se um valor é coercivel para um objeto. Como vimos, apenas `undefined` e `null` não são:

```js
({} = [true, false]); // OK, Arrays are coercible to objects
({} = 'abc'); // OK, strings are coercible to objects

({} = undefined); // TypeError
({} = null); // TypeError
```

Os parênteses em torno das expressões são necessários porque as instruções não devem começar com chaves em JavaScript.

### Array patterns work with iterables

Array destructuring uses an iterator to get to the elements of a source. Therefore, you can Array-destructure any value that is iterable. Let’s look at examples of iterable values.

As strings são iteráveis:

```js
const [x,...y] = 'abc'; // x='a'; y=['b', 'c']
```

#### Failing to Array-destructure a value

Um valor é iterável se tiver um método cuja chave é `Symbol.iterator` que retorna um objeto. Array-desestruturação lança um `TypeError` se o valor a ser desestruturado não é iterável:

```js
let x;
[x] = [true, false]; // OK, Arrays are iterable
[x] = 'abc'; // OK, strings are iterable
[x] = { * [Symbol.iterator]() { yield 1 } }; // OK, iterable

[x] = {}; // TypeError, empty objects are not iterable
[x] = undefined; // TypeError, not iterable
[x] = null; // TypeError, not iterable
```

The `TypeError` is thrown even before accessing elements of the *iterable*, which means that you can use the empty Array pattern `[]` to check whether a value is *iterable*:

```js
[] = {}; // TypeError, empty objects are not iterable
[] = undefined; // TypeError, not iterable
[] = null; // TypeError, not iterable
```

## Default values

Default values are an optional feature of patterns. They provide a fallback if nothing is found in the source. If a part (an object property or an Array element) has no match in the source, it is matched against:

* its *default value* (if specified; it’s optional)
* `undefined` (otherwise)

Let’s look at an example. In the following destructuring, the element at index 0 has no match on the right-hand side. Therefore, destructuring continues by matching `x` against 3, which leads to `x` being set to 3.

```js
const [x=3, y] = []; // x = 3; y = undefined
```

Você também pode usar valores padrão em padrões de objeto:

```js
const {foo: x=3, bar: y} = {}; // x = 3; y = undefined
```

### `undefined` triggers default values

Default values are also used if a part does have a match and that match is `undefined`:

```js
const [x=1] = [undefined]; // x = 1
const {prop: y=2} = {prop: undefined}; // y = 2
```

### Default values are computed on demand

The default values themselves are only computed when they are needed. In other words, this destructuring:

```js
const {prop: y=someFunc()} = someValue;
```

é equivalente a:

```js
let y;
if (someValue.prop === undefined) {
    y = someFunc();
} else {
    y = someValue.prop;
}
```

Você pode observar que se você usar `console.log()`:

```js
function log(x) {
    console.log(x);
    return 'YES'
}

const [a=log('hello')] = [];
a   // 'YES'

const [b=log('hello')] = [123];
b   // 123
```

In the second destructuring, the default value is not triggered and `log()` is not called.

### Default values can refer to other variables in the pattern

Um valor padrão pode se referir a qualquer variável, incluindo outras variáveis ​​no mesmo padrão:

```js
const [x=3, y=x] = [];     // x=3; y=3
const [x=3, y=x] = [7];    // x=7; y=7
const [x=3, y=x] = [7, 2]; // x=7; y=2
```

However, order matters: the variables `x` and `y` are declared from left to right and produce a `ReferenceError` if they are accessed before their declarations:

```js
const [x=y, y=3] = []; // ReferenceError
```

### Default values for patterns

So far we have only seen default values for variables, but you can also associate them with patterns:

```js
const [{ prop: x } = {}] = [];
```

What does this mean? Recall the rule for default values: If a part has no match in the source, destructuring continues with the default value.

The element at index 0 has no match, which is why destructuring continues with:

```js
const { prop: x } = {}; // x = undefined
```

You can more easily see why things work this way if you replace the pattern `{ prop: x }` with the variable *pattern*:

```js
const [pattern = {}] = [];
```

### More complex default values

Let’s further explore default values for patterns. In the following example, we assign a value to `x` via the default value `{ prop: 123 }`:

```js
const [{ prop: x } = { prop: 123 }] = [];
```

Because the Array element at index `0` has no match on the right-hand side, destructuring continues as follows and x is set to `123`.

```js
const { prop: x } = { prop: 123 };  // x = 123
```

However, `x` is not assigned a value in this manner if the right-hand side has an element at index `0`, because then the default value isn’t triggered.

```js
const [{ prop: x } = { prop: 123 }] = [{}];
```

In this case, destructuring continues with:

```js
const { prop: x } = {}; // x = undefined
```

Thus, if you want `x` to be `123` if either the object or the property is missing, you need to specify a default value for `x` itself:

```js
const [{ prop: x=123 } = {}] = [{}];
```

Here, destructuring continues as follows, independently of whether the right-hand side is `[{}]` or `[]`.

```js
const { prop: x=123 } = {}; // x = 123
```

## More object destructuring features

### Property value shorthands

Property value shorthands are a feature of object literals: If the property value is a variable that has the same name as the property key then you can omit the key. This works for destructuring, too:

```js
const { x, y } = { x: 11, y: 8 }; // x = 11; y = 8

// Same as:
const { x: x, y: y } = { x: 11, y: 8 };
```

Você também pode combinar shorthands de valores de propriedade com valores padrão:

```js
const { x, y = 1 } = {}; // x = undefined; y = 1
```

### Computed property keys

Computed property keys are another object literal feature that also works for destructuring. You can specify the key of a property via an expression, if you put it in square brackets:

```js
const FOO = 'foo';
const { [FOO]: f } = { foo: 123 }; // f = 123
```

Computed property keys allow you to destructure properties whose keys are symbols:

```js
// Create and destructure a property whose key is a symbol
const KEY = Symbol();
const obj = { [KEY]: 'abc' };
const { [KEY]: x } = obj; // x = 'abc'

// Extract Array.prototype[Symbol.iterator]
const { [Symbol.iterator]: func } = [];
console.log(typeof func); // function
```

## More Array destructuring features

### Elision

Elision lets you use the syntax of Array “holes” to skip elements during destructuring:

```js
const [,, x, y] = ['a', 'b', 'c', 'd']; // x = 'c'; y = 'd'
```

### Rest operator (`...`)

The rest operator lets you extract the remaining elements of an iterable into an Array. If this operator is used inside an Array pattern, it must come last:

```js
const [x, ...y] = ['a', 'b', 'c']; // x='a'; y=['b', 'c']
```

*The spread operator has exactly the same syntax as the rest operator – three dots. But they are different: the former contributes data to Array literals and function calls, whereas the latter is used for destructuring and extracts data.*

If the operator can’t find any elements, it matches its operand against the empty Array. That is, it never produces `undefined` or `null`. For example:

```js
const [x, y, ...z] = ['a']; // x='a'; y=undefined; z=[]
```

The operand of the rest operator doesn’t have to be a variable, you can use patterns, too:

```js
const [x, ...[y, z]] = ['a', 'b', 'c']; // x = 'a'; y = 'b'; z = 'c'
```

The rest operator triggers the following destructuring:

```js
[y, z] = ['b', 'c']
```

## You can assign to more than just variables

If you assign via destructuring, each assignment target can be everything that is allowed on the left-hand side of a normal assignment.

Por exemplo, uma referência a uma propriedade (`obj.prop`):

```js
const obj = {};
({foo: obj.prop } = { foo: 123 });
console.log(obj); // {prop:123}
```

Ou uma referência a um elemento Array (`arr[0]`):

```js
const arr = [];
({ bar: arr[0] } = { bar: true });
console.log(arr); // [true]
```

You can also assign to object properties and Array elements via the rest operator `(...)`:

```js
const obj = {};
[first, ...obj.prop] = ['a', 'b', 'c']; // first = 'a'; obj.prop = ['b', 'c']
```

> If you declare variables or define parameters via destructuring then you must use simple identifiers, you can’t refer to object properties and Array elements.

## Pitfalls of destructuring

Há duas coisas a serem consideradas ao usar a desestruturação:

* You can’t start a statement with a curly brace.
* During destructuring, you can either declare variables or assign to them, but not both.

### Don’t start a statement with a curly brace

Because code blocks begin with a curly brace, statements must not begin with one. This is unfortunate when using object destructuring in an assignment:

```js
{ a, b } = someObject; // SyntaxError
```

O trabalho consiste em colocar a expressão completa entre parênteses:

```js
({ a, b } = someObject); // OK
```

A seguinte sintaxe não funciona:

```js
({ a, b }) = someObject; // SyntaxError
```

Com `let`, `var` e `const`, chaves nunca causam problemas:

```js
const { a, b } = someObject; // OK
```

## Examples of destructuring

**`for-of`**

```js
const map = new Map().set(false, 'no').set(true, 'yes');

for (const [key, value] of map) {
    console.log(key + ' is ' + value);
}
```

You can use destructuring to swap values. That is something that engines could optimize, so that no Array would be created.

```js
[a, b] = [b, a];
```

Você pode usar a desestruturação para dividir uma matriz:

```js
const [first, ...rest] = ['a', 'b', 'c']; // first = 'a'; rest = ['b', 'c']
```

### Destructuring returned Arrays

Some built-in JavaScript operations return Arrays. Destructuring helps with processing them:

```js
const [all, year, month, day] = /^(\d\d\d\d)-(\d\d)-(\d\d)$/.exec('2999-12-31');
```

`Array.prototype.split()` retorna uma matriz. Portanto, a desestruturação é útil se você estiver interessado nos elementos, e não na matriz:

```js
const cells = 'Allyson\tSilva\tCTO'
const [firstName, lastName, title] = cells.split('\t');
console.log(firstName, lastName, title);
```

### Destructuring returned objects

Destructuring is also useful for extracting data from objects that are returned by functions or methods. For example, the iterator method `next()` returns an object with two properties, `done` and `value`. The following code logs all elements of Array `arr` via the iterator `iter`. Destructuring is used in line `A`.

```js
const arr = ['a', 'b'];
const iter = arr[Symbol.iterator]();
while (true) {
    const {done, value} = iter.next(); // (A)
    if (done) break;
    console.log(value);
}
```

### Array-destructuring iterable values

Array-destructuring works with any *iterable* value. That is occasionally useful:

```js
const [x,y] = new Set().add('a').add('b'); // x = 'a'; y = 'b'
const [a,b] = 'foo'; // a = 'f'; b = 'o'
```
