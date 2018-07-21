# Swift

-   **UIKit**: Dá acesso ao framework `UIKit`, fornece a infraestrutura principal para aplicativos _iOS_.
-   Constantes(`let`) e variáveis(`var`) devem ser definidas antes de usá-las.
-   Para definir uma constante, você usa a palavra-chave `let` e para definir uma variável, você usa a palavra-chave `var`.
-   A diferença entre uma constante e uma variável é que uma variável pode ser atualizada ou alterada, enquanto uma constante não pode ser alterada quando um valor é atribuído.
-   As constantes são boas para definir os valores que você sabe que nunca mudarão.
-   Os valores de uma variável nunca são implicitamente convertidos em outro tipo. Se você precisar converter um valor para um tipo diferente, faça explicitamente uma instância do tipo desejado.
-   Parênteses ao redor de condições(declarações condicionais) ou loops são _opcionais_.
-   É obrigatório o uso de _curly brackets_ no uso de instruções de controle.
-   Os opcionais garantem que os `nil` valores sejam tratados de forma explícita.

## Básico em Swift

### Constantes e Variáveis

Constantes e variáveis ​​devem ser declarados antes de serem usadas. Você declara constantes com a palavra-chave `let` e variáveis ​​com a palavra-chave `var`.

-   Pode ser declaradas constantes ou variáveis ​​múltiplas em uma única linha separando-as com uma vírgula.
-   Podemos alterar o valor de uma variável para outro valor de um tipo compatível.
-   A inferência de tipos nos permite omitir o tipo de variável quando o definimos.

```swift
// Constants
let freezingTemperatureOfWaterCelsius = 0
let speedOfLightKmSec = 300000

// Variables
var currentTemperature = 22
var currentSpeed = 55

// Constants
let freezingTempertureOfWaterCelsius: Int = 0, speedOfLightKmSec = 300000

// Variables
var currentTemperture: Double = 22, currentSpeed = 55
```

> If a stored value in your code won’t change, always declare it as a constant with the `let`keyword. Use variables only for storing values that need to be able to change.

-   **Tipos explícitos**: Utilize para tipos explícitos para definir explicitamente o tipo da variável, com `:Type` dois pontos após o identificador da variável com seu tipo.

```swift
var x: Float = 3.14

// --

var x: Float
x = 3.14
```

Se o valor inicial não fornecer informações suficientes (ou se não houver valor inicial), especifique o tipo, escrevendo-o após a variável, separados por dois pontos.

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

Os nomes das constantes e variáveis ​​podem conter praticamente qualquer caractere, incluindo caracteres Unicode:

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

_Depois de declarar uma constante ou variável de um determinado tipo, você não poderá declará-la novamente com o mesmo nome ou alterá-la para armazenar valores de um tipo diferente. Nem você pode alterar de uma constante para uma variável ou de uma variável para uma constante._

_Ao contrário de uma variável, o valor de uma constante não pode ser alterado depois de definido._

**_A verificação de tipos ajuda a evitar erros quando você está trabalhando com diferentes tipos de valores. No entanto, isso não significa que você precise especificar o tipo de cada constante e variável declarada. Se você não especificar o tipo de valor necessário, o Swift usará a *inferência de tipos* para elaborar o tipo apropriado. A inferência de tipos permite que um compilador deduza automaticamente o tipo de uma determinada expressão quando compilar seu código, simplesmente examinando os valores fornecidos por você._**

_O Swift sempre escolhe `Double`(em vez de `Float`) ao inferir o tipo de números de ponto flutuante._

_Se você combinar literais inteiros com ponto flutuante em uma expressão, um tipo de `Double`será inferido do contexto._

```swift
let anotherPi = 3 + 0.14159
// anotherPi is also inferred to be of type Double
```

-   Swift também nos permite inserir sublinhados arbitrários em nossos literais numéricos. Isso pode melhorara legibilidade do nosso código. A linguagem ignorará esses sublinhados; portanto, eles não afetam o valor do numérico literais de nenhuma forma.

```swift
var x:Int = 300_000
var y:Int = 300000
var z:Bool = x == y // true

let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

#### Conversão de inteiro e ponto flutuante

Conversões entre tipos numéricos inteiros e de ponto flutuante devem ser explicitadas:

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi equals 3.14159, and is inferred to be of type Double
```

Conversão de ponto flutuante para número inteiro também deve ser explicitada.

```swift
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
```

Floating-point values are always truncated when used to initialize a new integer value in this way. This means that `4.75` becomes `4`, and `-3.9` becomes `-3`.

---

Os valores nunca são convertidos de forma implícita para outro tipo. Se você precisa converter um valor para um tipo diferente, faça explicitamente uma instância do tipo desejado.

```swift
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```

### Tuplas

> _As tuplas_ agrupam vários valores em um único valor composto. Os valores dentro de uma tupla podem ser de qualquer tipo e não precisam ser do mesmo tipo que os outros.

Um tipo de tupla é uma lista de tipos separados por vírgulas, entre parênteses.

```swift
var team = ("Boston", "Red Sox", 97, 65, 59.9)
```

Neste exemplo, é uma tupla que descreve um _código de status HTTP_.

```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")
```

You can create tuples from any permutation of types, and they can contain as many different types as you like. There’s nothing stopping you from having a tuple of type `(Int, Int, Int)`, or `(String, Bool)`, or indeed any other permutation you require.

Você pode _decompor_ o conteúdo de uma tupla em constantes ou variáveis ​​separadas, que você acessa normalmente:

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// Prints "The status code is 404"
print("The status message is \(statusMessage)")
// Prints "The status message is Not Found"
```

Se você só precisa de alguns dos valores da tupla, ignore partes da tupla com um sublinhado ( `_`) quando você decompõe a tupla:

```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"
```

Poderíamos também recuperar os valores de uma tupla especificando a localização do valor.

```swift
var team = ("Boston", "Red Sox", 97, 65, 59.9)

var city = team.0
var name = team.1
var wins = team.2
var loses = team.3
var percent = team.4
```

Para evitar este passo de decomposição, podemos criar **tuplas nomeadas**. Uma _tupla nomeada_ associa um nome(chave) a cada elemento da tupla.

_Se você nomear os elementos em uma tupla, poderá usar os nomes dos elementos para acessar os valores desses elementos_:

```swift
var team = (city:"Boston", name:"Red Sox", wins:97, loses:65, percent:59.9)
```

Para acessar os valores de uma tupla nomeada, usamos a sintaxe de ponto.

```swift
var city = team.city
var name = team.name
var wins = team.wins
var loses = team.loses
var percent = team.percent
```

Veja mais um exemplo a seguir:

```swift
let http200Status = (statusCode: 200, description: "OK")

print("The status code is \(http200Status.statusCode)")
// Prints "The status code is 200"
print("The status message is \(http200Status.description)")
// Prints "The status message is OK"
```

### Opcionais

You use _optionals_ in situations where a value may be absent. An optional represents two possibilities: Either there _is_ a value, and you can unwrap the optional to access that value, or there _isn’t_ a value at all.

**_`nil` significando “a ausência de um objeto válido”._**

The example below uses the initializer to try to convert a `String` into an `Int`:

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
```

