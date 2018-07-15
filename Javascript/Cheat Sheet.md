# Cheat Sheet

## Functions

### Functions as First-Class Things

Some programmers familiar with JavaScript, myself included, consider it to be a functional language. Of course, to say such a thing implies that others disagree with that assessment. The reason for this disagreement stems from the fact that functional programming often has a relative definition, differing in minor and major ways from one practitioner or theorist to another.

This is a sad state of affairs, indeed. Thankfully, however, almost every single relative definition of functional programming seems to agree on one point: a functional programming language is one facilitating the use and creation of first-class functions.

Typically, you will see this point accompanied by other definitional qualifications including but not limited to static typing, pattern matching, immutability, purity, and so on. However, while these other points describe certain implementations of functional programming languages, they fail in broad applicability. If I boil down the definition to its essence, consisting of the terms “facilitating” and “first-class functions,” then it covers a broad range of languages from Haskell to JavaScript—the latter being quite important to this book. Thankfully, this also allows first-class functions to be defined in a paragraph.

The term “first-class” means that something is just a value. A first-class function is one that can go anywhere that any other value can go—there are few to no restrictions. A number in JavaScript is surely a first-class thing, and therefore a first-class function has a similar nature:

-   A number can be stored in a variable and so can a function:

```js
var fortytwo = function() {
    return 42
}
```

-   A number can be stored in an array slot and so can a function:

```js
var fortytwos = [
    42,
    function() {
        return 42
    }
]
```

-   A number can be stored in an object field and so can a function:

```js
var fortytwos = {
    number: 42,
    fun: function() {
        return 42
    }
}
```

-   A number can be created as needed and so can a function:

```js
;(function() {
    return 42
})() //=> 84
```

-   A number can be passed to a function and so can a function:

```js
function weirdAdd(n, f) {
    return n + f()
}

weirdAdd(42, function() {
    return 42
}) //=> 84
```

-   A number can be returned from a function and so can a function:

```js
return 42
return function() {
    return 42
}
```

The last two points define by example what we would call a “higher-order” function; put directly, a higher-order function can do one or both of the following:

-   Take a function as an argument
-   Return a function as a result

### Calling Versus Referencing

Em JavaScript, as funções são objetos e, como tal, podem ser transmitidas e atribuídas como qualquer outro objeto. É importante entender a distinção entre chamar uma função e simplesmente referenciá-la. Quando você inferi um identificador de função com parênteses, o JavaScript sabe que o está chamando: ele executa o corpo da função e a expressão resolve o valor de retorno. Quando você não fornece os parênteses, você está simplesmente se referindo à função como qualquer outro valor e não é invocado. Experimente o seguinte em um console do JavaScript:

```js
getGreeting() // "Hello, World!"
getGreeting // function getGreeting()
```

Ser capaz de fazer referência a uma função como qualquer outro valor (sem chamá-lo, executá-lo) permite muita flexibilidade na linguagem. Por exemplo, você pode atribuir uma função a uma variável, que permite chamar a função por outro nome:

```js
const f = getGreeting
f() // "Hello, World!"
```

Ou atribua uma função a uma propriedade de objeto:

```js
const o = {}
o.f = getGreeting
o.f() // "Hello, World!"
```

Ou até mesmo adicionar uma função a uma matriz:

```js
const arr = [1, 2, 3]
arr[1] = getGreeting // arr is now [1, function getGreeting(), 2]
arr[1]() // "Hello, World!"
```

Este último exemplo deve deixar claro o papel dos parênteses: se o JavaScript encontrar parênteses que seguem um valor, o valor é assumido como uma função e essa função é chamada. No exemplo anterior, `arr[1]` é uma expressão que resolve um valor. Esse valor é seguido por parênteses, que é um sinal para JavaScript que o valor é uma função e deve ser chamado.

_Se você tentar adicionar parênteses a um valor que não é uma função, você receberá um erro. Por exemplo, `"whoops"()` resultará no erro `TypeError: "whoops" is not a function`._

#### Do Arguments Make the Function?

Em muitas linguagens, a assinatura de uma função inclui seus argumentos. Por exemplo, em C, `f()` (sem argumentos) é uma função diferente de `f(x)` (um argumento), que é uma função diferente de `f(x, y)` (dois argumentos). O JavaScript não faz essa distinção, e quando você possui uma função chamada `f`, pode chamá-la com 0 ou 1 ou 10 argumentos e você está chamando a mesma função.

A implicação disso é que você pode chamar qualquer função com qualquer número de argumentos. Se você não fornecer argumentos, eles receberão o valor indefinido implicitamente:

```js
function f(x) {
    return `in f: x=${x}`
}

f() // "in f: x=undefined"
```

#### Destructuring Arguments

Assim como podemos ter _destructured assignment_, podemos ter _destructured arguments_ (os argumentos são, afinal, muito parecido com a definição de variável). Considere destructuring um objeto em variáveis individuais:

```js
function getSentence({ subject, verb, object }) {
    return `${subject} ${verb} ${object}`
}

const o = {
    subject: 'I',
    verb: 'love',
    object: 'JavaScript'
}

getSentence(o) // "I love JavaScript"
```

Tal como acontece com a destructuring assignment, os nomes das propriedades devem ser cadeias de identificadores e uma variável que não possui uma propriedade correspondente no objeto recebido receberá o valor indefinido.

Você também pode criar matrizes de desestruturação:

```js
function getSentence([subject, verb, object]) {
    return `${subject} ${verb} ${object}`
}

const arr = ['I', 'love', 'JavaScript']
getSentence(arr) // "I love JavaScript"
```

Finalmente, você pode usar o spread operator(...) para juntar quaisquer argumentos adicionais:

```js
function addPrefix(prefix, ...words) {
    // we will learn a better way to do this later!
    const prefixedWords = []
    for (let i = 0; i < words.length; i++) {
        prefixedWords[i] = prefix + words[i]
    }
    return prefixedWords
}

addPrefix('con', 'verse', 'vex') // ["converse", "convex"]
```

Observe que se você usar o spread operator em uma declaração de função, ele deve ser o último argumento. Se você colocar argumentos após isso, o JavaScript não teria uma maneira razoável de distinguir entre o que deveria acontecer no argumento de propagação e o que deveria acontecer nos argumentos restantes.

### Function Expressions and Anonymous Functions

Até agora, lidamos exclusivamente com declarações de função, que dão funções tanto a um corpo (que define o que a função faz) quanto a um identificador (para que possamos chamá-lo mais tarde). O JavaScript também oferece suporte a funções anônimas, que não possuem necessariamente um identificador.

Você pode razoavelmente se perguntar o que é usar uma função sem um identificador. Sem um identificador, como devemos chamá-lo? A resposta reside na compreensão das expressões de função. Sabemos que uma expressão é algo que avalia um valor, e também sabemos que as funções são valores como qualquer outro em JavaScript. Uma expressão de função é simplesmente uma maneira de declarar uma função (possivelmente sem nome). Uma expressão de função pode ser atribuída a algo (dando assim um identificador), ou chamado imediatamente.

As expressões de função são sintaticamente idênticas às declarações de função, exceto que você pode omitir o nome da função. Consideremos o exemplo em que usamos uma expressão de função e atribuímos o resultado a uma variável (que é efetivamente equivalente a uma declaração de função):

```js
const f = function() {
    // ...
}
```

O resultado é o mesmo que se tivéssemos declarado a função da maneira usual: ficamos com um identificador `f` que se refere a uma função. Assim como com a declaração de função regular, podemos chamar a função com `f()`. A única diferença é que estamos criando uma função anônima (usando uma expressão de função) e atribuindo-a a uma variável.

As funções anônimas são usadas o tempo todo: como argumentos para outras funções ou métodos, ou para criar propriedades de função em um objeto.

Como a declaração de função e as expressões de função parecem idênticas, você pode estar se perguntando como o JavaScript sabe que os dois estão separados (ou se houver alguma diferença). A resposta é contexto: se a declaração de função é usada como expressão, é uma expressão de função, e se não for, é uma declaração de função.

A diferença é principalmente acadêmica, e você normalmente não precisa pensar sobre isso. Quando você está definindo uma função nomeada que você pretende chamar mais tarde, provavelmente usará uma declaração de função sem pensar nisso e, se você precisar criar uma função para atribuir algo ou passar para outra função, você usará uma expressão de função.

### call, apply, and bind

Já vimos o modo "normal" que `this` está vinculado (o que é consistente com outros idiomas orientados a objetos). No entanto, o JavaScript permite que você especifique o que `this` está vinculado, independentemente de como ou quando a função em questão é chamada. Começaremos com `call`, que é um método disponível em todas as funções que lhe permitem chamar a função com um valor específico `this`:

```js
const bruce = {
    name: 'Bruce'
}

const madeline = {
    name: 'Madeline'
}

// this function isn't associated with any object, yet
// it's using 'this'!
function greet() {
    return `Hello, I'm ${this.name}!`
}

greet() // "Hello, I'm !" - 'this' not bound
greet.call(bruce) // "Hello, I'm Bruce!" - 'this' bound to 'bruce'
greet.call(madeline) // "Hello, I'm Madeline!" - 'this' bound to 'madeline'
```

Você pode ver que `call` nos permite chamar uma função como se fosse um método, fornecendo-lhe um objeto para vincular `this`. O primeiro argumento de `call` é o valor que você deseja que `this` seja vinculado e os argumentos restantes tornam-se argumentos para a função que você está chamando:

```js
function update(birthYear, occupation) {
    this.birthYear = birthYear
    this.occupation = occupation
}

update.call(bruce, 1949, 'singer') // bruce is now { name: "Bruce", birthYear: 1949, occupation: "singer" }
update.call(madeline, 1942, 'actress') // madeline is now { name: "Madeline", birthYear: 1942, occupation: "actress" }
```

`apply` é idêntico à `call`, exceto a maneira como ele lida com argumentos de função. `call` leva argumentos diretamente, assim como uma função normal. `apply` leva seus argumentos como uma matriz:

```js
update.apply(bruce, [1955, 'actor']) // bruce is now { name: "Bruce", birthYear: 1955, occupation: "actor" }
update.apply(madeline, [1918, 'writer']) // madeline is now { name: "Madeline", birthYear: 1918, occupation: "writer" }
```

`apply` é útil se você tiver uma matriz e deseja usar seus valores como argumentos para uma função. O exemplo clássico é encontrar o número mínimo ou máximo em uma matriz. As funções incorporadas de `Math.min` e `Math.max` tomam qualquer número de argumentos e retornam o mínimo ou o máximo, respectivamente. Podemos usar `apply` para usar essas funções com uma matriz existente:

```js
const arr = [2, 3, -5, 15, 7]
Math.min.apply(null, arr) // -5
Math.max.apply(null, arr) // 15
```

Note que simplesmente passamos `null` para o valor de `this`. Isso porque `Math.min` e `Math.max` não usam `this` em absoluto; Não importa o que passamos aqui.

Com ES6 spread operator(...), podemos realizar o mesmo resultado como `apply`. Na instância do nosso método de `update`, onde nos preocupamos com `this` valor, ainda precisamos usar `call`, mas para `Math.min` e `Math.max`, onde não importa, podemos usar o spread operator para chamar essas funções diretamente:

```js
const newBruce = [1940, 'martial artist']
update.call(bruce, ...newBruce) // equivalent to apply(bruce, newBruce)
Math.min(...arr) // -5
Math.max(...arr) // 15
```

Existe uma função final que permite especificar o valor para `this`: `bind`. `bind` permite que você associe permanentemente um valor para `this` com uma função. Imagine que estamos passando nosso método de `update` ao redor, e queremos garantir que ele sempre seja chamado com `bruce` como o valor para `this`, não importa como ele é chamado (mesmo com `call`, `apply` ou outro bind). bind nos permite fazer isso:

```js
const updateBruce = update.bind(bruce)

updateBruce(1904, 'actor') // bruce is now { name: "Bruce", birthYear: 1904, occupation: "actor" }
updateBruce.call(madeline, 1274, 'king')
// bruce is now { name: "Bruce", birthYear: 1274, occupation: "king" };
// madeline is unchanged
```

O fato de que a ação de `bind` é permanente torna potencialmente uma fonte de erros difíceis de encontrar: em essência, você fica com uma função que não pode ser usada efetivamente com `call`, `apply` ou `bind` (uma segunda vez). Imagine passar em torno de uma função que é invocada com uma `call` ou `apply` em algum local distante, esperando que `this` seja vinculado de acordo. Não estou sugerindo que você evite o uso de `bind`; é bastante útil, mas tenha em atenção a maneira como as funções vinculadas serão usadas.

Você também pode fornecer parâmetros para `bind`, o que tem o efeito de criar uma nova função que sempre é invocada com parâmetros específicos. Por exemplo, se você quisesse uma função de `update` que sempre estabelecesse o ano de nascimento de Bruce para 1949, mas ainda permitiu que você alterasse a ocupação, você poderia fazer isso:

```js
const updateBruce1949 = update.bind(bruce, 1949)
updateBruce1949('singer, songwriter')
// bruce is now { name: "Bruce", birthYear: 1949, occupation: "singer, songwriter" }
```

## Scope

### Block Scope

`let` and `const` declara identificadores no que é conhecido como escopo de bloco. Um bloco é uma lista de declarações cercadas por chaves(curly braces). O escopo do bloco, então, refere-se a identificadores que são apenas no escopo dentro do bloco:

```js
console.log('before block')
{
    console.log('inside block')
    const x = 3
    console.log(x) // logs 3
}
console.log(`outside block; x=${x}`) // ReferenceError: x is not defined
```

Aqui temos um bloco autônomo: geralmente um bloco é parte de uma declaração de fluxo de controle, como `if` ou `for`, mas é uma sintaxe válida para ter um bloco por conta própria. Dentro do bloco, `x` é definido, e assim que deixamos o bloco, `x` sai do escopo e é considerado indefinido.

### Variable Masking

Uma fonte comum de confusão é variáveis ou constantes com o mesmo nome em diferentes escopos. É relativamente direto quando os escopos vêm um após o outro:

```js
{
    // outer block
    let x = 'blue'
    console.log(x) // logs "blue"
    {
        // inner block
        let x = 3
        console.log(x) // logs "3"
    }
    console.log(x) // logs "blue"
}

