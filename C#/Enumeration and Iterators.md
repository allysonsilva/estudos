# Enumeration and Iterators

## Enumeration

- An *enumerator* is a read-only, forward-only cursor over a sequence of values.
- Um enumerator é um objeto que implementa qualquer uma das seguintes interfaces:
    - `System.Collections.IEnumerator`
    - `System.Collections.Generic.IEnumerator<T>`
- `foreach` itera sobre um objeto *enumerable*.
- An *enumerable* object is the logical representation of a sequence. It is not itself a cursor, but an object that produces cursors over itself.
- Um objeto *enumerable*:
    - Implementa `IEnumerable` ou `IEnumerable<T>`.
    - Tem um método chamado `GetEnumerator` que retorna um `enumerator`.
- `IEnumerator` and `IEnumerable` are defined in `System.Collections`. `IEnumerator<T>` and `IEnumerable<T>` are defined in `System.Collections.Generic`.

-------------

`IEnumerator` and `IEnumerable` are defined in `System.Collections`. `IEnumerator<T>` and `IEnumerable<T>` are defined in `System.Collections.Generic`.

`IEnumerable` é uma interface que define um método `GetEnumerator` que retorna uma interface do `IEnumerator`, isso, por sua vez, permite acesso direto a uma coleção.

-------------

*If the enumerator implements `IDisposable`, the foreach statement also acts as a using statement, implicitly disposing the enumerator object.*

O padrão *enumeration* é o seguinte:

```csharp
class Enumerator // Typically implements IEnumerator or IEnumerator<T>
{
    public IteratorVariableType Current { get {...} }
    public bool MoveNext() {...}
}

class Enumerable // Typically implements IEnumerable or IEnumerable<T>
{
    public Enumerator GetEnumerator() {...}
}
```

Aqui está a maneira **de alto nível(*high-level*)** de iteração através dos caracteres na palavra `beer` usando uma instrução `foreach`:

```csharp
foreach(char c in "beer")
    Console.WriteLine(c);
```

Aqui está a **maneira de baixo nível** de iteração através dos caracteres da palavra `beer` sem usar uma declaração `foreach`:

```csharp
using (var enumerator = "beer".GetEnumerator())
    while (enumerator.MoveNext())
    {
        var element = enumerator.Current;
        Console.WriteLine(element);
    }
```

Se o *enumerator* implementar `IDisposable`, a instrução `foreach` também atua como uma declaração de `using`, descartando implicitamente o objeto *enumerator*.

## Collection Initializers

Você pode instanciar e popular o objeto enumerable em uma única etapa. Veja a seguir:

```csharp
using System.Collections.Generic;
...

List<int> list = new List<int> {1, 2, 3};
```

The compiler translates this to the following:

```csharp
using System.Collections.Generic;
...

List<int> list = new List<int>();
list.Add(1);
list.Add(2);
list.Add(3);
```

Isso requer que o objeto enumerável implemente a interface `System.Collections.IEnumerable` e que ele tenha um método `Add` que tenha o número apropriado de parâmetros para a chamada. Você pode inicializar dicionários da seguinte maneira:

```csharp
var dict = new Dictionary<int, string>()
{
    { 5, "five" },
    { 10, "ten" }
};
```

Or more succinctly:

```csharp
var dict = new Dictionary<int, string>()
{
    [3] = "three",
    [10] = "ten"
};
```

O último é válido não apenas com dicionários, mas com qualquer tipo para o qual existe um indexador.

## Iterators

Enquanto uma declaração `foreach` é um consumidor de um *enumerador*, um *iterador* é um produtor de um *enumerador*. Neste exemplo, usamos um *iterador* para retornar uma seqüência de números de Fibonacci (onde cada número é a soma dos dois anteriores):

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static void Main()
    {
        foreach(int fib in Fibs(6))
            Console.Write(fib + " ");
    }

    static IEnumerable<int> Fibs(int fibCount)
    {
        for(int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++) {
            yield return prevFib;
            int newFib = prevFib+curFib;
            prevFib = curFib;
            curFib = newFib;
        }
    }
}

