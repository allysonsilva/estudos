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

Variáveis não-opcionais significa que as variáveis são necessárias para ter um valor não-nulo; No entanto, há momentos em que queremos ou precisamos de nossas variáveis para conter valores `nil`(nulos). Isso pode ocorrer se retornarmos um `nil`(nulo) de uma função cuja operação falhou ou se um valor não for encontrado.

Em *Swift*, uma **variável opcional** é uma variável na qual podemos atribuir `nil` (sem valor). Variáveis e constantes opcionais são definidas usando `?` (ponto de interrogação).

Os opcionais são um tipo especial no Swift. Estas duas declarações são equivalentes. Ambas as linhas declaram um tipo opcional que pode conter um tipo de string ou pode estar ausente de um valor.

**_`nil` significando “a ausência de um objeto válido”._**

The example below uses the initializer to try to convert a `String` into an `Int`:

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
```

Because the initializer might fail, it returns an _optional_ `Int`, rather than an `Int`. An optional `Int` is written as `Int?`, not `Int`. The question mark indicates that the value it contains is optional, meaning that it might contain _some_ `Int` value, or it might contain _no value at all_. (It can’t contain anything else, such as a `Bool` value or a `String` value. It’s either an `Int`, or it’s nothing at all.)

```swift
// Optional Variable
var stringOne : String?
// Non-Optional Variable
var stringTwo : String

stringOne = nil
stringTwo = nil // error: nil cannot be assigned to type 'String'
```

Abordagem geral:

```swift
//Optional Variable
var stringOne : String?

//--------stringOne is nil ---------------//
//Explicitly check for nil
if stringOne != nil {
    print(stringOne)
} else {
    print("Explicit Check:  stringOne is nil")
}

//option binding
if let tmp = stringOne {
    print(tmp)
} else {
    print("Optional Binding: stringOne is nil")
}

//Optional chainging
var charCount1 = stringOne?.characters.count

//--------adding value to stringONe ---------------//
stringOne = "http://www.example.com"

//--------stringOne is nil ---------------//
//Explicitly check for nil
if stringOne != nil {
    print(stringOne)
} else {
    print("Explicit Check:  stringOne is nil")
}

//option binding
if let tmp = stringOne {
    print(tmp)
} else {
    print("Optional Binding: stringOne is nil")
}

//Optional chainging
var charCount2 = stringOne?.characters.count
```

```swift
var myString1: String?
var myString2: Optional<String>
```

#### Forced unwrapping of an optional

Para unwrap ou recuperar o valor de um opcional, colocamos um ponto de exclamação (!) Após o nome da variável. O unwrapping forçado, desta maneira, pode ser muito perigoso e deve ser usado somente se tivermos certeza de que o valor não é nulo.

Quando usamos o ponto de exclamação para unwrap um opcional, estamos dizendo ao compilador que sabemos que o opcional contém um valor, então vá em frente e entregue-nos.

```swift
var myString1: String?
myString1 = "test"
var test: String = myString1!
```

```swift
var myString: String?

myString = "test"
if myString != nil {
    var test:String = myString!
}
```

#### Returning optionals from functions, methods, and subscripts

```swift
func getName(index: Int) -> String?
{
    let names = ["Jon", "Kim", "Kailey", "Kara"]
    if index >= names.count || index < 0 {
        return nil
    } else {
        return names[index]
    }
}
```

```swift
subscript(index: Int) -> String?
{
	//some statements
}
```

#### Using an optional as a parameter in a function or method

```swift
func optionalParam(myString: String?)
{
    if let temp = myString {
        print("Contains value \(temp)")
    } else {
        print("Does not contain value")
    }
}
```

#### Optional binding with the guard statement

```swift
func sayHello(name: String?)
{
    guard let internalName = name else
    {
        print("Name has not value")
        return
    }

    print("Hello \(internalName)")
}
```

#### Optional types with tuples

Podemos definir uma tupla inteira como opcional ou qualquer um dos elementos dentro de uma tupla como opcional.

```swift
var tuple1: (one: String, two: Int)?
var tuple2: (one: String, two: Int?)
```

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

### Generic Functions

```swift
func swapGeneric<T>(a: inout T, b: inout T)
{
    let tmp = a
    a = b
    b = tmp
}

var a = 5
var b = 10

swapGeneric(a: &a, b: &b)

print("a: \(a) b: \(b)")
```

We can declare multiple constraints just like we declare multiple generic types.

```swift
func testFunction<T: MyClass, E: MyProtocol>(a: T, b: E) {
}
```

### Generic Types

```swift
class List<T> { }

struct GenericStruct<T> { }

enum GenericEnum<T> { }
```

```swift
class List<T>
{
    var items = [T]()

    func add(item: T)
    {
        items.append(item)
    }

    func getItemAtIndex(index: Int) -> T?
    {
        if items.count > index {
            return items[index]
        } else {
            return nil
        }
    }
}

var list = List<String>()
list.add(item: "Hello")
list.add(item: "World")

print(list.getItemAtIndex(index: 1)) // Optional("World")
```

Nós também podemos usar restrições de tipo com tipos genéricos. Mais uma vez, usar uma restrição de tipo para um tipo genérico é exatamente o mesmo que usar um com uma função genérica.

```swift
class MyClass<T: Comparable>{}
```

### Associated Types

```swift
protocol QueueProtocol
{
    associatedtype QueueType
    func add(item: QueueType)
    func getItem() -> QueueType?
    func count() -> Int
}

class GenericQueue<T>: QueueProtocol
{
    var items = [T]()

    func add(item: T)
    {
        items.append(item)
    }

    func getItem() -> T?
    {
        if items.count > 0 {
            return items.remove(at: 0)
        } else {
            return nil
        }
    }

    func count() -> Int
    {
        return items.count
    }
}

var intQ2 = GenericQueue<Int>()
intQ2.add(item: 2)
intQ2.add(item: 4)

print(intQ2.items) // [2, 4]

print(intQ2.getItem()) // Optional(2)

intQ2.add(item: 6)

print(intQ2.items) // [4, 6]
```

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

## Using Swift Collections and the Tuple Type

* **Matriz**
* **Dicionário**
* **Conjunto**
* **Tupla**

### Swift collection types

Uma coleção agrupa vários itens em uma única unidade. Swift fornece três tipos de coleção nativos. Esses tipos de coleção são arrays, sets e dictionaries. Arrays armazenam dados em uma coleção ordenada, sets(conjuntos) são coleções não ordenadas de dados exclusivos, e os dictionaries(dicionários) são coleções não ordenadas de pares chave/valor. Em uma matriz, acessamos os dados pela localização (índice) da matriz; Em um conjunto, tendemos a iterar sobre ele; e os dicionários geralmente são acessados ​​usando uma chave exclusiva.

Os dados armazenados em uma coleção Swift devem ser do mesmo tipo.

### Mutability

Swift não contém classes separadas para coleções mutáveis e imutáveis. Em vez disso usa-se as palavras chaves de declaração de variáveis `let` e `var` para dizer se uma coleção é mutável ou imutável. Utilize `let` para criar constantes de coleções imutáveis, que não alteram seu valor e `var` para criar coleções mutáveis, que seus valores podem ser alterados.

É uma boa prática criar coleções imutáveis(como constantes `let`), a menos que exista uma necessidade específica de alterar os objetos dentro da coleção. Isso permite que o compilador otimize o desempenho.

### Arrays

**Em Swift, uma matriz é uma lista ordenada de objetos do mesmo tipo.**

Quando uma matriz é criada, devemos declarar o tipo de dados a serem armazenados por declaração de tipo explícito ou por inferência de tipo. Normalmente, declaramos explicitamente o tipo de dados de uma matriz quando criamos uma matriz vazia. Se inicializarmos uma matriz com dados, devemos permitir que o compilador use a inferência de tipo para inferir o tipo de dados mais apropriado para a matriz.

Cada objeto em uma matriz é chamado de elemento. Cada um desses elementos é armazenado em uma ordem definida e pode ser acessado pela sua localização (índice) na matriz.

#### Creating and initializing arrays

Podemos inicializar uma matriz com um literal de matriz e com inferência do tipo pelo compilador.

```swift
let arrayOne = [10,20,30]
var shoppingList: [String] = ["Eggs", "Milk"]
```

Se quisermos criar um array vazio, precisamos declarar explicitamente o tipo de valores a serem armazenados na matriz.

```swift
var arrayOne = [String]()
let arrayTwo = [Double]()
var arrayThree = [Int]()
let arrayFour = [MyObject]()
```

*Uma vez que uma matriz é definida como contendo um tipo específico, todos os elementos da matriz devem ser desse mesmo tipo*.

Swift fornece alias de tipo especial para trabalhar com tipos não específicos. Esses alias são `AnyObject` e `Any`. Podemos usar estes alias para definir arrays cujos elementos são de diferentes tipos, como este:

```swift
var myArray: [Any] = [1,"Two"]
```

Os alias de `AnyObject` podem representar uma instância de qualquer tipo de classe, enquanto os `Any` alias podem representar uma instância de qualquer tipo. Devemos usar os alias `Any` e `AnyObject` somente quando houver uma necessidade explícita desse comportamento. É sempre melhor ser específico sobre os tipos de dados que nossas coleções contêm.

Também podemos inicializar uma matriz para um determinado tamanho, com todos os elementos do conjunto de matrizes para um valor predefinido. O exemplo a seguir define uma matriz com sete elementos e cada elemento contém o número 3:

```swift
var arrayFour = [Int](repeating: 3, count: 7) // [3, 3, 3, 3, 3, 3, 3]
```

Embora a matriz mais comum seja uma matriz unidimensional, também podemos criar matrizes multidimensionais. Uma matriz multidimensional é realmente nada mais do que uma matriz de arrays. Por exemplo, uma matriz bidimensional é uma matriz de matrizes, enquanto uma matriz tridimensional é uma matriz de arrays de arrays. Os exemplos a seguir mostram as duas maneiras de criar uma matriz bidimensional em Swift:

```swift
var multiArrayOne = [[1,2],[3,4],[5,6]]
var multiArrayTwo = [[Int]]()
```

#### Accessing the array elements

*Índice*

```swift
let arrayOne = [1,2,3,4,5,6]
print(arrayOne[0]) //Displays '1'
print(arrayOne[3]) //Displays '4'
```

*Recuperando valor de uma matriz multidimensional*.

```swift
var multiArray = [[1,2],[3,4],[5,6]]
var arr = multiArray[0] //arr contains the array [1,2]
var value = multiArray[0][1] // value contains 2
```

*Com propriedades de `first` e `last`.

```swift
let arrayOne = [1,2,3,4,5,6]
let first = arrayOne.first //first contains 1
let last = arrayOne.last //last contains 6

