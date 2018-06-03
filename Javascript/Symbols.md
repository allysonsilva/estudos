# Symbols

## Visão geral

Os símbolos são um novo tipo primitivo no *ECMAScript 6*. Eles são criados através de uma factory function:

```js
const mySymbol = Symbol('mySymbol');
```

Toda vez que você chama a factory function, um novo e exclusivo símbolo é criado. O parâmetro opcional é uma string descritiva que é exibida ao imprimir o símbolo(não tem outro propósito):

```js
mySymbol // Symbol(mySymbol)
```

> Em contraste com as strings, os símbolos são únicos e evitam conflitos de nomes.

A função `Symbol()` é uma factory para símbolos - cada valor que retorna é exclusivo:

```js
Symbol('foo') !== Symbol('foo') // true
```

O único método útil que os símbolos possuem é `toString()` (via `Symbol.prototype.toString()`).

### Use case 1: unique property keys

Os símbolos são usados ​​principalmente como únicas *property keys* - um símbolo nunca entra em conflito com qualquer outra property key (símbolo ou string). Por exemplo, você pode fazer um objeto iterável (utilizável através do loop `for-of` e outros mecanismos da linguagem), usando o símbolo armazenado `Symbol.iterator` como a chave de um método:

```js
const iterableObject = {
    [Symbol.iterator]() {   // (A)
        ···
    }
}

for (const x of iterableObject) {
    console.log(x);
}

// Output:
// hello
// world
```

Na linha (A), um símbolo é usado como a chave do método. Este marcador exclusivo torna o objeto iterável e nos permite usar no loop `for-of`.

### Use case 2: constants representing concepts

No *ECMAScript 5*, você pode ter usado strings para representar conceitos como cores. No *ES6*, você pode usar símbolos e ter certeza de que eles são sempre únicos:

```js
const COLOR_RED = Symbol('Red');
const COLOR_ORANGE = Symbol('Orange');
const COLOR_YELLOW = Symbol('Yellow');
const COLOR_GREEN  = Symbol('Green');
const COLOR_BLUE = Symbol('Blue');
const COLOR_VIOLET = Symbol('Violet');

function getComplement(color) {
    switch (color) {
        case COLOR_RED:
            return COLOR_GREEN;
        case COLOR_ORANGE:
            return COLOR_BLUE;
        case COLOR_YELLOW:
            return COLOR_VIOLET;
        case COLOR_GREEN:
            return COLOR_RED;
        case COLOR_BLUE:
            return COLOR_ORANGE;
        case COLOR_VIOLET:
            return COLOR_YELLOW;
        default:
            throw new Exception('Unknown color: '+color);
    }
}
```

Toda vez que você chama `Symbol('Red')`, um novo símbolo é criado. Portanto, `COLOR_RED` nunca pode ser confundido com outro valor. Isso seria diferente se fosse a string 'Red'.

### Pitfall: you can’t coerce symbols to strings

A coerção (convertendo implicitamente) de símbolos para strings lança exceções:

```js
const sym = Symbol('desc');

const str1 = '' + sym;  // TypeError
const str2 = `${sym}`;  // TypeError
```

A única solução é converter explicitamente:

```js
const str2 = String(sym);       // 'Symbol(desc)'
const str3 = sym.toString();    // 'Symbol(desc)'
```

A proibição da coerção impede alguns erros, mas também torna o trabalho com símbolos mais complicado.

### Which operations related to property keys are aware of symbols?

As seguintes operações estão cientes de símbolos como *property keys*:

* `Reflect.ownKeys()`
* Property access via `[]`
* `Object.assign()`

As seguintes operações ignoram símbolos como property keys:

* `Object.keys()`
* `Object.getOwnPropertyNames()`
* `for-in loop`

## Um novo tipo primitivo

O *ECMAScript 6* introduziu um novo tipo primitivo: `symbols`. São *tokens* que servem como IDs únicos. Você cria símbolos através da factory function `Symbol()` (que é vagamente semelhante a `String` retornando strings se chamado como uma função):

```js
const symbol1 = Symbol();
```

`Symbol()` tem um parâmetro opcional *string-valued* que permite que você dê o símbolo recém-criado uma descrição. Essa descrição é usada quando o símbolo é convertido em uma string (via `toString()` ou `String()`):

```js
const symbol2 = Symbol('symbol2');
String(symbol2) // 'Symbol(symbol2)'
```

Cada símbolo retornado pelo `Symbol()` é único, cada símbolo tem sua própria identidade:

```js
Symbol() === Symbol() // false
```

Você pode ver que os símbolos são primitivos se você aplicar o operador `typeof` a um deles – ele retornará um novo resultado específico do símbolo:

```js
typeof Symbol()   // 'symbol'
```

### Symbols as property keys

Os símbolos podem ser usados ​​como *property keys*:

```js
const MY_KEY = Symbol();
const obj = {};

obj[MY_KEY] = 123;
console.log(obj[MY_KEY]);   // 123
```

Classes e object literals têm um recurso chamado *computed property keys*: você pode especificar a chave de uma propriedade por meio de uma expressão, colocando-o em colchetes. No object literals a seguir, usamos uma computed property key para tornar o valor de `MY_KEY` a chave de uma propriedade.

