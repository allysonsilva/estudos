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

Você também pode usar a sintaxe de subscript para alterar um intervalo de valores ao mesmo tempo, mesmo que o conjunto de valores de substituição tenha um comprimento diferente do intervalo que você está substituindo. O exemplo a seguir substitui "Chocolate Spread", "Cheese" e "Butter" por "Bananas" e "Apples":

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

The three `switch` cases declare placeholder constants `x` and `y`, which temporarily take on one or both tuple values from `anotherPoint`. The first case `case  (let  x,  0)`, matches any point with a `y` value of `0` and assigns the point’s `x` value to the temporary constant `x`. Similarly, the second case, `case  (0,  let  y)`, matches any point with an `x`value of `0` and assigns the point’ `y` value to the temporary constant `y`.

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

As in the previous example, the final case matches all possible remaining values, and so a `default` case is not needed to make the `switch` statement exhaustive.

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

You call the `greet(person:alreadyGreeted:)` function by passing it both a `String`argument value labeled `person` and a `Bool` argument value labeled `alreadyGreeted` in parentheses, separated by commas. Note that this function is distinct from the `greet(person:)` function shown in an earlier section. Although both functions have names that begin with `greet`, the `greet(person:alreadyGreeted:)` function takes two arguments but the `greet(person:)` function takes only one.

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

You can rewrite the `chooseStepFunction(backward:)` example above to use and return nested functions:

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

## Enumerations

Uma enumeração define um tipo comum para um grupo de valores relacionados e permite que você trabalhe com esses valores de forma segura em seu código.

Enumerations in Swift are first-class types in their own right. They adopt many features traditionally supported only by classes, such as computed properties to provide additional information about the enumeration’s current value, and instance methods to provide functionality related to the values the enumeration represents. Enumerations can also define initializers to provide an initial case value; can be extended to expand their functionality beyond their original implementation; and can conform to protocols to provide standard functionality.

### Enumeration Syntax

```swift
enum SomeEnumeration {
    // enumeration definition goes here
}
```

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

The values defined in an enumeration (such as `north`, `south`, `east`, and `west`) are its _enumeration_ cases. You use the `case` keyword to introduce new enumeration cases.

Vários casos podem aparecer em uma única linha, separados por vírgulas:

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

```swift
var directionToHead = CompassPoint.west
```

The type of `directionToHead` is inferred when it is initialized with one of the possible values of `CompassPoint.` Once `directionToHead` is declared as a `CompassPoint`, you can set it to a different `CompassPoint` value using a shorter dot syntax:

```swift
directionToHead = .east
```

### Matching Enumeration Values with a Switch Statement

Você pode combinar valores de enumeração individuais com uma declaração `switch`:

```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}

// Prints "Watch out for penguins"
```

When it is not appropriate to provide a `case` for every enumeration case, you can provide a `default` case to cover any cases that are not addressed explicitly:

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}

// Prints "Mostly harmless"
```

### Associated Values

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}

var productBarcode = Barcode.upc(8, 85909, 51226, 3)
//
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

Se todos os valores associados para um caso de enumeração são extraídos como constantes, ou se todos são extraídos como variáveis, você pode colocar um único `var` ou `let` anotação antes do nome do caso, por brevidade:

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

### Raw Values

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

Os valores brutos podem ser strings, caracteres ou qualquer um dos tipos de número inteiro ou de ponto flutuante. **Cada valor bruto deve ser exclusivo dentro de sua declaração de enumeração**.

_Raw values are not the same as associated values. Raw values are set to prepopulated values when you first define the enumeration in your code, like the three ASCII codes above. The raw value for a particular enumeration case is always the same. Associated values are set when you create a new constant or variable based on one of the enumeration’s cases, and can be different each time you do so._

#### Implicitly Assigned Raw Values

Quando você trabalha com enumerações que armazenam valores brutos inteiros ou de string, não é necessário atribuir explicitamente um valor bruto a cada caso. Quando você não fizer isso, o Swift atribuirá automaticamente os valores para você.

Por exemplo, quando números inteiros são usados ​​para valores brutos, o valor implícito para cada caso é um a mais que o caso anterior. Se o primeiro caso não tiver um valor definido, seu valor será 0.

A enumeração abaixo é um refinamento da enumeração `Planet` anterior, com valores brutos inteiros para representar a ordem de cada planeta do sistema solar:

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

No exemplo acima, `Planet.mercury` tem um valor bruto explícito de 1, `Planet.venus` tem um valor bruto implícito de 2 e assim por diante.

Quando as seqüências de caracteres são usadas para valores brutos, o valor implícito para cada caso é o texto do nome desse caso.

A enumeração abaixo é um refinamento da enumeração anterior `CompassPoint`, com valores brutos de string para representar o nome de cada direção:

```swift
enum CompassPoint: String {
    case north, south, east, west
}
```

No exemplo acima, `CompassPoint.south` tem um valor bruto implícito de "south" e assim por diante.

Você acessa o valor bruto de um caso de enumeração com sua propriedade `rawValue`:

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"
```

#### Initializing from a Raw Value

If you define an enumeration with a raw-value type, the enumeration automatically receives an initializer that takes a value of the raw value’s type (as a parameter called `rawValue`) and returns either an enumeration case or `nil`. You can use this initializer to try to create a new instance of the enumeration.

This example identifies Uranus from its raw value of 7:

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus
```

Not all possible `Int` values will find a matching planet, however. Because of this, the raw value initializer always returns an optional enumeration case. In the example above, `possiblePlanet` is of type `Planet?`, or “optional `Planet`.”

If you try to find a planet with a position of 11, the optional `Planet` value returned by the raw value initializer will be `nil`:

```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// Prints "There isn't a planet at position 11"
```

This example uses optional binding to try to access a planet with a raw value of 11. The statement `if let somePlanet = Planet(rawValue: 11)` creates an optional Planet, and sets `somePlanet` to the value of that optional `Planet` if it can be retrieved. In this case, it is not possible to retrieve a planet with a position of 11, and so the `else` branch is executed instead.

## Classes and Structures

**Classes**: Tipos de referências.
**Estruturas**: Tipos de valores.

Classes e estruturas são construções flexíveis e de propósito geral que se tornam os blocos de construção do código do seu programa. Você define propriedades e métodos para adicionar funcionalidade às suas classes e estruturas usando exatamente a mesma sintaxe como para constantes, variáveis ​​e funções.

Unlike other programming languages, Swift does not require you to create separate interface and implementation files for custom classes and structures. In Swift, you define a class or a structure in a single file, and the external interface to that class or structure is automatically made available for other code to use.

### Comparando Classes e Estruturas

Classes e estruturas em Swift têm muitas coisas em comum. Ambos podem:

* Definir propriedades para armazenar valores.
* Definir métodos para fornecer funcionalidade.
* Defina subscripts para fornecer acesso aos seus valores usando subscript syntax.
* Defina initializers para configurar seu estado inicial.
* Ser estendido para expandir suas funcionalidades para além de uma implementação padrão.
* De acordo com protocolos para fornecer funcionalidades padrão de um determinado tipo.

For more information, see [Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html), [Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html), [Subscripts](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html), [Initialization](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html), [Extensions](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html), and [Protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html).

As classes possuem capacidades adicionais que as estruturas não:

* A herança permite que uma classe herde as características de outra.
* Type casting enables you to check and interpret the type of a class instance at runtime.
* Os Deinitializers permitem que uma instância de uma classe libere todos os recursos que atribuiu.
* Reference counting allows more than one reference to a class instance.

The additional capabilities that classes support come at the cost of increased complexity. As a general guideline, prefer structures and enumerations because they’re easier to reason about, and use classes when they’re appropriate or necessary. In practice, this means most of the custom data types you define will be structures and enumerations. For a more detailed comparison, see [Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes).

#### Definition Syntax

Structures and classes have a similar definition syntax. You introduce structures with the `struct` keyword and classes with the `class` keyword. Both place their entire definition within a pair of braces:

```swift
class SomeClass {
    // class definition goes here
}

struct SomeStructure {
    // structure definition goes here
}
```

```swift
class MyClass
{
    // MyClass definition
}

struct MyStruct
{
    // MyStruct definition
}
```

Here’s an example of a structure definition and a class definition:

```swift
struct Resolution {
    var width = 0
    var height = 0
}

class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```

As propriedades armazenadas são constantes ou variáveis ​​agrupadas e armazenadas como parte da classe ou estrutura.

#### Instâncias de Classe e Estrutura

A sintaxe para criar instâncias é muito semelhante para estruturas e classes:

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

Structures and classes both use initializer syntax for new instances. The simplest form of initializer syntax uses the type name of the class or structure followed by empty parentheses, such as `Resolution()` or `VideoMode()`. This creates a new instance of the class or structure, with any properties initialized to their default values. Class and structure initialization is described in more detail in [Initialization](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html).

#### Acessando propriedades

```swift
print("The width of someResolution is \(someResolution.width)")
// Prints "The width of someResolution is 0"

print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is 0"

someVideoMode.resolution.width = 1280

print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is now 1280"
```

#### Memberwise Initializers for Structure Types

All structures have an automatically-generated _memberwise initializer_, which you can use to initialize the member properties of new structure instances. Initial values for the properties of the new instance can be passed to the memberwise initializer by name:

```swift
let vga = Resolution(width: 640, height: 480)
```

_Unlike structures, class instances do not receive a default memberwise initializer._

### Properties

**Stored properties**: These store variable or constant values as part of an instance of a class or structure. Stored properties can also have property observers that can monitor the property for changes and respond with custom actions when the value of the property changes.

**Computed properties**: These do not store a value themselves, but retrieve and possibly set other properties. The value returned by a computed property can also be calculated when it is requested.

*Within our structure and class, we can now access these properties by using the name of the property and the self keyword. Every instance of a structure or class has a property named self. This property refers to the instance itself; therefore, we can use it to access the properties within the instance.*

```swift
struct EmployeeStruct
{
    var firstName = ""
    var lastName = ""
    var salaryYear = 0.0
}

public class EmployeeClass
{
    var firstName = ""
    var lastName = ""
    var salaryYear = 0.0
}
```

#### Stored properties

```swift
struct MyStruct
{
    let c = 5
    var v = ""
}

class MyClass
{
    let c = 5
    var v = ""
}
```

One of the differences between a structure and a class is that, by default, a structure creates an initializer that lets us populate the stored properties when we create an instance of the structure.

```swift
var myStruct = MyStruct(v: "Hello")
```

The order in which the parameters appear in the initializer is the order that we defined them.

#### Computed properties

```swift
var salaryWeek: Double {
    get {
        return self.salaryYear/52
    }
}
```

We can simplify the definition of the read-only computed property by removing the get keyword.

```swift
var salaryWeek: Double {
    return self.salaryYear/52
}
```

As propriedades computed não se limitam a ser somente leitura, também podemos escrever para elas.

```swift
var salaryWeek: Double {
    get {
        return self.salaryYear / 52
    } set (newSalaryWeek) {
        self.salaryYear = newSalaryWeek * 52
    }
}
```

Podemos simplificar a definição do `set` ao não definir um nome para o novo valor. Nesse caso, o valor será atribuído a um nome de variável padrão, `newValue`.

```swift
var salaryWeek: Double {
    get {
        return self.salaryYear / 52
    } set {
        self.salaryYear = newValue * 52
    }
}
```

### Property observers

Os observadores de propriedade são chamados sempre que o valor da propriedade é definido. Podemos adicionar observadores de propriedades a qualquer propriedade armazenada non-lazy. Podemos também adicionar observadores de propriedades a qualquer propriedade armazenada ou computadas herdada, substituindo a propriedade na subclasse.

Existem dois observadores de propriedade que podemos definir no Swift - `willSet` e no `didSet`. O observador `willSet` é chamado imediatamente antes da configuração da propriedade, e o observador do `didSet` é chamado logo após a propriedade estar configurada.

```swift
var salaryYear: Double = 0.0 {
    willSet(newSalary) {
        print("About to set salaryYear to \(newSalary)")
    }
    didSet {
        if salaryWeek > oldValue {
            print("\(firstName) got a raise")
        } else {
            print("\(firstName) did not get a raise")
        }
    }
}