let multiArray = [[1,2],[3,4],[5,6]]
let arrFirst1 = multiArray[0].first //arrFirst1 contains 1
let arrFirst2 = multiArray.first //arrFirst2 contains[1,2]
let arrLast1 = multiArray[0].last //arrLast1 contains 2
let arrLast2 = multiArray.last //arrLast2 contains [5,6]
```

#### Counting the elements of an array

```swift
let arrayOne = [1,2,3]
let multiArrayOne = [[3,4],[5,6],[7,8]]
print(arrayOne.count) //Displays 3
print(multiArrayOne.count) //Displays 3 for the three arrays
print(multiArrayOne[0].count) //Displays 2 for the two elements
```

#### Is the array empty?

```swift
var arrayOne = [1,2]
var arrayTwo = [Int]()
arrayOne.isEmpty //Returns false because the array is not empty
arrayTwo.isEmpty //Returns true because the array is empty
```

#### Appending to an array

**`append`: Adicionando novos itens no final da matriz**

```swift
var arrayOne = [1,2]
arrayOne.append(3) //arrayOne will now contain 1, 2 and 3
```

Swift também nos permite usar o operador de atribuição de adição (+ =) para anexar uma matriz a outra matriz. O exemplo a seguir mostra como usar o operador de atribuição de adição para anexar uma matriz ao final de outra matriz:

```swift
var arrayOne = [1,2]
arrayOne += [3,4] //arrayOne will now contain 1, 2, 3 and 4
```

#### Inserting a value into an array

Podemos inserir um valor em uma matriz usando o método de `insert`. O método `insert` moverá todos os itens até um ponto, começando no índice especificado, para abrir espaço para o novo elemento e, em seguida, inserirá o valor no índice especificado.

```swift
var arrayOne = [1,2,3,4,5]
arrayOne.insert(10, at: 3) //arrayOne now contains 1, 2, 3, 10, 4 and 5
```

#### Removing elements from an array

Existem três métodos que podemos usar para remover um ou todos os elementos em uma matriz. Esses métodos são `removeLast()`, `remove(at:)`, and `removeAll()`.

```swift
var arrayOne = [1,2,3,4,5]
arrayOne.removeLast() //arrayOne now contains 1, 2, 3 and 4
arrayOne.remove(at:2) //arrayOne now contains 1, 2 and 4
arrayOne.removeAll() //arrayOne is now empty
```

Os métodos `removeLast()` e `remove(at:)` também retornarão o valor do elemento que está sendo removido.

```swift
var arrayOne = [1,2,3,4,5]
var removedLast = arrayOne.removeLast() //removedLast contains the value 5
var removed = arrayOne.remove(at: 2)  //removed contains the value 3
```

#### Merging two arrays

```swift
let arrayOne = [1,2]
let arrayTwo = [3,4]
var combine = arrayOne + arrayTwo //combine contains 1, 2, 3 and 4
```

#### Reversing an array

```swift
let arrayOne = [1,2,3]
var reverse = arrayOne.reversed() //reverse contains 3, 2 and 1
```

#### Retrieving a subarray from an array

Podemos recuperar um subarray de uma matriz existente usando a sintaxe de subíndice com um intervalo.

```swift
let arrayOne = [1,2,3,4,5]
var subArray = arrayOne[2...4] //subArray contains 3, 4 and 5
```

O operador `...` (three periods) é conhecido como um **range** operator. The range operator, no código anterior, diz que eu quero todos os elementos entre a partir do segundo índice e até o quarto índice. Existe outro, que é `..<`; é o mesmo que o operador range`...`, mas exclui o último elemento.

```swift
let arrayOne = [1,2,3,4,5]
var subArray = arrayOne[2..<4] //subArray contains 3 and 4
```

#### Making bulk changes to an array

```swift
var arrayOne = [1,2,3,4,5]
arrayOne[1...2] = [12,13] //arrayOne contains 1,12,13,4 and 5
```

```swift
var arrayOne = [1,2,3,4,5]
arrayOne[1...3] = [12,13] //arrayOne now contains 1, 12, 13 and 5 (four elements)
```

```swift
var arrayOne = [1,2,3,4,5]
arrayOne[1...3] = [12,13,14,15] //arrayOne now contains 1, 12, 13, 14, 15 and 5 (six elements)
```

#### Algorithms for arrays

##### Sort

```swift
var arrayOne = [9,3,6,2,8,5]
arrayOne.sort(){ $0 < $1 } //arrayOne contains 2,3,5,6,8 and 9
arrayOne.sort(){ $1 > $0 } //arrayOne contains [9, 8, 6, 5, 3, 2]
```

##### Sorted

Enquanto o algoritmo sorts classifica a matriz no local (substitui a matriz original), o algoritmo sorted não altera a matriz original; Em vez disso, cria uma nova matriz com os elementos ordenados da matriz original.

```swift
var arrayOne = [9,3,6,2,8,5]
let sorted = arrayOne.sorted(){ $0 < $1 } //sorted contains 2,3,5,6,8 and 9
//arrayOne contains 9,3,6,2,8 and 5
```

##### Filter

O algoritmo do filtro retornará uma nova matriz, filtrando a matriz original. Este é um dos algoritmos de matriz mais poderosos. Se você precisa recuperar um subconjunto de uma matriz, com base em um conjunto de regras, recomendo usar esse algoritmo em vez de tentar escrever seu próprio método para filtrar a matriz. A closure leva um argumento e ele deve retornar um booleano verdadeiro se o elemento deve ser incluído na nova matriz.

```swift
var arrayFiltered = [4,1,6,9,3,5,2,8,7]
let filtered = arrayFiltered.filter{$0 > 3 && $0 < 7} // filtered contains 4,5 and 6
```

```swift
var city = ["Boston", "London", "Chicago", "Atlanta"]
let filtered = city.filter{$0.range(of: "o") != nil} // ["Boston", "London", "Chicago"]
```

No código anterior, usamos o método `range()` para retornar verdadeiro se a seqüência contiver a letra "o". Se o método retornar verdadeiro, a cadeia está incluída na matriz filtrada.

##### Map

O algoritmo de `map` retorna uma nova matriz que contém os resultados da aplicação das regras no fechamento para cada elemento da matriz.

```swift
var arrayOne = [10, 20, 30, 40]
let applied = arrayOne.map{ $0 / 10} // applied contains 1,2,3 and 4
```

```swift
var arrayOne = [1, 2, 3, 4]
let applied = arrayOne.map{"num: \($0)"} // ["num: 1", "num: 2", "num: 3", "num: 4"]
```

##### forEach

```swift
var arrayOne = [10, 20, 30, 40]
arrayOne.forEach{print($0)}

/*
10
20
30
40
*/
```

#### Iterating over an array

```swift
var arrayOne = ["one", "two", "three"]
for item in arrayOne { print(item) }
```

```swift
var arrayOne = ["one", "two", "three"]
for (index,value) in arrayOne.enumerated() {
    print("\(index) \(value)")
}

/*
0 one
1 two
2 three
*/
```

### Dictionaries

Enquanto os dicionários não são tão comumente usados como arrays, eles têm uma funcionalidade adicional que os torna incrivelmente poderosos. Um dicionário é um recipiente que armazena múltiplos pares de valores-chave, onde todas as chaves são do mesmo tipo e todos os valores são do mesmo tipo. A chave é usada como um identificador exclusivo para o valor. Um dicionário não garante a ordem em que os pares chave-valor são armazenados, pois buscamos os valores pela chave e não pelo índice do valor.

#### Creating and initializing dictionaries

```swift
let countries = ["US":"UnitedStates","IN":"India","UK":"United Kingdom"]
var us = countries["US"]
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

Nos exemplo anterior, criamos um dicionário onde a chave e o valor eram ambas `string`. O compilador inferiu que a chave e o valor eram strings porque esse era o tipo de chaves e valores usados para imitar o dicionário. Se quisermos criar um dicionário vazio, precisamos dizer ao compilador quais são os tipos de chave e de valor.

```swift
var dic1 = [String:String]()
var dic2 = [Int:String]()
var dic3 = [String:MyObject]()
```

#### Updating the value of a key

```swift
var countries = ["US":"United States", "IN":"India","UK":"United Kingdom"]

countries["UK"] = "Great Britain" // The value of UK is now set to "Great Britain"

var orig = countries.updateValue("Britain", forKey: "UK") // The value of UK is now set to "Britain" and orig now contains "Great Britain"
```

#### Adding a key-value pair

```swift
var countries = ["US":"United States", "IN":"India","UK":"United Kingdom"]

countries["FR"] = "France" // The value of "FR" is set to "France"

var orig = countries.updateValue("Germany", forKey: "DE") // The value of "DE" is set to "Germany" and orig is nil

print(countries) // ["FR": "France", "US": "United States", "DE": "Germany", "UK": "United Kingdom", "IN": "India"]
print(orig) // nil
```

#### Removing a key-value pair