```js
const MY_KEY = Symbol();

const obj = {
    [MY_KEY]: 123
};
```

Uma definição de método também pode ter uma computed key:

```js
const FOO = Symbol();

const obj = {
    [FOO]() {
        return 'bar';
    }
};

console.log(obj[FOO]());    // bar
```

#### Enumerating own property keys

Dado que existe agora um novo tipo de valor que pode se tornar a chave de uma propriedade, a seguinte terminologia é usada para *ECMAScript 6*:

* As property keys são strings ou símbolos.
* As property keys com valor de strings são chamadas de *property names*.
* As property keys com valor de símbolo são chamadas de *property symbols*.

Vamos examinar as APIs para enumerar as próprias property keys criando primeiro um objeto.

```js
const obj = {
    [Symbol('my_key')]: 1,
    enum: 2,
    nonEnum: 3
};

Object.defineProperty(obj, 'nonEnum', { enumerable: false });
```

`Object.getOwnPropertyNames()` ignores *symbol-valued* property keys:

```js
Object.getOwnPropertyNames(obj) // ['enum', 'nonEnum']
```

`Object.getOwnPropertySymbols()` ignores *string-valued* property keys:

```js
Object.getOwnPropertySymbols(obj)   // [Symbol(my_key)]
```

`Reflect.ownKeys()` considers all kinds of keys:

```js
Reflect.ownKeys(obj)  // [Symbol(my_key), 'enum', 'nonEnum']
```

`Object.keys()` only considers enumerable property keys that are strings:

```js
Object.keys(obj)  // ['enum']
```

O nome `Object.keys` entra em conflito com a nova terminologia (apenas as chaves de string estão listadas). `Object.names` ou `Object.getEnumerableOwnPropertyNames` seria uma escolha melhor agora.

## Using symbols to represent concepts

No *ECMAScript 5*, muitas vezes representam conceitos (think enum constantes) através de strings. Por exemplo:

```js
var COLOR_RED = 'Red';
var COLOR_ORANGE = 'Orange';
var COLOR_YELLOW = 'Yellow';
var COLOR_GREEN  = 'Green';
var COLOR_BLUE = 'Blue';
var COLOR_VIOLET = 'Violet';
```

No entanto, as strings não são tão únicas como gostaríamos que elas fossem. Para ver o porque, vejamos a seguinte função.

```js
function getComplement(color) {
    switch (color) {
        case COLOR_RED:
            return COLOR_GREEN;
        case COLOR_ORANGE:
            return COLOR_BLUE;
        case COLOR_YELLOW:
            return COLOR_VIOLET;
        case COLOR_GREEN:
            return COLOR_RED;
        case COLOR_BLUE:
            return COLOR_ORANGE;
        case COLOR_VIOLET:
            return COLOR_YELLOW;
        default:
            throw new Exception('Unknown color: '+color);
    }
}
```

Vale ressaltar que você pode usar expressões arbitrárias com `switch` cases, você não está limitado de forma alguma. Por exemplo:

```js
function isThree(x) {
    switch (x) {
        case 1 + 1 + 1:
            return true;
        default:
            return false;
    }
}
```

Usamos a flexibilidade que `switch` nos oferece e se referem às cores através de nossas constantes(COLOR_RED etc.) em vez de codificá-las ('RED' etc.).

É interessante notar que, mesmo que fazendo dessa forma, ainda pode haver confusões. Por exemplo, alguém pode definir uma constante para um humor:

```js
var MOOD_BLUE = 'BLUE';
```

Agora, o valor de `BLUE` não é mais exclusivo e `MOOD_BLUE` pode ser confundido com ele. Se você usá-lo como um parâmetro para `getComplement()`, ele retornará `Orange` onde deveria lançar uma exceção.

Vamos usar símbolos para corrigir este exemplo. Agora, também podemos usar o recurso *ES6* `const`, o que nos permite declarar constantes reais (você não pode alterar o valor vinculado a uma constante, mas o valor em si pode ser mutável).

```js
const COLOR_RED = Symbol('Red');
const COLOR_ORANGE = Symbol('Orange');
const COLOR_YELLOW = Symbol('Yellow');
const COLOR_GREEN  = Symbol('Green');
const COLOR_BLUE = Symbol('Blue');
const COLOR_VIOLET = Symbol('Violet');
```

Cada valor retornado `Symbol` é exclusivo, e é por isso que nenhum outro valor pode ser confundido por `BLUE`. Curiosamente, o código de `getComplement()` não se altera se usarmos símbolos em vez de strings, o que mostra como eles são semelhantes.

## Symbols as keys of properties

Ser capaz de criar propriedades cujas chaves nunca confrontar com outras chaves é útil em duas situações:

* For non-public properties in inheritance hierarchies.
* To keep meta-level properties from clashing with base-level properties.

### Symbols as keys of non-public properties

Whenever there are inheritance hierarchies in JavaScript (e.g. created via classes, mixins or a purely prototypal approach), you have two kinds of properties:

* *Public properties* are seen by clients of the code.
* *Private properties* are used internally within the pieces (e.g. classes, mixins or objects) that make up the inheritance hierarchy. (*Protected properties* are shared between several pieces and face the same issues as private properties.)

For usability’s sake, public properties usually have string keys. But for private properties with string keys, accidental name clashes can become a problem. Therefore, symbols are a good choice. For example, in the following code, symbols are used for the private properties `_counter` and `_action`.

```js
const _counter = Symbol('counter');
const _action = Symbol('action');

class Countdown {
    constructor(counter, action) {
        this[_counter] = counter;
        this[_action] = action;
    }

    dec() {
        let counter = this[_counter];
        if (counter < 1) return;
        counter--;
        this[_counter] = counter;
        if (counter === 0) {
            this[_action]();
        }
    }
}
```

Note that symbols only protect you from name clashes, not from unauthorized access, because you can find out all own property keys – including symbols – of an object via `Reflect.ownKeys()`.

## Converting symbols to other primitive types

A tabela a seguir mostra o que acontece se você converter explicitamente ou implicitamente símbolos em outros tipos primitivos:

| **Conversion to** | **Explicit conversion**   | **Coercion (implicit conversion)** |
|-------------------|---------------------------|------------------------------------|
| *boolean*         | `Boolean(sym)` → OK       | `!sym` → OK                        |
| *number*          | `Number(sym)` → TypeError | `sym*2` → TypeError                |
| *string*          | `String(sym)` → OK        | `''+sym` → TypeError               |
|                   | `sym.toString()` → OK     | ``${sym}`` → TypeError             |

### Pitfall: coercion to string

Coercion to string being forbidden can easily trip you up:

```js
const sym = Symbol();

console.log('A symbol: '+sym);      // TypeError
console.log(`A symbol: ${sym}`);    // TypeError
```

Para corrigir esses problemas, você precisa de uma conversão explícita para string:

```js
console.log('A symbol: '+String(sym));      // OK
console.log(`A symbol: ${String(sym)}`);    // OK
```

### Explicit and implicit conversion in the spec

#### Converting to boolean

To explicitly convert a symbol to boolean, you call `Boolean()`, which returns `true` for symbols:

```js
const sym = Symbol('hello');
Boolean(sym)    // true
```

`Boolean()` computes its result via the internal operation `ToBoolean()`, which returns `true` for symbols and other truthy values.

Coercion also uses `ToBoolean()`:

```js
!sym // false
```

#### Converting to number

Para converter explicitamente um símbolo em número, você chama `Number()`:

```js
const sym = Symbol('hello');
Number(sym) // TypeError: can't convert symbol to number
```

`Number()` computes its result via the internal operation `ToNumber()`, which throws a `TypeError` for symbols.

A coerção também usa `ToNumber()`:

```js
+sym // TypeError: can't convert symbol to number
```

#### Converting to string

Para converter explicitamente um símbolo em string, você chama `String()`:

```js
const sym = Symbol('hello');
String(sym)   // 'Symbol(hello)'
```

If the parameter of `String()` is a symbol then it handles the conversion to string itself and returns the string `Symbol()` wrapped around the description that was provided when creating the symbol. If no description was given, the empty string is used:

```js
String(Symbol()) // 'Symbol()'
```

The `toString()` method returns the same string as `String()`, but neither of these two operations calls the other one, they both call the same internal operation `SymbolDescriptiveString()`.

```js
Symbol('hello').toString()    // 'Symbol(hello)'
```

Coercion is handled via the internal operation `ToString()`, which throws a `TypeError` for symbols. One method that coerces its parameter to string is `Number.parseInt()`:

```js
Number.parseInt(Symbol())   // TypeError: can't convert symbol to string
```

## Wrapper objects for symbols

Enquanto todos os outros valores primitivos têm literais, você precisa criar símbolos por função chamada `Symbol`. Assim, existe o risco de invocar acidentalmente `Symbol` como construtor. Que produz instâncias de `Symbol`, que não são muito úteis. Portanto, uma exceção é lançada quando você tenta fazer isso:

```js
new Symbol()  // TypeError: Symbol is not a constructor
```

There is still a way to create wrapper objects, instances of `Symbol: Object`, called as a function, converts all values to objects, including symbols.

```js
const sym = Symbol();
typeof sym // 'symbol'

const wrapper = Object(sym);
typeof wrapper // 'object'
wrapper instanceof Symbol // true
```

### Accessing properties via [ ] and wrapped keys

The square bracket operator `[ ]` normally coerces its operand to string. There are now two exceptions: symbol wrapper objects are unwrapped and symbols are used as they are. Let’s use the following object to examine this phenomenon.

```js
const sym = Symbol('yes');
const obj = {
    [sym]: 'a',
    str: 'b',
};
```

The square bracket operator unwraps wrapped symbols:

```js
const wrappedSymbol = Object(sym);
typeof wrappedSymbol // 'object'
obj[wrappedSymbol]  // 'a'
```

Like any other value not related to symbols, a wrapped string is converted to a string by the square bracket operator:

```js
const wrappedString = new String('str');
typeof wrappedString // 'object'
obj[wrappedString]  // 'b'
```