Because the initializer might fail, it returns an _optional_ `Int`, rather than an `Int`. An optional `Int` is written as `Int?`, not `Int`. The question mark indicates that the value it contains is optional, meaning that it might contain _some_ `Int` value, or it might contain _no value at all_. (It can’t contain anything else, such as a `Bool` value or a `String` value. It’s either an `Int`, or it’s nothing at all.)

#### `nil`

> In Swift, `nil` isn’t a pointer—it’s the absence of a value of a certain type. Optionals of _any_ type can be set to `nil`, not just object types.

You set an optional variable to a valueless state by assigning it the special value `nil`:

```swift
var serverResponseCode: Int? = 404
// serverResponseCode contains an actual Int value of 404
serverResponseCode = nil
// serverResponseCode now contains no value
```

> Você não pode usar `nil`com constantes e variáveis não opcionais. Se uma constante ou variável em seu código precisar trabalhar com a ausência de um valor sob certas condições, sempre a declare como um valor opcional do tipo apropriado.

_Se você definir uma variável opcional sem fornecer um valor padrão, a variável será definida automaticamente para `nil`_

```swift
var surveyAnswer: String?
// surveyAnswer is automatically set to nil
```

Once you’re sure that the optional _does_ contain a value, you can access its underlying value by adding an exclamation mark (`!`) to the end of the optional’s name. The exclamation mark effectively says, “I know that this optional definitely has a value; please use it.” This is known as _forced unwrapping_ of the optional’s value:

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}

// Prints "convertedNumber has an integer value of 123."
```

> Trying to use `!` to access a nonexistent optional value triggers a runtime error. Always make sure that an optional contains a non-`nil` value before using `!` to force-unwrap its value.

#### Optional Binding

```swift
if var variableName = optional {
    statements
}

// OR

if let constantName = optional {
    statements
}
```

```swift
if let actualNumber = Int(possibleNumber) {
    print("\"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("\"\(possibleNumber)\" could not be converted to an integer")
}

// Prints ""123" has an integer value of 123"
```

#### Implicitly Unwrapped Optionals

Opcionais implicitamente não-desembrulhados são úteis quando um valor opcional é confirmado para existir imediatamente depois que o opcional é definido pela primeira vez e pode definitivamente ser assumido como existindo em todos os pontos posteriores.

These kinds of optionals are defined as _implicitly unwrapped optionals_. You write an implicitly unwrapped optional by placing an exclamation mark (`String!`) rather than a question mark (`String?`) after the type that you want to make optional.

```swift
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // requires an exclamation mark

let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // no need for an exclamation mark
```

> If an implicitly unwrapped optional is `nil` and you try to access its wrapped value, you’ll trigger a runtime error. The result is exactly the same as if you place an exclamation mark after a normal optional that doesn’t contain a value.

You can still treat an implicitly unwrapped optional like a normal optional, to check if it contains a value:

```swift
if assumedString != nil {
    print(assumedString!)
}

// Prints "An implicitly unwrapped optional string."
```

You can also use an implicitly unwrapped optional with optional binding, to check and unwrap its value in a single statement:

```swift
if let definiteString = assumedString {
    print(definiteString)
}

// Prints "An implicitly unwrapped optional string."
```

> _Don’t use an implicitly unwrapped optional when there’s a possibility of a variable becoming `nil` at a later point. Always use a normal optional type if you need to check for a `nil` value during the lifetime of a variable._

### Error Handling

```swift
func canThrowAnError() throws {
    // this function may or may not throw an error
}
```

Uma função indica que pode lançar um erro incluindo a palavra-chave `throws` em sua declaração. Quando você chama uma função que pode gerar um erro, você prefixa a palavra-chave `try` à expressão.

```swift
do {
    try canThrowAnError()
    // no error was thrown
} catch {
    // an error was thrown
}
```

Veja um exemplo de como o tratamento de erros pode ser usado para responder a diferentes condições de erro:

```swift
func makeASandwich() throws {
    // ...
}