//

willSet {
    print("About to set salaryYear to \(newValue)")
}
```

### Methods

Os métodos são funções associadas a uma classe ou estrutura. Um método, como uma função, encapsulará o código para uma tarefa específica ou funcionalidade associada à classe ou estrutura.

```swift
func getFullName() -> String
{
    return firstName + " " + lastName
}
```

Nós definimos esse método exatamente como nós definiríamos qualquer função. Um método é simplesmente uma função associada a uma classe ou estrutura específicas.

```swift
var e = EmployeeClass()
var f = EmployeeStruct(firstName: "Jon", lastName: "Hoffman", salaryYear: 50000)

e.firstName = "Jon"
e.lastName = "Hoffman"
e.salaryYear = 50000.00

print(e.getFullName()) // Jon Hoffman is printed to the console
print(f.getFullName()) //Jon Hoffman is printed to the console
```

Por padrão, não podemos atualizar valores de propriedade dentro de um método de uma estrutura. Se quisermos modificar uma propriedade, podemos optar por um comportamento mutante para esse método, adicionando a palavra-chave `mutating` antes da palavra-chave `func` da declaração do método.

```swift
mutating func giveRase(amount: Double)
{
    self.salaryYear += amount
}
```

### Custom initializers

Os inicializadores são chamados quando inicializamos uma nova instância de um tipo específico (classe ou estrutura). A inicialização é o processo de preparação de uma instância para uso. O processo de inicialização pode incluir a definição de valores iniciais para as propriedades armazenadas, verificar se há recursos externos disponíveis ou configurar a interface do usuário corretamente. Os inicializadores geralmente são usados para garantir que a instância da classe ou estrutura seja devidamente inicializada antes do primeiro uso.

```swift
init() {
    //Perform initialization here
}
```

```swift
init() {
    self.firstName = ""
    self.lastName = ""
    self.salaryYear = 0.0
}

init(firstName: String, lastName: String) {
    self.firstName = firstName
    self.lastName = lastName
    self.salaryYear = 0.0
}

init(firstName: String, lastName: String, salaryYear: Double) {
    self.firstName = firstName
    self.lastName = lastName
    self.salaryYear = salaryYear
}
```

**Um inicializador não tem um valor de retorno**.

Uma classe, ao contrário de uma estrutura, pode ter um desinstalador. Um desinstalador é chamado apenas antes de uma instância da classe ser destruída e removida da memória.

### Internal and external parameter names

Assim como as funções, os parâmetros associados a um inicializador podem ter nomes internos e externos separados. Se não fornecemos nomes de parâmetros externos para nossos parâmetros, o Swift os gerará automaticamente para nós. Nos exemplos anteriores, não incluímos nomes de parâmetros externos na definição dos inicializadores, portanto Swift os criou para usar o nome do parâmetro interno como o nome do parâmetro externo.

Se quisermos fornecer nossos próprios nomes de parâmetros, faria isso colocando o nome do parâmetro externo antes do nome do parâmetro interno, exatamente como fazemos com qualquer função normal.

```swift
init(employeeWithFirstName firstName: String, lastName lastName: String, andSalary salaryYear: Double) {
    self.firstName = firstName
    self.lastName = lastName
    self.salaryYear = salaryYear
}
```

```swift
var i = EmployeeClass(employeeWithFirstName: "Me", lastName: "Moe", andSalary: 45000)
```

### Access levels

In Swift, there are four access levels:

* **Public**: Este é o nível de controle de acesso mais visível. Isso nos permite usar a propriedade, o método, a classe, etc., onde queremos importar o módulo. Basicamente, qualquer coisa pode usar um item que tenha um nível de controle de acesso de público. Este nível é usado principalmente por frameworks para expor a API pública da estrutura.

* **Internal**: Este é o nível de acesso padrão. This access level allows us to use the property, method, class, and so on in the defining source as well as the module that the source is in (the application or framework). If this level is used in a framework, it lets other parts of the framework use the item but code outside the framework will be unable to access it.

* **Private**: este é o nível de controle de acesso menos visível. Só nos permite usar a propriedade, método, classe e assim por diante no arquivo de origem que o define.

* **Fileprivate**: This access control allows access to the properties and methods from any code within the same source file that the item is defined in.

To define access levels, we place the name of the level before the definition of the entity.

```swift
private struct EmployeeStruct {}
public class EmployeeClass {}
internal class EmployeeClass2 {}
public var firstName = "Jon"
internal var lastName = "Hoffman"
private var salaryYear = 0.0
public func getFullName() -> String {}
private func giveRaise(amount: Double) {}
```

### Preventing overrides

```swift
override func getDetails() -> String
{
    let details = super.getDetails()
    return "\(details) Leaves: \(leaves)"
}
```

```swift
override var description: String
{
    return "\(super.description) I am a Tree class."
}
```

Para evitar substituições ou subclassing, usamos a palavra-chave `final`. Para usar a palavra-chave `final`, nós a adicionamos antes da definição do item. Exemplos são `final func`, `final var` e `final class`.

Qualquer tentativa de substituir um item marcado como `final` dará um erro de compilação.

### Protocols (Interfaces)

Há momentos em que gostaríamos de descrever as implementações (métodos, propriedades e outros requisitos) de uma classe sem realmente fornecer qualquer implementação. Para isso, usaríamos protocolos.

Os protocolos definem um modelo de métodos, propriedades e outros requisitos para uma classe ou uma estrutura. Uma classe ou uma estrutura podem então fornecer uma implementação que esteja em conformidade com esses requisitos. A classe ou estrutura que fornece a implementação é dito em conformidade com o protocolo.

#### Protocol syntax

```swift
protocol MyProtocol
{
    //protocol definition here
}

class myClass: MyProtocol
{
    //class implementation here
}

class MyClass: MyProtocol, AnotherProtocol, ThirdProtocol
{
    // class implementation here
}

Class MyClass: MySuperClass, MyProtocol, MyProtocol2
{
    // Class implementation here
}
```

#### Property requirements

Ao definir uma propriedade dentro de um protocolo, devemos especificar se a propriedade é uma propriedade de somente leitura ou de leitura e gravação usando as palavras-chave `get` e `set`.

```swift
protocol FullName
{
    var firstName: String {get set}
    var lastName: String {get set}
}
```

Se quisermos especificar que a propriedade é somente leitura, nós a definimos com apenas a palavra-chave `get`.

```swift
var readOnly: String {get}
```

Let's see how we can create a `Scientist` class that conforms to this protocol:

```swift
class Scientist: FullName
{
    var firstName = ""
    var lastName = ""
}
```

#### Method requirements

Um protocolo pode exigir que a classe ou estrutura conforme forneça certos métodos. Nós definimos um método dentro de um protocolo exatamente como fazemos dentro de uma classe ou estrutura normal.

```swift
protocol FullName
{
    var firstName: String {get set}
    var lastName: String {get set}
    func getFullName() -> String
}
```

```swift
class Scientist: FullName
{
    var firstName = ""
    var lastName = ""
    var field = ""

    func getFullName() -> String
    {
        return "\(firstName) \(lastName) studies \(field)"
    }
}
```

```swift
var scientist = Scientist()
scientist.firstName = "Kara"
scientist.lastName = "Hoffman"
scientist.field = "Physics"

var person: FullName
person = scientist
print(person.getFullName()) // Kara Hoffman studies Physics
```

#### Protocols as types

Embora nenhuma funcionalidade seja implementada em um protocolo, eles ainda são considerados um tipo completo na linguagem de programação Swift e podem ser usados como qualquer outro tipo. O que isso significa é que podemos usar protocolos como um tipo de parâmetro ou como um tipo de retorno em uma função. Também podemos usá-los como tipo de variáveis, constantes e coleções.

```swift
protocol PersonProtocol
{
    var firstName: String {get set}
    var lastName: String {get set}
    var birthDate: NSDate {get set}
    var profession: String {get}

    init (firstName: String, lastName: String, birthDate: NSDate)
}
```

In this first example, we will see how we can use protocols as a parameter type or return type in functions, methods, or initializers:

```swift
func updatePerson(person: PersonProtocol) -> PersonProtocol
{
    // Code to update person goes here return person
}
```

Now let's see how we can use protocols as a type for constants, variables, or properties:

```swift
var myPerson: PersonProtocol
var people: [PersonProtocol] = []
```

Como vimos anteriormente, podemos usar nosso protocolo `PersonProtocol` como o tipo de uma matriz. Isso significa que podemos preencher a matriz com instâncias de qualquer tipo que estejam em conformidade com o protocolo `PersonProtocol`. Mais uma vez, não importa se o tipo for uma classe ou uma estrutura, desde que esteja em conformidade com o protocolo `PersonProtocol`.

```swift
var programmer = SwiftProgrammer(firstName: "Jon", lastName: "Hoffman", birthDate: bDateProgrammer)

var player = FootballPlayer(firstName: "Dan", lastName: "Marino", birthDate: bDatePlayer)

var people: [PersonProtocol] = []
people.append(programmer)
people.append(player)
```

#### Type casting with protocols

```swift
for person in people where person is SwiftProgrammer {
    print("\(person.firstName) is a Swift Programmer")
}
```

```swift
for person in people {
    if let p = person as? SwiftProgrammer {
        print("\(person.firstName) is a Swift Programmer")
    }
}
```

```swift
for person in people where person is SwiftProgrammer {
    let p = person as! SwiftProgrammer
}
```

#### Protocol extensions

As extensões de protocolo nos permitem estender um protocolo para fornecer implementações de métodos e propriedades para os tipos de conformidade. Eles também nos permitem fornecer implementações comuns a todos os tipos de confirmação, eliminando a necessidade de fornecer uma implementação em cada tipo individual ou a necessidade de criar uma hierarquia de classe. Embora as extensões de protocolo possam não parecer muito excitantes, uma vez que você veja o quão poderosas elas realmente são, elas vão transformar a maneira como você pensa e escreve código.

```swift
protocol DogProtocol
{
    var name: String {get set}
    var color: String {get set}
}

extension DogProtocol
{
    func speak() -> String { return "Woof Woof" }
}
```

### Extensions

Com extensões, podemos adicionar novas propriedades, métodos, inicializações e subscripts, ou tornar uma classe ou estrutura existente de acordo com um protocolo sem modificar o código fonte para a classe ou estrutura. Uma coisa a observar é que as extensões não podem substituir a funcionalidade existente.

```swift
extension String {

    var firstLetter: Character? {
        get {
            return self.characters.first
        }
    }

    func reverse() -> String
    {
        var reverse = ""
        for letter in self.characters {
            reverse = "\(letter)" + reverse
        }

        return reverse
    }
}
```

```swift
var myString = "Learning Swift is fun" print(myString.reverse())
print(myString.firstLetter)
```

### Failable initializers

Um inicializador failable é um inicializador que pode não inicializar os recursos necessários para uma classe ou uma estrutura, tornando assim a instância inutilizável. Ao usar um inicializador failable, o resultado do inicializador é um tipo opcional, contendo uma instância válida do tipo ou nulo.

Um inicializador pode ser disponibilizado adicionando um ponto de interrogação (?) Após a palavra-chave `init`.

```swift
init?(firstName: String, lastName: String, salaryYear: Double) {
    self.firstName = firstName
    self.lastName = lastName
    self.salaryYear = salaryYear

    if self.salaryYear < 20000 {
        return nil
    }
}
```

```swift
if let f = EmployeeClass(firstName: "Jon", lastName: "Hoffman", salaryYear: 19000) {
    print(f.getFullName())
} else {
    print("Failed to initialize")
}
```

### Structures and Enumerations Are Value Types

Um tipo de valor é um tipo cujo valor é copiado quando é atribuído a uma variável ou constante, ou quando é passado para uma função.

Todas as estruturas e enumerações são tipos de valor em Swift. Isso significa que todas as instâncias de estrutura e enumeração que você cria - e quaisquer tipos de valor que possuem como propriedades - sempre são copiadas quando são transmitidas em seu código.

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd

cinema.width = 2048

print("cinema is now \(cinema.width) pixels wide")
// Prints "cinema is now 2048 pixels wide"

print("hd is still \(hd.width) pixels wide")
// Prints "hd is still 1920 pixels wide"
```