```swift
var countries = ["US":"United States", "IN":"India", "UK":"United Kingdom"];

countries["IN"] = nil // The "IN" key/value pair is removed

print(countries) // ["US": "United States", "UK": "United Kingdom"]

var orig = countries.removeValue(forKey: "UK")

print(countries) // ["US": "United States"]
print(orig) // "United Kingdom"

countries.removeAll() //Removes all key/value pairs from the countries dictionary
```

### Set (Conjunto)

O tipo `set` é uma coleção genérica que é semelhante ao tipo de matriz. Enquanto o tipo de matriz é uma coleção ordenada que pode conter itens duplicados, o tipo de conjunto é uma coleção não ordenada em que cada item deve ser exclusivo, não permite itens duplicados.

#### Initializing a set

```swift
//Initializes an empty set of the String type
var mySet = Set<String>()

//Initializes a mutable set of the String type with initial values
var mySet = Set(["one", "two", "three"])

//Creates a immutable set of the String type.
let mySet = Set(["one", "two", "three"])
```

#### Inserting items into a set

```swift
var mySet = Set<String>()
mySet.insert("One")
mySet.insert("Two")
mySet.insert("Three")
```

#### Checking whether a set contains an item

```swift
var mySet = Set<String>()
mySet.insert("One")
mySet.insert("Two")
mySet.insert("Three")
var contain = mySet.contains("Two") // true
```

#### Removing items in a set

```swift
var mySet = Set<String>()
mySet.insert("One")
mySet.insert("Two")
mySet.insert("Three")

//The remove method will return and remove an item from a set
var item = mySet.remove("Two") // Two

//The removeAll method will remove all items from a set
mySet.removeAll() // Set([])
```

#### Set operations

A Apple forneceu quatro métodos que podemos usar para construir um conjunto de dois outros conjuntos. Essas operações podem ser executadas no lugar, em um dos conjuntos, ou usadas para criar um novo conjunto. Essas operações são as seguintes:

- `union`: *Eles criam um conjunto com todos os valores exclusivos de ambos os conjuntos*
- `subtracting`: *Estes criam um conjunto com valores do primeiro conjunto que não estão no segundo conjunto*
- `intersection`: *Estes criam um conjunto com valores comuns a ambos os conjuntos*
- `symmetricDifference`: *Estes criam um novo conjunto com valores que estão em conjunto, mas não em ambos os conjuntos*

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

```swift
var mySet1 = Set(["One", "Two", "Three", "abc"])
var mySet2 = Set(["abc","def","ghi", "One"])

var newSetSymmetricDifference = mySet1.symmetricDifference(mySet2) // ["Three", "Two", "def", "ghi"]
var newSetUnion = mySet1.union(mySet2) // ["Three", "Two", "One", "abc", "def", "ghi"]
var newSetIntersection = mySet1.intersection(mySet2) // ["One", "abc"]
var newSetSubtracting = mySet1.subtracting(mySet2) // ["Three", "Two"]
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

Os índices, no idioma Swift, são usados como atalhos para acessar elementos de uma coleção, lista ou seqüência. Podemos usá-los em nossos tipos personalizados para definir ou recuperar os valores por índice, em vez de usar métodos `getter` e `setter`. Subscripts, se usadas corretamente, podem melhorar significativamente a usabilidade e a legibilidade de nossos tipos personalizados.

Usamos subscripts personalizados, assim como usamos subscripts para arrays e dicionários. Por exemplo, para acessar um elemento em uma matriz, usaremos a sintaxe `anArray[index]`. Quando definimos um subscripts personalizado para nossos tipos personalizados, também os acessamos com essa mesma sintaxe, `ourType[key]`.

### Read and write custom subscripts

```swift
class MyNames
{
    private var names = ["Jon", "Kim", "Kailey", "Kara"]

    subscript(index: Int) -> String
    {
        get { return names[index] }
        set { names[index] = newValue }
    }
}

var nam = MyNames()
print(nam[0]) // Displays 'Jon'
nam[0] = "Buddy"
print(nam[0]) // Displays 'Buddy'
```

### Read-only custom subscripts

Também podemos fazer o subscript de somente leitura, não declarando um método setter dentro do subscript ou declarando não implicitamente um método getter ou setter.

```swift
// No getter/setters implicitly declared
subscript(index: Int) -> String { return names[index] }
```

```swift
// Declaring only a getter
subscript(index: Int) -> String {
    get {
        return names[index]
    }
}
```

### Subscript values

Nos exemplos subscript anteriores, todos os subscritos aceitaram inteiros como o valor para o subscript; no entanto, não estamos limitados a números inteiros.

```swift
struct Hello
{
    subscript (name: String) ->String
    {
        return "Hello \(name)"
    }
}

var hello = Hello()
print(hello["Jon"]) // Hello Jon
```

### External names for subscripts

```swift
struct MathTable
{
    var num: Int

    subscript(multiply index: Int) -> Int
    {
        return num * index
    }

    subscript(addition index: Int) -> Int
    {
        return num + index
    }
}

var table = MathTable(num: 5)
print(table[multiply: 4]) // Displays 20 because 5*4=20
print(table[addition: 4]) // Displays 9 because 5+4=9
```

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

## Error Handling (Manipulação de erros)

O tratamento de erros é o processo de resposta e recuperação de condições de erro em seu programa. O Swift oferece suporte de primeira classe para lançar, capturar, propagar e manipular erros recuperáveis ​​em tempo de execução.

Algumas operações não são garantidas para sempre completar a execução ou produzir uma saída útil. Os opcionais são usados ​​para representar a ausência de um valor, mas quando uma operação falha, muitas vezes é útil entender o que causou a falha, para que seu código possa responder de acordo.

### Representando e lançando erros

Em Swift, os erros são representados por valores de tipos que estão em conformidade com o protocolo de `Error`. Este protocolo vazio indica que um tipo pode ser usado para o tratamento de erros.

As enumerações Swift são particularmente adequadas para modelar um grupo de condições de erro relacionadas, com valores associados que permitem que informações adicionais sobre a natureza de um erro sejam comunicadas. For example, here’s how you might represent the error conditions of operating a vending machine inside a game:

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

Você usa uma a declaração de `throw` para lançar um erro.

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```

### Handling Errors (Manipulação de erros)

Quando um erro é lançado, algum código envolvente deve ser responsável por lidar com o erro, por exemplo, corrigindo o problema, tentando uma abordagem alternativa ou informando o usuário da falha.

There are four ways to handle errors in Swift. You can propagate the error from a function to the code that calls that function, handle the error using a `do-catch` statement, handle the error as an optional value, or assert that the error will not occur. Each approach is described in a section below.

When a function throws an error, it changes the flow of your program, so it’s important that you can quickly identify places in your code that can throw errors. To identify these places in your code, write the `try` keyword—or the `try?` or `try!` variation—before a piece of code that calls a function, method, or initializer that can throw an error. These keywords are described in the sections below.

#### Propagating Errors Using Throwing Functions

To indicate that a function, method, or initializer can throw an error, you write the `throws` keyword in the function’s declaration after its parameters. A function marked with `throws` is called a throwing function. If the function specifies a return type, you write the `throws` keyword before the return arrow (`->`).

```swift
func canThrowErrors() throws -> String

func cannotThrowErrors() -> String
```

A throwing function propagates errors that are thrown inside of it to the scope from which it’s called.

_Only throwing functions can propagate errors. Any errors thrown inside a nonthrowing function must be handled inside the function._

```swift
struct Item {
    var price: Int
    var count: Int
}

class VendingMachine {
    var inventory = [
        "Candy Bar": Item(price: 12, count: 7),
        "Chips": Item(price: 10, count: 4),
        "Pretzels": Item(price: 7, count: 11)
    ]
    var coinsDeposited = 0

    func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }

        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }

        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }

        coinsDeposited -= item.price

        var newItem = item
        newItem.count -= 1
        inventory[name] = newItem

        print("Dispensing \(name)")
    }
}
```

Because the `vend(itemNamed:)` method propagates any errors it throws, any code that calls this method must either handle the errors—using a `do-catch` statement, `try?`, or `try!`—or continue to propagate them. For example, the `buyFavoriteSnack(person:vendingMachine:)` in the example below is also a throwing function, and any errors that the `vend(itemNamed:)` method throws will propagate up to the point where the `buyFavoriteSnack(person:vendingMachine:)` function is called.

```swift
let favoriteSnacks = [
    "Alice": "Chips",
    "Bob": "Licorice",
    "Eve": "Pretzels",
]

func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}
```

Throwing initializers can propagate errors in the same way as throwing functions. For example, the initializer for the `PurchasedSnack` structure in the listing below calls a throwing function as part of the initialization process, and it handles any errors that it encounters by propagating them to its caller.

```swift
struct PurchasedSnack {
    let name: String
    init(name: String, vendingMachine: VendingMachine) throws {
        try vendingMachine.vend(itemNamed: name)
        self.name = name
    }
}
```

#### Handling Errors Using Do-Catch

You use a `do-catch` statement to handle errors by running a block of code. If an error is thrown by the code in the `do` clause, it is matched against the `catch` clauses to determine which one of them can handle the error.

Here is the general form of a `do-catch` statement:

```
do {
    try expression
    statements
} catch pattern 1 {
    statements
} catch pattern 2 where condition {
    statements
}
```

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
}
// Prints "Insufficient funds. Please insert an additional 2 coins."
```

#### Conversão de erros em valores opcionais

You use `try?` to handle an error by converting it to an optional value. If an error is thrown while evaluating the `try?` expression, the value of the expression is `nil`. For example, in the following code `x` and `y` have the same value and behavior:

```swift
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