console.log(typeof x) // logs "undefined"; x out of scope
```

É fácil entender aqui que existem duas variáveis ​​distintas, ambas chamadas `x` em diferentes escopos. Agora, considere o que acontece quando os escopos estão aninhados:

```js
{
    // outer block
    let x = 'blue'
    console.log(x) // logs "blue"
    {
        // inner block
        let x = 3
        console.log(x) // logs "3"
    }
    console.log(x) // logs "blue"
}

console.log(typeof x) // logs "undefined"; x out of scope
```

Este exemplo demonstra o mascaramento variável. No bloco interno, `x` é uma variável distinta do bloco externo (com o mesmo nome), que efetivamente mascara (ou esconde) o `x` que está definido no escopo externo.

O que é importante entender aqui é que, quando a execução entra no bloco interno, e uma nova variável `x` é definida, ambas as variáveis estão no escopo; simplesmente não temos como acessar a variável no âmbito externo (porque tem o mesmo nome). Contraste isso com o exemplo anterior em que um `x` entrou no escopo e, em seguida, encerrou o escopo antes que a segunda variável chamada `x` fizesse o mesmo.

Para conduzir este ponto para casa, considere este exemplo:

```js
{
    // outer block
    let x = { color: 'blue' }
    let y = x // y and x refer to the same object
    let z = 3
    {
        // inner block
        let x = 5 // outer x now masked
        console.log(x) // logs 5
        console.log(y.color) // logs "blue"; object pointed to by
        // y (and x in the outer scope) is still in scope

        y.color = 'red'
        console.log(z) // logs 3; z has not been masked
    }
    console.log(x.color) // logs "red"; object modified in inner scope
    console.log(y.color) // logs "red"; x and y point to the same object
    console.log(z) // logs 3
}
```

O mascaramento variável às vezes é chamado de sombreamento variável (ou seja, uma variável com o mesmo nome irá obscurecer a variável no escopo externo). Nunca me importei com este termo porque as sombras geralmente não obscurecem completamente as coisas, apenas as deixam mais escuras. Quando uma variável é mascarada, a variável mascarada é completamente inacessível usando esse nome.

Até agora, deve ficar claro que o escopo é hierárquico: você pode inserir um novo escopo sem deixar o antigo. Isso estabelece uma cadeia de escopo que determina quais variáveis estão no escopo: todas as variáveis na cadeia de escopo atual estão no escopo e (desde que não sejam mascaradas) podem ser acessadas.

### Function Hoisting

Como variáveis declaradas com `var`, as declarações de função são elevadas para o topo do escopo, permitindo que você chame as funções antes de serem declaradas:

```js
f() // logs "f"
function f() {
    console.log('f')
}
```

Observe que as expressões de função que são atribuídas a variáveis não são içadas; Em vez disso, eles estão sujeitos às regras de escopo das variáveis. Por exemplo:

```js
f() // TypeError: f is not a function
let f = function() {
    console.log('f')
}
```

## Objetos

-   O tipo de dados fundamental do JavaScript é o objeto. Um objeto é um valor composto: agrega vários valores (valores primitivos ou outros objetos) e permite que você armazene e recupere esses valores pelo nome. Um objeto é uma coleção não ordenada de propriedades, cada uma com um nome e um valor.
-   Objetos são mutáveis e são manipulados por meio de referência e não por valor. Se a variável `x` se refere a um objeto, e o código `var y = x;` é executado, a variável `y` contém uma referência ao mesmo objeto, não uma cópia desse objeto. Todas as modificações feitas no objeto através da variável `y` também são visíveis através da variável `x`.
-   Lendo uma propriedade que não existe irá produzir o valor `undefined`.
-   É possível atribuir um valor a uma expressão de propriedade com o operador ‘=’. Isso irá substituir o valor da propriedade, se ele já existia, ou criar uma nova propriedade sobre o objeto se ele não o fez.
-   Objetos podem também ser usados como mapas, associando valores com seus nomes. O operador `in` pode ser usado para verificar se um objeto contém a propriedade com o nome informado.
-   Métodos são propriedades simples que comportam valores de funções.

### Property attributes

-   The _writable_ attribute specifies whether the value of the property can be set.
-   The _enumerable_ attribute specifies whether the property name is returned by a for/in loop.
-   The _configurable_ attribute specifies whether the property can be deleted and whether its attributes can be altered.

### New Object APIs

Let's take a look at the new APIs that let you play with property attributes, namely:

-   `Object.create()`
-   `Object.defineProperty()`
-   `Object.defineProperties()`
-   `Object.getOwnPropertyDescriptor()`

#### `Object.create()`

This method can be used to create a new object and at the same time:

-   Define the inheritance
-   Define the properties of the object
-   Define the attributes of the properties

Consider the following snippet:

```js
var human = {
    name: 'John'
}

var programmer = Object.create(human, {
    secret: stealth_descriptor,
    skill: {
        value: 'Code ninja'
    }
})
```

The programmer object inherits from the human object by way of `__proto__`, so it has (but does not own) a name property:

```js
programmer.name // "John"
programmer.hasOwnProperty('name') // false
programmer.__proto__.hasOwnProperty('name') // true
```

The other two properties are own properties:

```js
programmer.hasOwnProperty('secret') // true
programmer.hasOwnProperty('skill') // true
```

A full descriptor was used for the secret property: the stealth_descriptor which sets the value, as well as the three attributes.

The skill property only defines a value. This means that all the attributes are set to their default values, which are all false.

Earlier in this book, it was explained that “empty” objects (e.g., var o = {};) are not really empty, because they inherit methods such as toString() from Object.proto type. Well, in ES5, you can create truly empty objects using:

```js
var o = Object.create(null) // Inherit nothing
typeof o.toString // "undefined"
```

#### `Object.getOwnPropertyDescriptor()`

The getOwnPropertyDescriptor() method lets you examine the descriptor objects.

#### `Object.defineProperty()` and `Object.defineProperties()`

The two methods `Object.defineProperty()` and `Object.defineProperties()` allow you to define properties with a descriptor at a later time, after the object was created.

#### Restricting Object Mutations

In ES3, all objects are mutable, with a few exceptions found in the built-in objects. Sometimes this is not a good idea because users of your objects can change them beyond repair. In ES5, you can restrict access to your objects. For example, you can make selected properties read-only by setting their writable attribute to false.

But you can restrict access to the whole object by:

-   Preventing extensions (i.e., don't allow new properties)
-   "Sealing" an object, which means making all properties nonconfigurable (cannot delete them) on top of making the object nonextensible
-   "Freezing" an object, which is like sealing but with the additional step of setting all properties to nonwritable

Consider this regular object:

```js
var pizza = {
    tomatoes: true,
    cheese: true
}
```

You can always add more properties:

```js
pizza.pepperoni = 'lots'
```

You can change and delete properties, because they are all configurable and writable:

```js
pizza.cheese = 'mozzarella'
delete pizza.pepperoni // true
```

However, if you prevent extensions, you'll no longer be able to add properties. You can check for extensibility with `Object.isExtensible()` and disallow extensions (irreversibly) with `Object.preventExtensions()`:

```js
Object.isExtensible(pizza) // true
Object.preventExtensions(pizza) // Returns the `pizza` object
Object.isExtensible(pizza) // false

pizza.broccoli = 'eww' // Error, cannot add properties anymore
typeof pizza.broccoli // "undefined"
```

Additionally, you can prevent deletions with `seal()`:

```js
Object.isSealed(pizza) // false
Object.seal(pizza) // Returns the `pizza` object
Object.isSealed(pizza) // true

delete pizza.cheese // Error, can't delete
pizza.cheese // "mozzarella"
```

You can still change a property value though:

```js
pizza.cheese = 'ricotta'
```

But once you `freeze()` an object, that's it, all properties become nonwritable:

```js
Object.isFrozen(pizza) // false
Object.freeze(pizza) // Returns the `pizza` object
Object.isFrozen(pizza) // true

pizza.cheese = 'gorgonzola' // Error, `cheese` is read-only
pizza.cheese // "ricotta"
```

So, in review:

-   `freeze()` does what `seal()` does plus sets the writable attribute to false for all properties.
-   `seal()` does what `preventExtensions()` does plus sets the configurable attribute to false for all properties.
-   `preventExtensions()` doesn't set any attributes, but it doesn't let you add more properties afterward.
-   None of these actions can be turned back on (i.e., there's no `unfreeze()`, de `frost()`, `allowExtensions()`, etc.).

#### `Object.getPrototypeOf()`

Wrapping up the novelties in Object() in ES5, let's take a look at the getPrototy peOf() method. In ES3, you can only inspect with `isPrototypeOf()`, in which case you need to suspect an object and ask if that is indeed the prototype.

In ES5, you can directly ask, “Who is your prototype?”

```js
Object.getPrototypeOf(pizza_v20) === pizza // true
```

As you can see, this is the same as using `__proto__`, but remember that `__proto__` is not standard (and doesn't exist in IE). ES5 admits there's a need for at least a readable version of `__proto__` and introduces getPrototypeOf() as a replacement (a writable version is still a topic for heated debate).

### Criando Objetos

Objetos podem ser criados:

-   Por meio de uma função construtora `new()`.
-   Com literais de objetos (`{}`) - Cria e inicializa o novo objeto.
-   Com `Object.create()`.

### Prototype

Cada objeto JavaScript tem um segundo objeto JavaScript associado a ele. Este segundo objeto é conhecido como um protótipo e o primeiro objeto herda as propriedades do protótipo.

Um prototype é outro objeto que é usado como fonte de fallback para as propriedades. Quando um objeto recebe uma chamada em uma propriedade que ele não possui, seu prototype designado para aquela propriedade será buscado, e então o prototype daquele prototype e assim por diante.

Então quem é o prototype de um objeto vazio? É o ancestral de todos os prototypes, a entidade por trás de quase todos os objetos, `Object.prototype`.

```js
console.log(Object.getPrototypeOf({}) == Object.prototype) // true
console.log(Object.getPrototypeOf(Object.prototype)) // null
console.log(Object.getPrototypeOf(isNaN) == Function.prototype) // true
console.log(Object.getPrototypeOf([]) == Array.prototype) // true
```

_A função `Object.getPrototypeOf` retorna o prototype de um objeto como o esperado._

_Funções derivam do `Function.prototype`, e arrays derivam do `Array.prototype`_.

Por diversas vezes, o prototype de um objeto também terá um prototype, dessa forma ele ainda fornecerá indiretamente métodos como `toString`.

A função `Object.getPrototypeOf` obviamente retornarão o prototype de um objeto. Você pode usar `Object.create` para criar um objeto com um prototype específico.

As relações dos objetos JavaScript formam uma estrutura em forma de árvore, e na raiz dessa estrutura se encontra o `Object.prototype`. Ele fornece alguns métodos que estão presentes em todos os objetos, como o `toString`, que converte um objeto para uma representação em string.

Muitos objetos não possuem o `Object.prototype` diretamente em seu prototype. Ao invés disso eles têm outro objeto que fornece suas propriedades padrão. Funções derivam do `Function.prototype`, e arrays derivam do `Array.prototype`.

A maneira mais conveniente de criar objetos que herdam algum prototype compartilhado é usar um construtor. No JavaScript, chamar uma função precedida pela palavra-chave `new` vai fazer com que ela seja tratada como um construtor. O construtor terá sua variável `this` atrelada a um novo objeto, e a menos que ele explicite o retorno do valor de outro objeto, esse novo objeto será retornado a partir da chamada.

Um objeto criado com `new` é chamado de instância do construtor.

Construtores (todas as funções, na verdade) pegam automaticamente uma propriedade chamada prototype, que por padrão possui um objeto vazio que deriva do `Object.prototype`. Toda instância criada com esse construtor terá esse objeto assim como seu prototype.

Todos os objetos criados por literais de objeto têm o mesmo objeto protótipo, e podemos referir este objeto protótipo no código JavaScript como `Object.prototype`. Os objetos criados usando a palavra-chave `new` e uma invocação do construtor usam o valor da propriedade do protótipo da função do construtor como seu protótipo. Portanto, o objeto criado pelo `new Object()` herda do `Object.prototype` assim como o objeto criado por `{}` faz. Da mesma forma, o objeto criado pela `new Array()` usa `Array.prototype` como seu protótipo e o objeto criado pela `new Date()` usa `Date.prototype` como seu protótipo.

Por exemplo, `Date.prototype` herda propriedades de `Object.prototype`, então um objeto `Date` criado por `new Date()` herda propriedades de `Date.prototype` e `Object.prototype`. Esta ligação de objetos protótipos é conhecida como uma cadeia de protótipo (_prototype chain_).

O atributo protótipo de um objeto especifica o objeto do qual ele herda as propriedades. Este é um atributo tão importante que geralmente simplesmente diremos "o protótipo de `o`" em vez de "o protótipo do atributo de `o`". Além disso, é importante entender que, quando o protótipo aparece na fonte de código, ele se refere a uma propriedade de objeto comum e não ao atributo protótipo.

---

O atributo protótipo é definido quando um objeto é criado. Lembre-se objetos criados a partir de literais de objeto usam `Object.prototype` como seu protótipo.

Os objetos criados com `new` usam o protótipo do valor da propriedade do protótipo de sua função de construtor.

E os objetos criados com `Object.create()` usam o primeiro argumento para essa função (que pode ser nula) como seu protótipo.

Você pode consultar o protótipo de qualquer objeto passando esse objeto para `Object.getPrototypeOf()`. Os objetos criados com `new` geralmente herdam uma propriedade do construtor que se refere à função do construtor usada para criar o objeto. E, como descrito acima, as funções do construtor têm uma propriedade protótipo que especifica o protótipo para os objetos criados usando esse construtor.

Para determinar se um objeto é o protótipo de (ou faz parte da cadeia de protótipo de) outro objeto, use o método `isPrototypeOf()`. Para descobrir se `p` é o protótipo de `o` escrever `p.isPrototypeOf(o)`. Por exemplo:


```js
var p = { x: 1 } // Define a prototype object.
var o = Object.create(p) // Create an object with that prototype.
p.isPrototypeOf(o) // => true: o inherits from p
bject.prototype.isPrototypeOf(o) // => true: p inherits from Object.prototype
```

#### Methods in Object Literals

Can you add methods to an object using object literal notation? Absolutely—methods are just properties, which happen to point to function objects:

```js
var obj = {
    name: 'Fluffy',
    legs: 4,
    tail: 1,
    getDescription: function() {
        return obj.name + ' has ' + obj.legs + ' legs and ' + obj.tail + ' tail(s)'
    }
}

