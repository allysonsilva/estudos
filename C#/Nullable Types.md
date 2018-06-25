# Nullable Types

> Os tipos de referência podem representar um valor inexistente com uma referência nula. Tipos de valor, no entanto, normalmente não podem representar valores nulos.

Por exemplo:

```csharp
string s = null;    // OK, Reference Type
int i = null;       // Compile Error, Value Type cannot be null
```

Para representar `null` em um tipo de valor, você deve usar um `construct` especial chamada de _nullable type_. Um _nullable type_ é denotado com um tipo de valor seguido pelo símbolo `?`:

```csharp
int? i = null;                  // OK, Nullable Type
Console.WriteLine(i == null);  // True
```

## Nullable<T> struct

`T?` translates into `System.Nullable<T>` que é uma estrutura leve e imutável, com apenas dois campos, para representar `Valeu` e `HasValue`. A essência de `System.Nullable<T>` é muito simples:

```csharp
public struct Nullable<T> where T : struct
{
    public T Value { get; }
    public bool HasValue { get; }

    public T GetValueOrDefault();
    public T GetValueOrDefault(T defaultValue);
    ...
}
```

Assim o código:

```csharp
int? i = null;
Console.WriteLine(i == null);       // True
```

É convertido para:

```csharp
Nullable<int> i = new Nullable<int>();
Console.WriteLine(!i.HasValue);    // True
```

Attempting to retrieve Value when `HasValue` is false throws an `InvalidOperationException`. `GetValueOrDefault()` returns Value if `HasValue` is `true`; otherwise, it returns `new T()` or a specified custom default value.

O valor padrão de `T?` é `null`.

## Implicit and explicit nullable conversions

> The conversion from `T` to `T?` is implicit, and from `T?` to `T` is explicit.

For example:

```csharp
int? x = 5;     // implicit
int y = (int)x; // explicit
```

The explicit cast is directly equivalent to calling the nullable object’s `Value` property. Hence, an `InvalidOperationException` is thrown if `HasValue` is `false`.

## Boxing and Unboxing Nullable Values

When `T?` is boxed, the boxed value on the heap contains `T`, not `T?`. This optimization is possible because a boxed value is a reference type that can already express null.

C# also permits the unboxing of nullable types with the as operator. The result will be `null` if the cast fails:

```csharp
object o = "string";
int? x = o as int?;
Console.WriteLine(x.HasValue); // False
```

## Nullable Types & Null Operators

Os tipos anuláveis ​​funcionam particularmente bem com o `??` operador:

```csharp
int? x = null;
int y = x ?? 5; // y is 5

int? a = null, b = 1, c = 2;
Console.WriteLine(a ?? b ?? c); // 1 (first non-null value)
```

Using `??` on a nullable value type is equivalent to calling `GetValueOrDefault` with an explicit default value, except that the expression for the default value is never evaluated if the variable is not null.

Os tipos anuláveis ​​também funcionam bem com o operador nulo-condicional. In the following example, `length` evaluates to `null`:

```csharp
System.Text.StringBuilder sb = null;
int? length = sb?.ToString().Length;
```

Podemos combinar isso com o operador null-coalescing para avaliar a zero em vez de nulo:

```csharp
int length = sb?.ToString().Length ?? 0; // Evaluates to 0 if sb is null
```

## Scenarios for Nullable Types

One of the most common scenarios for nullable types is to represent unknown values. This frequently occurs in database programming, where a class is mapped to a table with nullable columns. If these columns are strings(e.g., an `EmailAddress` column on a Customer table), there is no problem, as string is a reference type in the CLR, which can be null. However, most other SQL column types map to CLR struct types, making nullable types very useful when mapping SQL to the CLR. For example:

```csharp
// Maps to a Customer table in a database
public class Customer
{
    ...
    public decimal? AccountBalance;
}
```

A nullable type can also be used to represent the backing field of what’s sometimes called an _ambient property_. An ambient property, if null, returns the value of its parent. For example:

```csharp
public class Row
{
    ...
    Grid parent;
    Color? color;

    public Color Color
    {
        get { return color ?? parent.Color; }
        set { color = value == parent.Color ? (Color?)null : value; }
    }
}
```