do {
    try makeASandwich()
    eatASandwich()
} catch SandwichError.outOfCleanDishes {
    washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```

### Assertions and Preconditions

_Assertions_ and _preconditions_ are checks that happen at runtime. You use them to make sure an essential condition is satisfied before executing any further code. If the Boolean condition in the assertion or precondition evaluates to `true`, code execution continues as usual. If the condition evaluates to `false`, the current state of the program is invalid; code execution ends, and your app is terminated.

The difference between assertions and preconditions is in when they’re checked: Assertions are checked only in debug builds, but preconditions are checked in both debug and production builds. In production builds, the condition inside an assertion isn’t evaluated. This means you can use as many assertions as you want during your development process, without impacting performance in production.

#### Debugging with Assertions

You write an assertion by calling the [`assert(_:_:file:line:)`](https://developer.apple.com/documentation/swift/1541112-assert) function from the Swift standard library. You pass this function an expression that evaluates to `true` or `false`and a message to display if the result of the condition is `false`. For example:

```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// This assertion fails because -3 is not >= 0.
```

In this example, code execution continues if `age >= 0` evaluates to `true`, that is, if the value of `age` is nonnegative. If the value of `age` is negative, as in the code above, then `age >= 0` evaluates to `false`, and the assertion fails, terminating the application.

If the code already checks the condition, you use the [`assertionFailure(_:file:line:)`](https://developer.apple.com/documentation/swift/1539616-assertionfailure) function to indicate that an assertion has failed. For example:

```swift
if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age > 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

#### Enforcing Preconditions

Use a precondition whenever a condition has the potential to be false, but must _definitely_ be true for your code to continue execution. For example, use a precondition to check that a subscript is not out of bounds, or to check that a function has been passed a valid value.

You write a precondition by calling the [`precondition(_:_:file:line:)`](https://developer.apple.com/documentation/swift/1540960-precondition) function. You pass this function an expression that evaluates to `true` or `false` and a message to display if the result of the condition is `false`. For example:

```swift
// In the implementation of a subscript...
precondition(index > 0, "Index must be greater than zero.")
```

You can also call the [`preconditionFailure(_:file:line:)`](https://developer.apple.com/documentation/swift/1539374-preconditionfailure) function to indicate that a failure has occurred—for example, if the default case of a switch was taken, but all valid input data should have been handled by one of the switch’s other cases.

> If you compile in unchecked mode (`-Ounchecked`), preconditions aren’t checked. The compiler assumes that preconditions are always true, and it optimizes your code accordingly. However, the `fatalError(_:file:line:)` function always halts execution, regardless of optimization settings.
>
> You can use the `fatalError(_:file:line:)` function during prototyping and early development to create stubs for functionality that hasn’t been implemented yet, by writing `fatalError("Unimplemented")` as the stub implementation. Because fatal errors are never optimized out, unlike assertions or preconditions, you can be sure that execution always halts if it encounters a stub implementation.

## Collection Types

Swift provides three primary collection types, known as `arrays`, `sets`, and `dictionaries`, for storing collections of values.

-   Arrays are ordered collections of values.
-   Sets are unordered collections of unique values.
-   Dictionaries are unordered collections of key-value associations.

![Collection Types](./Images/CollectionTypes.png)

Arrays, conjuntos e dicionários em Swift são sempre claros sobre os tipos de valores e chaves que eles podem armazenar. Isso significa que você não pode inserir um valor do tipo errado em uma coleção por engano.

### Mutability of Collections

Se você criar uma matriz, um conjunto ou um dicionário e atribuí-lo a uma **variável**, a coleção criada será **_mutable_**. Isso significa que você pode alterar (ou _mutate_) a coleção depois que ela é criada adicionando, removendo ou alterando itens na coleção. Se você atribuir uma matriz, um conjunto ou um dicionário a uma **constante**, essa coleção é **_immutable_** e seu tamanho e conteúdo não podem ser alterados.

### Arrays

Uma matriz armazena valores do mesmo tipo em uma lista ordenada. O mesmo valor pode aparecer em uma matriz várias vezes em diferentes posições.

#### Array Type Shorthand Syntax

The type of a Swift array is written in full as `Array<Element>`, where Element is the type of values the array is allowed to store. You can also write the type of an array in shorthand form as `[Element]`.

#### Creating an Empty Array

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// Prints "someInts is of type [Int] with 0 items."

someInts.append(3)
// someInts now contains 1 value of type Int

someInts = []
// someInts is now an empty array, but is still of type [Int]
```

#### Creating an Array with a Default Value

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```

#### Creating an Array by Adding Two Arrays Together

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

#### Creating an Array with an Array Literal

Você também pode inicializar uma matriz com um literal de matriz, que é uma maneira abreviada de escrever um ou mais valores como uma coleção de matrizes. Um literal de matriz é escrito como uma lista de valores, separados por vírgulas, cercados por um par de colchetes quadrados:

```
[ valor1 , valor2 , valor3 ]
```

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList has been initialized with two initial items
```

Graças à inferência de tipo de Swift, você não precisa escrever o tipo de matriz se você estiver iniciando com um array literal contendo valores do mesmo tipo. A inicialização do `shoppingList` poderia ter sido escrita em uma forma mais curta em vez disso:

```swift
var shoppingList = ["Eggs", "Milk"]
```

Como todos os valores no array literal são do mesmo tipo, Swift pode inferir que `[String]` é o tipo correto a ser usado para a variável `shoppingList`.

#### Accessing and Modifying an Array

To find out the number of items in an array, check its read-only `count` property:

```swift
print("The shopping list contains \(shoppingList.count) items.")
// Prints "The shopping list contains 2 items."
```

Use the Boolean `isEmpty` property as a shortcut for checking whether the `count` property is equal to `0`:

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// Prints "The shopping list is not empty."
```

You can add a new item to the end of an array by calling the array’s `append(_:)` method:

```swift
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```

Alternatively, append an array of one or more compatible items with the addition assignment operator (`+=`):

```swift
shoppingList += ["Baking Powder"]
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```

Retrieve a value from the array by using subscript syntax, passing the index of the value you want to retrieve within square brackets immediately after the name of the array:

```swift
var firstItem = shoppingList[0]
// firstItem is equal to "Eggs"
```

You can use subscript syntax to change an existing value at a given index:

```swift
shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```

Você também pode usar a sintaxe de subíndice para alterar um intervalo de valores ao mesmo tempo, mesmo que o conjunto de valores de substituição tenha um comprimento diferente do intervalo que você está substituindo. O exemplo a seguir substitui "Chocolate Spread", "Cheese" e "Butter" por "Bananas" e "Apples":

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList now contains 6 items
```

To insert an item into the array at a specified index, call the array’s `insert(_:at:)` method:

```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```

Similarly, you remove an item from the array with the `remove(at:)` method. Este método remove o item no índice especificado e retorna o item removido.

```swift
let mapleSyrup = shoppingList.remove(at: 0)
// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string
```

#### Iterating Over an Array

```swift
for item in shoppingList {
    print(item)
}

// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

Se você precisar do índice inteiro de cada item, bem como seu valor, use o `enumerated()` método para iterar sobre a matriz em vez disso. Para cada item na matriz, o método `enumerated()` retorna uma tupla composta por um número inteiro e o item. Os números inteiros começam em zero e conta mais um para cada item; Se você enumerar em toda uma matriz, esses números inteiros correspondem aos índices dos itens. Você pode decompor a tupla em constantes temporárias ou variáveis ​​como parte da iteração:

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}

// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

### Sets

Um conjunto armazena valores distintos do mesmo tipo em uma coleção sem ordenação definida. Você pode usar um conjunto em vez de uma matriz quando a ordem dos itens não é importante, ou quando você precisa garantir que um item só apareça uma vez.

#### Set Type Syntax

O tipo de um conjunto Swift está escrito como `Set<Element>`, onde `Element` é o tipo que o conjunto está autorizado a armazenar. Ao contrário dos arrays, os conjuntos não possuem uma forma abreviada equivalente.

#### Creating and Initializing an Empty Set

You can create an empty set of a certain type using initializer syntax:

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."
```

Alternativamente, se o contexto já fornecer informações de tipo, como um argumento de função ou uma variável ou constante já digitada, você pode criar um conjunto vazio com um literal de matriz vazio:

```swift
letters.insert("a")
// letters now contains 1 value of type Character
letters = []
// letters is now an empty set, but is still of type Set<Character>
```

#### Creating a Set with an Array Literal

You can also initialize a set with an array literal, as a shorthand way to write one or more values as a set collection.

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres has been initialized with three initial items
```

Um tipo de conjunto não pode ser inferido a partir de um literal de matriz sozinho, então o tipo `Set` deve ser explicitamente declarado. No entanto, devido à inferência de tipo de Swift, você não precisa escrever o tipo do conjunto se você estiver iniciando com um array literal contendo valores do mesmo tipo.

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

Como todos os valores no array literal são do mesmo tipo, Swift pode inferir que `Set<String>` é o tipo correto a ser usado para a variável `favoriteGenres`.

#### Acessando e Modificando um Conjunto

```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// Prints "I have 3 favorite music genres."

if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// Prints "I have particular music preferences."

favoriteGenres.insert("Jazz")
// favoriteGenres now contains 4 items

if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// Prints "Rock? I'm over it."

if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."
```

#### Iterando sobre um conjunto

```swift
for genre in favoriteGenres {
    print("\(genre)")
}

// Jazz
// Hip hop
// Classical
```

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}

// Classical
// Hip hop
// Jazz
```

### Performing Set Operations

Você pode executar eficientemente operações fundamentais de conjunto, como combinar dois conjuntos juntos, determinar quais valores dois conjuntos têm em comum ou determinar se dois conjuntos contêm todos, alguns ou nenhum dos mesmos valores.

#### Fundamental Set Operations

![Set Venn Diagram](./Images/SetVennDiagram.png)

-   Use o método `intersection(_:)` para criar um novo conjunto com apenas os valores comuns a ambos os conjuntos.
-   Use o método `symmetricDifference(_:)` para criar um novo conjunto com valores que estão nos dois conjuntos mais não em sua interseção, são todos os valores dos dois conjuntos menos os da interseção.
-   Use o método `union(_:)` para criar um novo conjunto com todos os valores em ambos os conjuntos.
-   Use o método `subtracting(_:)` para criar um novo conjunto com valores não no conjunto especificado.

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

#### Set Membership and Equality

The illustration below depicts three sets—`a`, `b` and `c`—with overlapping regions representing elements shared among sets. Set `a` is a _superset_ of set `b`, because `a`contains all elements in `b`. Conversely, set `b` is a _subset_ of set `a`, because all elements in `b` are also contained by `a`. Set `b` and set `c` are _disjoint_ with one another, because they share no elements in common.

![set Euler Diagram](./Images/SetEulerDiagram.png)

-   Use o operador "is equal" (`==`) para determinar se dois conjuntos possuem todos os mesmos valores.
-   Use o método `isSubset(of:)` para determinar se todos os valores de um conjunto estão contidos no conjunto especificado.
-   Use o método `isSuperset(of:)` para determinar se um conjunto contém todos os valores em um conjunto especificado.
-   Use os métodos `isStrictSubset(of:)` ou `isStrictSuperset(of:)` para determinar se um conjunto é um subconjunto ou superconjunto, mas não é igual a um conjunto especificado.
-   Use o método `isDisjoint(with:)` para determinar se dois conjuntos não têm valores em comum.

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```

### Dicionários

Um dicionário armazena associações entre chaves do mesmo tipo e valores do mesmo tipo em uma coleção sem ordenação definida. Cada valor está associado a uma chave exclusiva , que atua como um identificador desse valor dentro do dicionário. Ao contrário dos itens em uma matriz, os itens em um dicionário não possuem uma ordem especificada. Você usa um dicionário quando precisa pesquisar valores com base em seu identificador, da mesma forma que um dicionário do mundo real é usado para procurar a definição de uma determinada palavra.

#### Dictionary Type Shorthand Syntax

O tipo de um dicionário Swift está escrito na íntegra como `Dictionary<Key, Value>`, onde `Key` é o tipo de valor que pode ser usado como uma chave de dicionário e `Value` é o tipo de valor que o dicionário armazena para essas chaves.

Você também pode escrever o tipo de dicionário em forma abreviada como `[Key: Value]`.

#### Criando um Dicionário Vazio

To create an empty array or dictionary, use the initializer syntax.

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

```swift
var namesOfIntegers = [Int: String]()
// namesOfIntegers is an empty [Int: String] dictionary
```

Este exemplo cria um dicionário vazio de tipo `[Int: String]`. Suas chaves são de tipo `Int`, e seus valores são de tipo `String`.

Se o contexto já fornecer informações de tipo, você pode criar um dicionário vazio com um literal de dicionário vazio, que está escrito como `[:]`(dois pontos dentro de um par de colchetes):

```swift
namesOfIntegers[16] = "sixteen"
// namesOfIntegers now contains 1 key-value pair
namesOfIntegers = [:]
// namesOfIntegers is once again an empty dictionary of type [Int: String]
```

#### Creating a Dictionary with a Dictionary Literal

```
[ chave1 : valor1 , chave2 : valor2 , chave3 : valor3 ]
```

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

Tal como acontece com os arrays, você não precisa escrever o tipo do dicionário se você estiver iniciando com um dicionário literal cujas chaves e valores tenham tipos consistentes.

```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

#### Acessando e Modificando um Dicionário

```swift
print("The airports dictionary contains \(airports.count) items.")
// Prints "The airports dictionary contains 2 items."

if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// Prints "The airports dictionary is not empty."

airports["LHR"] = "London"
// the airports dictionary now contains 3 items

if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// Prints "The old value for DUB was Dublin."

// You can also use subscript syntax to retrieve a value from the dictionary for a particular key.
// Because it is possible to request a key for which no value exists,
// a dictionary’s subscript returns an optional value of the dictionary’s value type.
// If the dictionary contains a value for the requested key, the subscript returns an optional
// value containing the existing value for that key. Otherwise, the subscript returns nil:

if let airportName = airports["DUB"] {
    print("The name of the airport is \(airportName).")
} else {
    print("That airport is not in the airports dictionary.")
}
// Prints "The name of the airport is Dublin Airport."

// You can use subscript syntax to remove a key-value pair from a dictionary
// by assigning a value of nil for that key:

airports["APL"] = "Apple International"
// "Apple International" is not the real airport for APL, so delete it
airports["APL"] = nil
// APL has now been removed from the dictionary

// Alternatively, remove a key-value pair from a dictionary with the
// removeValue(forKey:) method. This method removes the key-value pair if it exists
// and returns the removed value, or returns nil if no value existed:

if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// Prints "The removed airport's name is Dublin Airport."
```

#### Iterating Over a Dictionary

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow
```

You can also retrieve an iterable collection of a dictionary’s keys or values by accessing its `keys` and `values` properties:

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR

for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```

If you need to use a dictionary’s keys or values with an API that takes an `Array` instance, initialize a new array with the `keys` or `values` property:

```swift
let airportCodes = [String](airports.keys)
// airportCodes is ["YYZ", "LHR"]

let airportNames = [String](airports.values)
// airportNames is ["Toronto Pearson", "London Heathrow"]
```

## Control Flow

### For-In Loops

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}

// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

#### Dictionary - (key, value)

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}

// ants have 6 legs
// cats have 4 legs
// spiders have 8 legs
```

#### Numeric ranges

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}

// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

If you don’t need each value from a sequence, you can ignore the values by using an underscore in place of a variable name.

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}

print("\(base) to the power of \(power) is \(answer)")
// Prints "3 to the power of 10 is 59049"
```

O caractere de sublinhado (`_`) usado no lugar de uma variável de loop faz com que os valores individuais sejam ignorados e não forneça acesso ao valor atual durante cada iteração do loop.

In some situations, you might not want to use closed ranges, which include both endpoints. Consider drawing the tick marks for every minute on a watch face. You want to draw `60` tick marks, starting with the `0` minute. Use the half-open range operator (`..<`) to include the lower bound but not the upper bound. For more about ranges, see [Range Operators](https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html#ID73).

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // render the tick mark each minute (60 times)
}
```

**`stride(from:to:by:)`**

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
```

Closed ranges are also available, by using `stride(from:through:by:)` instead:

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // render the tick mark every 3 hours (3, 6, 9, 12)
}
```

### While Loops

#### While

A `while` loop starts by evaluating a single condition. If the condition is `true`, a set of statements is repeated until the condition becomes `false`.

```
while [condition] {
    [statements]
}
```

#### Repeat-While

The other variation of the `while` loop, known as the `repeat-while` loop, performs a single pass through the loop block first, *before* considering the loop’s condition. It then continues to repeat the loop until the condition is `false`.

*The repeat-while loop in Swift is analogous to a do-while loop in other languages.*

```
repeat {
    [statements]
} while [condition]
```

### Conditional Statements

Swift provides two ways to add conditional branches to your code: the `if` statement and the `switch` statement. Typically, you use the `if` statement to evaluate simple conditions with only a few possible outcomes. The `switch` statement is better suited to more complex conditions with multiple possible permutations and is useful in situations where pattern matching can help select an appropriate code branch to execute.

#### If

In its simplest form, the `if` statement has a single `if` condition. It executes a set of statements only if that condition is `true`.

```swift
var temperatureInFahrenheit = 90

if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}

// Prints "It's really warm. Don't forget to wear sunscreen."
```

#### Switch