obj.getDescription() // "Fluffy has 4 legs and 1 tail(s)"
```

_In `getDescription()`, you can substitute obj with `this`._

You can add methods to an existing object at a later time:

```js
obj.getAllProps = function() {
    var all = []
    for (var property in obj) {
        if (typeof obj[property] !== 'function') {
            all.push(property + ': ' + obj[property])
        }
    }
    return all.join(', ')
}

obj.getAllProps()
```

#### Own Properties

Own properties are those added to an object using the object literal notation or via an assignment:

```js
var literal = {
    a: 'b'
}

var assigned = {}
assigned.a = 'b'
```

Own properties are also those added to this and returned by constructor functions:

```js
function Builder(what) {
    this.prop = what
}

let constructed = new Builder('abcd')
```

Notice, however, that these two objects have access to a `toString()` method that neither of them defined:

```js
literal.toString() // "[object Object]"
constructed.toString() // "[object Object]"
```

The method `toString()` is not an own method for either of the two objects. It's something that came from a prototype.

If you want to tell the difference between own properties and prototype properties, you can use another method called `hasOwnProperty()`, which takes a name of a property/method as a string:

```js
literal.hasOwnProperty('mine') // true
constructed.hasOwnProperty('mine') // true
literal.hasOwnProperty('toString') // false
constructed.hasOwnProperty('toString') // false

literal.hasOwnProperty('hasOwnProperty') // false
```

Let's do some more introspection. Where did that toString() come from? How can you find out which is the prototype?

#### `__proto__`

Objects have prototypes, but they don't have a prototype property—only functions do. However, many environments offer a special `__proto__` property for each object. `__proto__` is not available everywhere, so it's useful only for debugging and learning.

`__proto__` is a property that exposes the secret link between the object and the `prototype` property of the constructor that created the object:

```js
constructed.prototype // undefined, objects don't have this property
constructed.constructor === Builder // true, "who's your constructor?"

// Secret link - exposed!
constructed.constructor.prototype === constructed.__proto__ // true
```

Chaining `__proto__` calls lets you get to the bottom of things. And the bottom is the built-in Object() constructor function.

Object.prototype is the mother of all objects. All objects inherit from it. That's where `toString()` was defined:

```js
Object.prototype.hasOwnProperty('toString') // true
```

You can trace down toString from the `constructed` object:

```js
constructed.__proto__.__proto__.hasOwnProperty('toString') // true
```

What about the literal object? Its chain is one link shorter:

```js
literal.__proto__.hasOwnProperty('toString') // true
```

This is because literal wasn't created by a custom constructor, which means it was created by `Object()` behind the scenes, not by something (e.g., `Builder()`) that inherits from `Object()`.

When you need a simple object temporarily, you can use ({}), and the following syntax also works:

```js
;({}.__proto__.hasOwnProperty('toString')) // true
```

#### this or prototype

When using constructor functions, you can add properties to this or to the constructor's prototype. You may be wondering which one you should use.

Adding to the prototype is more efficient and takes less memory because the properties and functions are created only once and reused by all objects created with the same constructor. Anything you add to this will be created every time you instantiate a new object.

Therefore, any members you plan to reuse and share among instances should be added to the prototype, and any properties that have different values in each instance should be own properties added to this. Most commonly, methods go to the prototype and properties to this, unless they are constant among the instances.

### `Object.create()`

> Cria um novo objeto, usando seu primeiro argumento como o protótipo desse objeto.
> Retorna um novo objeto que herda as propriedades do objeto de argumento.

Você pode passar `null` para criar um novo objeto que não tenha um protótipo, mas se você fizer isso, o objeto recém-criado não herdará nada, nem mesmo métodos básicos, como `toString()`.

### Enumerating Properties

Em vez de testar a existência de propriedades individuais, às vezes queremos iterar ou obter uma lista de todas as propriedades de um objeto. Isso geralmente é feito com o loop `for/in`(Ele executa o corpo do loop uma vez para cada propriedade enumerable (própria ou herdada) do objeto especificado, atribuindo o nome da propriedade à variável de loop). Os métodos incorporados que os objetos herdam não são enumeráveis, mas as propriedades que seu código adiciona aos objetos são enumeráveis (a menos que você use uma das funções descritas mais tarde para torná-las nonenumerable).

```js
var o = {
    // Three enumerable own properties
    x: 1,
    y: 2,
    z: 3
}

o.propertyIsEnumerable('toString') // => false: not enumerable

for (p in o) // Loop through the properties
    console.log(p) // Prints x, y, and z, but not toString
```

Além do `for/in` loop, o ECMAScript 5 define duas funções que enumeram nomes de propriedade. O primeiro é `Object.keys()`, que retorna uma matriz dos nomes das propriedades próprias enumeráveis ​​de um objeto. A segunda função de enumeração de propriedade ECMAScript 5 é `Object.getOwnPropertyNames()`. Funciona como `Object.keys()`, mas retorna os nomes de todas as propriedades próprias do objeto especificado, não apenas as propriedades enumeráveis.

### Property Getters and Setters

As propriedades definidas por _getters_ e _setters_ são às vezes conhecidas como _accessor properties_(propriedades de acesso) para distingui-las de data properties(propriedades de dados) que possuem um valor simples.

Quando um programa consulta o valor de uma propriedade de acesso, o JavaScript invoca o método getter (não passando argumentos). O valor de retorno desse método torna-se o valor da expressão de acesso à propriedade. Quando um programa define o valor de uma accessor properties, o JavaScript invoca o método setter, passando o valor do lado direito da tarefa. Este método é responsável por "configuração", em algum sentido, o valor da propriedade. O valor de retorno do método setter é ignorado.

As accessor properties não possuem um atributo gravável como propriedades de dados. Se uma propriedade possui um getter e um método setter, é uma propriedade de leitura/gravação. Se tiver apenas um método getter, é uma propriedade somente leitura. E se ele tem apenas um método setter, é uma propriedade somente de gravação (algo que não é possível com propriedades de dados) e tenta lê-lo sempre avaliar para indefinido.

Definindo via literal:

```js
var o = {
    // An ordinary data property
    data_prop: value,

    // An accessor property defined as a pair of functions
    get accessor_prop() {
        /* function body here */
    },
    set accessor_prop(value) {
        /* function body here */
    }
}
```

As accessor properties são definidas como uma ou duas funções, cujo nome é o mesmo que o nome da propriedade e com a palavra-chave da função substituída por get e/ou set.

```js
var p = {
    // x and y are regular read-write data properties.
    x: 1.0,
    y: 1.0,
    // r is a read-write accessor property with getter and setter.
    // Don't forget to put a comma after accessor methods.
    get r() {
        return Math.sqrt(this.x * this.x + this.y * this.y)
    },
    set r(newvalue) {
        var oldvalue = Math.sqrt(this.x * this.x + this.y * this.y)
        var ratio = newvalue / oldvalue
        this.x *= ratio
        this.y *= ratio
    },
    // theta is a read-only accessor property with getter only.
    get theta() {
        return Math.atan2(this.y, this.x)
    }
}
```

```js
var random = {
    get octet() {
        return Math.floor(Math.random() * 256)
    },
    get uint16() {
        return Math.floor(Math.random() * 65536)
    },
    get int16() {
        return Math.floor(Math.random() * 65536) - 32768
    }
}
```

### Property Attributes

Além de um nome e valor, as propriedades possuem atributos que especificam se eles podem ser gravados, enumerados e configurados. Esta API é particularmente importante para os autores da biblioteca porque:

-   Permite que eles adicionem métodos para protótipos de objetos e os tornem enumeráveis, como métodos internos.
-   Permite-lhes "bloquear" os seus objectos, definindo propriedades que não podem ser alteradas ou eliminadas.

Os quatro atributos de uma propriedade de dados são value, `writable`, `enumerable` e `configurable`.

As accessor properties não possuem um atributo de valor ou um atributo gravável: sua capacidade de gravação é determinada pela presença ou ausência de um setter. Portanto, os quatro atributos de uma propriedade de acesso são `get`, `set`, `enumerable` e `configurable`.

Para obter o descritor de propriedade para uma propriedade nomeada de um objeto especificado, chame `Object.getOwnPropertyDescriptor()`:

```js
// Returns {value: 1, writable:true, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor({ x: 1 }, 'x')

// Now query the octet property of the random object defined above.
// Returns { get: /*func*/, set:undefined, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor(random, 'octet')

// Returns undefined for inherited properties and properties that don't exist.
Object.getOwnPropertyDescriptor({}, 'x') // undefined, no such prop
Object.getOwnPropertyDescriptor({}, 'toString') // undefined, inherited
```

`Object.getOwnPropertyDescriptor()` funciona apenas para propriedades próprias. Para consultar os atributos das propriedades herdadas, você deve percorrer explicitamente a cadeia do protótipo (`Object.getPrototypeOf()`).

Para definir os atributos de uma propriedade ou para criar uma nova propriedade com os atributos especificados, chame `Object.defineProperty()`, passando o objeto a ser modificado, o nome da propriedade a ser criada ou alterada e o descritor de propriedade objeto:

```js
var o = {} // Start with no properties at all
// Add a nonenumerable data property x with value 1.
Object.defineProperty(o, 'x', { value: 1, writable: true, enumerable: false, configurable: true })

// Check that the property is there but is nonenumerable
o.x // => 1
Object.keys(o) // => []

// Now modify the property x so that it is read-only
Object.defineProperty(o, 'x', { writable: false })

// Try to change the value of the property
o.x = 2 // Fails silently or throws TypeError in strict mode
o.x // => 1

// The property is still configurable, so we can change its value like this:
Object.defineProperty(o, 'x', { value: 2 })
o.x // => 2

