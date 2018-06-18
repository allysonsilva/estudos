# Definições

## Object

### Propriedades

#### `hasOwnProperty()`

You already know a bit about the method `hasOwnProperty()`. It lets you differentiate between own methods and methods that have come down the prototype chain:

```js
({}).hasOwnProperty('toString'); // false
({}).constructor.prototype.hasOwnProperty('toString'); // true
```

Or another example using your own objects:

```js
var obj = { name: "Name" };
var constructor = function() {};
constructor.prototype = obj;

var a = new constructor();

a.name; // "Name"
a.hasOwnProperty('name'); // false
obj.hasOwnProperty('name'); // true
```

#### `propertyIsEnumerable()`

The method `propertyIsEnumerable()` lets you introspect an object and see if a property will come up in a for-in loop.

For example, arrays have a length property that doesn’t show up in a for-in loop:

```js
[].hasOwnProperty('length'); // true
[].propertyIsEnumerable('length'); // false
```

To avoid confusion when iterating with a for-in loop, it’s a common practice to check `hasOwnProperty()` inside the loop just to make sure you don’t get any unexpected properties (e.g., properties that are added to `Object.prototype`):

```js
var obj = {};

for (var prop in obj) {
    if (!obj.hasOwnProperty(prop)) {
        continue; // Filter out
    }

    // Real work here ...
}
```

#### `isPrototypeOf()`

As the name suggests, the method `isPrototypeOf()` helps you introspect objects:

```js
Array.isPrototypeOf({}); // false
Array.prototype.isPrototypeOf({}); // false
Array.prototype.isPrototypeOf([]); // true
```

This method also follows the inheritance chain:

```js
Object.prototype.isPrototypeOf([]); // true - Arrays are objects
```

#### constructor

Constructor is a property that lets you figure out which constructor function was used to create the object:

```js
({}).constructor === Object; // true
({}).constructor === Array; // false
```

Since only one specific constructor was used to create the object, you cannot use the constructor property to look up the inheritance chain the way you’d do with `isPrototypeOf()`:

```js
([]).constructor === Array; // true
([]).constructor === Object; // false
```

### Descritores de propriedade

Antes do ES5, a linguagem JavaScript não dava nenhuma forma direta no seu código para inspecionar ou obter qualquer distinção entre as características de propriedades, tais como se a propriedade era somente-leitura ou não.

Mas como no ES5, todas as propriedades são descritas em termos de um **descritor de propriedade**.

Considere esse código:

```js
var myObject = {
    a: 2
};

Object.getOwnPropertyDescriptor( myObject, "a" );
// {
//    value: 2,
//    writable: true,
//    enumerable: true,
//    configurable: true
// }
```

Enquanto nós podemos ver o que os valores padrão para as características do descritor de propriedade são quando criamos uma propriedade normal, nós podemos usar `Object.defineProperty(..)` para adicionar uma nova propriedade ou modificar uma já existente (se ela for `configurable`!), com as características desejadas.

```js
var myObject = {};

Object.defineProperty( myObject, "a", {
    value: 2,
    writable: true,
    configurable: true,
    enumerable: true
} );

myObject.a; // 2
```

Usando `defineProperty(..)`, adicionamos uma simples propriedade `a` ao `myObject` de maneira explícita manualmente. Entretanto, geralmente não usaríamos essa forma manual a menos que quiséssemos modificar uma das características do descritor a partir de seu comportamento normal.

#### Gravável

A habilidade de você mudar o valor de uma propriedade é controlada por `writable`.

Considere:

```js
var myObject = {};

Object.defineProperty( myObject, "a", {
    value: 2,
    writable: false, // não gravável!
    configurable: true,
    enumerable: true
} );

myObject.a = 3;

myObject.a; // 2
```

Como pode ver, nossa modificação do `value` falhou silenciosamente. Se tentarmos em `strict mode`, obtemos um erro:

```js
"use strict";

var myObject = {};

Object.defineProperty( myObject, "a", {
    value: 2,
    writable: false, // não gravável!
    configurable: true,
    enumerable: true
} );

myObject.a = 3; // TypeError
```
O `TypeError` diz que não podemos mudar uma propriedade não gravável.

**Lembrete:** Nós discutiremos getters/setters mais adiante, mas resumidamente, você pode observar que `writable:false` significa um valor que não pode ser alterado, equivalente um pouco no caso de quando uma operação de setter não é definida. Na verdade, a não-operação de setter precisaria lançar um `TypeError` quando fosse chamada, para se comportar de maneira similar ao `writable:false`.

#### Configurável