A `switch` statement considers a value and compares it against several possible matching patterns. It then executes an appropriate block of code, based on the first pattern that matches successfully. A `switch` statement provides an alternative to the `if` statement for responding to multiple potential states.

```
switch [some value to consider] {
case [value 1]:
    [respond to value 1]
case [value 2],
     [value 3]:
    [respond to value 2 or 3]
default:
    [otherwise, do something else]
}
```

This example uses a `switch` statement to consider a single lowercase character called `someCharacter`:

```swift
let someCharacter: Character = "z"

switch someCharacter {
case "a":
    print("The first letter of the alphabet")
case "z":
    print("The last letter of the alphabet")
default:
    print("Some other character")
}

// Prints "The last letter of the alphabet"
```

The body of each case must contain at least one executable statement. It is not valid to write the following code, because the first case is empty:

```swift
let anotherCharacter: Character = "a"

switch anotherCharacter {
case "a": // Invalid, the case has an empty body
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}

// This will report a compile-time error.
```

To make a switch with a single case that matches both "a" and "A", combine the two values into a compound case, separating the values with commas.

```swift
let anotherCharacter: Character = "a"

switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}

// Prints "The letter A"
```

##### Interval Matching

Values in `switch` cases can be checked for their inclusion in an interval. This example uses number intervals to provide a natural-language count for numbers of any size:

```swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
let naturalCount: String

switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}

print("There are \(naturalCount) \(countedThings).")
// Prints "There are dozens of moons orbiting Saturn."
```

##### Tuples

You can use tuples to test multiple values in the same `switch` statement. Each element of the tuple can be tested against a different value or interval of values. Alternatively, use the underscore character (`_`), also known as the wildcard pattern, to match any possible value.

```swift
let somePoint = (1, 1)

switch somePoint {
case (0, 0):
    print("\(somePoint) is at the origin")
case (_, 0):
    print("\(somePoint) is on the x-axis")
case (0, _):
    print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
default:
    print("\(somePoint) is outside of the box")
}

// Prints "(1, 1) is inside the box"
```

#####  Value Bindings

A `switch` case can name the value or values it matches to temporary constants or variables, for use in the body of the case. This behavior is known as value binding, because the values are bound to temporary constants or variables within the case’s body.

```swift
let anotherPoint = (2, 0)

switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}

// Prints "on the x-axis with an x value of 2"
```

The `switch` statement determines whether the point is on the red x-axis, on the orange y-axis, or elsewhere (on neither axis).

The three `switch` cases declare placeholder constants `x` and `y`, which temporarily take on one or both tuple values from `anotherPoint`. The first case  `case  (let  x,  0)`, matches any point with a `y` value of `0` and assigns the point’s `x` value to the temporary constant `x`. Similarly, the second case, `case  (0,  let  y)`, matches any point with an `x`value of `0` and assigns the point’  `y` value to the temporary constant `y`.

After the temporary constants are declared, they can be used within the case’s code block. Here, they are used to print the categorization of the point.

This `switch` statement does not have a `default` case. The final case, `case  let  (x,  y)`, declares a tuple of two placeholder constants that can match any value. Because `anotherPoint` is always a tuple of two values, this case matches all possible remaining values, and a `default` case is not needed to make the `switch` statement exhaustive.

##### Where

A `switch` case can use a `where` clause to check for additional conditions.

```swift
let yetAnotherPoint = (1, -1)

switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}

// Prints "(1, -1) is on the line x == -y"
```

The `switch` statement determines whether the point is on the green diagonal line where `x == y`, on the purple diagonal line where `x  ==  -y`, or neither.

The three `switch` cases declare placeholder constants `x` and `y`, which temporarily take on the two tuple values from `yetAnotherPoint`. These constants are used as part of a `where` clause, to create a dynamic filter. The `switch` case matches the current value of `point` only if the `where` clause’s condition evaluates to `true` for that value.

As in the previous example, the final case matches all possible remaining values, and so a  `default`  case is not needed to make the  `switch`  statement exhaustive.

##### Compound Cases

Multiple switch cases that share the same body can be combined by writing several patterns after `case`, with a comma between each of the patterns. If any of the patterns match, then the case is considered to match. The patterns can be written over multiple lines if the list is long.

```swift
let someCharacter: Character = "e"

switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}

// Prints "e is a vowel"
```

Compound cases can also include value bindings. All of the patterns of a compound case have to include the same set of value bindings, and each binding has to get a value of the same type from all of the patterns in the compound case. This ensures that, no matter which part of the compound case matched, the code in the body of the case can always access a value for the bindings and that the value always has the same type.

```swift
let stillAnotherPoint = (9, 0)

switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}

// Prints "On an axis, 9 from the origin"
```

### Checking API Availability

Swift has built-in support for checking API availability, which ensures that you don’t accidentally use APIs that are unavailable on a given deployment target.

The compiler uses availability information in the SDK to verify that all of the APIs used in your code are available on the deployment target specified by your project. Swift reports an error at compile time if you try to use an API that isn’t available.

You use an availability condition in an `if` or `guard` statement to conditionally execute a block of code, depending on whether the APIs you want to use are available at runtime. The compiler uses the information from the availability condition when it verifies that the APIs in that block of code are available.

```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```

The availability condition above specifies that in iOS, the body of the if statement executes only in iOS 10 and later; in macOS, only in macOS 10.12 and later. The last argument, *, is required and specifies that on any other platform, the body of the if executes on the minimum deployment target specified by your target.

```
if #available([platform name] [version], [...], *) {
    [statements to execute if the APIs are available]
} else {
    [fallback statements to execute if the APIs are unavailable]
}
```

## Functions

Todas as funções no Swift têm um tipo, consistindo nos tipos de parâmetros da função e no tipo de retorno. Você pode usar esse tipo como qualquer outro tipo em Swift, o que facilita a passagem das funções como parâmetros para outras funções e para retornar as funções das funções. As funções também podem ser escritas dentro de outras funções para encapsular funcionalidades úteis dentro de um escopo de função aninhada.

Em Swift, uma função é um bloco de código autônomo que executa uma tarefa específica. As funções geralmente são usadas para quebra logicamente nosso código em blocos reutilizáveis. O nome da função é usado para chamar a função.

Toda função Swift tem um tipo associado a ele. Este tipo é referido como o tipo de retorno e define o tipo de dados retornados da função para o código que o chamou. Se um valor não for retornado de uma função, o tipo de retorno é Vazio `void`.

### Defining and Calling Functions

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

All of this information is rolled up into the function’s definition, which is prefixed with the `func` keyword. You indicate the function’s return type with the return arrow `->` (a hyphen followed by a right angle bracket), which is followed by the name of the type to return.

```swift
print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
```

You call the `greet(person:)` function by passing it a `String` value after the `person`argument label, such as `greet(person:  "Anna")`. Because the function returns a `String` value, `greet(person:)` can be wrapped in a call to the `print(_:separator:terminator:)` function to print that string and see its return value, as shown above.

### Using a single parameter function

```swift
func sayHello(name: String) -> Void
{
    let retString = "Hello " + name
    print(retString)
}
```

```swift
func sayHello2(name: String) -> String
{
    return "Hello " + name
}
```

The `->` string defines that the return type associated with the function.