// Now change x from a data property to an accessor property
Object.defineProperty(o, 'x', {
    get: function() {
        return 0
    }
})
o.x // => 0
```

Defining Accessors via Property Descriptors:

```js
var obj = Object.create(Object.prototype, {
    // object with property descriptors
    foo: {
        // property descriptor
        get: function() {
            return 'getter'
        },
        set: function(value) {
            console.log('setter: ' + value)
        }
    }
})
```

The properties are copied from orig to copy via this function:

```js
function copyOwnPropertiesFrom(target, source) {
    Object.getOwnPropertyNames(source) // (1)
        .forEach(function(propKey) {
            // (2)
            var desc = Object.getOwnPropertyDescriptor(source, propKey) // (3)
            Object.defineProperty(target, propKey, desc) // (4)
        })

    return target
}
```

```js
function copyObject(orig) {
    // 1. copy has same prototype as orig
    var copy = Object.create(Object.getPrototypeOf(orig))
    // 2. copy has all of orig’s properties
    copyOwnPropertiesFrom(copy, orig)
    return copy
}
```

O descritor de propriedade que você passa para `Object.defineProperty()` não precisa incluir todos os quatro atributos. Se você estiver criando uma nova propriedade, os atributos omitidos serão considerados falsos ou indefinidos. Se você estiver modificando uma propriedade existente, os atributos que você omite simplesmente permaneceram inalterados. Observe que este método altera uma propriedade própria existente ou cria uma nova propriedade própria, mas não irá alterar uma propriedade herdada.

Se você quer criar ou modificar mais de uma propriedade por vez, use `Object.defineProperties()`. O primeiro argumento é o objeto a ser modificado. O segundo argumento é um objeto que mapeia os nomes das propriedades a serem criadas ou modificadas para os descritores de propriedade para essas propriedades.

```js
var p = Object.defineProperties(
    {},
    {
        x: { value: 1, writable: true, enumerable: true, configurable: true },
        y: { value: 1, writable: true, enumerable: true, configurable: true },
        r: {
            get: function() {
                return Math.sqrt(this.x * this.x + this.y * this.y)
            },
            enumerable: true,
            configurable: true
        }
    }
)
```

Este código começa com um objeto vazio e, em seguida, acrescenta duas propriedades de dados e uma propriedade de acesso somente leitura.

`Object.defineProperty()` e `Object.defineProperties()` lançar `TypeError` se a tentativa de criar ou modificar uma propriedade não for permitida. Isso acontece se você tentar adicionar uma nova propriedade a um objeto não extensível. Os outros motivos que esses métodos podem lançar `TypeError` têm que ver com os próprios atributos. O atributo `writable` determina as tentativas de alterar o atributo de `value`. E o atributo `configurable` determina as tentativas de alterar os outros atributos (e também especifica se uma propriedade pode ser excluída). As regras não são completamente diretas, no entanto. É possível alterar o valor de uma propriedade nonwritable se essa propriedade for configurável, por exemplo. Além disso, é possível alterar uma propriedade de `writable` para nonwritable, mesmo que essa propriedade é nonconfigurable. Aqui estão as regras completas. Chamadas para `Object.defineProperty()` ou `Object.defineProperties()` que tentam violá-los lançar `TypeError`:

-   Se um objeto não for extensível, você pode editar suas próprias propriedades existentes, mas você não pode adicionar novas propriedades a ela.
-   Se uma propriedade não for configurável, você não pode alterar seus atributos configuráveis ou enumeráveis.
-   Se uma accessor properties não for configurável, não pode alterar seu método getter ou setter e não pode alterá-lo para uma propriedade de dados.
-   Se uma propriedade de dados não for configurável, não poderá alterá-la para uma propriedade de acesso.
-   Se uma propriedade de dados não for configurável, você não pode alterar seu atributo gravável de falso para verdadeiro, mas você pode alterá-lo de verdadeiro para falso.
-   Se uma propriedade de dados não for configurável e não gravável, você não pode alterar seu valor. Você pode alterar o valor de uma propriedade que é configurável, mas não descritivo, no entanto (porque isso seria o mesmo que torná-lo gravável, depois alterando o valor e, em seguida, convertendo-o de novo para não escrito).

Em um objeto literal, a notação `get` ou `set` para propriedades permite que você especifique uma função a ser executada quando a propriedade for lida ou escrita. Você pode também adicionar tal propriedade em um objeto existente, por exemplo um protótipo, usando a função `Object.defineProperty`.

```js
Object.defineProperty(TextCell.prototype, 'heightProp', {
    get: function() {
        return this.text.length
    }
})

var cell = new TextCell('no\nway')
console.log(cell.heightProp) // → 2

cell.heightProp = 100
console.log(cell.heightProp) // → 2
```

### Constructor Invocation

Se uma função ou invocação de método é precedida pela palavra-chave `new`, então é uma invocação de construtor. As invocações do construtor diferem das invocações regulares de função e método no seu tratamento de argumentos, contexto de invocação e valor de retorno.

Uma invocação do construtor cria um objeto novo e vazio que herda da propriedade do protótipo do construtor. As funções do construtor destinam-se a inicializar objetos e este objeto recém-criado é usado como o contexto de invocação, de modo que a função do construtor pode se referir a ele com `this` palavra-chave. Observe que o novo objeto é usado como contexto de invocação, mesmo que a invocação do construtor se pareça com uma invocação do método. Ou seja, na expressão `new o.m()`, `o` não é usado como o contexto de invocação.

As funções do construtor normalmente não usam a palavra-chave de `return`. Eles tipicamente inicializam o novo objeto e retornam implicitamente quando alcançam o fim do corpo. Nesse caso, o novo objeto é o valor da expressão de invocação do construtor. Se, no entanto, um construtor usado explicitamente o retorno de declaração para retornar um objeto, em seguida, o objeto torna-se o valor da expressão invocação. Se o construtor usa o retorno sem valor, ou se ele retorna um valor primitivo, esse valor de retorno é ignorado e o novo objeto é usado como o valor da invocação.

### `call()` / `apply()`

`call()` e `apply()` permitem invocar indiretamente uma função como se fosse um método de algum outro objeto. O primeiro argumento tanto para `call()` e `apply()` é o objeto sobre o qual a função deve ser invocada; Este argumento é o contexto de invocação e torna-se o valor `this` palavra-chave dentro do corpo da função. No modo estrito do ECMAScript 5, o primeiro argumento para `call()` ou `apply()` torna-se o valor `this`, mesmo que seja um valor primitivo ou nulo ou indefinido.

Qualquer argumento para `call()` após o primeiro argumento de contexto de invocação são os valores que são passados ​​para a função que é invocada.

O método `apply()` é como o método `call()`, exceto que os argumentos a serem passados ​​para a função são especificados como uma matriz.

Se uma função é definida para aceitar um número arbitrário de argumentos, o método `apply()` permite que você invoque essa função no conteúdo de uma matriz de comprimento arbitrário.

### `bind()`

O método `bind()` foi adicionado no ECMAScript 5. Como o próprio nome indica, o objetivo principal de `bind()` é vincular uma função ou um contexto a um objeto. Todos os argumentos que você passa para a nova função são passados ​​para a função original.

O método ECMAScript 5 `bind()` faz mais do que apenas ligar uma função a um objeto. Ele também executa uma aplicação parcial: todos os argumentos que você passa para `bind()` após o primeiro são vinculados juntamente com o valor de `this`.

### A propriedade do construtor

Um construtor é uma função projetada para a inicialização de objetos recém-criados. Os construtores são invocados usando a palavra-chave `new`. As invocações de construtor usando `new` automaticamente cria o novo objeto, portanto, o próprio construtor precisa inicializar o estado desse novo objeto. A característica crítica das invocações do construtor é que o a propriedade protótipo do construtor é usada como o protótipo do novo objeto. Isso significa que todos os objetos criados com o mesmo construtor herdam do mesmo objeto e, portanto, são membros da mesma classe.

Dois objetos são instâncias da mesma classe se e somente se herdam do mesmo objeto protótipo. A função de construtor que inicializa o estado de um novo objeto não é fundamental: duas funções de construtor podem ter propriedades de protótipo que apontam para o mesmo protótipo de objeto. Então, ambos os construtores podem ser usados para criar instâncias da mesma classe.

Mesmo através dos construtores não são tão fundamentais quanto os protótipos, o construtor serve como rosto público de uma classe. Mais obviamente, o nome da função de construtor geralmente é adotado como o nome da classe. Dizemos, por exemplo, que o construtor `Range()` cria objetos `Range`. Mais fundamentalmente, no entanto, os construtores são usados com o operador `instanceof` quando testar objetos para associação em uma classe. Se tivermos um objeto `r` e queremos saber se é um objeto `Range`, podemos escrever:

```js
r instanceof Range // returns true if r inherits from Range.prototype
```

O operador `instanceof` não verifica se o `r` foi inicializado pelo construtor `Range`. Verifica se herda do `Range.prototype`.

No exemplo acima definimos `Range.prototype` para um novo objeto que continha os métodos para a nossa classe. Embora fosse conveniente expressar esses métodos como propriedades de um único objeto literal, não era realmente necessário criar um novo objeto. Qualquer função JavaScript pode ser usada como um construtor, e as invocações do construtor precisam de uma propriedade protótipo. Portanto, cada função JavaScript (exceto as funções retornadas pelo método ECMAScript 5 `Function.bind()`) possui automaticamente um propriedade de protótipo. O valor desta propriedade é um objeto que possui uma única propriedade de construtor nonenumerable. O valor da propriedade do construtor é o objeto de função:

```js
var F = function() {} // This is a function object.
var p = F.prototype // This is the prototype object associated with it.
var c = p.constructor // This is the function associated with the prototype.
c === F // => true: F.prototype.constructor == F for any function
```

A existência desse protótipo predefinido com sua propriedade do construtor significa que os objetos normalmente herdam uma propriedade do construtor que se refere ao seu construtor.

```js
var o = new F() // Create an object o of class F
o.constructor === F // => true: the constructor property specifies the class
```

A constructor is a function that is invoked via the `new` operator. By convention, the names of constructors start with uppercase letters, while the names of normal functions and methods start with lowercase letters.

```js
function Person(name) {
    this.name = name
}

Person.prototype.describe = function() {
    return 'Person named ' + this.name
}

function Constr() {}
var x = new Constr()
var y = new x.constructor()
console.log(y instanceof Constr) // true
```

Example: Constructor Inheritance in Use

```js
function Person(name) {
    this.name = name
}

Person.prototype.describe = function() {
    return 'Person called ' + this.name
}

function Employee(name, title) {
    Person.call(this, name)
    this.title = title
}

Employee.prototype = Object.create(Person.prototype)
Employee.prototype.constructor = Employee

Employee.prototype.describe = function() {
    return Person.prototype.describe.call(this) + ' (' + this.title + ')'
}

var allyson = new Employee('Allyson', 'CTO')
allyson.describe() // Person called Allyson (CTO)
allyson instanceof Employee // true
allyson instanceof Person // true
```

**Objeto Construtor**

Como observamos, a função de construtor (um objeto) define um nome para uma classe de JavaScript. As propriedades que você adiciona a este objeto construtor servem como campos de classe e métodos de classe (dependendo se os valores das propriedades são funções ou não).

**Protótipo de objeto**

As propriedades deste objeto são herdadas por todas as instâncias da classe e propriedades cujos valores são funções se comportam como métodos de instância da classe.

**Objeto de instância**

Cada instância de uma classe é um objeto por direito próprio, e as propriedades definidas diretamente em uma instância não são compartilhadas por nenhuma outra instância. As propriedades não funcionais definidas nas instâncias se comportam como os campos de instância da classe.

### Prevenção de Extensões de Objetos

O ECMAScript 5 permite que você evite isso, se desejar. `Object.preventExtensions()` torna um objeto inexistente, o que significa que nenhuma nova propriedade pode ser adicionada a ele. `Object.seal()` leva um passo adiante: impede a adição de novas propriedades e também torna todas as propriedades atuais não configuráveis, de modo que elas não podem ser excluídas(Uma propriedade não configurável ainda pode ser gravável, no entanto, e ainda pode ser convertida em uma propriedade somente leitura.). Outra maneira é com `Object.freeze()`, que faz tudo o que `Object.seal()` faz, mas também torna todas as propriedades somente leitura e não configuráveis.

Existe uma característica das propriedades de somente leitura que é importante entender ao trabalhar com as classes. Se um objeto `o` herda uma propriedade de somente leitura `p`, uma tentativa de atribuir a `o.p` falhará e não criará uma nova propriedade em `o`. Se você deseja substituir uma propriedade herdada de somente leitura, você deve usar `Object.defineProperty()` ou `Object.defineProperties()` ou `Object.create()` para criar a nova propriedade. Isso significa que, se você fizer os métodos de instância de uma classe somente leitura, torna-se significativamente mais difícil para subclasses substituir esses métodos.


## Arrays

### Array Methods

#### `indexOf() and lastIndexOf()`

The two new methods `Array.prototype.indexOf()` and `Array.prototype.lastIndexOf()` offer more ways to search in an array.

`Array.prototype.indexOf()` returns the index of the first occurrence of an element:

```js
var a = ['one', 'and', 'two', 'and', 'three', 'and', 'four']
a.indexOf('three') // 4
```

This is close to PHP's `array_search()`.

While `Array.prototype.indexOf()` returns the first occurrence, `Array.prototype.lastIndexOf()` returns the last:

```js
var a = ['one', 'and', 'two', 'and', 'three', 'and', 'four']
a.indexOf('and') // 1
a.lastIndexOf('and') // 5
```

The search for elements uses strict comparison to find matches:

```js
[1, 2, 100, '100'].indexOf('100') // 3
[1, 2, 100, '100'].indexOf(100) // 2
```

You can also specify start position in `indexOf()` or end position in `lastIndexOf()` to begin the search:

```js
var arr = [100, 1, 2, 100]

arr.indexOf(100) // 0
arr.indexOf(100, 2) // 3

arr.lastIndexOf(100) // 3
arr.lastIndexOf(100, 2) // 0
```

#### Walking the Array Elements

ES5 adds `Array.prototype.forEach()`, which allows you to loop over all array elements without a loop construct. It takes a callback function in which you can do an operation with each element or with the whole array.

```js
['a', 'b', 'c'].forEach(function() {
    console.log(arguments)
})
```

#### Filtering

ES5 adds `Array.prototype.filter()`, which, just like PHP's `array_filter()`, lets you execute a callback function on every array element. If the callback returns true, the elements gets pushed to a new array, which is returned at the end:

```js
function testVowels(char) {
    return /[aeiou]/i.test(char)
}

var input = ['a', 'b', 'c', 'd', 'e']
var output = input.filter(testVowels)

output.join(', ') // "a, e"
```

#### Testing the Array Content

Two new methods, `every()` and `some()`, let you iterate over all array elements and return a boolean, whether or not the array elements satisfy a check you provide in a callback.

Say you want to check if an array contains even numbers. The check will be:

```js
function isEven(num) {
    return num % 2 === 0
}
```

And the tests:

```js
// Are *all* of the numbers even?
;[1, 2, 4].every(isEven) // false

