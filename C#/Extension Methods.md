# Extension Methods

> Extension methods allow an existing type to be extended with new methods without altering the definition of the original type. An extension method is a static method of a static class, where the `this` modifier is applied to the first parameter. The type of the first parameter will be the type that is extended.

- Os métodos de extensão permitem que um tipo existente seja estendido com novos métodos sem alterar a definição do tipo original.
- Um método de extensão é um método estático de uma classe estática, onde `this` modificador é aplicado ao primeiro parâmetro. O tipo do primeiro parâmetro será o tipo que é estendido.
- Os métodos de extensão foram adicionados no C# 3.0.

Por exemplo:

```csharp
public static class StringHelper
{
    public static bool IsCapitalized(this string s)
    {
        if (string.IsNullOrEmpty(s))
            return false;

        return char.IsUpper(s[0]);
    }
}
```

The `IsCapitalized` *extension method* can be called as though it were an instance method on a string, as follows:

```csharp
Console.WriteLine("Allyson".IsCapitalized());
```

An extension method `call`, when compiled, is translated back into an ordinary static method call:

```csharp
Console.WriteLine(StringHelper.IsCapitalized ("Allyson"));
```

The translation works as follows:

```csharp
arg0.Method (arg1, arg2, ...); // Extension method call
StaticClass.Method (arg0, arg1, arg2, ...); // Static method call
```

Interfaces can be extended, too:

```csharp
public static T First<T>(this IEnumerable<T> sequence)
{
    foreach (T element in sequence)
        return element;

    throw new InvalidOperationException ("No elements!");
}
...
Console.WriteLine ("Seattle".First());  // S
```

## Extension Method Chaining

Extension methods, like instance methods, provide a tidy way to chain functions. Consider the following two functions:

```csharp
public static class StringHelper
{
    public static string Pluralize(this string s) {...}
    public static string Capitalize(this string s) {...}
}
```

`x` and `y` are equivalent and both evaluate to "Sausages", but `x` uses extension methods, whereas `y` uses static methods:

```csharp
string x = "sausage".Pluralize().Capitalize();
string y = StringHelper.Capitalize(StringHelper.Pluralize ("sausage"));
```

## Ambiguity and Resolution

### Namespaces

Um método de extensão não pode ser acessado a menos que sua classe esteja no escopo, geralmente pelo seu namespace sendo importado.

Consider the extension method `IsCapitalized` in the following example:

```csharp
using System;

namespace Utils
{
    public static class StringHelper
    {
        public static bool IsCapitalized(this string s)
        {
            if (string.IsNullOrEmpty(s))
                return false;

            return char.IsUpper(s[0]);
        }
    }
}
```

Para usar `IsCapitalized`, o seguinte application deve importar `Utils` para evitar um erro de compilação:

```csharp
namespace MyApp
{
    using Utils;
    class Test
    {
        static void Main() => Console.WriteLine("Allyson".IsCapitalized());
    }
}
```

### Extension methods versus instance methods

Qualquer método de instância compatível terá sempre precedência sobre um método de extensão.

In the following example, Test’s `Foo` method will always take precedenceeven when called with an argument `x` of type `int`:

```csharp
class Test
{
    public void Foo(object x) { } // This method always wins
}

static class Extensions
{
    public static void Foo(this Test t, int x) { }
}
```

The only way to call the extension method in this case is via normal static syntax; in other words, `Extensions.Foo(...)`.

### Extension methods versus extension methods

If two extension methods have the same signature, the extension method must be called as an ordinary static method to disambiguate the method to call. If one extension method has more specific arguments, however, the more specific method takes precedence.

To illustrate, consider the following two classes:

```csharp
static class StringHelper
{
    public static bool IsCapitalized(this string s) {...}
}

static class ObjectHelper
{
    public static bool IsCapitalized(this object s) {...}
}
```

O código a seguir chama o método `IsCapitalized` de `StringHelper`:

```csharp
bool test1 = "Allyson".IsCapitalized();
```

Classes and structs are considered more specific than interfaces.