![Shared State Struct](./Images/SharedStateStruct.png "Shared State Struct")

O mesmo comportamento aplica-se às enumerações:

```swift
enum CompassPoint {
    case north, south, east, west
}

var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection

currentDirection = .east

if rememberedDirection == .west {
    print("The remembered direction is still .west")
}

// Prints "The remembered direction is still .west"
```

### As classes são tipos de referência

Ao contrário dos tipos de valor, os tipos de referência não são copiados quando são atribuídos a uma variável ou constante, ou quando são passados ​​para uma função. Em vez de uma cópia, uma referência à mesma instância existente é usada em vez disso.

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0

print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// Prints "The frameRate property of tenEighty is now 30.0"
```

![Shared State Class](./Images/SharedStateClass.png "Shared State Class")

#### Identity Operators

Because classes are reference types, it is possible for multiple constants and variables to refer to the same single instance of a class behind the scenes. (The same is not true for structures and enumerations, because they are always copied when they are assigned to a constant or variable, or passed to a function.)

It can sometimes be useful to find out if two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:

Identical to (`===`)
Not identical to (`!==`)

Use esses operadores para verificar se duas constantes ou variáveis ​​se referem à mesma instância única:

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}

// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```

Note that “identical to” (represented by three equals signs, or `===`) does not mean the same thing as “equal to” (represented by two equals signs, or `==`):

* “Identical to” means that two constants or variables of class type refer to exactly the same class instance.
* “Equal to” means that two instances are considered “equal” or “equivalent” in value, for some appropriate meaning of “equal”, as defined by the type’s designer.

### Escolhendo entre classes e estruturas

Você pode usar classes e estruturas para definir tipos de dados personalizados para usar como blocos de construção do código do seu programa.

No entanto, as instâncias de estrutura são sempre passadas por valor , e as instâncias de classe são sempre aprovadas por referência . Isso significa que eles são adequados para diferentes tipos de tarefas. À medida que você considera as construções de dados e a funcionalidade que você precisa para um projeto, decida se cada construção de dados deve ser definida como uma classe ou como uma estrutura.

Como orientação geral, considere criar uma estrutura quando uma ou mais dessas condições se aplicam:

* O objetivo principal da estrutura é encapsular alguns valores de dados relativamente simples.
* É razoável esperar que os valores encapsulados sejam copiados e não referenciados quando você atribui ou passa uma instância dessa estrutura.
* Quaisquer propriedades armazenadas pela estrutura são elas próprias tipos de valor, que também deveriam ser copiados e não referenciados.
* A estrutura não precisa herdar propriedades ou comportamento de outro tipo existente.

Em todos os outros casos, defina uma classe e crie instâncias dessa classe para serem gerenciadas e aprovadas por referência. Na prática, isso significa que a maioria das construções de dados personalizados devem ser classes, não estruturas.

### Assignment and Copy Behavior for Strings, Arrays, and Dictionaries

In Swift, many basic data types such as `String`, `Array`, and `Dictionary` are implemented as structures. This means that data such as strings, arrays, and dictionaries are copied when they are assigned to a new constant or variable, or when they are passed to a function or method.

This behavior is different from Foundation: `NSString`, `NSArray`, and `NSDictionary` are implemented as classes, not structures. Strings, arrays, and dictionaries in Foundation are always assigned and passed around as a reference to an existing instance, rather than as a copy.

## Propriedades

Propriedades associam valores com uma classe, estrutura ou enumeração específica. As propriedades armazenadas armazenam valores constantes e variáveis ​​como parte de uma instância, enquanto que as propriedades computadas calculam (em vez de armazenar) um valor. As computed properties são fornecidas por classes, estruturas e enumerações. As propriedades armazenadas são fornecidas apenas por classes e estruturas.

As propriedades armazenadas e computadas geralmente são associadas a instâncias de um tipo específico. No entanto, as propriedades também podem ser associadas ao próprio tipo. Tais propriedades são conhecidas como propriedades de tipo.

Além disso, você pode definir observadores de propriedades para monitorar mudanças no valor de uma propriedade, que você pode responder com ações personalizadas. Os observadores de propriedade podem ser adicionados às propriedades armazenadas que você se define, e também às propriedades que uma subclasse herda de sua superclasse.

### Stored Properties (Propriedades Armazenadas)

Na sua forma mais simples, uma propriedade armazenada é uma constante ou variável que é armazenada como parte de uma instância de uma determinada classe ou estrutura. As propriedades armazenadas podem ser propriedades armazenadas variáveis (introduzidas pela palavra-chave `var`) ou propriedades armazenadas constantes (introduzidas pela palavra-chave `let`).

Você pode fornecer um valor padrão para uma propriedade armazenada como parte de sua definição.  Você também pode definir e modificar o valor inicial para uma propriedade armazenada durante a inicialização.

O exemplo abaixo define uma estrutura chamada `FixedLengthRange`, que descreve um intervalo de inteiros cujo comprimento de intervalo não pode ser alterado após a criação:

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}

var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2

rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8
```

Instâncias de `FixedLengthRangeter` uma propriedade variável armazenada chamada `firstValue` e uma propriedade armazenada constante chamada `length`. No exemplo acima, `length` é inicializado quando o novo intervalo é criado e não pode ser alterado posteriormente, porque é uma propriedade constante.

#### Stored Properties of Constant Structure Instances

Se você criar uma instância de uma estrutura e atribuir essa instância a uma constante, não pode modificar as propriedades da instância, mesmo que fossem declaradas como propriedades variáveis:

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3

rangeOfFourItems.firstValue = 6
// this will report an error, even though firstValue is a variable property
```

This behavior is due to structures being value types. When an instance of a value type is marked as a constant, so are all of its properties.

O mesmo não é verdadeiro para classes, que são tipos de referência. Se você atribuir uma instância de um tipo de referência a uma constante, você ainda pode alterar as propriedades variáveis ​​dessa instância.

#### Lazy Stored Properties

Uma _lazy stored property_ é uma propriedade cujo valor inicial não é calculado até a primeira vez que é usado. Você indica uma propriedade armazenada lazy escrevendo o `lazy` modificador antes de sua declaração.

As Lazy properties são úteis quando o valor inicial para uma propriedade depende de fatores externos cujos valores não são conhecidos até a conclusão da inicialização de uma instância. As Lazy properties também são úteis quando o valor inicial para uma propriedade requer uma configuração complexa ou computacionalmente dispendiosa que não deve ser realizada a menos que seja necessária.

```swift
class DataImporter {
    /*
     DataImporter is a class to import data from an external file.
     The class is assumed to take a nontrivial amount of time to initialize.
     */
    var filename = "data.txt"
    // the DataImporter class would provide data importing functionality here
}

class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // the DataManager class would provide data management functionality here
}

let manager = DataManager()

manager.data.append("Some data")
manager.data.append("Some more data")
// the DataImporter instance for the importer property has not yet been created
```

Como está marcado com o `lazy` modificador, a instância `DataImporter` para a propriedade `importer` só é criada quando a propriedade `importer` é acessada pela primeira vez, como quando a propriedade `filename` é consultada:

```swift
print(manager.importer.filename)
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```

#### Stored Properties and Instance Variables

### Computed Properties(Propriedades computadas)

Além das propriedades armazenadas, classes, estruturas e enumerações podem definir computed properties, que na verdade não armazenam um valor. Em vez disso, eles fornecem um `getter` e um opcional `setter` para recuperar e definir outras propriedades e valores indiretamente.

```swift
struct Point {
    var x = 0.0, y = 0.0
}

struct Size {
    var width = 0.0, height = 0.0
}

struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}

var square = Rect(origin: Point(x: 0.0, y: 0.0), size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)

print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

#### Shorthand Setter Declaration

Se o setter de uma computed properties não definir um nome para o novo valor a ser definido, um nome padrão `newValue` é usado.

```swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

#### Read-Only Computed Properties

Uma computed properties com um `getter`, mas nenhum setter é conhecida como uma propriedade computacional somente leitura  Uma propriedade computacional somente leitura retorna sempre um valor e pode ser acessada através de sintaxe de ponto, mas não pode ser configurada para um valor diferente.

*You must declare computed properties—including read-only computed properties—as variable properties with the `var` keyword, because their value is not fixed. The `let` keyword is only used for constant properties, to indicate that their values cannot be changed once they are set as part of instance initialization.*

Você pode simplificar a declaração de uma computed properties somente leitura, removendo a palavra-chave `get` e suas chaves:

```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}

let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"
```

### Property Observers

Os observadores da propriedade observam e respondem às mudanças no valor de uma propriedade. Os observadores de propriedade são chamados sempre que o valor de uma propriedade é definido, mesmo que o novo valor seja o mesmo que o valor atual da propriedade.

Você pode adicionar observadores de propriedades a quaisquer propriedades armazenadas que você define, exceto para Lazy properties armazenadas.

Você tem a opção de definir um ou ambos os observadores em uma propriedade:

* `willSet` é chamado antes do valor ser armazenado.
* `didSet` é chamado imediatamente após o novo valor ser armazenado.

Se você implementar um observador `willSet`, é passado o novo valor da propriedade como um parâmetro constante. Você pode especificar um nome para este parâmetro como parte de sua implementação `willSet`. Se você não escrever o nome do parâmetro e parênteses dentro de sua implementação, o parâmetro é disponibilizado com um nome de parâmetro padrão `newValue`.

Da mesma forma, se você implementar um observador `didSet`, é passado um parâmetro constante que contém o valor da propriedade antiga. Você pode nomear o parâmetro ou usar o nome do parâmetro padrão `oldValue`. Se você atribuir um valor a uma propriedade dentro de seu próprio observador `didSet`, o novo valor que você atribui substitui o que acabou de ser definido.

*The `willSet` and `didSet` observers of superclass properties are called when a property is set in a subclass initializer, after the superclass initializer has been called. They are not called while a class is setting its own properties, before the superclass initializer has been called.*

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}

let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

Os observadores `willSet` e `didSet` para `totalSteps` são chamados sempre que a propriedade recebe um novo valor. Isso é verdade mesmo se o novo valor for o mesmo que o valor atual.

O observador `willSet` deste exemplo usa um nome de parâmetro personalizado `newTotalSteps` para o próximo novo valor. Neste exemplo, ele simplesmente imprime o valor que está prestes a ser configurado.

O observador `didSet` é chamado após o valor de `totalSteps` ser atualizado. Ele compara o novo valor de `totalSteps` com o valor antigo. Se o número total de etapas aumentou, uma mensagem é impressa para indicar quantos passos novos foram feitos. O observador `didSet` não fornece um nome de parâmetro personalizado para o valor antigo e o nome padrão do `oldValue` é usado em vez disso.

### Variáveis ​​globais e locais

Os recursos descritos acima para as propriedades de computação e observação também estão disponíveis para variáveis globais e variáveis locais. Variáveis globais são variáveis que são definidas fora de qualquer função, método, encerramento ou contexto de tipo. As variáveis locais são variáveis que são definidas dentro de uma função, método ou contexto de encerramento.

### Type Properties (Propriedades Estáticas)

Propriedades de instância são propriedades que pertencem a uma instância de um tipo específico. Toda vez que você cria uma nova instância desse tipo, possui seu próprio conjunto de valores de propriedade, separado de qualquer outra instância.