Using `try?` lets you write concise error handling code when you want to handle all errors in the same way. For example, the following code uses several approaches to fetch data, or returns `nil` if all of the approaches fail.

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
```

#### Disabling Error Propagation (Desativando a Propagação de Erros)

Sometimes you know a throwing function or method won’t, in fact, throw an error at runtime. On those occasions, you can write `try!` before the expression to disable error propagation and wrap the call in a runtime assertion that no error will be thrown. If an error actually is thrown, you’ll get a runtime error.

```swift
let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
```

### Specifying Cleanup Actions

You use a `defer` statement to execute a set of statements just before code execution leaves the current block of code. This statement lets you do any necessary cleanup that should be performed regardless of how execution leaves the current block of code—whether it leaves because an error was thrown or because of a statement such as `return` or `break`. For example, you can use a `defer` statement to ensure that file descriptors are closed and manually allocated memory is freed.

A `defer` statement defers execution until the current scope is exited. This statement consists of the `defer` keyword and the statements to be executed later. The deferred statements may not contain any code that would transfer control out of the statements, such as a `break` or a `return` statement, or by throwing an error. Deferred actions are executed in the reverse of the order that they’re written in your source code. That is, the code in the first `defer` statement executes last, the code in the second `defer` statement executes second to last, and so on. The last `defer` statement in source code order executes first.

```swift
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try file.readline() {
            // Work with the file.
        }
        // close(file) is called here, at the end of the scope.
    }
}
```

The above example uses a `defer` statement to ensure that the `open(_:)` function has a corresponding call to `close(_:)`.

## Type Casting

Type casting is a way to check the type of an instance, or to treat that instance as a different superclass or subclass from somewhere else in its own class hierarchy.

### Defining a Class Hierarchy for Type Casting

You can use type casting with a hierarchy of classes and subclasses to check the type of a particular class instance and to cast that instance to another class within the same hierarchy. The three code snippets below define a hierarchy of classes and an array containing instances of those classes, for use in an example of type casting.

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```

```swift
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// the type of "library" is inferred to be [MediaItem]
```

### Checking Type

Use the *type check operator* (`is`) to check whether an instance is of a certain subclass type. The type check operator returns `true` if the instance is of that subclass type and `false` if it is not.

```swift
var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// Prints "Media library contains 2 movies and 3 songs"
```

### Downcasting

A constant or variable of a certain class type may actually refer to an instance of a subclass behind the scenes. Where you believe this is the case, you can try to *downcast* to the subclass type with a type cast operator (`as?` or `as!`).

Because downcasting can fail, the type cast operator comes in two different forms. The conditional form, `as?`, returns an optional value of the type you are trying to downcast to. The forced form, `as!`, attempts the downcast and force-unwraps the result as a single compound action.

Use the conditional form of the type cast operator (`as?`) when you are not sure if the downcast will succeed. This form of the operator will always return an optional value, and the value will be `nil` if the downcast was not possible. This enables you to check for a successful downcast.

Use the forced form of the type cast operator (`as!`) only when you are sure that the downcast will always succeed. This form of the operator will trigger a runtime error if you try to downcast to an incorrect class type.

```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}

// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley
```

### Type Casting for Any and AnyObject

Swift fornece dois tipos especiais para trabalhar com tipos não específicos:

* `Any` pode representar uma instância de qualquer tipo, incluindo tipos de função.
* `AnyObject` pode representar uma instância de qualquer tipo de classe.

Use `Any` and `AnyObject` only when you explicitly need the behavior and capabilities they provide. It is always better to be specific about the types you expect to work with in your code.

```swift
var things = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })
```

```swift
for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
```

## Extensions

As extensões adicionam novas funcionalidades a uma classe existente, estrutura, enumeração ou tipo de protocolo. Isso inclui a capacidade de estender os tipos para os quais você não tem acesso ao código fonte original.

As extensões no Swift podem:

- Adicionar propriedades da instância calculada e propriedades do tipo calculado
- Definir métodos de instância e métodos de tipo
- Fornecer novos inicializadores
- Definir índices
- Definir e usar novos tipos aninhados
- Criar um tipo existente de acordo com um protocolo

Em Swift, você pode até estender um protocolo para fornecer implementações de seus requisitos ou adicionar funcionalidades adicionais que os tipos conformes podem aproveitar.

*As extensões podem adicionar novas funcionalidades a um tipo, mas não podem substituir a funcionalidade existente.*

### Sintaxe de extensão

Declare as extensões com a palavra-chave `extension`:

```swift
extension SomeType {
    // new functionality to add to SomeType goes here
}
```

Uma extensão pode estender um tipo existente para que ele adote um ou mais protocolos. Para adicionar a conformidade do protocolo, você escreve os nomes dos protocolos da mesma maneira que os escreve para uma classe ou estrutura:

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // implementation of protocol requirements goes here
}
```

*Se você definir uma extensão para adicionar novas funcionalidades a um tipo existente, a nova funcionalidade estará disponível em todas as instâncias existentes desse tipo, mesmo que elas tenham sido criadas antes da extensão ser definida*.

### Computed Properties

As extensões podem adicionar propriedades da instância calculada e propriedades do tipo calculado aos tipos existentes.

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}

let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// Prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// Prints "Three feet is 0.914399970739201 meters"
```

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// Prints "A marathon is 42195.0 meters long"
```

*Extensions can add new computed properties, but they cannot add stored properties, or add property observers to existing properties.*

### Inicializadores

As extensões podem adicionar novas inicializações a tipos existentes. Isso permite expandir outros tipos para aceitar seus próprios tipos personalizados como parâmetros de inicialização ou para fornecer opções de inicialização adicionais que não foram incluídas como parte da implementação original do tipo.

As extensões podem adicionar novas inicializações de conveniência a uma classe, mas não podem adicionar novos inicializadores designados ou desinitializadores a uma classe. Os inicializadores designados e os desinitializadores devem sempre ser fornecidos pela implementação original da classe.

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
}
```

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
                          size: Size(width: 5.0, height: 5.0))
```

Você pode estender a estrutura `Rect` para fornecer um inicializador adicional que leve um ponto e tamanho do centro específico:

```swift
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

### Métodos

Extensions can add new instance methods and type methods to existing types.

```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
```

Depois de definir esta extensão, você pode chamar o método `repetitions(task:)` em qualquer número inteiro para executar uma tarefa muitas vezes:

```swift
3.repetitions {
    print("Hello!")
}

// Hello!
// Hello!
// Hello!
```

#### Mutating Instance Methods

Os métodos de instância adicionados com uma extensão também podem modificar (ou mutar) a própria instância. Os métodos de estrutura e enumeração que modificam `self` ou as suas propriedades devem marcar o método da instância como `mutating`, assim como os métodos de mutação de uma implementação original.

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}

var someInt = 3
someInt.square()
// someInt is now 9
```

### Subscripts

As extensões podem adicionar novos subíndices a um tipo existente. Este exemplo adiciona um subíndice inteiro ao tipo `Int` incorporado do Swift. Este subíndice `[n]` retorna os `n` locais de dígitos decimais a partir da direita do número:

```
123456789[0] retorna 9
123456789[1] retorna 8
```

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}

746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
```

Se o valor `Int` não tiver dígitos suficientes para o índice solicitado, a implementação do subíndice retorna 0, como se o número tivesse sido preenchido com zeros à esquerda:

```
746381295[9]
// returns 0, as if you had requested:
0746381295[9]
```

## Automatic Reference Counting

Swift usa contagem de referência automática (ARC) para rastrear e gerenciar o uso de memória do seu aplicativo. Na maioria dos casos, isso significa que o gerenciamento de memória "apenas funciona" no Swift, e você não precisa pensar em gerenciamento de memória. O ARC liberta automaticamente a memória usada pelas instâncias da classe quando essas instâncias não são mais necessárias.

A contagem de referência aplica-se apenas a instâncias de classes. Estruturas e enumerações são tipos de valor, não tipos de referência, e não são armazenados e passados ​​por referência.

### Como o ARC funciona

Toda vez que você cria uma nova instância de uma classe, o ARC aloca um pedaço de memória para armazenar informações sobre essa instância. Esta memória contém informações sobre o tipo de instância, juntamente com os valores de quaisquer propriedades armazenadas associadas a essa instância.

Além disso, quando uma instância não é mais necessária, o ARC liberta a memória usada por essa instância para que a memória possa ser usada para outros fins. Isso garante que as instâncias da classe não ocupem espaço na memória quando elas não são mais necessárias.

No entanto, se a ARC estivesse deallocated uma instância que ainda estava em uso, não seria mais possível acessar as propriedades dessa instância ou chamar os métodos dessa instância. Na verdade, se você tentar acessar a instância, seu aplicativo provavelmente irá falhar.

Para garantir que as instâncias não desapareçam enquanto elas ainda são necessárias, o ARC rastreia quantas propriedades, constantes e variáveis ​​estão atualmente se referindo a cada instância da classe. ARC não irá desalocar uma instância, desde que pelo menos uma referência ativa para essa instância ainda exista.

Para tornar isso possível, sempre que você atribuir uma instância de classe a uma propriedade, constante ou variável, essa propriedade, constante ou variável faz uma referência forte à instância. A referência é chamada de referência "forte" porque mantém uma firme retenção naquela instância, e não permite que ela seja deallocated desde que essa referência forte permaneça.

### ARC in Action

### Ciclos de Referência Forte entre Instâncias de Classe

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

Here’s how the strong references look after creating and assigning these two instances. The `john` variable now has a strong reference to the new `Person` instance, and the `unit4A` variable has a strong reference to the new `Apartment` instance:

![Reference Cycle](./Images/ReferenceCycle01.png "Reference Cycle")