```swift
sayHello(name: "Allyson")
var message = sayHello2(name: "Allyson")
_ = sayHello2(name: "Allyson")
```

O sublinhado diz ao compilador que você está ciente do valor de retorno, mas você não deseja usá-lo.

### Parâmetros de função e valores de retorno

#### Funções sem Parâmetros

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}

print(sayHelloWorld())
// Prints "hello, world"
```

#### Funções com vários parâmetros

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}

print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```

You call the  `greet(person:alreadyGreeted:)`  function by passing it both a  `String`argument value labeled  `person`  and a  `Bool`  argument value labeled  `alreadyGreeted`  in parentheses, separated by commas. Note that this function is distinct from the  `greet(person:)`  function shown in an earlier section. Although both functions have names that begin with  `greet`, the  `greet(person:alreadyGreeted:)`  function takes two arguments but the  `greet(person:)`  function takes only one.

Ao chamar uma função de vários parâmetros, separamos os parâmetros com vírgulas. Também precisamos incluir o nome do parâmetro para todos os parâmetros.

```swift
func sayHello(name: String, greeting: String)
{
    print("\(greeting) \(name)")
}

sayHello(name:"Allyson", greeting:"Olá")
```

#### Funções sem valores de retorno

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}

greet(person: "Dave")
// Prints "Hello, Dave!"
```

```swift
func greet(person: String) -> Void {
    print("Hello, \(person)!")
}

greet(person: "Dave")
// Prints "Hello, Dave!"
```

```swift
func greet(person: String) -> () {
    print("Hello, \(person)!")
}

greet(person: "Dave")
// Prints "Hello, Dave!"
```

```swift
func printAndCount(string: String) -> Int {
    print(string)
    return string.count
}

func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}

printAndCount(string: "hello, world")
// prints "hello, world" and returns a value of 12

printWithoutCounting(string: "hello, world")
// prints "hello, world" but does not return a value
```

#### Funções com vários valores de retorno

Você pode usar um tipo de tupla como o tipo de retorno para uma função para retornar valores múltiplos como parte de um valor de retorno composto.

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]

    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }

    return (currentMin, currentMax)
}
```

```swift
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```

```swift
func getTeam() -> (team:String, wins:Int, percent:Double)
{
    return ("Red Sox", 99, 0.611)
}

var t = getTeam()
print("\(t.team) had \(t.wins) wins")
```

##### Tipos de retorno de Tuplas opcionais

If the tuple type to be returned from a function has the potential to have “no value” for the entire tuple, you can use an optional tuple return type to reflect the fact that the entire tuple can be `nil`. You write an optional tuple return type by placing a question mark after the tuple type’s closing parenthesis, such as `(Int, Int)?` or `(String, Int, Bool)?`.

_An optional tuple type such as `(Int, Int)?` is different from a tuple that contains optional types such as `(Int?, Int?)`. With an optional tuple type, the entire tuple is optional, not just each individual value within the tuple._

To handle an empty array safely, write the `minMax(array:)` function with an optional tuple return type and return a value of `nil` when the array is empty:

```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]

    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }

    return (currentMin, currentMax)
}
```

You can use optional binding to check whether this version of the `minMax(array:)` function returns an actual tuple value or `nil`:

```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// Prints "min is -6 and max is 109"
```

#### Returning optional values

```swift
func getName() ->String?
{
    return nil
}
```

```swift
func getTeam(id: Int) -> (team:String, wins:Int, percent:Double)?
{
    if id == 1 {
        return ("Red Sox", 99, 0.611)
    }

    return nil
}
```

```swift
func getTeam() -> (team:String, wins:Int, percent:Double?)
{
    let retTuple: (String, Int, Double?) = ("Red Sox", 99, nil)
    return retTuple
}
```

### Function Argument Labels and Parameter Names

Each function parameter has both an argument label and a parameter name. The argument label is used when calling the function; each argument is written in the function call with its argument label before it. The parameter name is used in the implementation of the function. By default, parameters use their parameter name as their argument label.

```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}

someFunction(firstParameterName: 1, secondParameterName: 2)
```

#### Specifying Argument Labels

You write an argument label before the parameter name, separated by a space:

```swift
func someFunction(argumentLabel parameterName: Int) {
    // In the function body, parameterName refers to the argument value
    // for that parameter.
}
```

```swift
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}

print(greet(person: "Bill", from: "Cupertino"))
// Prints "Hello Bill!  Glad you could visit from Cupertino."
```

#### Omitting Argument Labels

If you don’t want an argument label for a parameter, write an underscore (`_`) instead of an explicit argument label for that parameter.

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)
```

_If a parameter has an argument label, the argument must be labeled when you call the function._

#### Default Parameter Values

```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // If you omit the second argument when calling this function, then
    // the value of parameterWithDefault is 12 inside the function body.
}

someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6)
// parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4)
// parameterWithDefault is 12
```

Coloque parâmetros que não tenham valores padrão no início da lista de parâmetros de uma função, antes dos parâmetros que possuem valores padrão. Parâmetros que não possuem valores padrão geralmente são mais importantes para o significado da função; declarando-os primeiro torna mais fácil reconhecer que a mesma função está sendo chamada, independentemente de quaisquer parâmetros padrão serem omitidos.

```swift
func sayHello(name: String, greeting: String = "Bonjour")
{
    print("\(greeting) \(name)")
}

sayHello(name:"Allyson") // Bonjour Allyson
sayHello(name:"Allyson", greeting: "Hello") // Hello Allyson
```

```swift
func sayHello(name: String, name2: String = "Kim", greeting: String = "Bonjour")
{
    print("\(greeting) \(name) and \(name2)")
}

sayHello(name:"Jon", greeting: "Hello") // Hello Jon and Kim
```

#### Variadic Parameters

A variadic parameter accepts zero or more values of a specified type. You use a variadic parameter to specify that the parameter can be passed a varying number of input values when the function is called. Write variadic parameters by inserting three period characters (`...`) after the parameter’s type name.

The values passed to a variadic parameter are made available within the function’s body as an array of the appropriate type. For example, a variadic parameter with a name of `numbers` and a type of `Double...` is made available within the function’s body as a constant array called `numbers` of type `[Double]`.

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}

arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers

arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```

```swift
func sayHello(greeting: String, names: String...)
{
    for name in names {
        print("\(greeting) \(name)")
    }
}

sayHello(greeting: "Hello", names: "Allyson", "Bianca")
```

#### In-Out Parameters

**Parâmetros de função são constantes por padrão**. Tentando alterar o valor de um parâmetro de função dentro do corpo dessa função resulta em um erro de tempo de compilação. Isso significa que você não pode alterar o valor de um parâmetro por engano. Se você deseja que uma função modifique o valor de um parâmetro e que essas alterações persistam após a conclusão da chamada de função, defina esse parâmetro como um parâmetro _in-out_.

Você escreve um parâmetro in-out colocando a palavra-chave `inout` antes do tipo de um parâmetro. Um parâmetro *in-out* tem um valor que é passado *in* para a função, é modificado pela função, e é passado de volta para fora da função para substituir o valor original.