Você também pode definir propriedades que pertencem ao próprio tipo, e não a nenhuma instância desse tipo. Só haverá uma cópia dessas propriedades, independentemente de quantas instâncias desse tipo você crie. Esses tipos de propriedades são chamados de *type properties*.

As propriedades de tipo são úteis para definir valores que são universais para todas as instâncias de um tipo específico, como uma propriedade constante que todas as instâncias podem usar ou uma propriedade variável que armazena um valor global para todos instâncias desse tipo.

Stored type properties can be variables or constants. Computed type properties are always declared as variable properties, in the same way as computed instance properties.

#### Type Property Syntax

You define type properties with the `static` keyword. For computed type properties for class types, you can use the `class` keyword instead to allow subclasses to override the superclass’s implementation.

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}

enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}

class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```

_The computed type property examples above are for read-only computed type properties, but you can also define read-write computed type properties with the same syntax as for computed instance properties._

#### Querying and Setting Type Properties

As propriedades de tipo são consultadas e definidas com sintaxe de ponto, assim como as propriedades da instância. No entanto, as propriedades de tipo são consultadas e definidas no tipo , não em uma instância desse tipo.

```swift
print(SomeStructure.storedTypeProperty)
// Prints "Some value."
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty)
// Prints "Another value."
print(SomeEnumeration.computedTypeProperty)
// Prints "6"
print(SomeClass.computedTypeProperty)
// Prints "27"
```

The figure below illustrates how two of these audio channels can be combined to model a stereo audio level meter. When a channel’s audio level is 0, none of the lights for that channel are lit. When the audio level is 10, all of the lights for that channel are lit. In this figure, the left channel has a current level of 9, and the right channel has a current level of 7:

![Static Properties VUMeter](./Images/StaticPropertiesVUMeter.png "Static Properties VUMeter")

Os canais de áudio descritos acima são representados por instâncias da estrutura `AudioChannel`:

```swift
struct AudioChannel {
    static let thresholdLevel = 10
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
        didSet {
            if currentLevel > AudioChannel.thresholdLevel {
                // cap the new audio level to the threshold level
                currentLevel = AudioChannel.thresholdLevel
            }
            if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                // store this as the new overall maximum input level
                AudioChannel.maxInputLevelForAllChannels = currentLevel
            }
        }
    }
}
```

The `AudioChannel` structure defines two stored type properties to support its functionality. The first, `thresholdLevel`, defines the maximum threshold value an audio level can take. This is a constant value of `10` for all `AudioChannel` instances. If an audio signal comes in with a higher value than `10`, it will be capped to this threshold value (as described below).

The second type property is a variable stored property called `maxInputLevelForAllChannels`. This keeps track of the maximum input value that has been received by  _any_ `AudioChannel` instance. It starts with an initial value of `0`.

The `AudioChannel` structure also defines a stored instance property called `currentLevel`, which represents the channel’s current audio level on a scale of `0` to `10`.

The `currentLevel` property has a `didSet` property observer to check the value of `currentLevel` whenever it is set. This observer performs two checks:

-   If the new value of `currentLevel` is greater than the allowed `thresholdLevel`, the property observer caps `currentLevel` to `thresholdLevel`.
-   If the new value of `currentLevel` (after any capping) is higher than any value previously received by  _any_ `AudioChannel` instance, the property observer stores the new `currentLevel` value in the `maxInputLevelForAllChannels` type property.

Você pode usar a estrutura `AudioChannel` para criar dois novos canais de áudio `leftChannel` e `rightChannel`, para representar os níveis de áudio de um sistema de som estéreo:

```swift
var leftChannel = AudioChannel()
var rightChannel = AudioChannel()
```

If you set the `currentLevel` of the left channel to 7, you can see that the `maxInputLevelForAllChannels` type property is updated to equal 7:

```swift
leftChannel.currentLevel = 7
print(leftChannel.currentLevel)
// Prints "7"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "7"
```

If you try to set the `currentLevel` of the right channel to 11, you can see that the right channel’s `currentLevel` property is capped to the maximum value of 10, and the `maxInputLevelForAllChannels` type property is updated to equal 10:

```swift
rightChannel.currentLevel = 11
print(rightChannel.currentLevel)
// Prints "10"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "10"
```

## Métodos

Os métodos são funções associadas a um tipo específico. Classes, estruturas e enumerações podem definir métodos de instância, que encapsulam tarefas e funcionalidades específicas para trabalhar com uma instância de um determinado tipo. Classes, estruturas e enumerações também podem definir métodos de tipo, que estão associados ao próprio tipo.

### Métodos de instância

Os métodos de instância são funções que pertencem a instâncias de uma determinada classe, estrutura ou enumeração. Eles suportam a funcionalidade dessas instâncias, fornecendo maneiras de acessar e modificar as propriedades da instância, ou fornecendo funcionalidades relacionadas à finalidade da instância. Os métodos de instância têm exatamente a mesma sintaxe que as funções.

```swift
class Counter
{
    var count = 0
    func increment() {
        count += 1
    }

    func increment(by amount: Int) {
        count += amount
    }

    func reset() {
        count = 0
    }
}
```

A classe `Counter` define três métodos de instância:

- `increment()` incrementa o contador por 1.
- `increment(by: Int)` incrementa o contador por um valor inteiro especificado.
- `reset()` reinicia o contador em zero.

Você chama métodos de instância com a mesma sintaxe de ponto como propriedades:

```swift
let counter = Counter()
// the initial counter value is 0

counter.increment()
// the counter's value is now 1

counter.increment(by: 5)
// the counter's value is now 6

counter.reset()
// the counter's value is now 0
```

#### The self Property

Cada instância de um tipo tem uma propriedade implícita chamada `self`, que é exatamente equivalente à própria instância. Você usa a propriedade `self` para se referir à instância atual em seus próprios métodos de instância.

O método `increment()` no exemplo acima poderia ter sido escrito assim:

```swift
func increment() {
    self.count += 1
}
```

A principal exceção a esta regra ocorre quando um nome de parâmetro para um método de instância tem o mesmo nome que uma propriedade dessa instância. Nessa situação, o nome do parâmetro tem precedência e torna-se necessário consultar a propriedade de forma mais qualificada. Você usa a propriedade `self` para distinguir entre o nome do parâmetro e o nome da propriedade.

```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
    }
}

let somePoint = Point(x: 4.0, y: 5.0)

if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// Prints "This point is to the right of the line where x == 1.0"
```

#### Modifying Value Types from Within Instance Methods

Em swift, as classes são do tipo de referência, enquanto as estruturas e enumerações são tipos de valor . As propriedades dos tipos de valor não podem ser modificadas dentro de seus métodos de instância por padrão. Para modificar as propriedades de um tipo de valor, você precisa usar a palavra-chave mutating no método da instância. Com essa palavra-chave, seu método pode ter a capacidade de alterar os valores das propriedades e gravá-lo de volta na estrutura original quando a implementação do método terminar.

**Estruturas e enumerações são tipos de valor . Por padrão, as propriedades de um tipo de valor não podem ser modificadas a partir de seus métodos de instância**.

No entanto, se você precisar modificar as propriedades de sua estrutura ou enumeração dentro de um método específico, você pode optar pelo comportamento mutante desse método. O método pode então mutar(isto é, alterar) suas propriedades dentro do método, e todas as alterações que faz são escritas de volta à estrutura original quando o método termina. O método também pode atribuir uma instância completamente nova à sua propriedade `self` implícita , e esta nova instância substituirá a existente quando o método terminar.

Você pode optar por esse comportamento colocando a palavra-chave `mutating` antes da palavra-chave `func` para esse método:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}

var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```

Observe que você não pode chamar um método de mutação em uma constante de tipo de estrutura, porque suas propriedades não podem ser alteradas, mesmo que sejam propriedades variáveis.

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error
```

#### Assigning to self Within a Mutating Method (Atribuindo-se a si mesmo dentro de um método mutante)

Os métodos mutantes podem atribuir uma instância inteiramente nova à propriedade `self` implícita.

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

Os métodos mutantes para enumerações podem definir o parâmetro `self` implícito como um caso diferente da mesma enumeração:

```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}

var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
```

### Type Methods

Os métodos de instância, conforme descrito acima, são métodos que são chamados de uma instância de um tipo específico. Você também pode definir métodos que são chamados no próprio tipo. Esses tipos de métodos são chamados de métodos de tipo. Você indica métodos de tipo escrevendo a palavra-chave `static` antes da palavra-chave do método `func`. As classes também podem usar a palavra-chave `class` para permitir que as subclasses anulem a implementação da superclasse desse método.

```swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}

SomeClass.someTypeMethod()
```

Within the body of a type method, the implicit `self` property refers to the type itself, rather than an instance of that type. This means that you can use `self` to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.

More generally, any unqualified method and property names that you use within the body of a type method will refer to other type-level methods and properties. A type method can call another type method with the other method’s name, without needing to prefix it with the type name. Similarly, type methods on structures and enumerations can access type properties by using the type property’s name without a type name prefix.

The example below defines a structure called `LevelTracker`, which tracks a player’s progress through the different levels or stages of a game. It is a single-player game, but can store information for multiple players on a single device.

All of the game’s levels (apart from level one) are locked when the game is first played. Every time a player finishes a level, that level is unlocked for all players on the device. The `LevelTracker` structure uses type properties and methods to keep track of which levels of the game have been unlocked. It also tracks the current level for an individual player.

```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1

    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }

    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }

    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```

The `LevelTracker` structure keeps track of the highest level that any player has unlocked. This value is stored in a type property called `highestUnlockedLevel`.

`LevelTracker` also defines two type functions to work with the `highestUnlockedLevel`property. The first is a type function called `unlock(_:)`, which updates the value of `highestUnlockedLevel` whenever a new level is unlocked. The second is a convenience type function called `isUnlocked(_:)`, which returns `true` if a particular level number is already unlocked. (Note that these type methods can access the `highestUnlockedLevel` type property without your needing to write it as `LevelTracker.highestUnlockedLevel`.)

In addition to its type property and type methods, `LevelTracker` tracks an individual player’s progress through the game. It uses an instance property called `currentLevel`to track the level that a player is currently playing.

To help manage the `currentLevel` property, `LevelTracker` defines an instance method called `advance(to:)`. Before updating `currentLevel`, this method checks whether the requested new level is already unlocked. The `advance(to:)` method returns a Boolean value to indicate whether or not it was actually able to set `currentLevel`. Because it’s not necessarily a mistake for code that calls the `advance(to:)` method to ignore the return value, this function is marked with the `@discardableResult` attribute. For more information about this attribute, see  [Attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html).

The `LevelTracker` structure is used with the `Player` class, shown below, to track and update the progress of an individual player:

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```

The `Player` class creates a new instance of `LevelTracker` to track that player’s progress. It also provides a method called `complete(level:)`, which is called whenever a player completes a particular level. This method unlocks the next level for all players and updates the player’s progress to move them to the next level. (The Boolean return value of `advance(to:)` is ignored, because the level is known to have been unlocked by the call to `LevelTracker.unlock(_:)` on the previous line.)

You can create an instance of the `Player` class for a new player, and see what happens when the player completes level one:

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
```

If you create a second player, whom you try to move to a level that is not yet unlocked by any player in the game, the attempt to set the player’s current level fails:

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// Prints "level 6 has not yet been unlocked"
```

## Subscripts

Classes, structures, and enumerations can define subscripts, which are shortcuts for accessing the member elements of a collection, list, or sequence. You use subscripts to set and retrieve values by index without needing separate methods for setting and retrieval. For example, you access elements in an `Array` instance as `someArray[index]` and elements in a `Dictionary` instance as `someDictionary[key]`.

You can define multiple subscripts for a single type, and the appropriate subscript overload to use is selected based on the type of index value you pass to the subscript. Subscripts are not limited to a single dimension, and you can define subscripts with multiple input parameters to suit your custom type’s needs.

