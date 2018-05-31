# Delegates

> A delegate is an object that knows how to call a method.

**A *delegate type* defines the kind of method that *delegate instances* can call. Specifically, it defines the method’s return type and its *parameter types*.**

- Um *delegate type* define o modelo, tipo de método que as *delegate instances* podem chamar.
- Especificamente o *delegate* define o tipo de retorno do método e seus parâmetros, servindo assim como um modelo, uma forma de chamar determinados métodos.
- Atribuir um método a uma instância de `delegate` cria uma instância de delegate, sem a necessidade do operador `new`.
- Um `delegate` pode ser invocado da mesma maneira que um método, abrindo e fechando aspas.
- Um `delegate` nada mais é do que um simples ***callback***.
- Delegates tem a capacidade de ser *Multicast*. Isso significa que uma instância de `delegate` pode fazer referência não apenas de um único método de destino, mais também a uma lista de métodos de destino, por meio do operador `+` e `+=` combinando instâncias delegates.
- Delegates are immutable, so when you call += or -=, you’re in fact creating a new delegate instance and assigning it to the existing variable.

The following defines a delegate type called `Transformer`:

```csharp
delegate int Transformer(int x);
```

O delegate `Transformer` é compatível com qualquer método que retorne um `int` e tenha um único parâmetro do tipo `int`, não importando o nome mais o tipo do parâmetro, como o método a seguir:

```csharp
static int Square(int x) { return x * x; }
// Expression-bodied syntax
static int Square(int x) => x * x;
```

Criando instância de delegate, **atribuindo somente o nome do método ao nome do delegate**:

```csharp
Transformer t = Square;
```

Um delegate é invocado da mesma forma que qualquer método:

```csharp
int answer = t(3); // 9
```

Exemplo mais completo:

```csharp
delegate int Transformer(int x);

class Test
{
    static void Main()
    {
        Transformer t = Square;     // Create delegate instance
        int result = t(3);          // Invoke delegate
        Console.WriteLine(result); // 9
    }

    static int Square(int x) => x * x;
}
```

*A delegate instance literally acts as a delegate for the caller: the caller invokes the delegate, and then the delegate calls the target method. This indirection decouples the caller from the target method.*

A seguinte declaração:

```csharp
Transformer t = Square;
```

É o mesmo(shorthand) que:

```csharp
Transformer t = new Transformer(Square);
```

*Technically, we are specifying a method group when we refer to `Square` without brackets or arguments. If the method is overloaded, C# will pick the correct overload based on the signature of the delegate to which it’s being assigned.*

A Expressão:

```csharp
t(3)
```

É o mesmo(shorthand) que:

```csharp
t.Invoke(3)
```

## Writing Plug-in Methods with Delegates

> A delegate variable is assigned a method at runtime. This is useful for writing plug-in methods.

In this example, we have a utility method named `Transform` that applies a transform to each element in an integer array. The `Transform` method has a delegate parameter, for specifying a plug-in transform.

```csharp
public delegate int Transformer(int x);

class Util
{
    public static void Transform(int[] values, Transformer t)
    {
        for (int i = 0; i < values.Length; i++)
            values[i] = t(values[i]);
    }
}

class Test
{
    static void Main()
    {
        int[] values = { 1, 2, 3 };

        Util.Transform(values, Square); // Hook in the Square method

        foreach (int i in values)
            Console.Write (i + " ");    // 1 4 9
    }

    static int Square(int x) => x * x;
}
```

Our `Transform` method is a *higher-order* function, because it’s a function that takes a function as an argument. (A method that *returns* a delegate would also be a higher-order function.)

## Multicast Delegates

> All delegate instances have *multicast* capability. This means that a delegate instance can reference not just a single target method, but also a list of target methods. The `+` and `+=` operators combine delegate instances.

- Capacidade de referenciar a múltiplos métodos de destino, não apenas um mais quantos quiser. Utilizando o operador `+` e `+=` pode-se adicionar métodos a instância de delegate.
- São invocados na ordem em que são adicionados.
- The `-` and `-=` operators remove the right delegate operand from the left delegate operand.
- Adicionar e remover um valor nulo também funciona.
- All delegate types implicitly derive from System.MulticastDe legate, which inherits from System.Delegate. C# compiles `+`, `-`, `+=`, and `-=` operations made on a delegate to the static Com bine and Remove methods of the System.Delegate class.

Por exemplo:

```csharp
SomeDelegate d = SomeMethod1;
d += SomeMethod2;
```

The last line is functionally the same as:

```csharp
d = d + SomeMethod2;
```

Invoking `d` will now call both `SomeMethod1` and `SomeMethod2`. Delegates are invoked in the order they are added.

The `-` and `-=` operators remove the right delegate operand from the left delegate operand. For example:

```csharp
d -= SomeMethod1;
```

Invoking `d` will now cause only `SomeMethod2` to be invoked.