// Are at least some (or even one) of the numbers even?
;[1, 2, 4].some(isEven) // true
```

#### Map/Reduce

Similar to PHP’s `array_map()` and `array_reduce()`, in ES5 you have `Array.prototype.map()` and `Array.prototype.reduce()`, plus one more `Array.prototype.reduceRight()`.

`map()` returns a new array where each element has been changed by your custom callback function.

Here’s a function that turns a character into its HTML entity form:

```js
function entity(char) {
    return '&#' + char.charCodeAt(0) + ';'
}
```

Now let’s apply it to all elements of an array:

```js
var input = ['a', 'b', 'c']
var out = input.map(entity)
out // ["&#97;", "&#98;", "&#99;"]
```

`Array.prototype.reduce()` takes an array and reduces it to a single value, using a callback function.

Say you want to sum all numbers in an array and add 100 to the result. So you start with 100 and iteratively add the next value to the running sum:

```js
function sum(running_sum, value, index, array) {
    console.log(arguments)
    return running_sum + value
}

[1, 2, 3].reduce(sum, 100) // 106
```

During the iteration, you’ll see in the console:

```js
[100, 1, 0, Array[3]][(101, 2, 1, Array[3])][(103, 3, 2, Array[3])]
```

`Array.prototype.reduceRight()` is the same, only it loops the array elements from right to left, meaning it starts with the last elements and moves down.

In this example, the result is the same, but you see different output in the console:

```js
[1, 2, 3].reduceRight(sum, 100) // 106
```

And the output is:

```js
[100, 3, 2, Array[3]][(103, 2, 1, Array[3])][(105, 1, 0, Array[3])]
```

#### `join()/implode`

O método `Array.join()` converte todos os elementos de uma matriz em strings e as concatena, retornando a seqüência resultante. Você pode especificar uma seqüência opcional que separa os elementos na seqüência resultante. Se nenhuma string separadora for especificada, uma vírgula é usada.

```js
var a = [1, 2, 3] // Create a new array with these three elements
a.join() // => "1,2,3"
a.join(' ') // => "1 2 3"
a.join('') // => "123" var b = new Array(10); // An array of length 10 with no elements
b.join('-') // => '---------': a string of 9 hyphens
```

O método `Array.join()` é o inverso do método `String.split()/explode`, que cria uma matriz, quebrando uma string em pedaços.

#### `reverse()`

O método `Array.reverse()` invoca a ordem dos elementos de uma matriz e retorna a matriz invertida. Ele faz isso no lugar; em outras palavras, não cria uma nova matriz com os elementos rearranjados, mas reorganiza-os na matriz já existente.

```js
var a = [1, 2, 3]
a.reverse().join() // => "3,2,1" and a is now [3,2,1]
```

#### `slice()`

O método `Array.slice()` retorna uma fatia ou subarray da matriz especificada. Seus dois argumentos especificam o início e o final da fatia a serem retornados. A matriz retornada contém o elemento especificado pelo primeiro argumento e todos os elementos subseqüentes até, mas não incluindo, o elemento especificado pelo segundo argumento. Se apenas um argumento for especificado, a matriz retornada contém todos os elementos da posição inicial para o final da matriz. Se qualquer argumento é negativo, ele especifica um elemento de matriz relativo ao último elemento na matriz. Um argumento de -1, por exemplo, especifica o último elemento na matriz, e um argumento de -3 especifica o terceiro do último elemento da matriz. Observe que `slice()` não modifica a matriz em que é invocada.

```js
var a = [1, 2, 3, 4, 5]
a.slice(0, 3) // Returns [1,2,3]
a.slice(3) // Returns [4,5]
a.slice(1, -1) // Returns [2,3,4]
a.slice(-3, -2) // Returns [3]
```

#### `splice()`

O método `Array.splice()` é um método de propósito geral para inserir ou remover elementos de uma matriz. Ao contrário da `slice()` e `concat()`, `splice()` modifica a matriz na qual é invocada. Observe que `splice()` e `slice()` possuem nomes muito semelhantes, mas realizam operações substancialmente diferentes.

`splice()` pode excluir elementos de uma matriz, inserir novos elementos em uma matriz ou executar ambas as operações ao mesmo tempo. Elementos da matriz que vêm após o ponto de inserção ou eliminação têm seus índices aumentados ou diminuídos conforme necessário para que permaneçam contínuos com o resto da matriz. O primeiro argumento para `splice()` especifica a posição da matriz na qual a inserção e/ou eliminação devem começar. O segundo argumento especifica o número de elementos que devem ser excluídos de (spliced out of) da matriz. Se esse segundo argumento for omitido, todos os elementos da matriz do elemento inicial ao final da matriz serão removidos. `splice()` retorna uma matriz dos elementos excluídos ou uma matriz vazia se nenhum elemento foi excluído.

```js
var a = [1, 2, 3, 4, 5, 6, 7, 8]
a.splice(4) // Returns [5,6,7,8]; a is [1,2,3,4]
a.splice(1, 2) // Returns [2,3]; a is [1,4]
a.splice(1, 1) // Returns [4]; a is [1]
```

Os dois primeiros argumentos para `splice()` especificam quais elementos de matriz devem ser excluídos. Esses argumentos podem ser seguidos por qualquer número de argumentos adicionais que especifiquem elementos a serem inseridos na matriz, começando na posição especificada pelo primeiro argumento.

```js
var a = [1, 2, 3, 4, 5]
a.splice(2, 0, 'a', 'b') // Returns []; a is [1,2,'a','b',3,4,5]
a.splice(2, 2, [1, 2], 3) // Returns ['a','b']; a is [1,2,[1,2],3,3,4,5]
```

#### `every()` and `some()`

Os métodos `every()` e `some()` são predicados de matriz: eles aplicam uma função de predicado que você especifica para os elementos da matriz e, em seguida, retorna `true` ou `false`.

O método `every()` é como o quantificador matemático "para todos" ∀: ele retorna verdadeiro se e somente se sua função de predicado retornar verdadeiro para todos os elementos na matriz:

```js
a = [1, 2, 3, 4, 5]
a.every(function(x) {
    return x < 10
}) // => true: all values < 10.
a.every(function(x) {
    return x % 2 === 0
}) // => false: not all values even.
```

O método `some()` é como o quantificador matemático "existe": ele retorna verdadeiro se existir pelo menos um elemento na matriz para o qual o predicado retorna verdadeiro e retorna false se e somente se o predicado retornar falso para todos os elementos da matriz:

```js
a = [1, 2, 3, 4, 5]
a.some(function(x) {
    return x % 2 === 0
}) // => true a has some even numbers.
a.some(isNaN) // => false: a has no non-numbers.
```

Observe que `every()` e `some()` param os elementos da matriz de iteração assim que eles sabem qual valor retornar. `some()` retorna verdadeiro a primeira vez que seu predicado retorna verdadeiro e apenas itera através de toda a matriz se o seu predicado sempre retornar falso. `every()` é o oposto: ele retorna falso na primeira vez que o seu predicado retorna falso e apenas itera todos os elementos se seu predicado sempre retornar verdadeiro. Observe também que, por convenção matemática, `every()` retorna verdadeiro e alguns retornam falsos quando invocados em uma matriz vazia.

## Promises

Para criar um objeto promise, nós chamamos um construtor `Promise`, passando uma função que inicia a ação assíncrona. O construtor chama esta função, passando dois argumentos, os quais são tambem funções. A primeira deve ser chamada quando a ação terminar com sucesso, e a segunda quando ela falhar.

```js
function get(url) {
    return new Promise(function(succeed, fail) {
        var req = new XMLHttpRequest()
        req.open('GET', url, true)
        req.addEventListener('load', function() {
            if (req.status < 400) succeed(req.responseText)
            else fail(new Error('Request failed: ' + req.statusText))
        })
        req.addEventListener('error', function() {
            fail(new Error('Network error'))
        })
        req.send(null)
    })
}
```

Note que a interface da função em si é agora bem mais simples. Você passa uma URL, e ela retorna uma promise. Essa promise age como um manipulador. Ela agora tem um método `then` que pode ser chamado com duas funções: uma para o caso de sucesso e outra para o caso de falha.

```js
get('example/data.txt').then(
    function(text) {
        console.log('data.txt: ' + text)
    },
    function(error) {
        console.log('Failed to fetch data.txt: ' + error)
    }
)
```

_A chamada ao método `then` produz uma nova promise, a qual o resultado (o valor passado para o manipulador em caso de sucesso) depende do valor de retorno da primeira função que passamos para o `then`. Essa função pode retornar outra promise para indicar que mais tarefas assincronas estão sendo executadas. Neste caso, a própria promise retornado pelo `then` irá esperar pela promise retornada pela função manipuladora, obtendo sucesso ou falhando com o mesmo valor quando for resolvida. Quando a função manipuladora retornar um valor que não seja uma promise, a promise retornada pelo `then` imediatamente retorna com sucesso com tal valor como seu resultado._

Muito parecido com os eventos, você pode vincular como muitas reações como você gostaria, usando os `.then` e `.catch` métodos.

```js
const p = fetch('/items')
p.then(res => {
    // handle response
})
p.catch(err => {
    // handle error
})
```

As reações passadas para `.then` podem ser usadas para lidar com o cumprimento de uma promessa, que é acompanhada por um valor de realização; e as reações passadas para `.catch` são executadas com um motivo de rejeição que pode ser usado ao manusear rejeições. Você também pode registrar uma reação às rejeições no segundo argumento passado para `.then`. O código anterior também pode ser expresso como o seguinte.

```js
const p = fetch('/items')
p.then(
    res => {
        // handle response
    },
    err => {
        // handle error
    }
)
```

Outra alternativa é omitir a reação de realização em `.then(resolve, reject)`, sendo semelhante à omissão de uma reação de rejeição ao chamar `.then`. Usando `.then(null, reject)` é equivalente a
`.catch(reject)`.

```js
const p = fetch('/items')
p.then(res => {
    // handle response
})
p.then(null, err => {
    // handle error
})
```

Quando se trata de promessas, o encadeamento é uma fonte importante de confusão. Em uma API baseada em eventos, o encadeamento é possibilitado ao ter o método `.on` anexar o ouvinte de eventos e depois retornar o próprio emissor de eventos. As promessas são diferentes. Os métodos `.then` e `.catch` devolvem uma nova promessa sempre. Isso é importante porque o encadeamento pode ter resultados extremamente diferentes, dependendo de onde você anexa um `.then` ou uma chamada `.catch`.

Uma promessa é criada passando o construtor `Promise` um resolvedor que decide como e quando a promessa é resolvida, chamando um método de `resolve` que irá resolver a promessa em execução ou um método de `reject` que estabelecar a promessa como uma rejeição. Até que a promessa seja estabelecida ligando para qualquer uma das funções, estará em um estado pendente e quaisquer reações associadas a ela não serão executadas. O trecho de código a seguir cria uma promessa a partir do zero onde esperamos um segundo antes de estabelecer aleatoriamente a promessa com um resultado de resolvido,decidido ou rejeição.

```js
new Promise(function(resolve, reject) {
    setTimeout(function() {
        if (Math.random() > 0.5) {
            resolve('random success')
        } else {
            reject(new Error('random failure'))
        }
    }, 1000)
})
```

Promessas também podem ser criadas usando `Promise.resolve` e `Promise.reject`. Esses métodos criam promessas que imediatamente se estabelecerão com um valor de realização e um motivo de rejeição, respectivamente.

```js
Promise.resolve({ result: 123 }).then(data => console.log(data.result)) // <- 123
```

Quando uma promessa `p` é cumprida, as reações registradas com `p.then` são executadas. Quando uma promessa `p` é rejeitada, as reações registradas com `p.catch` são executadas. Essas reações podem, por sua vez, resultar em três situações diferentes, dependendo se eles retornam um valor, uma Promessa, um thenable, ou lançar um erro. `Thenables` são objetos considerados promissores - como que podem ser lançados em Promise usando `Promise.resolve`.

Uma reação pode retornar um valor, o que faria com que a promessa retornada por `.then` se tornasse cumprida com esse valor. Nesse sentido, as promessas podem ser encadeadas para transformar o valor de cumprimento da promessa anterior repetidamente, conforme mostrado no seguinte trecho de código.

```js
Promise.resolve(2)
    .then(x => x * 7)
    .then(x => x - 3)
    .then(x => console.log(x)) // <- 11
```

Uma reação pode retornar uma promessa. Em contraste com o pedaço de código anterior, a promessa retornado pela primeira chamada `.then` no seguinte trecho será bloqueado até que aquele retornado por sua reação é cumprida.

```js
Promise.resolve(2)
    .then(
        x =>
            new Promise(function(resolve) {
                setTimeout(() => resolve(x * 1000), x * 1000)
            })
    )
    .then(x => console.log(x)) // <- 2000
```

Uma reação também pode lançar um erro, o que faria com que a promessa retornada por `.then` para se tornar rejeitada e, portanto, siga o ramo `.catch`, usando o referido erro como motivo de rejeição. O exemplo a seguir mostra como anexamos uma reação de satisfação à operação de busca. Uma vez que a busca seja cumprida, a reação provocará um erro e provocará a reação de rejeição anexada à promessa retornada por `.then` a ser executada.

```js
const p = fetch('/items')
    .then(res => {
        throw new Error('unexpectedly')
    })
    .catch(err => console.error(err))