// Output: 1 1 2 3 5 8
```

Considerando que uma declaração de `return` expressa "Aqui está o valor que você me pediu para retornar deste método", uma declaração de `yield return` expressa "Aqui está o próximo elemento que você me pediu para produção deste enumerador." Em cada declaração de `yield`, o controle é retornado ao chamador, mas o estado do receptor é mantido para que o método possa continuar sendo executado assim que o chamador enumera o próximo elemento. A vida útil desse estado é vinculada ao enumerador, de modo que o estado possa ser liberado quando o responsável pela chamada terminar de enumerar.

*The compiler converts iterator methods into private classes that implement `IEnumerable<T>` and/or `IEnumerator<T>`. The logic within the iterator block is “inverted” and spliced into the `MoveNext` method and `Current` property on the compiler written enumerator class. This means that when you call an iterator method, all you’re doing is instantiating the compiler written class; none of your code actually runs! Your code runs only when you start enumerating over the resultant sequence, typically with a foreach statement.*

## Iterator Semantics

An *iterator* is a method, property, or indexer that contains one or more `yield` statements. An *iterator* must return one of the following four interfaces (otherwise, the compiler will generate an error):

```csharp
// Enumerable interfaces
System.Collections.IEnumerable
System.Collections.Generic.IEnumerable<T>

// Enumerator interfaces
System.Collections.IEnumerator
System.Collections.Generic.IEnumerator<T>
```

An iterator has different semantics, depending on whether it returns an *enumerable* interface or an *enumerator* interface.

*Multiple yield statements* are permitted. For example:

```csharp
class Test
{
    static void Main()
    {
        foreach(string s in Foo())
            Console.WriteLine(s);   // Prints "One","Two","Three"
    }

    static IEnumerable<string> Foo()
    {
        yield return "One";
        yield return "Two";
        yield return "Three";
    }
}
```

### yield break

The `yield break` statement indicates that the iterator block should exit early, without returning more elements. We can modify Foo as follows to demonstrate:

```csharp
static IEnumerable<string> Foo(bool breakEarly)
{
    yield return "One";
    yield return "Two";

    if (breakEarly)
        yield break;
        yield return "Three";
}
```

*A return statement is illegal in an iterator block—you must use a `yield break` instead.*

### Iterators and try/catch/finally blocks

A `yield return` statement cannot appear in a `try` block that has a `catch` clause:

```csharp
IEnumerable<string> Foo()
{
    try { yield return "One"; } // Illegal
    catch { ... }
}
```

Nor can `yield return` appear in a `catch` or `finally` block. These restrictions are due to the fact that the compiler must translate iterators into ordinary classes with `MoveNext`, `Current`, and `Dispose` members, and translating exception handling blocks would create excessive complexity.


```csharp
IEnumerable<string> Foo()
{
    try { yield return "One"; } // OK
    finally { ... }
}
```

The code in the `finally` block executes when the consuming enumerator reaches the end of the sequence or is disposed. A foreach statement implicitly disposes the enumerator if you break early, making this a safe way to consume enumerators. When working with enumerators explicitly, a trap is to abandon enumeration early without disposing it, circumventing the `finally` block. You can avoid this risk by wrapping explicit use of enumerators in a `using` statement:

```csharp
string firstElement = null;
var sequence = Foo();

using (var enumerator = sequence.GetEnumerator())
    if (enumerator.MoveNext())
        firstElement = enumerator.Current;
```

## Composing Sequences

Iterators are highly composable. We can extend our example, this time to output even Fibonacci numbers only:

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static void Main()
    {
        foreach(int fib in EvenNumbersOnly(Fibs(6)))
            Console.WriteLine(fib);
    }

    static IEnumerable<int> Fibs(int fibCount)
    {
        for (int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++) {
            yield return prevFib;
            int newFib = prevFib+curFib;
            prevFib = curFib;
            curFib = newFib;
        }
    }

    static IEnumerable<int> EvenNumbersOnly(IEnumerable<int> sequence)
    {
        foreach(int x in sequence)
            if ((x % 2) == 0)
                yield return x;
    }
}
```

Each element is not calculated until the last moment—when requested by a `Move` `Next()` operation. Figure shows the data requests and data output over time.

![Fibonacci Composing Sequences](./Images/Fibonacci%20Composing%20Sequences.png "Fibonacci Composing Sequences")

*The composability of the iterator pattern is extremely useful in LINQ;*