Você só pode passar uma variável como argumento para um parâmetro in-out. Você não pode passar um valor constante ou literal como o argumento, porque as constantes e os literais não podem ser modificados. Você coloca um e comercial (`&`) diretamente antes do nome de uma variável quando o passa como um argumento para um parâmetro _in-out_, para indicar que ele pode ser modificado pela função.

_In-out parameters cannot have default values, and variadic parameters cannot be marked as inout._

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)

print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

_In-out parameters are not the same as returning a value from a function. The swapTwoInts example above does not define a return type or return a value, but it still modifies the values of someInt and anotherInt. In-out parameters are an alternative way for a function to have an effect outside of the scope of its function body._

```swift
func reverse( first: inout String, second: inout String)
{
    let tmp = first
    first = second
    second = tmp
}

var one = "One"
var two = "Two"
reverse(first: &one, second: &two)

print("one: \(one) two: \(two)") // one: Two two: One
```

### Function Types (Delegate)

Every function has a specific function type, made up of the **parameter types** and the **return type** of the function.

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

#### Using Function Types

Você usa tipos de função exatamente como qualquer outro tipo em Swift. Por exemplo, você pode definir uma constante ou variável para ser de um tipo de função e atribuir uma função apropriada a essa variável:

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts

print("Result: \(mathFunction(2, 3))")
// Prints "Result: 5"
```

Uma função diferente com o mesmo tipo de correspondência pode ser atribuída à mesma variável, da mesma forma que para tipos não funcionais:

```swift
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```

Como com qualquer outro tipo, você pode deixar Swift para inferir o tipo de função quando atribuir uma função a uma constante ou variável:

```swift
let anotherMathFunction = addTwoInts
// anotherMathFunction is inferred to be of type (Int, Int) -> Int
```

#### Tipos de Função como Tipos de Parâmetros

Você pode usar um tipo de função, `(Int, Int) -> Int` como um tipo de parâmetro para outra função. Isso permite que você deixe alguns aspectos da implementação de uma função para o chamador da função para fornecer quando a função é chamada.

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}

printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```

Este exemplo define uma função chamada `printMathResult(_:_:_:)`, que possui três parâmetros. O primeiro parâmetro é chamado `mathFunction`, e é do tipo `(Int, Int) -> Int`. Você pode passar qualquer função desse tipo como o argumento para este primeiro parâmetro. Os segundo e terceiro parâmetros são chamados `a` e `b`, e ambos são de tipo `Int`. Estes são utilizados como os dois valores de entrada para a função matemática fornecida.

O papel `printMathResult(_:_:_:)` é imprimir o resultado de uma chamada para uma função matemática de um tipo apropriado. Não importa o que a implementação dessa função realmente faça, importa apenas que a função seja do tipo correto. Isso permite `printMathResult(_:_:_:)` distribuir algumas das suas funcionalidades ao chamador da função de forma segura.

#### Tipos de Função como Tipos de Retorno

Você pode usar um tipo de função como o tipo de retorno de outra função. Você faz isso escrevendo um tipo de função completo imediatamente após a seta de retorno (`->`) da função de retorno.

The next example defines two simple functions called `stepForward(_:)` and `stepBackward(_:)`. The `stepForward(_:)` function returns a value one more than its input value, and the `stepBackward(_:)` function returns a value one less than its input value. Both functions have a type of `(Int) -> Int`:

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}

func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```

Here’s a function called `chooseStepFunction(backward:)`, whose return type is `(Int) -> Int`. The `chooseStepFunction(backward:)` function returns the `stepForward(_:)` function or the `stepBackward(_:)` function based on a Boolean parameter called backward:

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
```

You can now use `chooseStepFunction(backward:)` to obtain a function that will step in one direction or the other:

```swift
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```

Now that `moveNearerToZero` refers to the correct function, it can be used to count to zero:

```swift
print("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}

print("zero!")
// 3...
// 2...
// 1...
// zero!
```

### Nested Functions

All of the functions you have encountered so far in this chapter have been examples of  _global functions_, which are defined at a global scope. You can also define functions inside the bodies of other functions, known as  _nested functions_.

Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also return one of its nested functions to allow the nested function to be used in another scope.

You can rewrite the  `chooseStepFunction(backward:)`  example above to use and return nested functions:

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}

var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}

print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```

## Closures

_Closures_ are self-contained blocks of functionality that can be passed around and used in your code. Closures in Swift are similar to blocks in C and Objective-C and to lambdas in other programming languages.

Closures can capture and store references to any constants and variables from the context in which they are defined. This is known as closing over those constants and variables. Swift handles all of the memory management of capturing for you.

### Simple closures

```swift
let clos = { () -> Void in
    print("Hello World")
}

clos()
```

```swift
let clos = { (name: String) -> Void in
    print("Hello \(name)")
}

clos("Allyson")
```

```swift
func testClosure(handler: (String) -> Void)
{
    handler("Silva")
}

testClosure(handler: clos) // Hello Silva
```

### Closure Expressions

#### Closure Expression Syntax

Closure expression syntax has the following general form:

```
{ ([parameters]) -> [return type] in
    [statements]
}
```

The start of the closure’s body is introduced by the `in` keyword. This keyword indicates that the definition of the closure’s parameters and return type has finished, and the body of the closure is about to begin.

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]

var reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

#### Inferring Type From Context

Because the sorting closure is passed as an argument to a method, Swift can infer the types of its parameters and the type of the value it returns. The `sorted(by:)` method is being called on an array of strings, so its argument must be a function of type `(String,  String)  ->  Bool`. This means that the `(String,  String)` and `Bool` types do not need to be written as part of the closure expression’s definition. Because all of the types can be inferred, the return arrow (`->`) and the parentheses around the names of the parameters can also be omitted:

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 })
```

#### Implicit Returns from Single-Expression Closures

Single-expression closures can implicitly return the result of their single expression by omitting the `return` keyword from their declaration, as in this version of the previous example:

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 })
```

#### Shorthand Argument Names

Swift automatically provides shorthand argument names to inline closures, which can be used to refer to the values of the closure’s arguments by the names `$0`, `$1`, `$2`, and so on.

If you use these shorthand argument names within your closure expression, you can omit the closure’s argument list from its definition, and the number and type of the shorthand argument names will be inferred from the expected function type. The `in` keyword can also be omitted, because the closure expression is made up entirely of its body:

```swift
reversedNames = names.sorted(by: { $0 > $1 })
```

Here, `$0` and `$1` refer to the closure’s first and second `String` arguments.

```swift
func testFunction(num: Int, handler: () -> Void)
{
    for _ in 0..<num {
        handler()
    }
}

let clos = { () -> Void in
    print("Hello from standard syntax")
}

testFunction(num: 5, handler: clos)

testFunction(num: 5,handler: { print("Hello from Shorthand closure") })
```

```swift
func testFunction2(num: Int, handler: (_ :String) -> Void)
{
    for _ in 0..<num {
        handler("Me")
    }
}

testFunction2(num: 5,handler: { print("Hello from \($0)") })
```

```swift
let clos: (String, String) -> Void = {
    print("\($0) \($1)")
}