### Sintaxe do Subscripts

Os subscript permitem consultar instâncias de um tipo escrevendo um ou mais valores entre colchetes após o nome da instância. Sua sintaxe é semelhante à sintaxe do método da instância e à sintaxe da propriedade calculada. Você escreve definições de subscript com a palavra-chave `subscript` e especifica um ou mais parâmetros de entrada e um tipo de retorno, da mesma forma que os métodos de instância. Ao contrário dos métodos de instância, os índices podem ser lidos ou escritos ou somente leitura. Esse comportamento é comunicado por um `getter` e `setter` da mesma maneira que para propriedades calculadas:

```swift
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
    }
}
```

Tal como acontece com as propriedades calculadas somente de leitura, você pode simplificar a declaração de um subscript somente de leitura removendo a palavra-chave `get` e as chaves:

```swift
subscript(index: Int) -> Int {
    // return an appropriate subscript value here
}
```

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}

let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// Prints "six times three is 18"
```

### Subscript Usage

O significado exato de "subscript" depende do contexto em que é usado. Os subscripts geralmente são usados ​​como um atalho para acessar os elementos do membro em uma coleção, lista ou seqüência. Você é livre para implementar subscripts da maneira mais apropriada para sua classe específica ou a funcionalidade da estrutura.

Por exemplo, o tipo `Dictionary` de Swift implementa um subscript para definir e recuperar os valores armazenados em uma `Dictionary` instância.

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

The example above defines a variable called `numberOfLegs` and initializes it with a dictionary literal containing three key-value pairs. The type of the `numberOfLegs`dictionary is inferred to be `[String:  Int]`. After creating the dictionary, this example uses subscript assignment to add a `String` key of `"bird"` and an `Int` value of `2` to the dictionary.

_Swift’s Dictionary type implements its key-value subscripting as a subscript that takes and returns an optional type. For the numberOfLegs dictionary above, the key-value subscript takes and returns a value of type Int?, or “optional int”. The Dictionary type uses an optional subscript type to model the fact that not every key will have a value, and to give a way to delete a value for a key by assigning a nil value for that key._

### Subscript Options

As subscripts podem levar qualquer número de parâmetros de entrada, e esses parâmetros de entrada podem ser de qualquer tipo. Os subscripts também podem retornar qualquer tipo. Os subscripts podem usar parâmetros variadic, mas não podem usar parâmetros de saída(in-out) ou fornecer valores de parâmetro padrão.

Uma classe ou estrutura pode fornecer tantas implementações de subscript quanto ela precisa, e o subscript apropriado a ser usado será inferido com base nos tipos de valores ou valores que estão contidos nos suportes do subscript no ponto em que o subscript é usado. Esta definição de subscritos múltiplos é conhecida como sobrecarga de subscript.

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```

```swift
var matrix = Matrix(rows: 2, columns: 2)
```

Os valores na matriz podem ser definidos passando valores de linha e coluna para o subscrito, separados por uma vírgula:

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

The `Matrix` subscript’s getter and setter both contain an assertion to check that the subscript’s `row` and `column` values are valid. To assist with these assertions, `Matrix`includes a convenience method called `indexIsValid(row:column:)`, which checks whether the requested `row` and `column` are inside the bounds of the matrix:

```swift
func indexIsValid(row: Int, column: Int) -> Bool {
    return row >= 0 && row < rows && column >= 0 && column < columns
}
```

## Inheritance (Herança)

Uma classe pode herdar métodos, propriedades e outras características de outra classe. Quando uma classe herda de outra, a classe herdada é conhecida como uma subclasse e a classe que ela herda é conhecida como sua superclasse. A herança é um comportamento fundamental que diferencia classes de outros tipos em Swift.

Classes em Swift podem chamar e acessar métodos, propriedades e subíndices pertencentes à sua superclasse e podem fornecer suas próprias versões primárias desses métodos, propriedades e subíndices para refinar ou modificar seu comportamento. O Swift ajuda a garantir que suas substituições sejam corretas ao verificar se a definição de substituição possui uma definição de superclasse correspondente.

As classes também podem adicionar observadores de propriedade para propriedades herdadas para serem notificadas quando o valor de uma propriedade mudar. Os observadores de propriedade podem ser adicionados a qualquer propriedade, independentemente de ter sido originalmente definido como uma propriedade armazenada ou calculada.

### Definindo uma classe base

Qualquer classe que não herda de outra classe é conhecida como uma classe base.

_As classes Swift não herdam de uma classe base universal. As classes que você define sem especificar uma superclasse se tornam automaticamente classes básicas para você construir._

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}
```

Você cria uma nova instância `Vehicle` com a sintaxe do inicializador, que é escrita como um nome de tipo seguido por parênteses vazios:

```swift
let someVehicle = Vehicle()
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```

### Subclassing

A subclasse é o ato de basear uma nova classe em uma classe existente. A subclasse herda as características da classe existente, que você pode refinar. Você também pode adicionar novas características à subclasse.

Para indicar que uma subclasse possui uma superclasse, escreva o nome da subclasse antes do nome da superclasse, separados por dois pontos:

```swift
class SomeSubclass: SomeSuperclass {
    // subclass definition goes here
}
```

O exemplo a seguir define uma subclasse chamada `Bicycle`, com uma superclasse de `Vehicle`:

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```

A nova classe `Bicycle` gera automaticamente todas as características de `Vehicle`.

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true

bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

Subclasses can themselves be subclassed. The next example creates a subclass of `Bicycle` for a two-seater bicycle known as a “tandem”:

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```

`Tandem` herda todas as propriedades e métodos de `Bicycle`, que, por sua vez, herdam todas as propriedades e métodos de `Vehicle`. A `Tandem` subclasse também adiciona uma nova propriedade armazenada chamada `currentNumberOfPassengers`, com um valor padrão de `0`.

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```

### Overriding

A subclass can provide its own custom implementation of an instance method, type method, instance property, type property, or subscript that it would otherwise inherit from a superclass. This is known as *overriding*.

Para substituir uma característica que de outra forma seria herdada, você prefixa sua definição de substituição com a palavra-chave `override`. Isso deixa claro que você pretende fornecer uma substituição e não forneceu uma definição correspondente por engano. Substituir por acidente pode causar um comportamento inesperado, e quaisquer substituições sem a palavra-chave `override` são diagnosticadas como um erro quando seu código é compilado.

#### Accessing Superclass Methods, Properties, and Subscripts

Where this is appropriate, you access the superclass version of a method, property, or subscript by using the `super` prefix:

* An overridden method named `someMethod()` can call the superclass version of `someMethod()` by calling `super.someMethod()` within the overriding method implementation.
* An overridden property called `someProperty` can access the superclass version of `someProperty` as `super.someProperty` within the overriding getter or setter implementation.
* An overridden subscript for `someIndex` can access the superclass version of the same subscript as `super[someIndex]` from within the overriding subscript implementation.

#### Substituindo Métodos

Você pode substituir um método de instância ou tipo herdado para fornecer uma implementação personalizada ou alternativa do método dentro de sua subclasse.

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}

let train = Train()
train.makeNoise()
// Prints "Choo Choo"
```

#### Substituindo Propriedades

You can override an inherited instance or type property to provide your own custom getter and setter for that property, or to add property observers to enable the overriding property to observe when the underlying property value changes.

##### Substituindo propriedade Getters e Setters

You can provide a custom getter (and setter, if appropriate) to override any inherited property, regardless of whether the inherited property is implemented as a stored or computed property at source. The stored or computed nature of an inherited property is not known by a subclass—it only knows that the inherited property has a certain name and type. You must always state both the name and the type of the property you are overriding, to enable the compiler to check that your override matches a superclass property with the same name and type.

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}

let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```

##### Overriding Property Observers

Você pode usar a substituição de propriedades para adicionar observadores de propriedade a uma propriedade herdada. Isso permite que você seja notificado quando o valor de uma propriedade herdada for alterado, independentemente de como essa propriedade foi implementada originalmente.

_Você não pode adicionar observadores de propriedade a propriedades armazenadas constantes herdadas ou propriedades computadas somente leitura herdadas. O valor dessas propriedades não pode ser definido e, portanto, não é apropriado fornecer uma `willSet` ou uma `didSet` implementação como parte de uma substituição._

```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}

let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```

### Preventing Overrides

You can prevent a method, property, or subscript from being overridden by marking it as `final`. Do this by writing the `final` modifier before the method, property, or subscript’s introducer keyword (such as `final var`, `final func`, `final class func`, and `final subscript`).

## Initialization

A inicialização é o processo de preparação de uma instância de uma classe, estrutura ou enumeração para uso. Esse processo envolve a configuração de um valor inicial para cada propriedade armazenada nessa instância e a realização de qualquer outra configuração ou inicialização necessária para que a nova instância esteja pronta para uso.

### Definir valores iniciais para propriedades armazenadas

Classes e estruturas devem definir todas as suas propriedades armazenadas como um valor inicial apropriado até o momento em que uma instância dessa classe ou estrutura for criada. As propriedades armazenadas não podem ser deixadas em um estado indeterminado.

Você pode definir um valor inicial para uma propriedade armazenada dentro de um inicializador ou atribuir um valor de propriedade padrão como parte da definição da propriedade.

_Quando você atribui um valor padrão a uma propriedade armazenada ou define seu valor inicial dentro de um inicializador, o valor dessa propriedade é definido diretamente, sem chamar nenhum observador de propriedade._

#### Inicializadores

Os inicializadores são chamados para criar uma nova instância de um tipo específico. Na sua forma mais simples, um inicializador é como um método de instância sem parâmetros, escrito usando a palavra-chave `init`:

```swift
init() {
    // perform some initialization here
}
```

The example below defines a new structure called `Fahrenheit` to store temperatures expressed in the Fahrenheit scale. The `Fahrenheit` structure has one stored property, `temperature`, which is of type `Double`:

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// Prints "The default temperature is 32.0° Fahrenheit"
```

The structure defines a single initializer, `init`, with no parameters, which initializes the stored temperature with a value of `32.0` (the freezing point of water in degrees Fahrenheit).

#### Valores de propriedade padrão

Você pode definir o valor inicial de uma propriedade armazenada dentro de um inicializador, como mostrado acima. Como alternativa, especifique um valor de propriedade padrão como parte da declaração da propriedade. Você especifica um valor de propriedade padrão atribuindo um valor inicial à propriedade quando é definido.

```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

###  Personalizando a Inicialização

Você pode personalizar o processo de inicialização com parâmetros de entrada e tipos de propriedades opcionais ou atribuindo propriedades constantes durante a inicialização, conforme descrito nas seções a seguir.

#### Parâmetros de inicialização

You can provide *initialization parameters* as part of an initializer’s definition, to define the types and names of values that customize the initialization process. Initialization parameters have the same capabilities and syntax as function and method parameters.

The following example defines a structure called `Celsius`, which stores temperatures expressed in degrees Celsius. The `Celsius` structure implements two custom initializers called `init(fromFahrenheit:)` and `init(fromKelvin:)`, which initialize a new instance of the structure with a value from a different temperature scale:

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}

let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0

let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```

#### Parameter Names and Argument Labels

As with function and method parameters, initialization parameters can have both a parameter name for use within the initializer’s body and an argument label for use when calling the initializer.

However, initializers do not have an identifying function name before their parentheses in the way that functions and methods do. Therefore, the names and types of an initializer’s parameters play a particularly important role in identifying which initializer should be called. Because of this, Swift provides an automatic argument label for every parameter in an initializer if you don’t provide one.

```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}
```

Both initializers can be used to create a new `Color` instance, by providing named values for each initializer parameter:

```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

Observe que não é possível chamar esses inicializadores sem usar argument labels. Os argument labels sempre devem ser usados ​​em um inicializador se estiverem definidos e, omitindo-os, é um erro de tempo de compilação:

```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// this reports a compile-time error - argument labels are required
```

