# Lambda Expressions

- É um método sem nome escrito no lugar de uma instância delegate.
- O compilador converte imediatamente a expressão lambda para:
    - Uma instância de delegate.
    - Uma árvore de expressões, to tipo `Expression<TDelegate>`. Isso permite que a expressão lambda seja interpretada mais tarde no tempo de execução.
- O código de declaração lambda pode ser um bloco de declaração em vez de uma expressão.
- Expressões Lambda foram introduzidas no `C# 3.0`.

> A lambda expression is an unnamed method written in place of a delegate instance.

The compiler immediately converts the lambda expression to either:

* A delegate instance.
* An expression tree, of type `Expression<TDelegate>`, representing the code inside the lambda expression in a traversable object model.

Given the following delegate type:

```csharp
delegate int Transformer(int i);
```

we could assign and invoke the lambda expression `x => x * x` as follows:

```csharp
Transformer sqr = x => x * x;
Console.WriteLine(sqr(3)); // 9
```

*Internally, the compiler resolves lambda expressions of this type by writing a private method, and moving the expression’s code into that method.*

Uma expressão lambda tem a seguinte forma:

***(parameters) => expression-or-statement-block***

Por conveniência, você pode omitir os parênteses se e somente se houver exatamente um parâmetro de um tipo inferível.

No nosso exemplo, existe um único parâmetro, `x` , e a expressão é `x * x`:

```
x => x * x;
```

Cada parâmetro da expressão lambda corresponde a um parâmetro delegate e o tipo da expressão (que pode ser `void`) corresponde ao tipo de retorno do delegate.

No nosso exemplo, `x` corresponde ao parâmetro `i` , e a expressão `x * x` corresponde ao tipo de retorno `int`, portanto, therefore being compatible with the `Transformer` delegate:

```csharp
delegate int Transformer(int i);
```

O código de declaração lambda pode ser um bloco de declaração em vez de uma expressão.

```csharp
x => { return x * x; };
```

As expressões Lambda são mais utilizadas com os delegates `Func` e `Action`.

```csharp
Func<int,int> sqr = x => x * x;
```

Utilizando um exemplo com dois parâmetros:

```csharp
Func<string,string,int> totalLength = (s1, s2) => s1.Length + s2.Length;
int total = totalLength("hello", "world"); // total is 10;
```

## Explicitly Specifying Lambda Parameter Types

O compilador geralmente pode inferir o tipo de parâmetros lambda. Quando este não for o caso, você deve especificar o tipo de cada parâmetro explicitamente.

```csharp
void Foo<T>(T x) {}
void Bar<T>(Action<T> a) {}
```

O código a seguir não compilará porque o compilador não pode inferir o tipo de `x`:

```csharp
Bar(x => Foo(x)); // What type is x?
```

Podemos corrigir isso especificando explicitamente o tipo de `x` da seguinte maneira:

```csharp
Bar((int x) => Foo(x));
```

Este exemplo particular é simples o suficiente para que ele possa ser aprimorado de duas maneiras diferentes:

```csharp
Bar<int>(x => Foo(x));  // Specify type parameter for Bar
Bar<int>(Foo);          // As above, but with method group
```

## Capturing Outer Variables

Uma expressão lambda pode fazer referência às variáveis ​​e parâmetros locais do método em que está definido *outer variables*(variáveis ​​externas).

```csharp
static void Main()
{
    int factor = 2;
    Func<int, int> multiplier = n => n * factor;
    Console.WriteLine(multiplier(3)); // 6
}
```

**Outer variables referenced by a lambda expression are called *captured variables*. A lambda expression that captures variables is called a *closure*.**

*Variables can also be captured by anonymous methods and local methods. The rules for captured variables, in these cases, are the same.*

Captured variables are evaluated when the delegate is actually *invoked*, not when the variables were *captured*:

```csharp
int factor = 2;
Func<int, int> multiplier = n => n * factor;
factor = 10;
Console.WriteLine(multiplier(3));   // 30
```

As expressões Lambda podem atualizar as variáveis ​​capturadas:

```csharp
int seed = 0; Func<int> natural = () => seed++;
Console.WriteLine(natural());  // 0
Console.WriteLine(natural());  // 1
Console.WriteLine(seed);       // 2
```

As variáveis capturadas têm suas lifetimes ampliadas para a do delegate. No exemplo a seguir, à variável local `seed` normalmente desapareceria do escopo quando o `Natural` fosse finalizado. Mas, como a `seed` foi capturada, sua lifetime se estende ao do delegate(`natural`):

```csharp
static Func<int> Natural()
{
   int seed = 0;
   return () => seed++; // Returns a closure
}

static void Main()
{
    Func<int> natural = Natural();

   Console.WriteLine(natural());    // 0
   Console.WriteLine(natural());    // 1
}
```

Uma variável local instanciada dentro de uma expressão lambda é única por invocação da instância delegate. If we refactor our previous example to instantiate `seed` *within* the lambda expression, we get a different (in this case, undesirable) result:

```csharp
static Func<int> Natural()
{
    return () => {
        int seed = 0;
        return seed++;
    };
}

static void Main()
{
    Func<int> natural = Natural();

    Console.WriteLine(natural());   // 0
    Console.WriteLine(natural());   // 0
}
```

### Capturing iteration variables

Quando você captura a variável de iteração de um *loop* `for`, C# trata essa variável como se fosse declarada fora do *loop*. Isso significa que a mesma variável é capturada em cada iteração. O seguinte programa grava `333` em vez de escrever `012`:

```csharp
Action[] actions = new Action[3];

for (int i = 0; i< 3; i++)
    actions[i] = () => Console.Write(i);

foreach (Action a in actions)
    a(); // 333
```

Each closure (shown in boldface) captures the same variable, i. (This actually makes sense when you consider that i is a variable whose value persists between loop iterations; you can even explicitly change i within the loop body if you want.) The consequence is that when the delegates are later invoked, each delegate sees i’s value at the time of *invocation—which* is 3. We can illustrate this better by expanding the for loop as follows:

```csharp
Action[] actions = new Action[3];

int i = 0;
actions[0] = () => Console.Write(i);

i = 1;
actions[1] = () => Console.Write(i);

i = 2;
actions[2] = () => Console.Write(i);

i = 3;

foreach (Action a in actions) a(); // 333
```

A solução é criar uma variável local a cada interação e atribuir o valor de `i` a variável que é do escopo do `for`:

```csharp
Action[] actions = new Action[3];

for (int i = 0; i< 3; i++) {
    int loopScopedi = i;
    actions[i] = () => Console.Write(loopScopedi);
}

foreach (Action a in actions) a(); // 012
```

Como a variável `loopScopedi` é criada em cada interação, cada *closure* captura uma variável diferente.

Com `foreach` é diferente pois á cada interação é uma nova variável imutável:

```csharp
Action[] actions = new Action[3];

int i = 0;

foreach (char c in "abc")
    actions[i++] = () => Console.Write(c);

foreach (Action a in actions) a(); // abc
```