Agora você pode vincular as duas instâncias juntas para que a pessoa tenha um apartamento e o apartamento tenha um inquilino. Observe que um ponto de exclamação (`!`) É usado para desempacotar e acessar as instâncias armazenadas dentro das variáveis opcionais `john` e `unit4A`, para que as propriedades dessas instâncias possam ser definidas:

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```

Infelizmente, a ligação destas duas instâncias cria um forte ciclo de referência entre eles. A instância da pessoa agora possui uma forte referência à instância do Apartamento, e a instância do Apartamento tem uma forte referência à instância `Person`. Portanto, quando você quebra as referências fortes detidas pelas variáveis `john` e `unit4A`, as contagens de referência não caem para zero e as instâncias não são desalocadas pela ARC:

```swift
john = nil
unit4A = nil
```

Observe que nenhum deinicializador foi chamado quando você definiu essas duas variáveis como nulas. O forte ciclo de referência evita que as instâncias Pessoa e Apartamento sejam desalocadas, causando um vazamento de memória em seu aplicativo.

Veja como as referências fortes ajudam a definir as variáveis `john` e `unit4A` como `nil`:

![Reference Cycle](./Images/ReferenceCycle03.png "Reference Cycle")

As referências fortes entre a instância `Person` e a instância do `Apartment` permanecem e não podem ser quebradas.

### Resolvendo ciclos de referência fortes entre instâncias de classe

Swift provides two ways to resolve strong reference cycles when you work with properties of class type: weak references and unowned references.

Use a weak reference when the other instance has a shorter lifetime—that is, when the other instance can be deallocated first. In the Apartment example above, it’s appropriate for an apartment to be able to have no tenant at some point in its lifetime, and so a weak reference is an appropriate way to break the reference cycle in this case. In contrast, use an unowned reference when the other instance has the same lifetime or a longer lifetime.

#### Referências fracas

Uma referência fraca é uma referência que não mantém uma forte retenção na instância a que se refere e, portanto, não impede a ARC de descartar a instância referenciada. Esse comportamento impede que a referência se torne parte de um ciclo de referência forte. Você indica uma referência fraca colocando a palavra-chave `weak` antes de uma declaração de propriedade ou variável.

Como uma referência fraca não mantém uma forte retenção na instância a que se refere, é possível que essa instância seja deallocated enquanto a referência fraca ainda está se referindo a ela. Portanto, o ARC define automaticamente uma referência fraca como `nil` quando a instância a que se refere é desalocada. E, porque as referências fracas precisam permitir que seu valor seja alterado para `nil` no tempo de execução, eles são sempre declarados como variáveis, em vez de constantes, de um tipo opcional.

> Property observers aren't called when ARC sets a weak reference to `nil`.

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

Veja como as referências aparecem agora que você vinculou as duas instâncias em conjunto:

![Weak Reference](./Images/WeakReference01.png "Weak Reference")

#### Unowned References

Como uma referência fraca, uma referência unowned não mantém um forte controle sobre a instância a que se refere. Ao contrário de uma referência fraca, no entanto, uma referência unowned é usada quando a outra instância tem a mesma vida útil ou uma vida útil mais longa. Você indica uma referência unowned colocando a palavra-chave `unowned` antes de uma declaração de propriedade ou variável.

Espera-se que uma referência unowned tenha sempre um valor. Como resultado, o ARC nunca define o valor de referência unowned como nulo, o que significa que as referências unowned são definidas usando tipos não-opcionais.

> Use apenas unowned reference quando tiver certeza de que a referência sempre se refere a uma instância que não foi desalocada. Se você tentar acessar o valor de unowned reference depois que essa instância foi desalocada, você receberá um erro de tempo de execução.

Como um cartão de crédito sempre terá um cliente, você define sua propriedade do cliente como uma referência unowned, para evitar um ciclo de referência forte:

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }

    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }

    deinit { print("Card #\(number) is being deinitialized") }
}
```

```swift
var john: Customer?

john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
```

Veja como as referências se parecem, agora que você vinculou as duas instâncias:

![Unowned Reference](./Images/UnownedReference01.png "Unowned Reference")

A instância do Cliente agora possui uma forte referência à instância do CreditCard e a instância do CreditCard possui uma unowned reference para a instância do Cliente.

Por causa da unowned reference do cliente, quando você quebra a referência forte detida pela variável john, não há mais referências fortes à instância do Cliente:

![Unowned Reference](./Images/UnownedReference02.png "Unowned Reference")

Because there are no more strong references to the `Customer` instance, it’s deallocated. After this happens, there are no more strong references to the `CreditCard` instance, and it too is deallocated:

```swift
john = nil
// Prints "John Appleseed is being deinitialized"
// Prints "Card #1234567890123456 is being deinitialized"
```

#### Unowned References and Implicitly Unwrapped Optional Properties

O exemplo abaixo define duas classes, País e Cidade, cada uma das quais armazena uma instância da outra classe como uma propriedade. Neste modelo de dados, cada país deve sempre ter uma capital e cada cidade deve sempre pertencer a um país. Para representar isso, a classe Country possui uma propriedade capitalCity e a classe City possui uma propriedade de país:

```swift
class Country {
    let name: String
    var capitalCity: City!
    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity = City(name: capitalName, country: self)
    }
}

class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}

var country = Country(name: "Canada", capitalName: "Ottawa")
print("\(country.name)'s capital city is called \(country.capitalCity.name)")
// Prints "Canada's capital city is called Ottawa"
```

### Strong Reference Cycles for Closures
???

## Segurança da Memória

Por padrão, o Swift evita o comportamento inseguro em seu código. Por exemplo, Swift garante que as variáveis sejam inicializadas antes de serem usadas, a memória não é acessada depois que ela foi desalocada e os índices da matriz são verificados para erros fora do limite.

O Swift também garante que vários acessos para a mesma área de memória não entrem em conflito, exigindo que o código que modifica um local na memória tenha acesso exclusivo a essa memória. Como Swift gerencia memória automaticamente, na maioria das vezes você não precisa pensar em acessar a memória. No entanto, é importante entender onde podem ocorrer conflitos potenciais, para que você possa evitar escrever código que tenha acesso conflitante à memória. Se o seu código contiver conflitos, você obterá um erro de compilação ou tempo de execução.

## Controle de acesso

O controle de acesso restringe o acesso a partes do seu código a partir do código em outros arquivos e módulos de origem. Esse recurso permite ocultar os detalhes de implementação do seu código e especificar uma interface preferida através da qual esse código pode ser acessado e usado.

Você pode atribuir níveis de acesso específicos a tipos individuais (classes, estruturas e enumerações), bem como propriedades, métodos, inicializações e índices pertencentes a esses tipos. Os protocolos podem ser restritos a um determinado contexto, assim como constantes globais, variáveis e funções.

Além de oferecer vários níveis de controle de acesso, o Swift reduz a necessidade de especificar níveis de controle de acesso explícitos, fornecendo níveis de acesso padrão para cenários típicos. Na verdade, se você estiver escrevendo um aplicativo de destino único, talvez não precise especificar níveis de controle de acesso explícitos.

### Modules and Source Files

O modelo de controle de acesso da Swift baseia-se no conceito de módulos e arquivos de origem.

Um módulo é uma única unidade de distribuição de código - uma estrutura ou aplicativo que é construído e enviado como uma única unidade e que pode ser importado por outro módulo com a palavra-chave de importação do Swift.

Cada alvo de compilação (como um pacote ou estrutura de aplicativos) no Xcode é tratado como um módulo separado em Swift. Se você agrupar aspectos do código do seu aplicativo como uma estrutura autônoma - talvez para encapsular e reutilizar esse código em vários aplicativos -, tudo o que você define dentro dessa estrutura será parte de um módulo separado quando ele for importado e usado em um aplicativo, ou quando é usado em outra estrutura.

Um arquivo de origem é um único arquivo de código-fonte Swift dentro de um módulo (de fato, um único arquivo dentro de um aplicativo ou framework). Embora seja comum definir tipos individuais em arquivos de origem separados, um único arquivo de origem pode conter definições para vários tipos, funções e assim por diante.

### Níveis de acesso

Swift fornece cinco níveis de acesso diferentes para entidades dentro do seu código. Esses níveis de acesso são relativos ao arquivo de origem em que uma entidade é definida e também relativa ao módulo ao qual o arquivo de origem pertence.

- **Open access and public access** permitem que as entidades sejam usadas em qualquer arquivo fonte de seu módulo de definição, e também em um arquivo de origem de outro módulo que importa o módulo de definição. Você normalmente usa o acesso aberto ou público ao especificar a interface pública em uma estrutura. A diferença entre acesso aberto e público é descrita abaixo.
- **Internal access** permite que as entidades sejam usadas dentro de qualquer arquivo fonte de seu módulo de definição, mas não em qualquer arquivo fonte fora desse módulo. Você normalmente usa o acesso interno ao definir a estrutura interna de um aplicativo ou de uma estrutura.
- **File-private access** restringe o uso de uma entidade ao seu próprio arquivo fonte definidor. Use o acesso privado de arquivos para ocultar os detalhes de implementação de um tipo específico de funcionalidade quando esses detalhes são usados ​​em um arquivo inteiro.
- **Private access** restringe o uso de uma entidade a enclosing declaration, e às extensões dessa declaração que estão no mesmo arquivo. Use o acesso privado para ocultar os detalhes de implementação de um tipo específica de funcionalidade quando esses detalhes são usados ​​apenas em uma única declaração.

O acesso *open* é o nível de acesso mais elevado (menos restritivo) e o acesso privado é o nível de acesso mais baixo (mais restritivo).

O acesso aberto aplica-se apenas a classes e membros da classe, e difere do acesso público da seguinte forma:

- Classes com acesso público ou qualquer nível de acesso mais restritivo podem ser subclassificadas somente dentro do módulo onde elas são definidas.
- Membros de classe com acesso público, ou qualquer nível de acesso mais restritivo, podem ser substituídos por subclasses somente dentro do módulo onde eles estão definidos.
- As classes abertas podem ser subclassificadas dentro do módulo onde elas estão definidas e dentro de qualquer módulo que importa o módulo onde elas estão definidas.
- Os membros da classe aberta podem ser substituídos por subclasses dentro do módulo onde eles estão definidos e dentro de qualquer módulo que importa o módulo onde eles estão definidos.