#### Initializer Parameters Without Argument Labels

If you do not want to use an argument label for an initializer parameter, write an underscore (`_`) instead of an explicit argument label for that parameter to override the default behavior.

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}
```

```swift
let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius is 37.0
```

The initializer call `Celsius(37.0)` is clear in its intent without the need for an argument label. It is therefore appropriate to write this initializer as `init(_  celsius:  Double)` so that it can be called by providing an unnamed `Double` value.

#### Optional Property Types

If your custom type has a stored property that is logically allowed to have “no value” — perhaps because its value cannot be set during initialization, or because it is allowed to have “no value” at some later point—declare the property with an optional type. Properties of optional type are automatically initialized with a value of `nil`, indicating that the property is deliberately intended to have “no value yet” during initialization.

```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}

let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// Prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```

The response to a survey question cannot be known until it is asked, and so the `response` property is declared with a type of `String?`, or “optional `String`”. It is automatically assigned a default value of `nil`, meaning “no string yet”, when a new instance of `SurveyQuestion` is initialized.

#### Assigning Constant Properties During Initialization

You can assign a value to a constant property at any point during initialization, as long as it is set to a definite value by the time initialization finishes. Once a constant property is assigned a value, it can’t be further modified.

You can revise the `SurveyQuestion` example from above to use a constant property rather than a variable property for the `text` property of the question, to indicate that the question does not change once an instance of `SurveyQuestion` is created. Even though the `text` property is now a constant, it can still be set within the class’s initializer:

```swift
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}

let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// Prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```

### Default Initializers

Swift provides a *default initializer* for any structure or class that provides default values for all of its properties and does not provide at least one initializer itself. The default initializer simply creates a new instance with all of its properties set to their default values.

This example defines a class called `ShoppingListItem`, which encapsulates the name, quantity, and purchase state of an item in a shopping list:

```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```

Because all properties of the `ShoppingListItem` class have default values, and because it is a base class with no superclass, `ShoppingListItem` automatically gains a default initializer implementation that creates a new instance with all of its properties set to their default values. (The `name` property is an optional `String` property, and so it automatically receives a default value of `nil`, even though this value is not written in the code.) The example above uses the default initializer for the `ShoppingListItem` class to create a new instance of the class with initializer syntax, written as `ShoppingListItem()`, and assigns this new instance to a variable called `item`.

#### Memberwise Initializers for Structure Types

Structure types automatically receive a memberwise initializer if they do not define any of their own custom initializers. Unlike a default initializer, the structure receives a memberwise initializer even if it has stored properties that do not have default values.

The memberwise initializer is a shorthand way to initialize the member properties of new structure instances. Initial values for the properties of the new instance can be passed to the memberwise initializer by name.

```swift
struct Size {
    var width = 0.0, height = 0.0
}

let twoByTwo = Size(width: 2.0, height: 2.0)
```

### Initializer Delegation for Value Types

Os inicializadores podem chamar outras inicializações para executar parte da inicialização de uma instância. Esse processo, conhecido como delegação de inicialização, evita duplicar o código em vários inicializadores.

As regras de como a delegação do inicializador funciona e para quais formas de delegação são permitidas são diferentes para os tipos de valor e classe. Os tipos de valor (estruturas e enumerações) não suportam herança e, portanto, o processo de delegação do inicializador é relativamente simples, porque eles podem apenas delegar a outro inicializador que eles próprios fornecem. No entanto, as classes podem herdar de outras classes, conforme descrito em Herança. Isso significa que as classes têm responsabilidades adicionais para garantir que todas as propriedades armazenadas que elas herdam recebem um valor adequado durante a inicialização. Essas responsabilidades são descritas em Herança e Inicialização de Classe abaixo.

For value types, you use `self.init` to refer to other initializers from the same value type when writing your own custom initializers. You can call `self.init` only from within an initializer.

Observe que, se você definir um inicializador personalizado para um tipo de valor, não terá mais acesso ao inicializador padrão (ou ao inicializador memberwise, se for uma estrutura) para esse tipo. Essa restrição evita uma situação na qual a configuração essencial adicional fornecida em um inicializador mais complexo é acidentalmente contornada por alguém usando um dos inicializadores automáticos.

```swift
struct Size {
    var width = 0.0, height = 0.0
}

struct Point {
    var x = 0.0, y = 0.0
}

struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

O primeiro inicializador `Rect`, `init()` é funcionalmente o mesmo que o inicializador padrão que a estrutura teria recebido se não tivesse seus próprios inicializadores personalizados. Este inicializador tem um corpo vazio, representado por um par vazio de chaves `{}`. Chamar esse inicializador retorna uma `Rect` instância cujas propriedades `origin` e `size` are both initialized with the default values of `Point(x: 0.0, y: 0.0)` and `Size(width: 0.0, height: 0.0)` from their property definitions.

```swift
let basicRect = Rect()
// basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)
```

O segundo inicializador `Rect`, `init(origin:size:)` é funcionalmente o mesmo que o inicializador memberwise que a estrutura teria recebido se não tivesse seus próprios inicializadores personalizados. Esse inicializador simplesmente atribui os valores `origin` e `size` argumentos às propriedades armazenadas apropriadas:

```swift
let originRect = Rect(origin: Point(x: 2.0, y: 2.0), size: Size(width: 5.0, height: 5.0))
// originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)
```

O terceiro inicializador `Rect`, `init(center:size:)` é um pouco mais complexo. Ele começa calculando um ponto de origem apropriado com base em um `center` ponto e um `size` valor. Em seguida, chama (ou delega ) o inicializador `init(origin:size:)`, que armazena os novos valores de origem e tamanho nas propriedades apropriadas:

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0), size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

### Class Inheritance and Initialization

**Todas as propriedades armazenadas de uma classe - incluindo quaisquer propriedades que a classe herda de sua superclasse - devem ter um valor inicial durante a inicialização.**

Swift define dois tipos de inicializações para os tipos de classe para ajudar a garantir que todas as propriedades armazenadas recebam um valor inicial. Estes são conhecidos como inicializadores designados e inicializadores de conveniência.

#### Designated Initializers and Convenience Initializers

**Os inicializadores designados são os inicializadores principais para uma classe**. Um inicializador designado inicializa completamente todas as propriedades introduzidas por essa classe e chama um inicializador de superclasse apropriado para continuar o processo de inicialização na cadeia de superclasse.

**Os inicializadores de conveniência são secundários, suportando inicializações para uma classe**. Você pode definir um iniciador de conveniência para chamar um inicializador designado da mesma classe que o iniciador de conveniência com alguns dos parâmetros designados do iniciador definidos como valores padrão. Você também pode definir um inicializador de conveniência para criar uma instância dessa classe para um caso de uso específico ou tipo de valor de entrada.

Você não precisa fornecer inicializações de conveniência se a sua classe não as exigir. Crie inicializadores de conveniência sempre que um atalho para um padrão de inicialização comum economize tempo ou torne a inicialização da classe mais clara na intenção.

#### Sintaxe para inicializadores designados e de conveniência

Os inicializadores designados para classes são escritos da mesma forma que os inicializadores simples para tipos de valor:

```
init([parameters]) {
    [statements]
}
```

Convenience initializers are written in the same style, but with the `convenience` modifier placed before the `init` keyword, separated by a space:

```
convenience init([parameters]) {
    [statements]
}
```

#### Initializer Delegation for Class Types

Para simplificar as relações entre inicializadores designados e de conveniência, Swift aplica as três regras a seguir para chamadas de delegação entre inicializadores:

**Regra 1**
    Um inicializador designado deve chamar um inicializador designado de sua superclasse imediata.

**Regra 2**
    Um iniciador de conveniência deve chamar outro inicializador da mesma classe.

**Regra 3**
    Inicialmente, um inicializador de conveniência deve chamar um inicializador designado.

Uma maneira simples de lembrar isso é:

* Designated initializers must always delegate up.
* Convenience initializers must always delegate across.

![Initializer Delegation](./Images/InitializerDelegation01.png "Initializer Delegation")

Aqui, a superclasse possui um único inicializador designado e dois inicializadores de conveniência. Um inicializador de conveniência chama outro iniciador de conveniência, que por sua vez chama o inicializador designado. Isso satisfaz as regras 2 e 3 de cima. A superclasse não possui uma superclasse adicional e, portanto, a regra 1 não se aplica.

A subclasse nesta figura tem dois inicializadores designados e um inicializador de conveniência. O iniciador de conveniência deve chamar um dos dois inicializadores designados, porque ele só pode chamar outro inicializador da mesma classe. Isso satisfaz as regras 2 e 3 de cima. Ambos os inicializadores designados devem chamar o inicializador designado único da superclasse, para satisfazer a regra 1 de cima.

*Essas regras não afetam a forma como os usuários de suas classes criam instâncias de cada classe. Qualquer inicialização no diagrama acima pode ser usada para criar uma instância totalmente inicializada da classe a que pertencem. As regras apenas afetam a forma como você escreve a implementação dos inicializadores da classe.*

The figure below shows a more complex class hierarchy for four classes. It illustrates how the designated initializers in this hierarchy act as “funnel” points for class initialization, simplifying the interrelationships among classes in the chain:

![Initializer Delegation](./Images/InitializerDelegation02.png "Initializer Delegation")

#### Inicialização em duas fases

A inicialização de classe no Swift é um processo em duas fases. Na primeira fase, cada propriedade armazenada recebe um valor inicial pela classe que a introduziu. Uma vez que o estado inicial para cada propriedade armazenada foi determinado, a segunda fase começa e cada classe tem a oportunidade de personalizar suas propriedades armazenadas ainda antes da nova instância ser considerada pronta para uso.

O compilador do Swift executa quatro verificações de segurança úteis para garantir que a inicialização em duas fases seja concluída sem erro:

**Verificação de segurança 1**
    Um inicializador designado deve garantir que todas as propriedades introduzidas pela sua classe sejam inicializadas antes de delegar até um inicializador de superclasse.

Conforme mencionado acima, a memória de um objeto só é considerada totalmente inicializada quando o estado inicial de todas as suas propriedades armazenadas é conhecido. Para que esta regra seja satisfeita, um inicializador designado deve garantir que todas as suas propriedades sejam inicializadas antes de hands off up the chain.

**Verificação de segurança 2**
    Um inicializador designado deve delegar até um inicializador de superclasse antes de atribuir um valor a uma propriedade herdada. Caso contrário, o novo valor designado pelo iniciador será substituído pela superclasse como parte de sua própria inicialização.

**Verificação de segurança 3**
    Um iniciador de conveniência deve delegar para outro inicializador antes de atribuir um valor a qualquer propriedade (incluindo propriedades definidas pela mesma classe). Se não, o novo valor que a inicialização de conveniência atribuir será substituído pelo inicializador designado da própria classe.

**Verificação de segurança 4**
    Um inicializador não pode chamar nenhum método de instância, ler os valores de quaisquer propriedades da instância ou se referir `self` como um valor até que a primeira fase de inicialização seja concluída.

A instância da classe não é totalmente válida até a primeira fase terminar. As propriedades só podem ser acessadas, e os métodos só podem ser chamados, uma vez que a instância da classe é conhecida como válida no final da primeira fase.

Veja como funciona a inicialização em duas fases, com base nas quatro verificações de segurança acima:

**Fase 1**
    * Um inicializador designado ou de conveniência é chamado em uma classe.
    * A memória para uma nova instância dessa classe é alocada. A memória ainda não foi inicializada.
    * Um inicializador designado para essa classe confirma que todas as propriedades armazenadas introduzidas por essa classe possuem um valor. A memória dessas propriedades armazenadas agora está inicializada.
    *O inicializador designado é transferido para um inicializador de superclasse para executar a mesma tarefa para suas próprias propriedades armazenadas.
    * Isso continua até a cadeia de herança da classe até chegar ao topo da cadeia.
    * Uma vez que o topo da cadeia é alcançado e a classe final na cadeia garantiu que todas as suas propriedades armazenadas tenham um valor, a memória da instância é considerada totalmente inicializada e a fase 1 está completa.

**Fase 2**
    * (Retornando) Trabalhando de volta para baixo do topo da cadeia, cada inicializador designado na cadeia tem a opção de personalizar a instância mais. Os inicializadores agora podem acessar `self` e podem modificar suas propriedades, chamar seus métodos de instância e assim por diante.
    * Finalmente, qualquer inicialização de conveniência na cadeia tem a opção de personalizar a instância e trabalhar com ela `self`.

#### Herança e substituição do inicializador

As subclasses Swift não herdam seus inicializadores de superclasse por padrão. A abordagem do Swift evita uma situação em que um inicializador simples de uma superclasse é herdado por uma subclasse mais especializada e é usado para criar uma nova instância da subclasse que não foi totalmente ou inicializada corretamente.

Se você deseja que uma subclasse personalizada apresente um ou mais dos mesmos inicializadores como sua superclasse, você pode fornecer uma implementação personalizada desses inicializadores dentro da subclasse.

Quando você escreve um inicializador de subclasse que corresponde a um inicializador designado de superclasse, você está efetivamente fornecendo uma substituição desse inicializador designado. Portanto, você deve escrever o modificador `override` antes da definição da inicialização da subclasse. Isso é verdade mesmo se você estiver substituindo um inicializador padrão fornecido automaticamente, conforme descrito em Inicializadores Padrão.

Tal como acontece com uma propriedade, método ou subíndice substituído, a presença do modificador `override` solicita Swift para verificar se a superclasse possui um inicializador designado correspondente e é validado que os parâmetros para o inicializador primário foram especificados como pretendido.

Por outro lado, se você escrever um inicializador de subclasse que corresponda a um inicializador de conveniência da superclasse, esse inicializador de conveniência da superclasse nunca poderá ser chamado diretamente por sua subclasse, conforme as regras descritas acima na Delegação de Inicializador para Tipos de Classe. Portanto, sua subclasse não está (estritamente falando) fornecendo uma substituição do inicializador da superclasse. Como resultado, você não escreve o overridemodificador ao fornecer uma implementação correspondente de um inicializador de conveniência da superclasse.

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
```

