# Anonymous Types

> An anonymous type is a simple class created by the compiler on the fly to store a set of values. To create an anonymous type, use the `new` keyword followed by an object initializer, specifying the properties and values the type will contain.

-   Um tipo anônimo é uma classe simples criada pelo compilador na hora de armazenar um conjunto de valores.
-   Para criar um tipo anônimo, use a `new` palavra chave seguida de um inicializador de objeto, especificando as propriedades e os valores que o tipo conterá.
-   Deve-se usar a palavra chave `var` para fazer referência a um tipo anônimo porque não possui um nome.
-   São usados principalmente para escrever consultas LINQ.
-   Foram adicionados na versão 3.0 do C#.

For example:

```csharp
var dude = new { Name = "Bob", Age = 23 };
```

O compilador traduz isso para(aproximadamente) o seguinte:

```csharp
internal class AnonymousGeneratedTypeName
{
    private string name;    // Actual field name is irrelevant
    private int age;        // Actual field name is irrelevant

    public AnonymousGeneratedTypeName(string name, int age)
    {
        this.name = name;
        this.age = age;
    }

    public string Name { get { return name; } }
    public int Age { get { return age; } }

    // The Equals and GetHashCode methods are overridden.
    // The ToString method is also overridden.
}
...
var dude = new AnonymousGeneratedTypeName("Bob", 23);
```

**You must use the `var` keyword to reference an anonymous type because it doesn’t have a name**.

The property name of an anonymous type can be inferred from an expression that is itself an identifier(or ends with one). For example:

```csharp
int Age = 23;
var dude = new { Name = "Bob", Age, Age.ToString().Length };
```

is equivalent to:

```csharp
var dude = new { Name = "Bob", Age = Age, Length = Age.ToString().Length };
```

Two anonymous type instances declared within the same assembly will have the same underlying type if their elements are named and typed identically:

```csharp
var a1 = new { X = 2, Y = 4 };
var a2 = new { X = 2, Y = 4 };

Console.WriteLine(a1.GetType() == a2.GetType()); // True
```

Additionally, the `Equals` method is overridden to perform equality comparisons:

```csharp
Console.WriteLine(a1 == a2);        // False
Console.WriteLine(a1.Equals(a2));  // True
```

You can create arrays of anonymous types as follows:

```csharp
var dudes = new[]
{
    new { Name = "Bob", Age = 30 },
    new { Name = "Tom", Age = 40 }
};
```

A method cannot(usefully) return an anonymously typed object, because it is illegal to write a method whose return type is `var`:

```csharp
var Foo() => new { Name = "Bob", Age = 30 };    // Not legal!
```

Instead, you must use `object` or `dynamic` and then whoever calls Foo has to rely on dynamic binding, with loss of static type safety(and IntelliSense in Visual Studio):

```csharp
dynamic Foo() => new { Name = "Bob", Age = 30 };    // No static type safety.
```

Anonymous types are used primarily when writing LINQ queries, and were added in C# 3.0.
