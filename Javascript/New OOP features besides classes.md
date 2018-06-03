# New OOP features besides classes

## Visão geral(Overview)

### New object literal features

*Method definitions:*
```js
const obj = {
    myMethod(x, y) {
        ···
    }
};
```

*Property value shorthands:*
```js
const first = 'Allyson';
const last = 'Silva';

const obj = { first, last };
// Same as:
const obj = { first: first, last: last };
```

*Computed property keys:*
```js
const propKey = 'foo';
const obj = {
    [propKey]: true,
    ['b' + 'ar']: 123
};
```

*This new syntax can also be used for method definitions:*
```js
const obj = {
    ['h' + 'ello']() {
        return 'hi';
    }
};

console.log(obj.hello()); // hi
```

*The main use case for computed property keys is to make it easy to use symbols as property keys.*

### New methods in Object

The most important new method of Object is `assign()`. Traditionally, this functionality was called `extend()` in the JavaScript world. In contrast to how this classic operation works, `Object.assign()` only considers own (non-inherited) properties.

```js
const obj = { foo: 123 };
Object.assign(obj, { bar: true });

console.log(JSON.stringify(obj));   // {"foo":123,"bar":true}
```

## New features of object literals

### Method definitions

No *ECMAScript 5*, os métodos são propriedades cujos valores são funções:

```js
var obj = {
    myMethod: function (x, y) {
        ···
    }
};
```

In *ECMAScript 6*, methods are still *function-valued* properties, but there is now a more compact way of defining them:

```js
const obj = {
    myMethod(x, y) {
        ···
    }
};
```

*Getters* e *setters* continuam funcionando como fizeram no *ECMAScript 5* (observe como são sinteticamente similares as definições do método):

```js
const obj = {
    get foo() {
        console.log('GET foo');

        return 123;
    },
    set bar(value) {
        console.log('SET bar to ' + value);
        // return value is ignored
    }
};
```

Let’s use `obj`:

```js
obj.foo // GET foo  // 123
obj.bar = true  // SET bar to true  // true
```

There is also a way to concisely define properties whose values are *generator* functions:

```js
const obj = {
    * myGeneratorMethod () {
        ···
    }
};
```

Este código é equivalente a:

```js
const obj = {
    myGeneratorMethod: function * () {
        ···
    }
};
```

### Property value shorthands

Property value shorthands let you abbreviate the definition of a property in an object literal: If the name of the variable that specifies the property value is also the property key then you can omit the key. This looks as follows.

```js
const x = 4;
const y = 1;

const obj = { x, y };
```

A última linha é equivalente a:

```js
const obj = { x: x, y: y };
```

Property value shorthands work well together with *destructuring*:

```js
const obj = { x: 4, y: 1 };
const {x, y} = obj;

console.log(x); // 4
console.log(y); // 1
```

### Computed property keys

Remember that there are two ways of specifying a key when you set a property.

1. Via a fixed name: `obj.foo = true`;
2. Via an expression: `obj['b' + 'ar'] = 123`;

In object literals, you only have option #1 in *ECMAScript 5*. *ECMAScript 6* additionally provides option #2:

```js
const propKey = 'foo';

const obj = {
    [propKey]: true,
    ['b' + 'ar']: 123
};
```

Esta nova sintaxe também pode ser usada para definições de métodos:

```js
const obj = {
    ['h' + 'ello']() {
        return 'hi';
    }
};

console.log(obj.hello()); // hi
```

The main use case for computed property keys are *symbols*: you can define a public symbol and use it as a special property key that is always unique. One prominent example is the symbol stored in `Symbol.iterator`. If an object has a method with that key, it becomes *iterable*: The method must return an iterator, which is used by constructs such as the `for-of` loop to iterate over the object. The following code demonstrates how that works.

```js
const obj = {
    * [Symbol.iterator]() { // (A)
        yield 'hello';
        yield 'world';
    }
};

for (const x of obj) {
    console.log(x);
}

// Output:
// hello
// world
```

`obj` is *iterable*, due to the *generator method definition* starting in line `A`.

## Property Attributes

Each object has zero or more *properties*. Each property has a key and three or more attributes, named slots that store the data of the property (in other words, a property is itself much like a JavaScript object or like a record with fields in a database).

*ECMAScript 6* supports the following attributes (as does *ES5*):

* All properties have the attributes:
    * ***enumerable***: Setting this attribute to `false` hides the property from some operations.
    * ***configurable***: Setting this attribute to `false` prevents several changes to a property (attributes except `value` can’t be change, property can’t be deleted, etc.).
* Normal properties (data properties, methods) have the attributes:
    * ***value***: holds the value of the property.
    * ***writable***: controls whether the property’s value can be changed.
* Accessors (*getters*/*setters*) have the attributes:
    * ***get***: holds the getter (a function).
    * ***set***: holds the setter (a function).

You can retrieve the attributes of a property via `Object.getOwnPropertyDescriptor()`, which returns the attributes as a JavaScript object:

```js
const obj = { foo: 123 };
Object.getOwnPropertyDescriptor(obj, 'foo')
/*
Output:

{ value: 123,
  writable: true,
  enumerable: true,
  configurable: true }
*/
```