```

#### Promise Continuation and Chaining

Quando ocorre um erro em um resolvedor de promessas, você pode pegar esse erro usando `p.catch` como mostrado a seguir.

```js
new Promise((resolve, reject) => reject(new Error('oops'))).catch(err => console.error(err))
```

Uma promessa se resolverá como uma rejeição quando o resolvedor chamar `reject`, mas também se uma exceção for lançada dentro do resolvedor também, conforme demonstrado pelo próximo trecho.

```js
new Promise((resolve, reject) => {
    throw new Error('oops')
}).catch(err => console.error(err))
```

Os erros que ocorrem durante a execução de uma reação de realização ou rejeição se comportam da mesma maneira: eles resultam em uma promessa sendo rejeitada, a retornada pela chamada `.then` ou `.catch` que passou a reação onde o erro se originou.

```js
Promise.resolve(2)
    .then(x => {
        throw new Error('failed')
    })
    .catch(err => console.error(err))
```

Pode ser mais fácil decompor essa série de chamadas de método em cadeia em variáveis, como mostrado a seguir. O seguinte código pode ajudá-lo a visualizar o fato de que, se você anexou a reação `.catch` a `p1`, você não conseguirá capturar o erro originado na reação `.then`. Enquanto `p1` está cumprido, `p2` - uma promessa diferente de `p1`, resultante da chamada `p1.then` - é rejeitada devido ao erro a ser lançado. Esse erro pode ser capturado, em vez disso, se anexamos a reação de rejeição a `p2`.

```js
const p1 = Promise.resolve(2)
const p2 = p1.then(x => {
    throw new Error('failed')
})
const p3 = p2.catch(err => console.error(err))
```

Nós estabelecemos que a promessa que você atribui suas reações é importante, pois determina quais erros podem capturar e quais erros ele não pode. Também vale a pena notar que, enquanto um erro continuar sem ser captado em uma cadeia de promessas, um manipulador de rejeição poderá capturá-lo.

```js
const p1 = Promise.resolve(2)
const p2 = p1.then(x => {
    throw new Error('failed')
})
const p3 = p2.then(x => x * 2)
const p4 = p3.catch(err => console.error(err))
```

Normalmente, as promessas como `p4` cumprem porque o manipulador de rejeição em `.catch` não levanta erros. Isso significa que um manipulador de cumprimento anexado com `p4.then` seria executado depois. O exemplo a seguir mostra como você pode imprimir uma declaração no console do navegador, criando um manipulador de cumprimento `p4` que depende da `p3` para se estabelecer com sucesso com a realização.

```js
const p1 = Promise.resolve(2)
const p2 = p1.then(x => {
    throw new Error('failed')
})
const p3 = p2.catch(err => console.error(err))
const p4 = p3.then(() => console.log('crisis averted'))
```

Da mesma forma, se um erro ocorreu no manipulador de rejeição `p3`, poderíamos capturar aquele também usando `.catch`.

```js
const p1 = Promise.resolve(2)

const p2 = p1.then(x => {
    throw new Error('failed')
})

const p3 = p2.catch(err => {
    throw new Error('oops')
})

const p4 = p3.catch(err => console.error(err))
```

O exemplo a seguir imprime `err.message` uma vez em vez de duas vezes. Isso ocorre porque não ocorreram erros no primeiro `.catch`, então o ramo de rejeição para essa promessa não foi executado.

```js
fetch('/items')
    .then(res => res.a.prop.that.does.not.exist)
    .catch(err => console.error(err.message))
    .catch(err => console.error(err.message)) // <- 'Cannot read property "prop" of undefined'
```

```js
const p = fetch('/items').then(res => res.a.prop.that.does.not.exist)
p.catch(err => console.error(err.message))
p.catch(err => console.error(err.message))
// <- 'Cannot read property "prop" of undefined'
// <- 'Cannot read property "prop" of undefined'
```

Devemos observar, então, que as promessas podem ser encadeadas arbitrariamente. Como acabamos de ver, você pode salvar uma referência para qualquer ponto da cadeia de promessas e, em seguida, acrescentar mais promessas em cima dela. Este é um dos pontos fundamentais para a compreensão das promessas.

Vamos usar o seguinte trecho como uma suporte para enumerar a seqüência de eventos decorrentes da criação e encadeamento de algumas promessas. Tome um momento para inspecionar o seguinte código.

```js
const p1 = fetch('/items')
const p2 = p1.then(res => res.a.prop.that.does.not.exist)
const p3 = p2.catch(err => {})
const p4 = p3.catch(err => console.error(err.message))
```

Aqui está uma enumeração do que está acontecendo na medida em que esse código é executado:

1.  fetch retorna uma nova promessa `p1`.
2.  `p1.then` retorna uma nova promessa de `p2`, que reagirá se `p1` for cumprida.
3.  `p2.catch` retorna uma nova promessa `p3`, que reagirá se `p2` for rejeitada.
4.  `p3.catch` retorna uma nova promessa `p4`, que reagirá se `p3` for rejeitado.
5.  Quando `p1` é cumprida, a reação `p1.then` é executada.
6.  Depois, `p2` é rejeitado devido a um erro na reação `p1.then`.
7.  Uma vez que `p2` foi rejeitado, `p2.catch` reações são executadas, e o ramo `p2.then` é ignorado.
8.  A promessa `p3` de `p2.catch` é cumprida, porque não produz um erro ou resulta em uma promessa rejeitada.
9.  Como o `p3` foi cumprido, o `p3.catch` nunca é seguido. O `p3.then` ramo teria sido usado em vez disso.

Tudo começa com uma única promessa, que depois aprenderemos a construir. Então você adiciona ramos com `.then` ou `.catch`. Você pode abordar tantas chamadas `.then` ou `.catch` como você deseja em cada ramo, criando novos ramos e assim por diante.

#### Promise States and Fates

As promessas podem estar em três estados distintos: pendentes, cumpridos e rejeitados. Pendente é o estado padrão. Uma promessa pode então se transformar em realização ou rejeição. Uma promessa pode ser resolvida ou rejeitada exatamente uma vez. Tentando resolver ou rejeitar uma promessa por segunda vez não terá qualquer efeito.

#### Leveraging Promise.all and Promise.race

```js
Promise.all([fetch('/products/chair'), fetch('/products/table')]).then(products =>
    console.log(products[0], products[1])
)
```

Com Array destructuring.

```js
Promise.all([fetch('/products/chair'), fetch('/products/table')]).then(([chair, table]) => console.log(chair, table))
```

```js
const p1 = Promise.reject('failed')
const p2 = fetch('/products/chair')
const p3 = fetch('/products/table')

const p = Promise.all([p1, p2, p3]).catch(err => console.log(err)) // <- 'failed'
```

```js
Promise.race([
    new Promise(resolve => setTimeout(() => resolve(1), 1000)),
    new Promise(resolve => setTimeout(() => resolve(2), 2000))
]).then(result => console.log(result)) // <- 1
```

```js
function timeout(delay) {
    return new Promise(function(resolve, reject) {
        setTimeout(() => reject('timeout'), delay)
    })
}

Promise.race([fetch('/large-resource-download'), timeout(5000)])
    .then(res => console.log(res))
    .catch(err => console.log(err))
```

## Using async/await

Funções assíncronas nos permitem realizar uma implementação baseada em promessas e tirar proveito do estilo gerador síncrono. Observe que aguardar só pode ser usado dentro de funções assíncronas, marcadas com a palavra - chave assíncrona. As funções assíncronas funcionam de forma semelhante aos geradores, suspendendo a execução no contexto local até uma promessa se estabelecer. Se a expressão aguardada não é originalmente uma promessa, ela é transformada em uma promessa.

```js
async function read() {
    try {
        const model = await getRandomArticle()
        const html = await renderView(model)
        await setPageContents(html)
        console.log('Successfully changed page!')
    } catch (err) {
        console.error(err)
    }
}
read()
```

Uma função assíncrona sempre retorna uma promessa. No caso de exceções não capturadas, a promessa retornada se estabelece em rejeição. Caso contrário, a promessa retornada resolve o valor de retorno. Este aspecto das funções assíncronas também nos permite misturá-las com uma continuação regular baseada em promessas.

```js
async function read() {
    const model = await getRandomArticle()
    const html = await renderView(model)
    await setPageContents(html)
    return 'Successfully changed page!'
}

read()
    .then(message => console.log(message))
    .catch(err => console.error(err))
```

Fazendo a função de leitura um pouco mais reutilizável, poderemos retornar o html resultante e permitir que os consumidores façam a continuação usando promessas ou ainda outra função assíncrona.

```js
async function read() {
    const model = await getRandomArticle()
    const html = await renderView(model)
    return html
}
```

Seguindo o exemplo, podemos usar promessas simples para imprimir o HTML.

```js
read().then(html => console.log(html))
```

O uso de funções assíncronas também não seria tão difícil para a continuação.

```js
async function write() {
    const html = await read()
    console.log(html)
}
```

#### Fluxos assíncronos simultâneos

Nos fluxos de códigos assíncronos, é comum executar duas ou mais tarefas simultaneamente. Enquanto as funções assíncronas tornam mais fácil a escrita de código assíncrono, eles também se prestam ao código que executa uma operação assíncrona de cada vez. Uma função com múltiplas expressões `await` nele será suspensa uma a cada vez em cada expressão `await` até que a Promessa seja resolvida, antes de implantar a execução e passar para a próxima expressão de await - este é um caso semelhante ao que observamos com geradores e rendimento.

```js
async function concurrent() {
    const p1 = new Promise(resolve => setTimeout(resolve, 500, 'fast'))
    const p2 = new Promise(resolve => setTimeout(resolve, 200, 'faster'))
    const p3 = new Promise(resolve => setTimeout(resolve, 100, 'fastest'))

    const r1 = await p1 // execution is blocked until p1 settles
    const r2 = await p2
    const r3 = await p3
}
```

```js
async function exercise() {
    const r1 = await new Promise(resolve => setTimeout(resolve, 500, 'slowest'))
    const r2 = await new Promise(resolve => setTimeout(resolve, 200, 'slow'))

    return [r1, r2]
}

exercise().then(result => console.log(result)) // <- ['slowest', 'slow']
```

Podemos usar `Promise.all` para resolver essa questão, criando uma única promessa que podemos aguardar. Desta forma, o nosso código bloqueia até que todas as promessas em uma lista sejam resolvidas, e elas podem ser resolvidas simultaneamente.

O exemplo a seguir mostra como você poderia aguardar três promessas diferentes que poderiam ser resolvidas simultaneamente.

```js
async function concurrent() {
    const p1 = new Promise(resolve => setTimeout(resolve, 500, 'fast'))
    const p2 = new Promise(resolve => setTimeout(resolve, 200, 'faster'))
    const p3 = new Promise(resolve => setTimeout(resolve, 100, 'fastest'))

    const [r1, r2, r3] = await Promise.all([p1, p2, p3])

    console.log(r1, r2, r3) // 'fast', 'faster', 'fastest'
}
```

Poderíamos usar Promise.race para obter o resultado da promessa que cumpre
mais rápido.

```js
async function race() {
    const p1 = new Promise(resolve => setTimeout(resolve, 500, 'fast'))
    const p2 = new Promise(resolve => setTimeout(resolve, 200, 'faster'))
    const p3 = new Promise(resolve => setTimeout(resolve, 100, 'fastest'))

    const result = await Promise.race([p1, p2, p3]) console.log(result) // 'fastest'
}
```

## ES6 Essentials

### Functions as Properties of Objects

Quando uma função é uma propriedade de um objeto, muitas vezes é chamado de método para distingui-lo de uma função normal. Podemos adicionar um método no objeto literal:

```js
const o = {
    name: 'Allyson', // primitive property
    bark: function() {
        // function property (method)
        return 'Woof!'
    }
}
```

O ES6 introduz uma nova sintaxe abreviada para os métodos. O seguinte é funcionalmente equivalente ao exemplo anterior:

```js
const o = {
    name: 'Allyson', // primitive property
    bark() {
        // function property (method)
        return 'Woof!'
    }
}
```

### Objeto literais

ES6 traz algumas melhorias para a sintaxe literal do objeto: property value shorthands, computed property names e method definitions.

#### Property Value Shorthands

Às vezes, declaramos objetos com uma ou mais propriedades cujos valores são referências a variáveis ​​com o mesmo nome. O snippet a seguir possui um exemplo típico em que temos uma declaração literal do objeto com algumas dessas propriedades repetitivas:

```js
var listeners = []
function listen() {}
var events = {
    listeners: listeners,
    listen: listen
}
```

Sempre que você se encontra nessa situação, você pode omitir o valor da propriedade e os dois pontos aproveitando a nova sintaxe da abreviação do valor da propriedade no ES6.

```js
var listeners = []
function listen() {}
var events = { listeners, listen }
```

#### Computed Property Names

Às vezes, você deve declarar objetos que contêm propriedades com nomes com base em variáveis ​​ou outras expressões de JavaScript.

```js
var expertise = 'journalism'
var person = {
    name: 'Sharon',
    age: 27
}
person[expertise] = {
    years: 5,
    interests: ['international', 'politics', 'internet']
}
```

Os literais de objetos no ES6 não são limitados a declarações com nomes estáticos. Com nomes de propriedade calculados, você pode envolver qualquer expressão entre colchetes e usar isso como o nome da propriedade. Quando a declaração é recuperada, sua expressão é avaliada e usada como o nome da propriedade. O exemplo a seguir mostra como o pedaço de código que acabamos de ver pode declarar o objeto de pessoa em uma única etapa, sem ter que recorrer a uma segunda declaração, adicionando a experiência da pessoa.

```js
var expertise = 'journalism'
var person = {
    name: 'Sharon',
    age: 27,
    [expertise]: {
        years: 5,
        interests: ['international', 'politics', 'internet']
    }
}
```

Você não pode combinar os shorthands do valor da propriedade com nomes de propriedade calculados. As abreviações de valor são açúcares sintáticos de compilação simples que ajudam a evitar a repetição, enquanto os nomes das propriedades calculadas são avaliados em tempo de execução. Dado que estamos tentando misturar esses dois recursos incompatíveis, o exemplo a seguir lançará um erro de sintaxe.

```js
var expertise = 'journalism'
var journalism = {
    years: 5,
    interests: ['international', 'politics', 'internet']
}
var person = {
    name: 'Sharon',
    age: 27,
    [expertise] // this is a syntax error!
}
```

#### Definições do Método

Normalmente, você pode declarar métodos em um objeto adicionando propriedades a ele. No próximo trecho, estamos criando um pequeno emissor de eventos que oferece suporte a vários tipos de eventos.

```js
var emitter = {
    events: {},
    on: function(type, fn) {
        if (this.events[type] === undefined) {
            this.events[type] = []
        }
        this.events[type].push(fn)
    },
    emit: function(type, event) {
        if (this.events[type] === undefined) {
            return
        }
        this.events[type].forEach(function(fn) {
            fn(event)
        })
    }
}
```

Iniciando no ES6, você pode declarar métodos em um objeto literal usando a nova sintaxe de definição de método. Nesse caso, podemos omitir os dois pontos e a palavra-chave `function`. Isto é entendido como uma alternativa concisa às declarações de métodos tradicionais onde você precisa usar a palavra-chave `function`.

```js
var emitter = {
    events: {},
    on(type, fn) {
        if (this.events[type] === undefined) {
            this.events[type] = []
        }
        this.events[type].push(fn)
    },
    emit(type, event) {
        if (this.events[type] === undefined) {
            return
        }
        this.events[type].forEach(function(fn) {
            fn(event)
        })
    }
}
```

### Arrow Functions

Arrow functions permitem simplificar a sintaxe de três maneiras:

-   Você pode omitir a palavra `function`.
-   Se a função usar um único argumento, você pode omitir os parênteses.
-   Se o corpo da função for uma única expressão, você pode omitir chaves e a declaração de retorno.

_Arrow functions são sempre anônimas. Você ainda pode atribuí-los a uma variável, mas você não pode criar uma função nomeada como você pode com a palavra-chave `function`._

Considere as seguintes expressões de função equivalentes:

```js
const f1 = function() {
    return 'hello!'
}
// OR
const f1 = () => 'hello!'