Marcar uma classe como aberta indica explicitamente que você considerou o impacto do código de outros módulos usando essa classe como uma superclasse e que você criou o código da sua classe de acordo.

#### Níveis de acesso padrão

Todas as entidades em seu código (com algumas exceções específicas, conforme descrito mais adiante neste capítulo) têm um nível de acesso padrão de interno se você não especificar um nível de acesso explícito. Como resultado, em muitos casos, você não precisa especificar um nível de acesso explícito no seu código.

### Sintaxe de controle de acesso

Define the access level for an entity by placing one of the `open`, `public`, `internal`, `fileprivate`, or `private` modifiers before the entity’s introducer:

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}

public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

### Tipos personalizados

If you want to specify an explicit access level for a custom type, do so at the point that you define the type. The new type can then be used wherever its access level permits. For example, if you define a file-private class, that class can only be used as the type of a property, or as a function parameter or return type, in the source file in which the file-private class is defined.

The access control level of a type also affects the default access level of that type’s members (its properties, methods, initializers, and subscripts). If you define a type’s access level as private or file private, the default access level of its members will also be private or file private. If you define a type’s access level as internal or public (or use the default access level of internal without specifying an access level explicitly), the default access level of the type’s members will be internal.

```swift
public class SomePublicClass {                  // explicitly public class
    public var somePublicProperty = 0            // explicitly public class member
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

class SomeInternalClass {                       // implicitly internal class
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

fileprivate class SomeFilePrivateClass {        // explicitly file-private class
    func someFilePrivateMethod() {}              // implicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

private class SomePrivateClass {                // explicitly private class
    func somePrivateMethod() {}                  // implicitly private class member
}
```

#### Tuple Types

The access level for a tuple type is the most restrictive access level of all types used in that tuple. For example, if you compose a tuple from two different types, one with internal access and one with private access, the access level for that compound tuple type will be private.

> Tuple types don't have a standalone definition in the way that classes, structures, enumerations, and functions do. A tuple type's access level is deduced automatically when the tuple type is used, and can't be specified explicitly.

#### Function Types

O nível de acesso para um tipo de função é calculado como o nível de acesso mais restritivo dos tipos de parâmetros da função e do tipo de retorno. Você deve especificar o nível de acesso explicitamente como parte da definição da função se o nível de acesso calculado da função não corresponder ao padrão contextual.

O exemplo abaixo define uma função global chamada `someFunction()`, sem fornecer um modificador de nível de acesso específico para a própria função. Você pode esperar que esta função tenha o nível de acesso padrão de "interno", mas este não é o caso. Na verdade, `someFunction()` não compilará conforme escrito abaixo:

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // function implementation goes here
}
```

O tipo de retorno da função é um tipo de tupla composto de duas das classes personalizadas definidas acima em Tipos personalizados. Uma dessas classes foi definida como "interna", e a outra foi definida como "privada". Portanto, o nível de acesso geral do tipo de tupla composto é "privado" (o nível de acesso mínimo dos tipos de componentes da tupla).

Como o tipo de retorno da função é privado, você deve marcar o nível de acesso geral da função com o modificador privado para que a declaração da função seja válida:

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // function implementation goes here
}
```

Não é válido marcar a definição de `someFunction()` com os modificadores públicos ou internos ou usar a configuração padrão do interno, porque os usuários públicos ou internos da função podem não ter acesso apropriado à classe privada usada no tipo de retorno da função .

#### Enumeration Types

The individual cases of an enumeration automatically receive the same access level as the enumeration they belong to. You can’t specify a different access level for individual enumeration cases.

#### Nested Types

Nested types defined within a private type have an automatic access level of private. Nested types defined within a file-private type have an automatic access level of file private. Nested types defined within a public type or an internal type have an automatic access level of internal. If you want a nested type within a public type to be publicly available, you must explicitly declare the nested type as public.

### Subclassing

Você pode subclasse qualquer classe que possa ser acessada no contexto de acesso atual. Uma subclasse não pode ter um nível de acesso maior do que a sua superclasse - por exemplo, você não pode escrever uma subclasse pública de uma superclasse interna.

Além disso, você pode substituir qualquer membro da classe (método, propriedade, inicializador ou subíndice) visível em um determinado contexto de acesso.

Uma substituição pode tornar um membro da classe herdada mais acessível do que a sua versão de superclasse.

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {}
}
```

É mesmo válido para um membro da subclasse chamar um membro de superclasse que tenha permissões de acesso mais baixas do que o membro da subclasse, desde que a chamada para o membro da superclasse ocorra dentro de um contexto de nível de acesso permitido (isto é, dentro do mesmo arquivo de origem que o superclasse para uma chamada de membro privado de arquivo ou dentro do mesmo módulo que a superclasse para uma chamada de membro interno):

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {
        super.someMethod()
    }
}
```

Como a superclasse `A` e a subclasse `B` são definidas no mesmo arquivo de origem, é válido para a implementação `B` de `someMethod()` para chamar `super.someMethod()`.

### Constantes, Variáveis, Propriedades e Subscritos

Uma constante, variável ou propriedade não pode ser mais pública que seu tipo. Não é válido escrever uma propriedade pública com um tipo particular, por exemplo. Da mesma forma, um subíndice não pode ser mais público do que seu tipo de índice ou tipo de retorno.

Se uma constante, variável, propriedade ou subíndice faz uso de um tipo privado, a constante, variável, propriedade ou subíndice também deve ser marcado como privado:

```swift
private var privateInstance = SomePrivateClass()
```

#### Getters and Setters

Getters and setters for constants, variables, properties, and subscripts automatically receive the same access level as the constant, variable, property, or subscript they belong to.

You can give a setter a lower access level than its corresponding getter, to restrict the read-write scope of that variable, property, or subscript. You assign a lower access level by writing `fileprivate(set)`, `private(set)`, or `internal(set)` before the var or subscript introducer.

> This rule applies to stored properties as well as computed properties. Even though you don't write an explicit getter and setter for a stored property, Swift still synthesizes an implicit getter and setter for you to provide access to the stored property's backing storage. Use `fileprivate(set)`, `private(set)`, and `internal(set)` to change the access level of this synthesized setter in exactly the same way as for an explicit setter in a computed property.

```swift
struct TrackedString {
    private(set) var numberOfEdits = 0
    var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
}
```

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("The number of edits is \(stringToEdit.numberOfEdits)")
// Prints "The number of edits is 3"
```

Note that you can assign an explicit access level for both a getter and a setter if required. The example below shows a version of the `TrackedString` structure in which the structure is defined with an explicit access level of public. The structure’s members (including the `numberOfEdits` property) therefore have an internal access level by default. You can make the structure’s `numberOfEdits` property getter public, and its property setter private, by combining the public and `private(set)` access-level modifiers:

```swift
public struct TrackedString {
    public private(set) var numberOfEdits = 0
    public var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
    public init() {}
}
```

### Inicializadores

Os inicializadores personalizados podem receber um nível de acesso inferior ou igual ao tipo que eles inicializam. A única exceção é para os inicializadores necessários (conforme definido em Inicializadores Requeridos). Um inicializador necessário deve ter o mesmo nível de acesso que a classe a que pertence.

Tal como acontece com os parâmetros de função e método, os tipos de parâmetros de um iniciador não podem ser mais privados que o próprio nível de acesso do inicializador.

#### Inicializadores padrão

Conforme descrito em Inicializadores Padrão, o Swift fornece automaticamente um inicializador padrão sem argumentos para qualquer estrutura ou classe base que forneça valores padrão para todas as suas propriedades e não forneça pelo menos um inicializador.

Um inicializador padrão tem o mesmo nível de acesso que o tipo que ele inicializa, a menos que esse tipo seja definido como público. Para um tipo que é definido como público, o inicializador padrão é considerado interno. Se você deseja que um tipo público seja inicializado com um inicializador sem argumentos quando usado em outro módulo, você deve fornecer explicitamente um inicializador público sem argumentos como parte da definição do tipo.

### Protocolos

Se você deseja atribuir um nível de acesso explícito a um tipo de protocolo, faça isso no ponto em que você define o protocolo. Isso permite que você crie protocolos que só podem ser adotados dentro de um determinado contexto de acesso.

O nível de acesso de cada requisito dentro de uma definição de protocolo é automaticamente configurado para o mesmo nível de acesso que o protocolo. Você não pode definir um requisito de protocolo para um nível de acesso diferente do protocolo que ele suporta. Isso garante que todos os requisitos do protocolo serão visíveis em qualquer tipo que adote o protocolo.

> If you define a public protocol, the protocol's requirements require a public access level for those requirements when they'r implemented. This behavior is different from other types, where a public type definition implies an access level of internal for the type's members.

#### Herança de protocolo

Se você definir um novo protocolo que herda de um protocolo existente, o novo protocolo pode ter no máximo o mesmo nível de acesso que o protocolo do qual ele herda. Você não pode escrever um protocolo público que herda de um protocolo interno, por exemplo.

#### Conformidade do Protocolo

Um tipo pode ser conforme a um protocolo com um nível de acesso inferior ao do próprio tipo. Por exemplo, você pode definir um tipo público que pode ser usado em outros módulos, mas cuja conformidade com um protocolo interno só pode ser usada dentro do módulo de definição do protocolo interno.

O contexto em que um tipo está em conformidade com um protocolo específico é o mínimo do nível de acesso do tipo e o nível de acesso do protocolo. Se um tipo é público, mas um protocolo conforme ele é interno, a conformidade do tipo com esse protocolo também é interna.

Quando você escreve ou amplia um tipo para se adequar a um protocolo, você deve garantir que a implementação do tipo de cada requisito de protocolo tenha pelo menos o mesmo nível de acesso que a conformidade do tipo com esse protocolo. Por exemplo, se um tipo público estiver em conformidade com um protocolo interno, a implementação do tipo de cada requisito de protocolo deve ser pelo menos "interna".

### Extensões

Você pode estender uma classe, estrutura ou enumeração em qualquer contexto de acesso no qual a classe, a estrutura ou a enumeração estão disponíveis. Qualquer membro do tipo adicionado em uma extensão possui o mesmo nível de acesso padrão que os membros do tipo declarados no tipo original que está sendo estendido. Se você estender um tipo público ou interno, todos os novos membros de tipo que você adiciona têm um nível de acesso padrão de interno. Se você estender um tipo de arquivo privado, todos os novos membros que você adiciona têm um nível de acesso padrão de arquivo privado. Se você estender um tipo privado, qualquer membro novo que você adicione tenha um nível de acesso padrão de privacidade.

Alternativamente, você pode marcar uma extensão com um modificador de nível de acesso explícito (por exemplo, `private extension`) para definir um novo nível de acesso padrão para todos os membros definidos dentro da extensão. Este novo padrão ainda pode ser substituído dentro da extensão para membros de tipo individuais.

Você não pode fornecer um modificador de nível de acesso explícito para uma extensão se você estiver usando essa extensão para adicionar a conformidade do protocolo. Em vez disso, o próprio nível de acesso do protocolo é usado para fornecer o nível de acesso padrão para cada implementação de requisito de protocolo dentro da extensão.

#### Membros Privados em Extensões

As extensões que estão no mesmo arquivo que a classe, a estrutura ou a enumeração que se estendem se comportam como se o código na extensão tivesse sido escrito como parte da declaração do tipo original. Como resultado, você pode:

* Declare um membro privado na declaração original e acesse esse membro de extensões no mesmo arquivo.
* Declare um membro privado em uma extensão e acesse esse membro de outra extensão no mesmo arquivo.
* Declare um membro privado em uma extensão e acesse esse membro da declaração original no mesmo arquivo.

Esse comportamento significa que você pode usar extensões da mesma forma para organizar seu código, independentemente de seus tipos possuírem entidades privadas. Por exemplo, dado o seguinte protocolo simples:

```swift
protocol SomeProtocol {
    func doSomething()
}
```

Você pode usar uma extensão para adicionar a conformidade do protocolo, assim:

```swift
struct SomeStruct {
    private var privateVariable = 12
}