```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
```

O próximo exemplo define uma subclasse de `Vehicle` chamada `Bicycle`:

```swift
class Bicycle: Vehicle {
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
```

```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
```

*Subclasses can modify inherited variable properties during initialization, but can not modify inherited constant properties.*

#### Automatic Initializer Inheritance

Como mencionado acima, as subclasses não herdam seus inicializadores de superclasse por padrão. No entanto, os inicializadores da superclasse são automaticamente herdados se determinadas condições forem atendidas. Na prática, isso significa que você não precisa escrever as substituições do inicializador em muitos cenários comuns e pode herdar seus inicializadores de superclasse com o mínimo de esforço sempre que for seguro fazê-lo.

Supondo que você forneça valores padrão para quaisquer novas propriedades introduzidas em uma subclasse, as duas regras a seguir se aplicam:

**Regra 1**

Se sua subclasse não definir nenhum inicializador designado, ele herda automaticamente todos os inicializadores designados pela superclasse.

**Regra 2**

Se sua subclasse fornecer uma implementação de todos os inicializadores designados pela superclasse - seja herdando-os conforme a regra 1 ou fornecendo uma implementação customizada como parte de sua definição - então ela herda automaticamente todos os inicializadores de conveniência da superclasse.

Essas regras se aplicam mesmo que sua subclasse adicione outros inicializadores de conveniência.

#### Designated and Convenience Initializers in Action

```swift
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}
```

A figura abaixo mostra a cadeia de inicialização para a classe `Food`:


![Initializers Example01](./Images/InitializersExample01.png "Initializers Example01")

As classes não têm um inicializador de membros padrão, portanto, a classe de `Food` fornece um inicializador designado que leva um único argumento chamado `name`. Este inicializador pode ser usado para criar uma nova instância `Food` com um nome específico:

```swift
let namedMeat = Food(name: "Bacon")
// namedMeat's name is "Bacon"
```

O inicializador `init(name: String)` da classe `Food` é fornecido como um inicializador designado, porque assegura que todas as propriedades armazenadas de uma nova instância `Food` sejam totalmente inicializadas. A classe `Food` não possui uma superclasse e, portanto, o inicializador `init(name: String)` não precisa chamar `super.init()` para completar sua inicialização.

A classe `Food` também fornece um inicializador de conveniência `init()`, sem argumentos.

```swift
let mysteryMeat = Food()
// mysteryMeat's name is "[Unnamed]"
```

A segunda classe na hierarquia é uma subclasse de `Food` chamada `RecipeIngredient`. A classe `RecipeIngredient` modela um ingrediente em uma receita de cozinha.

```swift
class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    override convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}
```

A figura abaixo mostra a cadeia de inicialização para a classe `RecipeIngredient`:

![Initializers Example02](./Images/InitializersExample02.png "Initializers Example02")

Neste exemplo, a superclasse para `RecipeIngredient` é `Food`, que tem um único inicializador de conveniência chamado `init()`. Este inicializador é, portanto, herdado por `RecipeIngredient`. A versão herdada das funções `init()` exatamente da mesma forma que a versão de `Food`,  except that it delegates to the `RecipeIngredient` version of `init(name: String)` rather than the `Food` version.

Os três desses inicializadores podem ser usados ​​para criar novas `RecipeIngredient` instâncias:

```swift
let oneMysteryItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
```

A terceira e última classe na hierarquia é uma subclasse de `RecipeIngredient` chamada `ShoppingListItem`. A classe `ShoppingListItem` modela um ingrediente de receita tal como aparece em uma lista de compras.

Cada item na lista de compras começa como "não comprado". Para representar esse fato, `ShoppingListItem` apresenta uma propriedade booleana chamada `purchased`, com um valor padrão de `false`. `ShoppingListItem` adiciona também uma propriedade calculada `description`, que fornece uma descrição textual de uma instância de `ShoppingListItem`:

```swift
class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
}
```

Como fornece um valor padrão para todas as propriedades que ele apresenta e não define nenhum inicializador, `ShoppingListItem` herda automaticamente todos os inicializadores designados e de conveniência de sua superclasse.

A figura abaixo mostra a cadeia de inicialização geral para as três classes:

![Initializers Example03](./Images/InitializersExample03.png "Initializers Example03")

Você pode usar os três inicializadores herdados para criar uma nova instância de `ShoppingListItem`:

```swift
var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6),
]

breakfastList[0].name = "Orange juice"
breakfastList[0].purchased = true

for item in breakfastList {
    print(item.description)
}

// 1 x Orange juice ✔
// 1 x Bacon ✘
// 6 x Eggs ✘
```

### Failable Initializers

Às vezes, é útil definir uma classe, estrutura ou enumeração para a qual a inicialização pode falhar. Essa falha pode ser desencadeada por valores de parâmetro de inicialização inválidos, a ausência de um recurso externo necessário ou alguma outra condição que impede a inicialização de sucesso.

To cope with initialization conditions that can fail, define one or more failable initializers as part of a class, structure, or enumeration definition. You write a failable initializer by placing a question mark after the init keyword (`init?`).

*You cannot define a failable and a nonfailable initializer with the same parameter types and names.*

A failable initializer creates an *optional* value of the type it initializes. You write `return nil` within a failable initializer to indicate a point at which initialization failure can be triggered.

For instance, failable initializers are implemented for numeric type conversions. To ensure conversion between numeric types maintains the value exactly, use the `init(exactly:)` initializer. If the type conversion cannot maintain the value, the initializer fails.

```swift
let wholeNumber: Double = 12345.0
let pi = 3.14159

if let valueMaintained = Int(exactly: wholeNumber) {
    print("\(wholeNumber) conversion to Int maintains value of \(valueMaintained)")
}
// Prints "12345.0 conversion to Int maintains value of 12345"

let valueChanged = Int(exactly: pi)
// valueChanged is of type Int?, not Int

if valueChanged == nil {
    print("\(pi) conversion to Int does not maintain value")
}
// Prints "3.14159 conversion to Int does not maintain value"
```

Exemplo:

```swift
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}
```

You can use this failable initializer to try to initialize a new `Animal` instance and to check if initialization succeeded:

```swift
let someCreature = Animal(species: "Giraffe")
// someCreature is of type Animal?, not Animal

if let giraffe = someCreature {
    print("An animal was initialized with a species of \(giraffe.species)")
}
// Prints "An animal was initialized with a species of Giraffe"
```

If you pass an empty string value to the failable initializer’s `species` parameter, the initializer triggers an initialization failure:

```swift
let anonymousCreature = Animal(species: "")
// anonymousCreature is of type Animal?, not Animal

if anonymousCreature == nil {
    print("The anonymous creature could not be initialized")
}
// Prints "The anonymous creature could not be initialized"
```

#### Failable Initializers for Enumerations

You can use a failable initializer to select an appropriate enumeration case based on one or more parameters. The initializer can then fail if the provided parameters do not match an appropriate enumeration case.

```swift
enum TemperatureUnit {
    case kelvin, celsius, fahrenheit
    init?(symbol: Character) {
        switch symbol {
        case "K":
            self = .kelvin
        case "C":
            self = .celsius
        case "F":
            self = .fahrenheit
        default:
            return nil
        }
    }
}
```

You can use this failable initializer to choose an appropriate enumeration case for the three possible states and to cause initialization to fail if the parameter does not match one of these states:

```swift
let fahrenheitUnit = TemperatureUnit(symbol: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// Prints "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(symbol: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// Prints "This is not a defined temperature unit, so initialization failed."
```

#### Propagation of Initialization Failure

A failable initializer of a class, structure, or enumeration can delegate across to another failable initializer from the same class, structure, or enumeration. Similarly, a subclass failable initializer can delegate up to a superclass failable initializer.

In either case, if you delegate to another initializer that causes initialization to fail, the entire initialization process fails immediately, and no further initialization code is executed.

```swift
class Product {
    let name: String
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}

class CartItem: Product {
    let quantity: Int
    init?(name: String, quantity: Int) {
        if quantity < 1 { return nil }
        self.quantity = quantity
        super.init(name: name)
    }
}
```

```swift
if let twoSocks = CartItem(name: "sock", quantity: 2) {
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}
// Prints "Item: sock, quantity: 2"
```

```swift
if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
    print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
} else {
    print("Unable to initialize zero shirts")
}
// Prints "Unable to initialize zero shirts"
```

```swift
if let oneUnnamed = CartItem(name: "", quantity: 1) {
    print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
} else {
    print("Unable to initialize one unnamed product")
}
// Prints "Unable to initialize one unnamed product"
```

#### Overriding a Failable Initializer

You can override a superclass failable initializer in a subclass, just like any other initializer. Alternatively, you can override a superclass failable initializer with a subclass nonfailable initializer. This enables you to define a subclass for which initialization cannot fail, even though initialization of the superclass is allowed to fail.

Note that if you override a failable superclass initializer with a nonfailable subclass initializer, the only way to delegate up to the superclass initializer is to force-unwrap the result of the failable superclass initializer.

```swift
class Document {
    var name: String?
    // this initializer creates a document with a nil name value
    init() {}
    // this initializer creates a document with a nonempty name value
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}

class AutomaticallyNamedDocument: Document {
    override init() {
        super.init()
        self.name = "[Untitled]"
    }

    override init(name: String) {
        super.init()
        if name.isEmpty {
            self.name = "[Untitled]"
        } else {
            self.name = name
        }
    }
}
```

You can use forced unwrapping in an initializer to call a failable initializer from the superclass as part of the implementation of a subclass’s nonfailable initializer. For example, the `UntitledDocument` subclass below is always named `"[Untitled]"`, and it uses the failable `init(name:)` initializer from its superclass during initialization.