Contanto que uma propriedade seja configurável, podemos modificar sua definição de descritor usando o mesmo método `defineProperty(..)`.

```js
var myObject = {
    a: 2
};

myObject.a = 3;
myObject.a; // 3

Object.defineProperty( myObject, "a", {
    value: 4,
    writable: true,
    configurable: false, // não configurável!
    enumerable: true
} );

myObject.a; // 4
myObject.a = 5;
myObject.a; // 5

Object.defineProperty( myObject, "a", {
    value: 6,
    writable: true,
    configurable: true,
    enumerable: true
} ); // TypeError
```

A última chamada de `defineProperty(..)` resulta em um `TypeError`, independente do `strict mode`, se você tentar mudar a definição do descritor de uma propriedade não configurável. Cuidado: como você pode ver, alterando `configurable` para `false` é uma **ação de via única, e não pode ser desfeita!**

**Nota:** Há uma exceção diferenciada para estar ciente: mesmo se a propriedade já está com `configurable:false`, `writable` sempre pode ser alterada de `true` para `false` sem erro, mas não o contrário, de `false` para `true`.

Outra coisa que `configurable:false` previne é a habilidade de usar o operador `delete` para remover uma propriedade existente.

```js
var myObject = {
    a: 2
};

myObject.a; // 2
delete myObject.a;
myObject.a; // undefined

Object.defineProperty( myObject, "a", {
    value: 2,
    writable: true,
    configurable: false,
    enumerable: true
} );

myObject.a; // 2
delete myObject.a;
myObject.a; // 2
```

Como pode ver, a última chamada de `delete` falhou (silenciosamente) porque definimos a propriedade `a` como não-configurável.

#### Enumerável

A última característica de descritor que vamos mencionar aqui (existem outras duas, que veremos em breve quando discutimos getter/setters) é `enumerable`.

O nome provavelmente é óbvio, mas essa característica controla se uma propriedade aparecerá em certas enumerações objeto-propriedade, tal como o laço `for..in`. Configure para `false` para não aparecer em tais enumerações, mesmo que ainda esteja completamente acessível. Configure `true` para mantê-lo presente.

Todas as propriedades normais definidas pelo usuário são padronizadas para `enumerable`, visto que isso é que você geralmente deseja. Mas se você tem uma propriedade especial que queira ocultar da enumeração, configure-a para `enumerable:false`.

Iremos demonstrar enumerabilidade em mais detalhes mais adiante, então lembre-se desse tópico

### Constante de Objeto

Combinando `writable:false` e `configurable:false`, você pode essencialmente criar uma *constante* (não pode ser alterada, redefinida ou removida) como uma propriedade de objeto, como:

```js
var myObject = {};

Object.defineProperty( myObject, "FAVORITE_NUMBER", {
    value: 42,
    writable: false,
    configurable: false
} );
```

### Prevenir Extensões

Se você quiser prevenir que um objeto tenha novas propriedades adicionadas a ele, mantendo apenas o resto das propriedades do objeto, chame `Object.preventExtensions(..)`:

```js
var myObject = {
    a: 2
};

Object.preventExtensions( myObject );

myObject.b = 3;
myObject.b; // undefined
```

No `non-strict mode` a criação de `b` falha silenciosamente. No `strict mode` é lançado um `TypeError`.

#### Seal

`Object.seal(..)` cria um objeto "selado", que pega um objeto existente e essencialmente chama `Object.preventExtensions(..)`, mas também marca todas as propriedades existentes como `configurable:false`.

Então, além de você não poder adicionar mais propriedades, também não pode reconfigurar ou deletar qualquer propriedade existente (embora ainda *possa* modificar seu valor).

#### Freeze

`Object.freeze(..)` cria um objeto "congelado", que pega um objeto existente e essencialmente chama `Object.seal(..)`, mas também marca todas as propriedades "acessores de dados" como `writable:false`, com isso os valores das proriedades não podem ser alteradas.

Essa abordagem é o nível mais alto de imutabilidade para uma objeto que você pode atingir, visto que previne qualquer mudança no objeto ou em qualquer de suas propriedades diretas (embora, como mecionado acima, o conteúdo de qualquer objeto referenciado não é afetado).

Você poderia "deep freeze" um objeto através do `Object.freeze(..)`, e então iterar recursivamente sobre todos os objetos que dado objeto referencia (que teriam sido afetados até agora), e chamando `Object.freeze(..)` para eles também. Cuidado, embora isso possa afetar outros objetos (compartilhados) você não tem intenção de afetá-los.

### `[[Put]]`

Uma vez que há uma operação `[[Get]]` definida internamente para obter o valor de uma propriedade, deveria ser óbvio que há também uma operação `[[Put]]` padrão.