extension SomeStruct: SomeProtocol {
    func doSomething() {
        print(privateVariable)
    }
}
```

### Genéricos

The access level for a generic type or generic function is the minimum of the access level of the generic type or function itself and the access level of any type constraints on its type parameters.

### Type Aliases

Any type aliases you define are treated as distinct types for the purposes of access control. A type alias can have an access level less than or equal to the access level of the type it aliases. For example, a private type alias can alias a private, file-private, internal, public, or open type, but a public type alias can’t alias an internal, file-private, or private type.

> This rule also applies to type aliases for associated types used to satisfy protocol conformances.

## Operadores avançados

### Métodos do operador

Classes e estruturas podem fornecer suas próprias implementações de operadores existentes. Isso é conhecido como sobrecarregar os operadores existentes.

O exemplo abaixo mostra como implementar o operador de adição aritmética (`+`) para uma estrutura personalizada. O operador de adição aritmética é um operador binário porque ele opera em dois alvos e é dito que é infix porque aparece entre esses dois alvos.

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
```

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
```

#### Operadores de prefixos e postfixos

O exemplo mostrado acima demonstra uma implementação personalizada de um operador de infixo binário. Classes e estruturas também podem fornecer implementações dos operadores unários padrão. Operadores unários operam em um único alvo. Eles são prefixos se precederem seus objetivos (como `-a`) e postfix se eles seguirem seu alvo (como `b!`).

Você implementar um prefixo ou postfix operador unário escrevendo o modificador `prefix` ou `postfix` antes da palavra-chave `func` quando declarando o método do operador:

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative is a Vector2D instance with values of (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive is a Vector2D instance with values of (3.0, 4.0)
```

#### Operadores de atribuição de compostos

Os operadores de atribuição de compostos combinam a atribuição (`=`) com outra operação. Por exemplo, o operador de atribuição de adição (`+=`) combina adição e atribuição em uma única operação. Você marca o tipo de parâmetro de entrada esquerda do operador de atribuição composta como `inout`, porque o valor do parâmetro será modificado diretamente do método do operador.

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
```

#### Operadores de Equivalência

Classes e estruturas personalizadas não recebem uma implementação padrão dos operadores de equivalência, conhecido como operador "igual a" (`==`) e operador "não igual" (`!=`). Não é possível para Swift adivinhar o que qualificaria como "igual" para seus próprios tipos personalizados, porque o significado de "igual" depende das funções que esses tipos desempenham no seu código.

Para usar os operadores de equivalência para verificar a equivalência de seu próprio tipo personalizado, forneça uma implementação dos operadores da mesma maneira que para outros operadores de infixas:

```swift
extension Vector2D {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
    static func != (left: Vector2D, right: Vector2D) -> Bool {
        return !(left == right)
    }
}
```

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// Prints "These two vectors are equivalent."
```

### Custom Operators

You can declare and implement your own custom operators in addition to the standard operators provided by Swift. For a list of characters that can be used to define custom operators, see Operators.

New operators are declared at a global level using the `operator` keyword, and are marked with the `prefix`, `infix` or `postfix` modifiers:

```swift
prefix operator +++
```

```swift
extension Vector2D {
    static prefix func +++ (vector: inout Vector2D) -> Vector2D {
        vector += vector
        return vector
    }
}

var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled now has values of (2.0, 8.0)
// afterDoubling also has values of (2.0, 8.0)
```

#### Precedence for Custom Infix Operators

```swift
infix operator +-: AdditionPrecedence
extension Vector2D {
    static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y - right.y)
    }
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector is a Vector2D instance with values of (4.0, -2.0)
```

## Protocols

A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

In addition to specifying requirements that conforming types must implement, you can extend a protocol to implement some of these requirements or to implement additional functionality that conforming types can take advantage of.

### Protocol Syntax

```swift
protocol SomeProtocol {
    // protocol definition goes here
}
```

Tipos personalizados indicam que eles adotam um protocolo específico, colocando o nome do protocolo após o nome do tipo, separados por dois pontos, como parte de sua definição. Múltiplos protocolos podem ser listados e separados por vírgulas:

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```

Se uma classe tiver uma superclasse, liste o nome da superclasse antes de qualquer protocolo adotado, seguido de uma vírgula:

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```

### Property Requirements

A protocol can require any conforming type to provide an instance property or type property with a particular name and type. The protocol doesn’t specify whether the property should be a stored property or a computed property—it only specifies the required property name and type. The protocol also specifies whether each property must be gettable or gettable and settable.

If a protocol requires a property to be gettable and settable, that property requirement can’t be fulfilled by a constant stored property or a read-only computed property. If the protocol only requires a property to be gettable, the requirement can be satisfied by any kind of property, and it’s valid for the property to be also settable if this is useful for your own code.

Property requirements are always declared as variable properties, prefixed with the `var` keyword. Gettable and settable properties are indicated by writing `{ get set }` after their type declaration, and gettable properties are indicated by writing `{ get }`.

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

Always prefix type property requirements with the `static` keyword when you define them in a protocol. This rule pertains even though type property requirements can be prefixed with the `class` or `static` keyword when implemented by a class:

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

Here’s an example of a protocol with a single instance property requirement:

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

Aqui está um exemplo de uma estrutura simples que adota e está em conformidade com o protocolo `FullyNamed`:

```swift
struct Person: FullyNamed {
    var fullName: String
}

let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed"
```

Aqui está uma classe mais complexa, que também adota e está em conformidade com o protocolo `FullyNamed`:

```swift
class Starship: FullyNamed {
    var prefix: String?
    var name: String

    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }

    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}

var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName is "USS Enterprise"
```

### Method Requirements

Protocols can require specific instance methods and type methods to be implemented by conforming types. These methods are written as part of the protocol’s definition in exactly the same way as for normal instance and type methods, but without curly braces or a method body. Variadic parameters are allowed, subject to the same rules as for normal methods. Default values, however, can’t be specified for method parameters within a protocol’s definition.

As with type property requirements, you always prefix type method requirements with the `static` keyword when they’re defined in a protocol. This is true even though type method requirements are prefixed with the `class` or `static` keyword when implemented by a class:

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}
```

The following example defines a protocol with a single instance method requirement:

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c).truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}

let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.37464991998171"
print("And another one: \(generator.random())")
// Prints "And another one: 0.729023776863283"
```

### Mutating Method Requirements