clos("Allyson", "Silva") // Allyson Silva
```

This next example is used when the closure doesn't return any value. Rather than defining the return type as Void, we can use parentheses, as the following example shows:

```swift
let clos: () -> () = { print("Howdy") }
clos()
```

If the entire closure body consists of only a single statement, then we can omit the return keyword, and the results of the statement will be returned.

```swift
let clos = {
    (first: Int, second: Int) -> Int in
    first + second
}
```

#### Operator Methods

There’s actually an even _shorter_ way to write the closure expression above. Swift’s `String` type defines its string-specific implementation of the greater-than operator (`>`) as a method that has two parameters of type `String`, and returns a value of type `Bool`. This exactly matches the method type needed by the `sorted(by:)` method. Therefore, you can simply pass in the greater-than operator, and Swift will infer that you want to use its string-specific implementation:

```swift
reversedNames = names.sorted(by: >)
```

### Using closures with Swift's array algorithms

```swift
let guests = ["Jon", "Kim", "Kailey", "Kara"]
```

```swift
guests.map({ (name: String) -> Void in
    print("Hello \(name)")
})
```

```swift
guests.map({ print("Hello \($0)") })
```

```swift
var messages = guests.map({ (name: String) -> String in
    return "Welcome \(name)"
})

for message in messages {
    print(message)
}
```

```swift
let greetGuest = { (name: String) -> Void in
    print("Hello guest named \(name)")
}

let sayGoodbye = { (name: String) -> Void in
    print("Goodbye \(name)")
}

guests.map(greetGuest)
guests.map(sayGoodbye)
```

```swift
func testFunction()
{
    var total = 0
    var count = 0

    let addTemps = { (num: Int) -> Void in
        total += num
        count += 1
    }

    temperatures(calculate: addTemps)

    print("Total: \(total)")
    print("Count: \(count)")
    print("Average: \(total/count)")
}

testFunction()
// Total: 498
// Count: 7
// Average: 71
```

### Changing functionality

```swift
class TestClass
{
    typealias getNumClosure = ((Int, Int) -> Int)

    var numOne = 5
    var numTwo = 8
    var results = 0

    func getNum(handler: getNumClosure) -> Int
    {
        results = handler(numOne, numTwo)
        return results
    }
}
```

```swift
var max: TestClass.getNumClosure = {
    if $0 > $1 {
        return $0
    } else {
        return $1
    }
}

var min: TestClass.getNumClosure = {
    if $0 < $1 {
        return $0
    } else {
        return $1
    }
}

var multiply: TestClass.getNumClosure = {
    return $0 * $1
}

var second: TestClass.getNumClosure = {
    return $1
}

var answer: TestClass.getNumClosure = {
    var tmp = $0 + $1
    return 42
}
```

```swift
var myClass = TestClass()

myClass.getNum(handler: max) // 8
myClass.getNum(handler: min) // 5
myClass.getNum(handler: multiply) // 40
myClass.getNum(handler: second) // 8
myClass.getNum(handler: answer) // 42
```

### Trailing Closures

If you need to pass a closure expression to a function as the function’s final argument and the closure expression is long, it can be useful to write it as a _trailing closure_instead. A trailing closure is written after the function call’s parentheses, even though it is still an argument to the function. When you use the trailing closure syntax, you don’t write the argument label for the closure as part of the function call.

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}

// Here's how you call this function without using a trailing closure:

someFunctionThatTakesAClosure(closure: {
    // closure's body goes here
})

// Here's how you call this function with a trailing closure instead:

someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}
```

The string-sorting closure from the [Closure Expression Syntax](https://docs.swift.org/swift-book/LanguageGuide/Closures.html#ID97) section above can be written outside of the `sorted(by:)` method’s parentheses as a trailing closure:

```swift
reversedNames = names.sorted() { $0 > $1 }
```

If a closure expression is provided as the function or method’s only argument and you provide that expression as a trailing closure, you do not need to write a pair of parentheses `()` after the function or method’s name when you call the function:

```swift
reversedNames = names.sorted { $0 > $1 }
```

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```

You can now use the `numbers` array to create an array of `String` values, by passing a closure expression to the array’s `map(_:)` method as a trailing closure:

```swift
let strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}

// strings is inferred to be of type [String]
// its value is ["OneSix", "FiveEight", "FiveOneZero"]
```

### Capturing Values

A closure can capture constants and variables from the surrounding context in which it is defined. The closure can then refer to and modify the values of those constants and variables from within its body, even if the original scope that defined the constants and variables no longer exists.

In Swift, the simplest form of a closure that can capture values is a nested function, written within the body of another function. A nested function can capture any of its outer function’s arguments and can also capture any constants and variables defined within the outer function.

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
incrementByTen()
// returns a value of 10
incrementByTen()
// returns a value of 20
incrementByTen()
// returns a value of 30

let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// returns a value of 7

incrementByTen()
// returns a value of 40
```

### Closures Are Reference Types

In the example above, `incrementBySeven` and `incrementByTen` are constants, but the closures these constants refer to are still able to increment the `runningTotal` variables that they have captured. This is because functions and closures are *reference types*.

Whenever you assign a function or a closure to a constant or a variable, you are actually setting that constant or variable to be a reference to the function or closure. In the example above, it is the choice of closure that `incrementByTen` refers to that is constant, and not the contents of the closure itself.

This also means that if you assign a closure to two different constants or variables, both of those constants or variables will refer to the same closure:

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50
```

### Escaping Closures

A closure is said to escape a function when the closure is passed as an argument to the function, but is called after the function returns. When you declare a function that takes a closure as one of its parameters, you can write `@escaping` before the parameter’s type to indicate that the closure is allowed to escape.

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```

The `someFunctionWithEscapingClosure(_:)` function takes a closure as its argument and adds it to an array that’s declared outside the function. If you didn’t mark the parameter of this function with `@escaping`, you would get a compile-time error.

Marking a closure with `@escaping` means you have to refer to self explicitly within the closure.

```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
```

```swift
let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"

completionHandlers.first?()
print(instance.x)
// Prints "100"
```

### Autoclosures

Um *autoclosure* é uma closure que é criado automaticamente para envolver uma expressão que está sendo passada como um argumento para uma função. Não leva nenhum argumento, e quando é chamado, ele retorna o valor da expressão que está envolvida dentro dele. Esta conveniência sintática permite que você omita braces em torno dos parâmetro de uma função escrevendo uma expressão normal em vez de um fechamento explícito.

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// Prints "5"

let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)
// Prints "5"

print("Now serving \(customerProvider())!")
// Prints "Now serving Chris!"
print(customersInLine.count)
// Prints "4"
```

You get the same behavior of delayed evaluation when you pass a closure as an argument to a function.

```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}

serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```

The `serve(customer:)` function in the listing above takes an explicit closure that returns a customer’s name. The version of `serve(customer:)` below performs the same operation but, instead of taking an explicit closure, it takes an *autoclosure* by marking its parameter’s type with the `@autoclosure` attribute. Now you can call the function as if it took a `String` argument instead of a closure. The argument is automatically converted to a closure, because the `customerProvider` parameter’s type is marked with the `@autoclosure` attribute.

```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}

serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
```

If you want an autoclosure that is allowed to escape, use both the `@autoclosure` and `@escaping` attributes. The `@escaping` attribute is described above in Escaping Closures.

```swift
// customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}

collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))

print("Collected \(customerProviders.count) closures.")

// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}

// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
```