const f2 = function(name) {
    return `Hello, ${name}!`
}
// OR
const f2 = name => `Hello, ${name}!`

const f3 = function(a, b) {
    return a + b
}
// OR
const f3 = (a, b) => a + b
```

Arrow functions são mais úteis quando você está criando e passando por funções anônimas.

Arrow functions têm uma grande diferença de funções regulares: `this` é vinculado lexicamente, como qualquer outra variável.

```js
const o = {
    name: 'Julie',
    greetBackwards: function() {
        const getReverseName = () => {
            let nameBackwards = ''
            for (let i = this.name.length - 1; i >= 0; i--) {
                nameBackwards += this.name[i]
            }
            return nameBackwards
        }
        return `${getReverseName()} si eman ym ,olleH`
    }
}

o.greetBackwards()
```

Enquanto as arrow functions parecem muito semelhantes à sua função anônima típica, elas são fundamentalmente diferentes: as arrow functions não podem ser chamadas explicitamente, embora os tempos de execução modernos possam inferir um nome com base na variável a que são atribuídos; eles não podem ser usados ​​como construtores nem possuem um protótipo de propriedade, o que significa que você não pode usar o novo em uma função de seta; e eles estão vinculados ao seu alcance lexical, razão pela qual eles não alteram o significado disso.

Enquanto as arrow functions parecem muito semelhantes à sua função anônima típica, elas são fundamentalmente diferentes: as arrow functions não podem ser chamadas explicitamente, embora os tempos de execução modernos possam inferir um nome com base na variável a que são atribuídos; eles não podem ser usados como construtores nem possuem uma propriedade protótipo, o que significa que você não pode usar `new` em uma função de seta; e eles estão vinculados ao seu alcance lexical, razão pela qual eles não alteram o significado `this`.

De forma semelhante, a ligação lexical nas arrow functions ES6 também significa que as chamadas de função não poderão mudar `this` contexto ao usar `.call`, `.apply`, `.bind`, etc. Essa limitação geralmente é mais útil do que não, pois ele garante que o contexto sempre seja preservado e constante.

Uma função de seta com exatamente um parâmetro pode omitir os parênteses. Isso é opcional.

Você pode combinar parênteses implícitos e retorno implícito.

Quando você precisa retornar implicitamente um objeto literal, você precisará envolver essa expressão literal do objeto entre parênteses. Caso contrário, o compilador interpretaria suas chaves como o início e o fim do bloco de função.

```js
var objectFactory = () => ({ modular: 'es6' })
```

Emparelhar a expressão entre parênteses corrige esses problemas, porque o compilador não o interpreta como um bloco de função. Em vez disso, a declaração do objeto torna-se uma expressão que avalia o objeto literal que queremos retornar de forma implícita:

```js
[1, 2, 3].map(value => ({ number: value, verified: true }))

/* <- [
    { number: 1, verified: true },
    { number: 2, verified: true },
    { number: 3, verified: true }]
*/
```

As arrow functions são legais quando se trata de definir funções anônimas que provavelmente deveriam ser ligadas de forma léxica de qualquer maneira, e podem certamente tornar seu código mais conciso em algumas situações.

### Assignment Destructuring

Ele vincula as propriedades a tantas variáveis ​​quanto você precisa. Ele funciona com objetos, arrays e até mesmo em listas de parâmetros de função.

#### Destructuring Objects

```js
var character = {
    name: 'Bruce',
    pseudonym: 'Batman',
    metadata: {
        age: 34,
        gender: 'male'
    },
    batarang: ['gas pellet', 'bat-mobile control', 'bat-cuffs']
}
```

```js
var pseudonym = character.pseudonym
```

```js
// a normal object
const obj = { b: 2, c: 3, d: 4 }
const { a, b, c } = obj
a // undefined: there was no property "a" in obj
b // 2
c // 3
d // reference error: "d" is not defined
```

```js
// a normal array
const arr = [1, 2, 3]

// array destructuring assignment
let [x, y] = arr
x // 1
y // 2
z // error: z hasn't been defined
```

```js
const arr = [1, 2, 3, 4, 5]
let [x, y, ...rest] = arr
x // 1
y // 2
rest // [3, 4, 5]
```

Com a desestruturação na atribuição, a sintaxe torna-se um pouco mais clara. Como você pode ver no próximo exemplo, você não precisa escrever pseudônimo duas vezes, enquanto ainda está claramente transmitindo a intenção.

```js
var { pseudonym } = character
```

Assim como você pode declarar várias variáveis separadas por vírgulas com uma única instrução `var`, você também pode declarar variáveis ​​múltiplas dentro das chaves de uma expressão de desestruturação:

```js
var { pseudonym, name } = character
```

De forma semelhante, você pode misturar e combinar a desestruturação com declarações de variáveis ​​regulares na mesma instrução `var`.

```js
var { pseudonym } = character,
    two = 2
```

Se você deseja extrair uma propriedade chamada `pseudonym`, mas gostaria de declarar como uma variável chamada `alias`, você pode usar a seguinte sintaxe de desestruturação, conhecida como `aliasing`. Observe que você pode usar `alias` ou qualquer outro nome de variável válido.

```js
var { pseudonym: alias } = character
console.log(alias) // <- 'Batman'
```

Eles começam a entender quando você considera que a desestruturação suporta estruturas profundas.

```js
var {
    metadata: { gender }
} = character
```

Em casos como o anterior, onde você possui propriedades profundamente aninhadas desestruturadas, você poderá transmitir um nome de propriedade mais claramente se você escolher um alias.

```js
var {
    metadata: { gender: characterGender }
} = character
```

Com a desestruturação, prevalece o mesmo comportamento. Ao declarar uma variável desestruturada para uma propriedade que está faltando, você também voltará `undefined`.

```js
var { boots } = character
console.log(boots) // <- undefined
```

Uma declaração desestruturada acessando uma propriedade aninhada de um objeto pai que seja nula ou indefinida lançará uma Exceção, assim como tentativas regulares para acessar propriedades de nulo ou indefinido, em outros casos.

```js
var {
    boots: { size }
} = character // <- Exception
var { missing } = null // <- Exception
```

Quando você pensa nesse pedaço de código como o código ES5 equivalente mostrado a seguir, torna-se evidente por que a expressão deve ser lançada, uma vez que a desestruturação é principalmente açúcar sintático.

```js
var nothing = null
var missing = nothing.missing // <- Exception
```

Como parte da desestruturação, você pode fornecer valores padrão para os casos em que o valor é indefinido. O valor padrão pode ser qualquer coisa que você possa pensar: números, strings, funções, objetos, uma referência para outra variável, etc.

```js
var { boots = { size: 10 } } = character
console.log(boots) // <- { size: 10 }
```

Os valores padrão também podem ser fornecidos na desestruturação da propriedade aninhada.

```js
var {
    metadata: { enemy = 'Satan' }
} = character
console.log(enemy) // <- 'Satan'
```

Para usar em combinação com alias, você deve colocar o alias primeiro e, em seguida, o valor padrão, como mostrado a seguir.

```js
var { boots: footwear = { size: 10 } } = character
```

É possível usar a sintaxe de nomes de propriedade calculada em padrões de desestruturação. Nesse caso, no entanto, você deve fornecer um alias para ser usado como o nome da variável. Isso porque os nomes de propriedades calculadas permitem expressões arbitrárias e, portanto, o compilador não seria capaz de inferir um nome de variável.

```js
var { ['boo' + 'ts']: characterBoots } = character
console.log(characterBoots) // <- true
```

#### Destructuring Arrays

A sintaxe para arrays de desestruturação é semelhante à dos objetos. Com desestruturação você pode transmitir seu significado de forma clara e sem referenciar explicitamente os índices, nomeando os valores em vez disso.

```js
var coordinates = [12, -7]
var [x, y] = coordinates
console.log(x) // <- 12
```

Quando as matrizes de desestruturação, você pode pular propriedades desinteressantes ou aqueles que você de outra forma não precisa fazer referência.

```js
var names = ['James', 'L.', 'Howlett']
var [firstName, , lastName] = names
console.log(lastName) // <- 'Howlett'
```

A desestruturação de matriz permite valores padrão, como a desestruturação de objetos.

```js
var names = ['James', 'L.']
var [firstName = 'John', , lastName = 'Doe'] = names
console.log(lastName) // <- 'Doe'
```

No ES5, quando você precisa trocar os valores de duas variáveis, normalmente você recorre a uma terceira variável temporária, como no seguinte trecho.

```js
var left = 5
var right = 7
var aux = left
left = right
right = aux
```

A desestruturação ajuda você a evitar a declaração auxiliar e se concentrar em sua intenção. Mais uma vez, a desestruturação nos ajuda a transmitir a intenção de forma mais rápida e eficaz para o caso de uso.

```js
var left = 5
var right = ((7)[(left, right)] = [right, left])
```

A última área de desestruturação que vamos abranger é os parâmetros da função.

#### Function Parameter Defaults

Os parâmetros de função no ES6 também são capazes de especificar valores padrão.

```js
function powerOf(base, exponent = 2) {
    return Math.pow(base, exponent)
}
```

Os padrões também podem ser aplicados aos parâmetros da arrow functions. Quando temos valores padrão em uma arrow functions, devemos enrolar a lista de parâmetros entre parênteses, mesmo quando houver um único parâmetro.

```js
var double = (input = 0) => input * 2
```

Os valores padrão não estão limitados aos parâmetros mais à direita de uma função, como em algumas outras linguagens de programação. Você pode fornecer valores padrão para qualquer parâmetro, em qualquer posição.

```js
function sumOf(a = 1, b = 2, c = 3) {
    return a + b + c
}

console.log(sumOf(undefined, undefined, 4)) // <- 1 + 2 + 4 = 7
```

Em JavaScript, não é incomum fornecer uma função com um objeto de opções, contendo várias propriedades. Você pode determinar um objeto de opções padrão se um não for fornecido, conforme mostrado no próximo fragmento.

```js
var defaultOptions = {
    brand: 'Volkswagen',
    make: 1999
}

function carFactory(options = defaultOptions) {
    console.log(options.brand)
    console.log(options.make)
}

carFactory() // <- 'Volkswagen' // <- 1999
```

O problema com esta abordagem é que assim que o consumidor da carFactory fornecer um objeto de opções, você perde todos os seus default.

```js
carFactory({ make: 2000 })
// <- undefined
// <- 2000
```

Podemos misturar os valores padrão dos parâmetros de função com a desestruturação e obter o melhor dos dois mundos.

Novo no ES6 é a capacidade de especificar valores padrão para argumentos. Normalmente, quando os valores para argumentos não são fornecidos, eles obtêm um valor de indefinido. Os valores padrão permitem que você especifique algum outro valor padrão:

```js
function f(a, b = 'default', c = 3) {
    return `${a} - ${b} - ${c}`
}