It’s sometimes necessary for a method to modify (or _mutate_) the instance it belongs to. For instance methods on value types (that is, structures and enumerations) you place the `mutating` keyword before a method’s `func` keyword to indicate that the method is allowed to modify the instance it belongs to and any properties of that instance. This process is described in [Modifying Value Types from Within Instance Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html#ID239).

If you define a protocol instance method requirement that is intended to mutate instances of any type that adopts the protocol, mark the method with the `mutating` keyword as part of the protocol’s definition. This enables structures and enumerations to adopt the protocol and satisfy that method requirement.

```swift
protocol Togglable {
    mutating func toggle()
}
```

```swift
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}

var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch is now equal to .on
```

### Initializer Requirements

Protocols can require specific initializers to be implemented by conforming types. You write these initializers as part of the protocol’s definition in exactly the same way as for normal initializers, but without curly braces or an initializer body:

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

#### Class Implementations of Protocol Initializer Requirements

You can implement a protocol initializer requirement on a conforming class as either a designated initializer or a convenience initializer. In both cases, you must mark the initializer implementation with the `required` modifier:

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```

The use of the `required` modifier ensures that you provide an explicit or inherited implementation of the initializer requirement on all subclasses of the conforming class, such that they also conform to the protocol.

If a subclass overrides a designated initializer from a superclass, and also implements a matching initializer requirement from a protocol, mark the initializer implementation with both the `required` and `override` modifiers:

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init() {
        // initializer implementation goes here
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass
    required override init() {
        // initializer implementation goes here
    }
}
```

#### Failable Initializer Requirements

Protocols can define failable initializer requirements for conforming types, as defined in Failable Initializers.

A failable initializer requirement can be satisfied by a failable or nonfailable initializer on a conforming type. A nonfailable initializer requirement can be satisfied by a nonfailable initializer or an implicitly unwrapped failable initializer.

### Protocols as Types

Os protocolos realmente não implementam nenhuma funcionalidade. No entanto, qualquer protocolo que você crie se tornará um tipo completo para uso em seu código.

Porque é um tipo, você pode usar um protocolo em muitos lugares onde outros tipos são permitidos, incluindo:

- Como tipo de parâmetro ou tipo de retorno em uma função, método ou inicializador
- Como o tipo de uma constante, variável ou propriedade
- Como o tipo de itens em uma matriz, dicionário ou outro recipiente

Aqui está um exemplo de um protocolo usado como um tipo:

```swift
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

```swift
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Random dice roll is \(d6.roll())")
}

// Random dice roll is 3
// Random dice roll is 5
// Random dice roll is 4
// Random dice roll is 5
// Random dice roll is 4
```

### Delegation

Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.

The example below defines two protocols for use with dice-based board games:

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}

protocol DiceGameDelegate {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```

```swift
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    init() {
        board = Array(repeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("Rolled a \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}
```

```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()

game.delegate = tracker
game.play()
// Started a new game of Snakes and Ladders
// The game is using a 6-sided dice
// Rolled a 3
// Rolled a 5
// Rolled a 4
// Rolled a 5
// The game lasted for 4 turns
```

### Adding Protocol Conformance with an Extension

Você pode estender um tipo existente para adotar e se adequar a um novo protocolo, mesmo se você não tiver acesso ao código-fonte para o tipo existente. As extensões podem adicionar novas propriedades, métodos e subíndices a um tipo existente e, portanto, podem adicionar quaisquer requisitos que um protocolo possa exigir.

Por exemplo, este protocolo, chamado `TextRepresentable`, pode ser implementado por qualquer tipo que tenha uma maneira de ser representado como texto. Esta pode ser uma descrição de si mesma, ou uma versão de texto do seu estado atual:

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}
```

A classe anterior `Dice` pode ser estendida para adotar e se adequar a `TextRepresentable`:

```swift
extension Dice: TextRepresentable {
    var textualDescription: String {
        return "A \(sides)-sided dice"
    }
}
```

```swift
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// Prints "A 12-sided dice"
```

Da mesma forma, a classe `SnakesAndLadders` do jogo pode ser estendida para adotar e se adequar ao protocolo `TextRepresentable`:

```swift
extension SnakesAndLadders: TextRepresentable {
    var textualDescription: String {
        return "A game of Snakes and Ladders with \(finalSquare) squares"
    }
}

print(game.textualDescription)
// Prints "A game of Snakes and Ladders with 25 squares"
```

#### Declaring Protocol Adoption with an Extension

If a type already conforms to all of the requirements of a protocol, but has not yet stated that it adopts that protocol, you can make it adopt the protocol with an empty extension:

```swift
struct Hamster {
    var name: String
    var textualDescription: String {
        return "A hamster named \(name)"
    }
}

extension Hamster: TextRepresentable {}
```

Instances of `Hamster` can now be used wherever `TextRepresentable` is the required type:

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster

print(somethingTextRepresentable.textualDescription)
// Prints "A hamster named Simon"
```

### Collections of Protocol Types

Um protocolo pode ser usado como o tipo a ser armazenado em uma coleção, como uma matriz ou um dicionário.

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]
```

Agora é possível iterar sobre os itens na matriz e imprimir a descrição textual de cada item:

```swift
for thing in things {
    print(thing.textualDescription)
}

// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

### Protocol Inheritance

Um protocolo pode herdar um ou mais outros protocolos e pode adicionar requisitos adicionais em cima dos requisitos que ele herda. A sintaxe para herança de protocolo é semelhante à sintaxe da herança de classe, mas com a opção de listar múltiplos protocolos herdados, separados por vírgulas:

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // protocol definition goes here
}
```

Aqui está um exemplo de um protocolo que herda o protocolo `TextRepresentable` de cima:

```swift
protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}
```

A classe `SnakesAndLadders` pode ser estendida para adotar e se adequar a `PrettyTextRepresentable`:

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

The `prettyTextualDescription` property can now be used to print a pretty text description of any `SnakesAndLadders` instance:

```swift
print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

### Class-Only Protocols

Você pode limitar a adoção do protocolo aos tipos de classe (e não a estruturas ou enumerações), adicionando o protocolo `AnyObject` à lista de herança de um protocolo.

```swift
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
```

No exemplo acima, `SomeClassOnlyProtocol` só pode ser adotado por tipos de classe. É um erro de tempo de compilação para escrever uma estrutura ou definição de enumeração que tenta adotar `SomeClassOnlyProtocol`.

### Protocol Composition

It can be useful to require a type to conform to multiple protocols at the same time. You can combine multiple protocols into a single requirement with a *protocol composition*. Protocol compositions behave as if you defined a temporary local protocol that has the combined requirements of all protocols in the composition. Protocol compositions don’t define any new protocol types.

Protocol compositions have the form `SomeProtocol` & `AnotherProtocol`. You can list as many protocols as you need, separating them with ampersands (`&`). In addition to its list of protocols, a protocol composition can also contain one class type, which you can use to specify a required superclass.

Here’s an example that combines two protocols called `Named` and `Aged` into a single protocol composition requirement on a function parameter:

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// Prints "Happy birthday, Malcolm, you're 21!"
```

Here’s an example that combines the `Named` protocol from the previous example with a `Location` class:

```swift
class Location {
    var latitude: Double
    var longitude: Double
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
class City: Location, Named {
    var name: String
    init(name: String, latitude: Double, longitude: Double) {
        self.name = name
        super.init(latitude: latitude, longitude: longitude)
    }
}
func beginConcert(in location: Location & Named) {
    print("Hello, \(location.name)!")
}

let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
beginConcert(in: seattle)
// Prints "Hello, Seattle!"
```

### Checking for Protocol Conformance

You can use the `is` and `as` operators described in Type Casting to check for protocol conformance, and to cast to a specific protocol. Checking for and casting to a protocol follows exactly the same syntax as checking for and casting to a type:

* The `is` operator returns `true` if an instance conforms to a protocol and returns `false` if it doesn’t.
* The `as?` version of the downcast operator returns an optional value of the protocol’s type, and this value is nil if the instance doesn’t conform to that protocol.
* The `as!` version of the downcast operator forces the downcast to the protocol type and triggers a runtime error if the downcast doesn’t succeed.

This example defines a protocol called `HasArea`, with a single property requirement of a gettable `Double` property called `area`:

```swift
protocol HasArea {
    var area: Double { get }
}
```

Here are two classes, `Circle` and `Country`, both of which conform to the `HasArea` protocol:

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

Here’s a class called `Animal`, which doesn’t conform to the `HasArea` protocol:

```swift
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}
```

```swift
let objects: [AnyObject] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]
```

The `objects` array can now be iterated, and each object in the array can be checked to see if it conforms to the `HasArea` protocol:

```swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        print("Area is \(objectWithArea.area)")
    } else {
        print("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```

### Optional Protocol Requirements

### Protocol Extensions

Protocols can be extended to provide method and property implementations to conforming types. This allows you to define behavior on protocols themselves, rather than in each type’s individual conformance or in a global function.

For example, the `RandomNumberGenerator` protocol can be extended to provide a `randomBool()` method, which uses the result of the required `random()` method to return a random `Bool` value:

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}
```

```swift
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.37464991998171"
print("And here's a random Boolean: \(generator.randomBool())")
// Prints "And here's a random Boolean: true"
```

#### Providing Default Implementations

You can use protocol extensions to provide a default implementation to any method or computed property requirement of that protocol. If a conforming type provides its own implementation of a required method or property, that implementation will be used instead of the one provided by the extension.

For example, the `PrettyTextRepresentable` protocol, which inherits the `TextRepresentable` protocol can provide a default implementation of its required `prettyTextualDescription` property to simply return the result of accessing the `textualDescription` property:

```swift
extension PrettyTextRepresentable  {
    var prettyTextualDescription: String {
        return textualDescription
    }
}
```

#### Adding Constraints to Protocol Extensions

When you define a protocol extension, you can specify constraints that conforming types must satisfy before the methods and properties of the extension are available. You write these constraints after the name of the protocol you’re extending using a generic `where` clause, as described in Generic Where Clauses.

For instance, you can define an extension to the `Collection` protocol that applies to any collection whose elements conform to the `TextRepresentable` protocol from the example above.

```swift
extension Collection where Iterator.Element: TextRepresentable {
    var textualDescription: String {
        let itemsAsText = self.map { $0.textualDescription }
        return "[" + itemsAsText.joined(separator: ", ") + "]"
    }
}
```

```swift
let murrayTheHamster = Hamster(name: "Murray")
let morganTheHamster = Hamster(name: "Morgan")
let mauriceTheHamster = Hamster(name: "Maurice")
let hamsters = [murrayTheHamster, morganTheHamster, mauriceTheHamster]
```

```swift
print(hamsters.textualDescription)
// Prints "[A hamster named Murray, A hamster named Morgan, A hamster named Maurice]"
```