Pode ser tentador pensar que a atribuição de uma propriedade de um objeto iria apenas invocar `[[Put]]` para definir ou criar a propriedade do objeto em questão. Mas esse caso possui algumas especificidades a mais.

Quando invocamos `[[Put]]`, o modo como ele se comporta muda baseado em alguns fatores, incluindo (o mais impactante) se a propriedade já está presente no objeto ou não.

Se a propriedade estiver presente, o algoritmo de `[[Put]]` irá checar grosseiramente:

1. A propriedade é um descritor de acessor? (veja a seção abaixo sobre "Getters & Setters") **Se sim, chame o setter, caso exista um**
2. A propriedade é um descritor de dado com `writable` definido como `false`? **Se sim, falha silenciosamente no `non-strict mode` ou lança `TypeError` no `strict mode`.**
3. Caso contrário, define o valor para a propriedade existente normalmente.

Se a propriedade ainda não estiver presente no objeto em questão, a operação `[[Put]]` é ainda mais diferenciada e complexa. Nós iremos rever esse caso com mais clareza no Capítulo 5 quando discutimos `[[Prototype]]`.

### Getters & Setters

As operações padrão `[[Put]]` e `[[Get]]` de objetos controlam como valores são definidos em propriedades novas ou já existentes, ou obtidos de propriedades existentes, respectivamente.

ES5 introduziu uma forma de sobrescrever parte dessas operações, não no nível de objeto mas no nível por-propriedade, através do uso de _getters_ e _setters_. Getters são propriedades que, na verdade, chamam uma função oculta para recuperar um valor. Setters são propriedades que chamam uma função oculta para estabelecer um valor.

Quando você define uma propriedade para ter um getter ou um setter ou ambos, a definição da propriedade se torna um "descritor de acessor" (o oposto de um "descritor de dados"). Para descritores de acessor, as características da propriedade de `value` e `writable` são discutíveis e ignoradas, em vez disso, o JS considera as características da propriedade de `set` e `get` (como também `configurable` e `enumerable`).

Considere:

```js
var myObject = {
    // define um getter para `a`
    get a() {
        return 2;
    }
};

Object.defineProperty(
    myObject, // alvo
    "b", // nome da propriedade
    { // descritor
        // define um getter para `b`
        get: function(){ return this.a * 2 },

        // certifica que `b` aparece como uma propriedade do objeto
        enumerable: true
    }
);

myObject.a; // 2

myObject.b; // 4
```

Através da sintaxe de objeto-literal com `get a() { .. }` ou por definição explícita com `defineProperty(..)`, nos dois casos nós criamos uma propriedade do objeto que realmente não mantém um valor, mas o acesso à propriedade resulta automaticamente em uma chamada de função oculta a uma função getter, com qualquer valor que ela retorna sendo o resultado do acesso à propriedade.

```js
var myObject = {
    // define um getter para `a`
    get a() {
        return 2;
    }
};

myObject.a = 3;

myObject.a; // 2
```

Desde que nós apenas definimos um getter para `a`, se tentarmos definir o valor de `a` depois, a operação de set não lançará um erro, mas irá descartar a atribuição silenciosamente. Mesmo se houvesse um setter válido, nosso getter personalizado é _hard-coded_ para retornar apenas `2`, logo o operação de set seria discutível.

Para deixar esse cenário mais sensível, propriedades deveriam ser definidades com setters também, que sobrescrevem a operação `[[Put]]` padrão (conhecida como atribuição), por-propriedade, apenas como esperaríamos. Você provavelmente vai sempre querer declarar tanto getter como setter (ter apenas um ou outro geralmente leva a um comportamento inesperado/espantoso):

```js
var myObject = {
    // define um getter para `a`
    get a() {
        return this._a_;
    },

    // define um setter para `a`
    set a(val) {
        this._a_ = val * 2;
    }
};

myObject.a = 2;

myObject.a; // 4
```

**Nota:** Nesse exemplo, nós armazenamos um específico valor `2` da atribuição (`[[Put]]` operation) em outra variável `_a_`. O nome `_a_` é uma mera convenção para esse exemplo e não implica em nada especial em relação a comportamento -- é uma propriedade normal assim como qualquer outra.

### Existência

Podemos perguntar a um objeto se ele possue certa propriedade *sem* pedir para obter o valor da propriedade:

```js
var myObject = {
    a: 2
};

("a" in myObject); // true
("b" in myObject); // false

myObject.hasOwnProperty( "a" ); // true
myObject.hasOwnProperty( "b" ); // false
```