Calling `+` or `+=` on a delegate variable with a `null` value works, and it is equivalent to assigning the variable to a new value.

```csharp
SomeDelegate d = null;
d += SomeMethod1;   // Equivalent (when d is null) to d = SomeMethod1;
```

Similarly, calling `-=` on a delegate variable with a single target is equivalent to assigning `null` to that variable.

*Delegates are immutable, so when you call `+=` or `-=`, you’re in fact creating a new delegate instance and assigning it to the existing variable.*

If a multicast delegate has a nonvoid return type, the caller receives the return value from the last method to be invoked. The preceding methods are still called, but their return values are discarded. In most scenarios in which multicast delegates are used, they have void return types, so this subtlety does not arise.

*All delegate types implicitly derive from `System.MulticastDelegate`, which inherits from `System.Delegate`. C# compiles `+`, `-`, `+=`, and `-=` operations made on a delegate to the static Com bine and Remove methods of the `System.Delegate` class.*

### Multicast delegate example

Suppose you wrote a method that took a long time to execute. That method could regularly report progress to its caller by invoking a delegate. In this example, the `HardWork` method has a `ProgressReporter` delegate parameter, which it invokes to indicate progress:

```csharp
public delegate void ProgressReporter(int percentComplete);

public class Util
{
    public static void HardWork(ProgressReporter p)
    {
        for (int i = 0; i < 10; i++)
        {
            p(i * 10);                          // Invoke delegate
            System.Threading.Thread.Sleep(100); // Simulate hard work
        }
    }
}
```

To monitor progress, the `Main` method creates a multicast delegate instance `p`, such that progress is monitored by two independent methods:

```csharp
class Test
{
    static void Main()
    {
        ProgressReporter p = WriteProgressToConsole;
        p += WriteProgressToFile;
        Util.HardWork(p);
    }

    static void WriteProgressToConsole(int percentComplete) => Console.WriteLine(percentComplete);

    static void WriteProgressToFile(int percentComplete) => System.IO.File.WriteAllText("progress.txt", percentComplete.ToString());
}
```

## Instance Versus Static Method Targets

When an *instance* method is assigned to a delegate object, the latter must maintain a reference not only to the method, but also to the instance to which the method belongs. The `System.Delegate` class’s `Target` property represents this instance (and will be null for a delegate referencing a static method).

For example:

```csharp
public delegate void ProgressReporter(int percentComplete);

class Test
{
    static void Main()
    {
        X x = new X();
        ProgressReporter p = x.InstanceProgress; p(99); // 99
        Console.WriteLine(p.Target == x);               // True
        Console.WriteLine(p.Method);                    // Void InstanceProgress(Int32)
    }
}

class X
{
    public void InstanceProgress(int percentComplete) => Console.WriteLine(percentComplete);
}
```

## Generic Delegate Types

- Um tipo de delegate pode conter parâmetros do tipo genéricos:

Por exemplo:

```csharp
public delegate T Transformer<T>(T arg);
```

With this definition, we can write a generalized `Transform` utility method that works on any type:

```csharp
public class Util
{
    public static void Transform<T>(T[] values, Transformer<T> t)
    {
        for (int i = 0; i < values.Length; i++)
            values[i] = t(values[i]);
    }
}

class Test
{
    static void Main()
    {
        int[] values = { 1, 2, 3 };
        Util.Transform(values, Square); // Hook in Square
        foreach (int i in values)
            Console.Write(i + " ");     // 1 4 9
    }

    static int Square(int x) => x * x;
}
```

## The Func and Action Delegates

With generic delegates, it becomes possible to write a small set of delegate types that are so general they can work for methods of any return type and any (reasonable) number of arguments. These delegates are the `Func` and `Action` delegates, defined in the `System` namespace (the `in` and `out` annotations indicate *variance*, which we will cover shortly):

```csharp
delegate TResult Func<out TResult>
delegate TResult Func<in T, out TResult>
delegate TResult Func<in T1, in T2, out TResult>
// ... and so on, up to T16

delegate void Action();
delegate void Action<in T>(T arg);
delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
// ... and so on, up to T16
```

These delegates are extremely general. The `Transformer` delegate in our previous example can be replaced with a `Func` delegate that takes a single argument of type `T` and returns a same-typed value:

```csharp
public static void Transform<T>(T[] values, Func<T,T> transformer)
{
    for (int i = 0; i < values.Length; i++)
        values[i] = transformer(values[i]);
}
```

The only practical scenarios not covered by these delegates are `ref`/`out` and pointer parameters.

*Prior to Framework 2.0, the `Func` and `Action` delegates did not exist (because generics did not exist). It’s for this historical reason that much of the Framework uses custom delegate types rather than `Func` and `Action`.*

## Delegates Versus Interfaces

Um problema que pode ser resolvido usando delegate, pode ser resolvido usando interface.

For instance, we can rewrite our original example with an interface called `ITransformer` instead of a delegate:

```csharp
public interface ITransformer
{
    int Transform(int x);
}

public class Util
{
    public static void TransformAll(int[] values, ITransformer t)
    {
        for (int i = 0; i < values.Length; i++)
            values[i] = t.Transform(values[i]);
    }
}

class Squarer : ITransformer
{
    public int Transform(int x) => x * x;
}

static void Main()
{
    int[] values = { 1, 2, 3 };
    Util.TransformAll(values, new Squarer());
    foreach (int i in values)
        Console.WriteLine(i);
}
```

Um design de delegate pode ser melhor que um design de interface se uma das seguintes condições forem verdadeiras:

- A interface define apenas um único método.
- Capacidade de *Multicast*(Múltiplos métodos adicionados a um delegate) é necessária.
- O assinante(subscriber) precisa implementar a interface várias vezes.

In the `ITransformer` example, we don’t need to multicast. However, the interface defines only a single method. Furthermore, our subscriber may need to implement `ITransformer` multiple times, to support different transforms, such as square or cube. With interfaces, we’re forced into writing a separate type per transform, since Test can implement `ITransformer` only once. This is quite cumbersome:

```csharp
class Squarer : ITransformer
{
    public int Transform(int x) => x * x;
}

class Cuber : ITransformer
{
    public int Transform(int x) => x * x * x;
}

...

static void Main()
{
    int[] values = { 1, 2, 3 };
    Util.TransformAll(values, new Cuber());
    foreach (int i in values)
        Console.WriteLine(i);
}
```

## Delegate Compatibility

### Type compatibility

Os tipos de Delegate são todos incompatíveis uns com os outros, mesmo que suas assinaturas sejam as mesmas:

```csharp
delegate void D1();
delegate void D2();
...
D1 d1 = Method1;
D2 d2 = d1; // Compile-time error
```

------------

The following, however, is permitted:

```csharp
D2 d2 = new D2(d1);
```

------------

As instâncias de delegates são iguais se tiverem os mesmos métodos de destinos:

```csharp
delegate void D();
...

D d1 = Method1;
D d2 = Method1;
Console.WriteLine(d1 == d2); // True
```

Os delegates de *Multicast* são considerados iguais se fizerem referência aos mesmos métodos *na mesma ordem*.

### Parameter compatibility

Um delegate pode ter vários tipos de parâmetros mais específicos, do que os que foram declarados no método alvo. Isso é chamado de *contravariance*.

When you call a method, you can supply arguments that have more specific types than the parameters of that method. This is ordinary polymorphic behavior. For exactly the same reason, a delegate can have more specific parameter types than its method target. This is called *contravariance*.

Veja um exemplo:

```csharp
delegate void StringAction(string s);

class Test
{
    static void Main()
    {
        StringAction sa = new StringAction(ActOnObject);
        sa("hello");
    }

    static void ActOnObject(object o) => Console.WriteLine(o); // hello
}
```

Tal como acontece com parâmetro do tipo de variância, os delegates são variante apenas para conversões do tipo de referência.

A delegate merely calls a method on someone else’s behalf. In this case, the `StringAction` is invoked with an argument of type `string`. When the argument is then relayed to the target method, the argument gets implicitly upcast to an `object`.

### Return type compatibility

O método de destino de um delegate, pode retornar um tipo mais específico do que o descrito pelo delegate. Isso é chamado de *covariance*.

If you call a method, you may get back a type that is more specific than what you asked for. This is ordinary polymorphic behavior. For exactly the same reason, a delegate’s target method may return a more specific type than described by the delegate. This is called *covariance*.

Veja um exemplo:

```csharp
delegate object ObjectRetriever();

class Test
{

    static void Main()
    {
        ObjectRetriever o = new ObjectRetriever(RetrieveString);
        object result = o();
        Console.WriteLine(result); // hello
    }

    static string RetrieveString() => "hello";
}
```

`ObjectRetriever` expects to get back an `object`, but an `object` subclass will also do: delegate return types are *covariant*.

### Generic delegate type parameter variance

- Interfaces genéricas suportam parâmetros de tipo covariant e contravariant. A mesma capacidade existe para os delegates também (a partir do C# 4.0 em diante).

Boas práticas para definir um tipo de delegate genérico:

- Mark a type parameter used only on the return value as covariant (`out`).
- Mark any type parameters used only on parameters as contravariant (`in`).

Doing so allows conversions to work naturally by respecting inheritance relationships between types.

The following delegate (defined in the `System` namespace) has a covariant `TResult`:

```csharp
delegate TResult Func<out TResult>();
```

allowing:

```csharp
Func<string> x = ...;
Func<object> y = x;
```

The following delegate (defined in the `System` namespace) has a contravariant `T`:

```csharp
delegate void Action<in T>(T arg);
```

allowing:

```csharp
Action<object> x = ...;
Action<string> y = x;
```