```swift
class UntitledDocument: Document {
    override init() {
        super.init(name: "[Untitled]")!
    }
}
```

In this case, if the `init(name:)` initializer of the superclass were ever called with an empty string as the name, the forced unwrapping operation would result in a runtime error. However, because it’s called with a string constant, you can see that the initializer won’t fail, so no runtime error can occur in this case.

#### The init! Failable Initializer

You typically define a failable initializer that creates an optional instance of the appropriate type by placing a question mark after the `init` keyword (`init?`). Alternatively, you can define a failable initializer that creates an implicitly unwrapped optional instance of the appropriate type. Do this by placing an exclamation mark after the `init` keyword (`init!`) instead of a question mark.

You can delegate from `init?` to `init!` and vice versa, and you can override `init?` with `init!` and vice versa. You can also delegate from `init` to `init!`, although doing so will trigger an assertion if the `init!` initializer causes initialization to fail.

### Required Initializers

Escreva o modificador `required` antes da definição de um inicializador de classe para indicar que cada subclasse da classe deve implementar esse inicializador:

```swift
class SomeClass {
    required init() {
        // initializer implementation goes here
    }
}
```

You must also write the `required` modifier before every subclass implementation of a required initializer, to indicate that the initializer requirement applies to further subclasses in the chain. You do not write the `override` modifier when overriding a required designated initializer:

```swift
class SomeSubclass: SomeClass {
    required init() {
        // subclass implementation of the required initializer goes here
    }
}
```

*You do not have to provide an explicit implementation of a required initializer if you can satisfy the requirement with an inherited initializer.*

### Setting a Default Property Value with a Closure or Function

If a stored property’s default value requires some customization or setup, you can use a closure or global function to provide a customized default value for that property. Whenever a new instance of the type that the property belongs to is initialized, the closure or function is called, and its return value is assigned as the property’s default value.

These kinds of closures or functions typically create a temporary value of the same type as the property, tailor that value to represent the desired initial state, and then return that temporary value to be used as the property’s default value.

Here’s a skeleton outline of how a closure can be used to provide a default property value:

```swift
class SomeClass {
    let someProperty: SomeType = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return someValue
    }()
}
```

## Optional Chaining

*Optional chaining* is a process for querying and calling properties, methods, and subscripts on an optional that might currently be `nil`. If the optional contains a value, the property, method, or subscript call succeeds; if the optional is `nil`, the property, method, or subscript call returns `nil`. Multiple queries can be chained together, and the entire chain fails gracefully if any link in the chain is `nil`.

### Optional Chaining as an Alternative to Forced Unwrapping

You specify optional chaining by placing a question mark (`?`) after the optional value on which you wish to call a property, method or subscript if the optional is non-`nil`. This is very similar to placing an exclamation mark (`!`) after an optional value to force the unwrapping of its value. The main difference is that optional chaining fails gracefully when the optional is `nil`, whereas forced unwrapping triggers a runtime error when the optional is `nil`.

To reflect the fact that optional chaining can be called on a `nil` value, the result of an optional chaining call is always an optional value, even if the property, method, or subscript you are querying returns a nonoptional value. You can use this optional return value to check whether the optional chaining call was successful (the returned optional contains a value), or did not succeed due to a `nil` value in the chain (the returned optional value is `nil`).

Specifically, the result of an optional chaining call is of the same type as the expected return value, but wrapped in an optional. A property that normally returns an `Int` will return an `Int?` when accessed through optional chaining.

The next several code snippets demonstrate how optional chaining differs from forced unwrapping and enables you to check for success.

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}
```

Se você criar uma nova instância `Person`, sua propriedade `residence` é inicializada inicialmente como `nil`, em virtude de ser opcional. No código abaixo, `john` tem o valor da propriedade de `residence` como `nil`:

```swift
let john = Person()
```

If you try to access the `numberOfRooms` property of this person’s `residence`, by placing an exclamation mark after `residence` to force the unwrapping of its value, you trigger a runtime error, because there is no `residence` value to unwrap:

```swift
let roomCount = john.residence!.numberOfRooms
// this triggers a runtime error
```

O encadeamento opcional fornece uma maneira alternativa de acessar o valor de `numberOfRooms`. Para usar o encadeamento opcional, use um ponto de interrogação no lugar do ponto de exclamação:

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "Unable to retrieve the number of rooms."
```

Because the attempt to access `numberOfRooms` has the potential to fail, the optional chaining attempt returns a value of type `Int?`, or “optional `Int”`. When residence is `nil`, as in the example above, this optional `Int` will also be `nil`, to reflect the fact that it was not possible to access `numberOfRooms`. The optional `Int` is accessed through optional binding to unwrap the integer and assign the nonoptional value to the `roomCount` variable.

Note that this is true even though `numberOfRooms` is a nonoptional `Int`. The fact that it is queried through an optional chain means that the call to `numberOfRooms` will always return an `Int?` instead of an `Int`.

You can assign a `Residence` instance to `john.residence`, so that it no longer has a `nil` value:

```swift
john.residence = Residence()
```

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "John's residence has 1 room(s)."
```

### Defining Model Classes for Optional Chaining

You can use optional chaining with calls to properties, methods, and subscripts that are more than one level deep. This enables you to drill down into subproperties within complex models of interrelated types, and to check whether it is possible to access properties, methods, and subscripts on those subproperties.

The code snippets below define four model classes for use in several subsequent examples, including examples of multilevel optional chaining. These classes expand upon the `Person` and `Residence` model from above by adding a `Room` and `Address` class, with associated properties, methods, and subscripts.

```swift
class Person {
    var residence: Residence?
}
```

The `Residence` class is more complex than before. This time, the `Residence` class defines a variable property called `rooms`, which is initialized with an empty array of type `[Room]`:

```swift
class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
    func printNumberOfRooms() {
        print("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if let buildingNumber = buildingNumber, let street = street {
            return "\(buildingNumber) \(street)"
        } else if buildingName != nil {
            return buildingName
        } else {
            return nil
        }
    }
}
```

### Acessando propriedades através do encadeamento opcional

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "Unable to retrieve the number of rooms."
```

Porque `john.residence` é `nil`, esta chamada encadeamento opcional falhar da mesma maneira como antes.

Você também pode tentar definir o valor de uma propriedade através de encadeamento opcional:

```swift
let someAddress = Address()
someAddress.buildingNumber = "29"
someAddress.street = "Acacia Road"
john.residence?.address = someAddress
```

Neste exemplo, a tentativa de definir a propriedade `address` de `john.residence` falha, porque `john.residence` está atualmente `nil`.

### Calling Methods Through Optional Chaining

Você pode usar encadeamento opcional para chamar um método em um valor opcional e verificar se essa chamada de método é bem-sucedida. Você pode fazer isso mesmo que esse método não defina um valor de retorno.

O método `printNumberOfRooms()` na classe `Residence` imprime o valor atual de `numberOfRooms`. Veja como o método se parece:

```swift
func printNumberOfRooms() {
    print("The number of rooms is \(numberOfRooms)")
}
```

This method does not specify a return type. However, functions and methods with no return type have an implicit return type of `Void`, as described in Functions Without Return Values. This means that they return a value of `()`, or an empty tuple.

If you call this method on an optional value with optional chaining, the method’s return type will be `Void?`, not `Void`, because return values are always of an optional type when called through optional chaining. This enables you to use an `if` statement to check whether it was possible to call the `printNumberOfRooms()` method, even though the method does not itself define a return value. Compare the return value from the `printNumberOfRooms` call against `nil` to see if the method call was successful:

```swift
if john.residence?.printNumberOfRooms() != nil {
    print("It was possible to print the number of rooms.")
} else {
    print("It was not possible to print the number of rooms.")
}
// Prints "It was not possible to print the number of rooms."
```

```swift
if (john.residence?.address = someAddress) != nil {
    print("It was possible to set the address.")
} else {
    print("It was not possible to set the address.")
}
// Prints "It was not possible to set the address."
```

### Accessing Subscripts Through Optional Chaining

Você pode usar encadeamento opcional para tentar recuperar e definir um valor a partir de um subíndice em um valor opcional e verificar se essa chamada de subscrito é bem-sucedida.

*Quando você acessa um subíndice em um valor opcional através de encadeamento opcional, você coloca o ponto de interrogação antes dos colchetes do subíndice, e não depois. O ponto de interrogação de encadeamento opcional sempre segue imediatamente após a parte da expressão que é opcional.*

```swift
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// Prints "Unable to retrieve the first room name."
```

Da mesma forma, você pode tentar definir um novo valor através de um subíndice com encadeamento opcional:

```swift
john.residence?[0] = Room(name: "Bathroom")
```

Esta tentativa de definição de subíndice também falha, porque `residence` está atualmente `nil`.

If you create and assign an actual `Residence` instance to `john.residence`, with one or more `Room` instances in its `rooms` array, you can use the `Residence` subscript to access the actual items in the `rooms` array through optional chaining:

```swift
let johnsHouse = Residence()
johnsHouse.rooms.append(Room(name: "Living Room"))
johnsHouse.rooms.append(Room(name: "Kitchen"))
john.residence = johnsHouse

if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// Prints "The first room name is Living Room."
```

### Accessing Subscripts of Optional Type

If a subscript returns a value of optional type—such as the key subscript of Swift’s `Dictionary` type—place a question mark *after* the subscript’s closing bracket to chain on its optional return value:

```swift
var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0] += 1
testScores["Brian"]?[0] = 72
// the "Dave" array is now [91, 82, 84] and the "Bev" array is now [80, 94, 81]
```

### Vinculando vários níveis de encadeamento

Você pode vincular vários níveis de encadeamento opcional para detalhar as propriedades, os métodos e os índices mais profundamente dentro de um modelo. No entanto, vários níveis de encadeamento opcional não adicionam mais níveis de opcionalidade ao valor retornado.

Em outras palavras:

* Se o tipo que você está tentando recuperar não for opcional, ele se tornará opcional por causa do encadeamento opcional.
* Se o tipo que você está tentando recuperar já é opcional, ele não se tornará mais opcional por causa do encadeamento.

Assim:

* Se você tentar recuperar um valor `Int` através de encadeamento opcional, sempre retorna `Int?`, independentemente de quantos níveis de encadeamento sejam usados.
* Da mesma forma, se você tentar recuperar um valor `Int?` através de encadeamento opcional, um `Int?` sempre retorna, independentemente de quantos níveis de encadeamento forem usados.

```swift
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Prints "Unable to retrieve the address."
```

```swift
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence?.address = johnsAddress

if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Prints "John's street name is Laurel Street."
```

### Encadeamento de métodos com valores de retorno opcionais

O exemplo anterior mostra como recuperar o valor de uma propriedade de tipo opcional através de encadeamento opcional. Você também pode usar encadeamento opcional para chamar um método que retorna um valor de tipo opcional e encadear o valor de retorno desse método, se necessário.

O exemplo abaixo chama o método `buildingIdentifier()` da classe `Address` através de encadeamento opcional. Esse método retorna um valor do tipo de `String?`. Conforme descrito acima, o tipo de retorno final deste método após o encadeamento opcional é também `String?`:

```swift
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    print("John's building identifier is \(buildingIdentifier).")
}
// Prints "John's building identifier is The Larches."
```

Se você deseja realizar encadeamento opcional adicional no valor de retorno deste método, coloque o ponto de interrogação de encadeamento opcional após os parênteses do método:

```swift
if let beginsWithThe = john.residence?.address?.buildingIdentifier()?.hasPrefix("The") {
    if beginsWithThe {
        print("John's building identifier begins with \"The\".")
    } else {
        print("John's building identifier does not begin with \"The\".")
    }
}
// Prints "John's building identifier begins with "The"."
```

*In the example above, you place the optional chaining question mark after the parentheses, because the optional value you are chaining on is the `buildingIdentifier()` method’s return value, and not the `buildingIdentifier()` method itself*.