O operador `in` verificará se a propriedade *está* no objeto ou se ela existe em algum nível mais alto da cadeia de `[[Prototype]]` do objeto (veja Capítulo 5). Em contraste ao `in`, `hasOwnProperty(..)` verifica *apenas* se `myObject` tem a propriedade ou não, e não consultará a cadeia de `[[Prototype]]`. Nós voltaremos a discutir importantes diferenças entre essas duas operações no Capítulo 5, onde examinamos `[[Prototype]]`s em detalhes.

`hasOwnProperty(..)` é acessível para todos os objetos normais via delegação ao `Object.prototype` (veja o Capítulo 5). Mas é possível criar um objeto que seja ligado ao `Object.prototype` (via `Object.create(null)`) -- (veja o Capítulo 5). Nesse caso, uma chamada de método como `myObject.hasOwnProperty(..)` falharia.

Nesse cenário, um modo mais robusto de realizar tal verificação é `Object.prototype.hasOwnProperty.call(myObject,"a")`, que toma emprestado o método base `hasOwnProperty(..)` e usa um *_binding_ explícito de `this`* (veja o Capítulo 2) para aplicar ao nosso `myObject`.

**Nota:** O operador `in` aparenta que verificará a existência de um *valor* dentro de um container, mas na verdade ele verifica a existência de um nome de propriedade. Essa diferença é importante de se notar no que se diz respeito a arrays, onde há uma forte tentação de verificar algo como `4 in [2, 4, 6]`, mas isso não se comportará da maneira esperada.

#### Enumeração

Anteriormente, explicamos brevemente a ideia de "enumerabilidade" quando olhamos para a característica do descritor de propriedade de `enumerable`. Vamos rever e examinar isso mais detalhadamente.

```js
var myObject = { };

Object.defineProperty(
    myObject,
    "a",
    // torne `a` enumarável normalmente
    { enumerable: true, value: 2 }
);

Object.defineProperty(
    myObject,
    "b",
    // torne `b` não-enumerável
    { enumerable: false, value: 3 }
);

myObject.b; // 3
("b" in myObject); // true
myObject.hasOwnProperty( "b" ); // true

// .......

for (var k in myObject) {
    console.log( k, myObject[k] );
}
// "a" 2
```

Você vai perceber que `myObject.b` de fato **existe** e tem um valor acessível, mas não aparece no laço `for..in` (embora, surpreendentemente, ele seja mostrado pelo operador de `in` que verifica se a propriedade existe). Isso acontece porque "enumerable" basicamente significa "será incluído se for possível iterar sobre as propriedades do objeto".

**Lembrete:** Laços `for..in` aplicados a arrays podem gerar resultados inesperados, nisso a enumeração de um array incluirá não apenas todos os índices numéricos, mas também qualquer propriedade enumerável. É uma boa ideia usar laços `for..in` *apenas* em objetos e laços tradicionais `for` com iteração com índices numéricos para os valores armazenados em arrays.

Outra maneira que propriedades enumeráveis e não-enumeráveis podem ser distinguidas:

```js
var myObject = { };

Object.defineProperty(
    myObject,
    "a",
    // Faz com que `a` seja enumerável
    { enumerable: true, value: 2 }
);

Object.defineProperty(
    myObject,
    "b",
    // Faz com que `b` seja não-enumerável
    { enumerable: false, value: 3 }
);

myObject.propertyIsEnumerable( "a" ); // true
myObject.propertyIsEnumerable( "b" ); // false

Object.keys( myObject ); // ["a"]
Object.getOwnPropertyNames( myObject ); // ["a", "b"]
```

`propertyIsEnumerable(..)` testa se dado nome de propriedade existe *diretamente* em um objeto e também se é `enumerable:true`.

`Object.keys(..)` retorna um array de todas propriedades enumeráveis, enquanto que `Object.getOwnPropertyNames(..)` retorna um array de *todas* as propriedades, enumerável ou não.

Enquanto que `in` vs. `hasOwnProperty(..)` divergem com relação se eles consultam a cadeia de `[[Prototype]]` ou não, `Object.keys(..)` e `Object.getOwnPropertyNames(..)` inspecionam *apenas* o objeto direto especificado.

Atualmente, não há uma maneira nativa de obter uma lista de **todas as propriedades** equivalente ao que o operator `in` faz (percorrendo todas as propriedades de toda a cadeia de `[[Prototype]]`, como foi explicado no Capítulo 5). Você poderia ter um recurso parecido percorrendo recursivamente a cadeia de `[[Prototype]]` de um objeto e, para cada nível, capturar a lista de propriedades de `Object.keys(..)` -- apenas propriedades enumeráveis.