f(5, 6, 7) // "5 - 6 - 7"
f(5, 6) // "5 - 6 - 3"
f(5) // "5 - default - 3"
f() // "undefined - default - 3"
```

#### Function Parameter Destructuring

Uma abordagem melhor do que simplesmente fornecer um valor padrão pode ser exclusivamente para desestruturar opções, fornecendo valores padrão para cada propriedade, individualmente, dentro do padrão de desestruturação. Esta abordagem também permite que você faça referência a cada opção sem passar por um objeto de opções, mas você perde a capacidade de fazer referência diretamente às opções, o que pode representar um problema em algumas situações.

```js
function carFactory({ brand = 'Volkswagen', make = 1999 }) {
    console.log(brand)
    console.log(make)
}

carFactory({
    make: 2000
})
// <- 'Volkswagen'
// <- 2000
```

Nesse caso, no entanto, perdemos novamente o valor padrão para o caso em que o consumidor não oferece nenhuma opção. O significado de `carFactory()` agora será lançado quando um objeto de opções não for fornecido. Isso pode ser corrigido usando a sintaxe mostrada no seguinte fragmento de código, que adiciona um valor de opções padrão de um objeto vazio. O objeto vazio é preenchido, propriedade por propriedade, com os valores padrão no padrão de desestruturação.

```js
function carFactory({ brand = 'Volkswagen', make = 1999 } = {}) {
    console.log(brand)
    console.log(make)
}

carFactory()
// <- 'Volkswagen'
// <- 1999
```

Além dos valores padrão, você pode usar a desestruturação em parâmetros de função para descrever a forma dos objetos que sua função pode manipular. Considere o seguinte trecho de código, onde temos um objeto de carro com várias propriedades. O objeto do carro descreve seu proprietário, que tipo de carro é, quem fabricou, quando e as preferências do proprietário quando comprou o carro.

```js
var car = {
    owner: {
        id: 'e2c3503a4181968c',
        name: 'Donald Draper'
    },
    brand: 'Peugeot',
    make: 2015,
    model: '208',
    preferences: {
        airbags: true,
        airconditioning: false,
        color: 'red'
    }
}
```

Se quisermos implementar uma função que leva em conta apenas certas propriedades de um parâmetro, pode ser uma boa idéia fazer referência dessas propriedades explicitamente pela desestruturação. O lado positivo é que nos tornamos conscientes de todas as propriedades necessárias ao ler a assinatura da função.

Quando desestrutarmos tudo, é fácil detectar quando a entrada não adere ao contrato de uma função. O exemplo a seguir mostra como cada propriedade que precisamos pode ser especificada na lista de parâmetros, colocando a forma dos objetos que podemos manipular na API `getCarProductModel`.

```js
var getCarProductModel = ({ brand, make, model }) => ({
    sku: brand + ':' + make + ':' + model,
    brand,
    make,
    model
})

getCarProductModel(car)
```

### Rest Parameter and Spread Operator

Antes do ES6, a interação com uma quantidade arbitrária de parâmetros de função era complicada. Você precisava usar `arguments`, que não é uma matriz, mas tem uma propriedade de length.

```js
function join() {
    var list = Array.prototype.slice.call(arguments) return list.join(', ')
}

join('first', 'second', 'third') // <- 'first, second, third'
```

ES6 tem uma solução melhor para o problema, e esses são os _rest parameters_.

#### Rest Parameters

Agora você pode preceder o último parâmetro em qualquer função JavaScript com três pontos, convertendo-o em um especial "rest parameter". Quando o parâmetro restante é o único parâmetro em uma função, ele obtém todos os argumentos passados ​​para a função: funciona como a solução `.slice` que vimos anteriormente, mas evita a necessidade de construção complicada, como `arguments`, e é especificada na lista de parâmetros.

```js
function join(...list) {
    return list.join(', ')
}

join('first', 'second', 'third') // <- 'first, second, third'
```

Os parâmetros nomeados antes do rest parameter não serão incluídos na lista.

```js
function join(separator, ...list) {
    return list.join(separator)
}

join('; ', 'first', 'second', 'third') // <- 'first; second; third'
```

Observe que as arrow functions com um rest parameter devem incluir parênteses, mesmo quando é o único parâmetro. Caso contrário, um `SyntaxError` seria lançado. O código a seguir é um belo exemplo de como a combinação de arrow functions e rest parameter pode produzir expressões funcionais concisas.

```js
var sumAll = (...numbers) => numbers.reduce((total, next) => total + next)
console.log(sumAll(1, 2, 5)) // <- 8
```

Em seguida, temos o spread operator. Também é denotado com três pontos, mas serve um propósito ligeiramente diferente.

#### Spread Operator

O spread operator pode ser usado para converter qualquer objeto iterável em uma matriz. Spreading efetivamente expande uma expressão em um alvo, como um literal de matriz ou uma chamada de função. O exemplo a seguir usa `...arguments` para converter parâmetros de função em um literal de matriz.

```js
function cast() {
    return [...arguments]
}

cast('a', 'b', 'c') // <- ['a', 'b', 'c']
```

Podemos usar o spread operator para dividir uma cadeia em uma matriz com cada ponto de código que compõe a seqüência de caracteres.

```js
[...'show me'] // <- ['s', 'h', 'o', 'w', ' ', 'm', 'e']
```

Você pode colocar elementos adicionais à esquerda e à direita de uma operação de propagação e ainda obter o resultado que você esperaria.

```js
function cast() {
    return ['left', ...arguments, 'right']
}
cast('a', 'b', 'c') // <- ['left', 'a', 'b', 'c', 'right']
```

A propagação é uma maneira útil de combinar vários arrays. O exemplo a seguir mostra como você pode espalhar matrizes em qualquer lugar em uma matriz literal, expandindo seus elementos no lugar.

```js
var all = [1, ...[2, 3], 4, ...[5], 6, 7]
console.log(all) // <- [1, 2, 3, 4, 5, 6, 7]
```

Observe que o spread operator não está limitado a arrays e argumentos. O spread operator pode ser usado com qualquer objeto iterable. Iterable é um protocolo no ES6 que permite transformar qualquer objeto em algo que pode ser iterado.

```js
var [first, second, ...other] = ['a', 'b', 'c', 'd', 'e']
console.log(other) // <- ['c', 'd', 'e']
```

Além de se espalhar em arrays, você também pode espalhar itens em chamadas de função.

```js
function multiply(left, right) {
    return left * right
}

var result = multiply(...[2, 3])
console.log(result) // <- 6
```

```js
function print(...list) {
    console.log(list)
}

print(1, ...[2, 3], 4, ...[5]) // <- [1, 2, 3, 4, 5]
```

```js
new (Date.bind.apply(Date, [null, 2015, 11, 31]))() // <- Thu Dec 31 2015
```

```js
new Date(...[2015, 11, 31]) // <- Thu Dec 31 2015
```

### Classes

O JavaScript é uma linguagem baseada em protótipo, e as classes são principalmente açúcares sintáticos em cima da herança prototípica. A diferença fundamental entre a herança prototípica e as classes é que as classes podem ampliar outras classes, possibilitando a extensão da Array incorporada - algo que foi muito complicado antes do ES6.

A palavra-chave da classe atua, então, como um dispositivo que torna o JavaScript mais convidativo para programadores provenientes de outros paradigmas, que talvez não estejam tão familiarizados com as cadeias de protótipos.

#### Fundamentos da classe

Ao aprender sobre novos recursos de linguagem, é sempre uma boa idéia olhar primeiro as construções existentes e depois ver como o novo recurso melhora esses casos de uso. Começaremos por analisar um construtor de JavaScript baseado em protótipos simples e, em seguida, compararemos isso com a sintaxe de classes mais novas no ES6.

```js
function Fruit(name, calories) {
    this.name = name
    this.calories = calories
    this.pieces = 1
}

Fruit.prototype.chop = function() {
    this.pieces++
}

Fruit.prototype.bite = function(person) {
    if (this.pieces < 1) {
        return
    }

    const calories = this.calories / this.pieces
    person.satiety += calories
    this.calories -= calories
    this.pieces--
}
```

Embora bastante simples, o código que acabamos de colocar deve ser suficiente para observar algumas coisas. Temos uma função de construtor que leva alguns parâmetros, um par de métodos e uma série de propriedades.

```js
const person = {
    satiety: 0
}

const apple = new Fruit('apple', 140)

apple.chop()
apple.chop()
apple.chop()
apple.bite(person)
apple.bite(person)
apple.bite(person)

console.log(person.satiety) // <- 105
console.log(apple.pieces) // <- 1
console.log(apple.calories) // <- 35
```

Ao usar a sintaxe da classe, conforme mostrado na lista de códigos a seguir, a função do construtor é declarada como um membro explícito da classe `Fruit` e os métodos seguem a sintaxe da definição do método do objeto literal. Quando comparamos a sintaxe da classe com a sintaxe baseada em protótipo, você notará que estamos reduzindo a quantidade de código de referência bastante, evitando referências explícitas ao `Fruit.prototype` ao declarar métodos. O fato de que toda a declaração é mantida dentro do bloco de classe também ajuda o leitor a entender o escopo deste código, tornando a intenção das nossas classes mais clara. Por último, ter o construtor explicitamente como um membro do método deFruit torna a sintaxe da classe mais fácil de entender quando comparada com o sabor baseado em protótipo da sintaxe da classe.

```js
class Fruit
{
    constructor(name, calories) {
        this.name = name
        this.calories = calories
        this.pieces = 1
    }

    chop() {
        this.pieces++
    }

    bite(person) {
        if (this.pieces < 1) {
            return
        }
        const calories = this.calories / this.pieces
        person.satiety += calories
        this.calories -= calories
        this.pieces--
    }
}
```

A solução baseada em classe é equivalente ao código de código baseado em protótipo que escrevemos anteriormente. Consumir uma fruta não mudaria ao mínimo; O API para frutas permanece inalterado.

Vale a pena notar que as declarações de classe não são elevadas para o topo do seu escopo, ao contrário das declarações de função. Isso significa que você não será capaz de instanciar, ou de outra forma acessar, uma classe antes que sua declaração seja alcançada e executada.

```js
new Person() // <- ReferenceError: Person is not defined
class Person {}
```

Além da sintaxe de declaração de classe apresentada anteriormente, as classes também podem ser declaradas como expressões, assim como com declarações de função e expressões de função. Você pode omitir o nome para uma expressão de classe.

```js
const Person = class {
    constructor(name) {
        this.name = name
    }
}
```

As expressões de classe podem ser facilmente retornadas de uma função, possibilitando a criação de uma fábrica de classes com um esforço mínimo. No exemplo a seguir, criamos uma classe `JakePerson` de forma dinâmica em uma arrow functions que leva um parâmetro de nome e, em seguida, alimenta isso para o construtor pessoa pai via `super()` .

As expressões de classe podem ser facilmente retornadas de uma função, possibilitando a criação de uma fábrica de classes com um esforço mínimo. No exemplo a seguir, criamos uma classe `JakePerson` de forma dinâmica em uma arrow functions que leva um parâmetro de `name` e, em seguida, mantem isso para o construtor pai `Person` via `super()`.

```js
const createPersonClass = name =>
    class extends Person {
        constructor() {
            super(name)
        }
    }

const JakePerson = createPersonClass('Jake')
const jake = new JakePerson()
```

#### Propriedades e métodos em classes

Às vezes, é necessário adicionar métodos estáticos ao nível da aula, em vez de membros no nível da instância. Usando a sintaxe disponível antes do ES6, os membros da instância devem ser explicitamente adicionados à cadeia do protótipo. Enquanto isso, métodos estáticos devem ser adicionados ao construtor diretamente.

```js
function Person() {
    this.hunger = 100
}

Person.prototype.eat = function() {
    this.hunger -
}

Person.isPerson = function(person) {
    return person instanceof Person
}
```

As classes de JavaScript permitem que você defina métodos estáticos como `Person.isPerson` usando a palavra-chave estática, assim como você usaria _get_ ou _set_ como um prefixo para uma definição de método que é um getter ou um setter.

```js
class MathHelper {
    static sum(...numbers) {
        return numbers.reduce((a, b) => a + b)
    }
}
console.log(MathHelper.sum(1, 2, 3, 4, 5)) // <- 15
```

Finalmente, vale a pena mencionar que você também pode declarar acessores de propriedades estáticas, como getters ou setters (`static get`, `static set`). Estes podem ser úteis ao manter o estado de configuração global para uma classe ou quando uma classe é usada sob um padrão singleton. Claro, provavelmente você está melhor usando objetos JavaScript nesse ponto, em vez de criar uma classe que você nunca pretende instanciar ou apenas pretender instanciar uma vez. Este é o JavaScript, uma linguagem altamente dinâmica, afinal.

#### Estendendo classes de JavaScript

As classes consolidam a herança prototípica, que até recentemente tinha sido altamente contestada no espaço do usuário por várias bibliotecas tentando tornar mais fácil lidar com a herança prototípica em JavaScript.

```js
class Fruit {}
function Fruit() {}

class Banana extends Fruit {
    constructor() {
        super('banana', 105)
    }
    slice() {
        this.pieces = 12
    }
}
```

```js
const createJuicyFruit = (...params) => class JuicyFruit extends Fruit {
    constructor() {
        this.juice = 0 super(...params)
    }
    squeeze() {
        if (this.calories <= 0) {
            return
        }
        this.calories -= 10 this.juice += 3
    }
}

class Plum extends createJuicyFruit('plum', 30) {}
```
