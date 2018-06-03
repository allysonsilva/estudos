# New string features

## Overview

*Novos métodos de `string`:*

```js
'hello'.startsWith('hell')  // true
'hello'.endsWith('ello')    // true
'hello'.includes('ell')     // true
'doo '.repeat(3)            // 'doo doo doo '
```

*ES6* tem um novo tipo de literal de `string`, o ***modelo literal***:

```js
// String interpolation via template literals (in backticks)
const first = 'Allyson';
const last = 'Silva';
console.log(`Hello ${first} ${last}!`); // Hello Allyson Silva!

// Template literals also let you create strings with multiple lines
const multiLine = `
This is
a string
with multiple
lines`;
```

## String interpolation, multi-line string literals and raw string literals

*Template literals* fornecem três características interessantes.

Primeiro, *template literals* suportam a *interpolação de strings*:

```js
const first = 'Allyson';
const last = 'Silva';
console.log(`Hello ${first} ${last}!`); // Hello Allyson Silva!
```

Em segundo lugar, *template literals* podem conter várias linhas:

```js
const multiLine = `
This is
a string
with multiple
lines`;
```

Em terceiro lugar, os *template literals* são "raw" se você os prefixar com a tag `String.raw`:

```js
const str = String.raw`Not a newline: \n`;
console.log(str === 'Not a newline: \\n');  // true
```

## Iterando sobre strings

As `strings` são *iterables*, o que significa que você pode usar `for-of` para iterar sobre seus caracteres:

```js
for (const ch of 'abc') {
    console.log(ch);
}

// Output:
// a
// b
// c
```

E você pode usar o operador *spread*(`...`) para transformar strings em *Arrays*:

```js
const chars = [...'abc'];   // ['a', 'b', 'c']
```

## Checking for inclusion

Três novos métodos verificam se uma string existe dentro de outra string:

```js
'hello'.startsWith('hell')    // true
'hello'.endsWith('ello')      // true
'hello'.includes('ell')       // true
```

Cada um desses métodos tem uma posição como um segundo parâmetro opcional, que especifica onde a string a ser pesquisada inicia ou termina:

```js
'hello'.startsWith('ello', 1)   // true
'hello'.endsWith('hell', 4)     // true

'hello'.includes('ell', 1)  // true
'hello'.includes('ell', 2)  // false
```

## Repeating strings

Use o método `repeat()` para repetir strings:

```js
'doo '.repeat(3)  // 'doo doo doo '
```
