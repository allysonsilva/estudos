# C# Language Basics

## Compilation

The C# compiler compiles source code, specified as a set of files with the `.cs` extension, into an *assembly*. An *assembly* is the unit of packaging and deployment in `.NET`. An assembly can be either an application or a library. A normal console or Windows application has a Main method and is an .exe file. A library is a .dll and is equivalent to an .exe without an entry point. Its purpose is to be called upon (referenced) by an application or by other libraries. The .NET Framework is a set of libraries.

## Syntax

*C# syntax is inspired by C and C++ syntax.*

### Identifiers and Keywords

*Identificadores são os nomes utilizados para distinguir os elementos nos seus programas, como namespaces, classes, métodos e variáveis.*

An identifier must be a whole word, essentially made up of Unicode characters starting with a letter or underscore. **C# identifiers are case-sensitive**. By convention, parameters, local variables, and private fields should be in camel case (e.g., `myVariable`), and all other identifiers should be in Pascal case (e.g., `MyMethod`).

*Keywords are names that mean something special to the compiler.*

Most keywords are *reserved*, which means that you can’t use them as identifiers. Here is the full list of C# reserved keywords:

|   |   |   |   |   |
|---|---|---|---|---|
| abstract   | do        | in         | protected  | true      |
| as         | double    | int        | public     | try       |
| base       | else      | interface  | readonly   | typeof    |
| bool       | enum      | internal   | ref        | uint      |
| break      | event     | is         | return     | ulong     |
| byte       | explicit  | lock       | sbyte      | unchecked |
| case       | extern    | long       | sealed     | unsafe    |
| catch      | false     | namespace  | short      | ushort    |
| char       | finally    | new        | sizeof     | using     |
| checked    | fixed      | null       | stackalloc | virtual   |
| class      | float      | object     | static     | void      |
| const      | for       | operator   | string     | volatile  |
| continue   | foreach   | out        | struct     | while     |
| decimal    | goto      | override   | switch     |   |
| default    | if        | params     | this       |   |
| delegate   | implicit  | private    | throw      |   |

### Literals, Punctuators, and Operators

*Literals* are primitive pieces of data lexically embedded into the program. The literals we used in our example program are 12 and 30.

Punctuators help demarcate the structure of the program. These are the punctuators we used in our example program:

```
{ } ;
```

The braces group multiple statements into a *statement* block.

The semicolon terminates a statement. (Statement blocks, however, do not require a semicolon.) Statements can wrap multiple lines:

```csharp
Console.WriteLine(1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10);
```

An *operator* transforms and combines expressions. Most operators in `C#` are denoted with a symbol, such as the multiplication operator, `*`. We will discuss operators in more detail later in this chapter. These are the operators we used in our example program:

```
.  ()  *   =
```

A period denotes a member of something (or a decimal point with numeric literals). Parentheses are used when declaring or calling a method; empty parentheses are used when the method accepts no arguments. (Parentheses also have other purposes that we’ll see later in this chapter.) An equals sign performs *assignment*. (The double equals sign, ==, performs equality comparison, as we’ll see later.)

## Type Basics

A *type* defines the blueprint for a value. In our example, we used two literals of type int with values 12 and 30. We also declared a *variable* of type int whose name was x:

```csharp
static void Main()
{
    int x = 12 * 30;
    Console.WriteLine(x);
}
```

A *variable* denotes a storage location that can contain different values over time. In contrast, a *constant* always represents the same value (more on this later):

```csharp
const int y = 360;
```

All values in C# are *instances* of a type. The meaning of a value, and the set of possible values a variable can have, is determined by its type.

### Value Types Versus Reference Types

All C# types fall into the following categories:

* Value types
* Reference types
* Generic type parameters
* Pointer types

*Value types* comprise most built-in types (specifically, all numeric types, the `char` type, and the `bool` type) as well as custom struct and `enum` types.

*Reference types* comprise all class, array, delegate, and interface types. (This includes the predefined string type.)

The fundamental difference between value types and reference types is how they are handled in memory.

#### Value types

The content of a *value* type variable or constant is simply a value. For example, the content of the built-in value type, int, is 32 bits of data.

You can define a custom value type with the struct keyword:

```csharp
public struct Point { public int X; public int Y; }
```

or more tersely:

```csharp
public struct Point { public int X, Y; }
```

![A value-type instance in memory](./Images/A%20value-type%20instance%20in%20memory.png "A value-type instance in memory")

The assignment of a value-type instance always copies the instance. For example:

```csharp
static void Main()
{
    Point p1 = new Point();
    p1.X = 7;

    Point p2 = p1;              // Assignment causes copy

    Console.WriteLine(p1.X);   // 7
    Console.WriteLine(p2.X);   // 7

    p1.X = 9;                  // Change p1.X

    Console.WriteLine(p1.X); // 9
    Console.WriteLine(p2.X); // 7
}
```

#### Reference types

A reference type is more complex than a value type, having two parts: an `object` and the *reference* to that object. The content of a reference-type variable or constant is a reference to an object that contains the value. Here is the Point type from our previous example rewritten as a class, rather than a `struct`:

```csharp
public class Point { public int X, Y; }
```

![A reference-type instance in memory](./Images/A%20reference-type%20instance%20in%20memory.png "A reference-type instance in memory")

Assigning a reference-type variable copies the reference, not the object instance. This allows multiple variables to refer to the same object—something not ordinarily possible with value types. If we repeat the previous example, but with Point now a class, an operation to `p1` affects `p2`:

```csharp
static void Main()
{
    Point p1 = new Point();
    p1.X = 7;

    Point p2 = p1;              // Copies p1 reference

    Console.WriteLine(p1.X);   // 7
    Console.WriteLine(p2.X);   // 7

    p1.X = 9;                   // Change p1.X

    Console.WriteLine(p1.X);   // 9
    Console.WriteLine(p2.X);   // 9
}
```

shows that p1 and p2 are two references that point to the same object.
![Assignment copies a reference](./Images/Assignment%20copies%20a%20reference.png "Assignment copies a reference")

#### Null

A *reference* can be assigned the literal `null`, indicating that the reference points to no object:

```csharp
class Point {...}
...

Point p = null;
Console.WriteLine(p == null);// True

// The following line generates a runtime error
// (a NullReferenceException is thrown):
Console.WriteLine(p.X);
```

In contrast, a value type cannot ordinarily have a `null` value:

```csharp
struct Point {...} ...

Point p = null; // Compile-time error
int x = null;   // Compile-time error
```

#### Storage overhead

Reference types require separate allocations of memory for the reference and object. The object consumes as many bytes as its fields, plus additional administrative overhead. The precise overhead is intrinsically private to the implementation of the .NET runtime, but at minimum, the overhead is eight bytes, used to store a key to the object’s type, as well as temporary information such as its lock state for multithreading and a flag to indicate whether it has been fixed from movement by the garbage collector. Each reference to an object requires an extra four or eight bytes, depending on whether the .NET runtime is running on a 32- or 64-bit platform.

### Predefined Type Taxonomy

The predefined types in C# are:

**Value types**

* Numeric
    - Signed integer (`sbyte`, `short`, `int`, `long`)
    - Unsigned integer (`byte`, `ushort`, `uint`, `ulong`)
    - Real number (`float`, `double`, `decimal`)
* Logical (`bool`)
* Character (`char`)

**Reference types**

* String (`string`)
* Object (`object`)

## Arrays

An array represents a fixed number of variables (called *elements*) of a particular type. The elements in an array are always stored in a contiguous block of memory, providing highly efficient access.

The `System.Collection` namespace and subnamespaces provide higher-level data structures, such as dynamically sized arrays and dictionaries.

An array *initialization expression* lets you declare and populate an array in a single step.

```csharp
char[] vowels = new char[] {'a','e','i','o','u'};
```

or simply:

```csharp
char[] vowels = {'a','e','i','o','u'};
```

All arrays inherit from the `System.Array` class, providing common services for all arrays.

### Default Element Initialization

Creating an array always preinitializes the elements with default values. The default value for a type is the result of a bitwise zeroing of memory.

#### Value types versus reference types

Whether an array element type is a value type or a reference type has important performance implications. When the element type is a value type, each element value is allocated as part of the array. For example:

```csharp
public struct Point { public int X, Y; }
...

Point[] a = new Point[1000];
int x = a[500].X;   // 0
```

Had Point been a class, creating the array would have merely allocated 1,000 null references:

```csharp
public class Point { public int X, Y; }
...
Point[] a = new Point[1000];
int x = a[500].X;   // Runtime error, NullReferenceException
```

To avoid this error, we must explicitly instantiate 1,000 Points after instantiating the array:

```csharp
Point[] a = new Point[1000];

for (int i = 0; i < a.Length; i++)  // Iterate i from 0 to 999
    a[i] = new Point();             // Set array element i with new point
```

An array *itself* is always a reference type object, regardless of the element type. For instance, the following is legal:

```csharp
int[] a = null;
```

### Multidimensional Arrays

Multidimensional arrays come in two varieties: *rectangular* and *jagged*. Rectangular arrays represent an *n*-dimensional block of memory, and jagged arrays are arrays of arrays.

#### Rectangular arrays

Rectangular arrays are declared using commas to separate each dimension. The following declares a rectangular two-dimensional array, where the dimensions are 3 by 3:

```csharp
int[,] matrix = new int[3,3];
```

The GetLength method of an array returns the length for a given dimension (starting at 0):

```csharp
for (int i = 0; i < matrix.GetLength(0); i++)
    for (int j = 0; j < matrix.GetLength(1); j++)
        matrix[i,j] = i * 3 + j;
```

A rectangular array can be initialized as follows (to create an array identical to the previous example):

```csharp
int[,] matrix = new int[,]
{
    {0,1,2},
    {3,4,5},
    {6,7,8}
};
```

#### Jagged arrays

Jagged arrays are declared using successive square brackets to represent each dimension. Here is an example of declaring a jagged two-dimensional array, where the outermost dimension is 3:

```csharp
int[][] matrix = new int[3][];
```

The inner dimensions aren’t specified in the declaration because, unlike a rectangular array, each inner array can be an arbitrary length. Each inner array is implicitly initialized to null rather than an empty array. Each inner array must be created manually:

```csharp
for (int i = 0; i < matrix.Length; i++)
{
    matrix[i] = new int[3]; // Create inner array
    for (int j = 0; j < matrix[i].Length; j++)
        matrix[i][j] = i * 3 + j;
}
```

A jagged array can be initialized as follows (to create an array identical to the previous example with an additional element at the end):

```csharp
int[][] matrix = new int[][]
{
    new int[] {0,1,2},
    new int[] {3,4,5},
    new int[] {6,7,8,9}
};
```

### Simplified Array Initialization Expressions

There are two ways to shorten array initialization expressions. The first is to omit the new operator and type qualifications:

```csharp
char[] vowels = {'a','e','i','o','u'};
```

```csharp
int[,] rectangularMatrix =
{
    {0,1,2},
    {3,4,5},
    {6,7,8}
};
```

```csharp
int[][] jaggedMatrix =
{
    new int[] {0,1,2},
    new int[] {3,4,5},
    new int[] {6,7,8}
};
```

The second approach is to use the `var` keyword, which tells the compiler to implicitly type a local variable:

```csharp
var i = 3;              // i is implicitly of type int
var s = "sausage";      // s is implicitly of type string

// Therefore:

var rectMatrix = new int[,]     // rectMatrix is implicitly of type int[,]
{
    {0,1,2},
    {3,4,5},
    {6,7,8}
};

var jaggedMat = new int[][] {   // jaggedMat is implicitly of type int[][]
    new int[] {0,1,2},
    new int[] {3,4,5},
    new int[] {6,7,8}
};
```

Implicit typing can be taken one stage further with arrays: you can omit the type qualifier after the new keyword and have the compiler infer the array type:

```csharp
var vowels = new[] {'a','e','i','o','u'}; // Compiler infers char[]
```

For this to work, the elements must all be implicitly convertible to a single type (and at least one of the elements must be of that type, and there must be exactly one best type). For example:

```csharp
var x = new[] {1,10000000000}; // all convertible to long
```

### Bounds Checking

All array indexing is bounds-checked by the runtime. An `IndexOutOfRangeException` is thrown if you use an invalid index:

```csharp
int[] arr = new int[3];
arr[3] = 1; // IndexOutOfRangeException thrown
```

## Variables and Parameters

A variable represents a storage location that has a modifiable value. A variable can be a *local variable*, *parameter* (*value*, *ref*, or *out*), field (*instance* or *static*), or *array element*.

### The Stack and the Heap

The stack and the heap are the places where variables and constants reside. Each has very different lifetime semantics.

#### Stack

The stack is a block of memory for storing local variables and parameters. The stack logically grows and shrinks as a function is entered and exited. Consider the following method (to avoid distraction, input argument checking is ignored):

```csharp
static int Factorial(int x)
{
    if (x == 0)
        return 1;

    return x * Factorial(x-1);
}
```

This method is recursive, meaning that it calls itself. Each time the method is entered, a new int is allocated on the stack, and each time the method exits, the int is deallocated.

#### Heap

The heap is a block of memory in which objects (i.e., reference-type instances) reside. Whenever a new object is created, it is allocated on the heap, and a reference to that object is returned. During a program’s execution, the heap starts filling up as new objects are created. The runtime has a garbage collector that periodically deallocates objects from the heap, so your program does not run out of memory. An object is eligible for deallocation as soon as it’s not referenced by anything that’s itself "alive."

In the following example, we start by creating a `StringBuilder` object referenced by the variable `ref1`, and then write out its content. That `StringBuilder` object is then immediately eligible for garbage collection, because nothing subsequently uses it.

Then, we create another `StringBuilder` referenced by variable `ref2`, and copy that reference to `ref3`. Even though `ref2` is not used after that point, `ref3` keeps the same `StringBuilder` object alive—ensuring that it doesn’t become eligible for collection until we’ve finished using `ref3`.

```csharp
using System;
using System.Text;

class Test
{
    static void Main()
    {
        StringBuilder ref1 = new StringBuilder("object1");
        Console.WriteLine(ref1);
        // The StringBuilder referenced by ref1 is now eligible for GC.

        StringBuilder ref2 = new StringBuilder("object2");
        StringBuilder ref3 = ref2;
        // The StringBuilder referenced by ref2 is NOT yet eligible for GC.

        Console.WriteLine(ref3);   // object2
    }
}
```

Value-type instances (and object references) live wherever the variable was declared. If the instance was declared as a field within a class type, or as an array element, that instance lives on the heap.

*You can’t explicitly delete objects in C#, as you can in C++. An unreferenced object is eventually collected by the garbage collector.*

The heap also stores static fields. Unlike objects allocated on the heap (which can get garbage-collected), these live until the application domain is torn down.

### Definite Assignment

C# enforces a definite assignment policy. In practice, this means that outside of an `unsafe` context, it’s impossible to access uninitialized memory. Definite assignment has three implications:

* Local variables must be assigned a value before they can be read.
* Function arguments must be supplied when a method is called.
* All other variables (such as fields and array elements) are automatically initialized by the runtime.

For example, the following code results in a compile-time error:

```csharp
static void Main()
{
    int x;
    Console.WriteLine(x);   // Compile-time error
}
```

Fields and array elements are automatically initialized with the default values for their type. The following code outputs 0, because array elements are implicitly assigned to their default values:

```csharp
static void Main()
{
    int[] ints = new int[2];
    Console.WriteLine(ints[0]); // 0
}
```

The following code outputs 0, because fields are implicitly assigned a default value:

```csharp
class Test
{
    static int x;

    static void Main()
    {
        Console.WriteLine(x);
    }
}
```

### Default Values

All type instances have a default value. The default value for the predefined types is the result of a bitwise zeroing of memory:

* **All reference types**: `null`
* **All numeric and enum types**: `0`
* **char type**: `"\0"`
* **bool type**: `false`

You can obtain the default value for any type with the `default` keyword:

```csharp
decimal d = default(decimal);
```

The *default* value in a custom value type (i.e., `struct`) is the same as the default value for each field defined by the custom type.

### Parameters

A method has a sequence of parameters. Parameters define the set of arguments that must be provided for that method. In this example, the method Foo has a single parameter named p, of type `int`:

```csharp
static void Foo(int p)
{
    p = p + 1;              // Increment p by 1
    Console.WriteLine(p);   // Write p to screen
}

static void Main()
{
    Foo(8);    // Call Foo with an argument of 8
}
```

You can control how parameters are passed with the `ref` and `out` modifiers:

| Parameter modifier | Passed by | Variable must be definitely assigned |
|--------------------|-----------|------------------------------------|
| (None)             | Value     | Going *in*                         |
| `ref`              | Reference | Going *in*                         |
| `out`              | Reference | Going *out*                        |

#### Passing arguments by value

*By default*, arguments in C# are **passed by value**, which is by far the most common case. This means a copy of the value is created when passed to the method:

```csharp
class Test
{
    static void Foo(int p)
    {
        p = p + 1;              // Increment p by 1
        Console.WriteLine(p);   // Write p to screen
    }

    static void Main()
    {
        int x = 8;
        Foo(x);                 // Make a copy of x
        Console.WriteLine(x);   // x will still be 8
    }
}
```

Assigning `p` a new value does not change the contents of x, since `p` and `x` reside in different memory locations.

**Passing a reference-type argument by value copies the reference, but not the object**. In the following example, Foo sees the same `StringBuilder` object that Main instantiated, but has an independent *reference* to it. In other words, `sb` and `fooSB` are separate variables that reference the same `StringBuilder` object:

```csharp
class Test
{
    static void Foo(StringBuilder fooSB)
    {
        fooSB.Append("test");
        fooSB = null;
    }

    static void Main()
    {
        StringBuilder sb = new StringBuilder();
        Foo(sb);
        Console.WriteLine(sb.ToString());   // test
    }
}
```

Because `fooSB` is a *copy* of a reference, setting it to `null` doesn’t make sb null. (If, however, `fooSB` was declared and called with the `ref` modifier, `sb` would become null.)

#### The `ref` modifier

To *pass by reference*, C# provides the `ref` parameter modifier. In the following example, `p` and `x` refer to the same memory locations:

```csharp
class Test
{
    static void Foo(ref int p)
    {
        p = p + 1;  // Increment p by 1
        Console.WriteLine(p);   // Write p to screen
    }

    static void Main()
    {
        int x = 8;
        Foo(ref x); // Ask Foo to deal directly with x
        Console.WriteLine(x);  // x is now 9
    }
}
```

Now assigning `p` a new value changes the contents of `x`. Notice how the `ref` modifier is required both when writing and when calling the method. 4 This makes it very clear what’s going on.

#### The `out` modifier

An out argument is like a ref argument, except it:

* Need not be assigned before going into the function
* Must be assigned before it comes `out` of the function

The `out` modifier is most commonly used to get multiple return values back from a method. For example:

```csharp
class Test
{
    static void Split(string name, out string firstNames, out string lastName)
    {
        int i = name.LastIndexOf(' ');
        firstNames = name.Substring(0, i);
        lastName = name.Substring(i + 1);
    }

    static void Main()
    {
        string a, b;
        Split("Stevie Ray Vaughan", out a, out b);  // Stevie Ray
        Console.WriteLine(a); Console.WriteLine(b); // Vaughan
    }
}
```

*Like a ref parameter, an out parameter is passed by reference.*

#### Out variables and discards (C# 7)

From C# 7, you can declare variables on the fly when calling methods with out parameters. We can shorten the Main method in our preceding example as follows:

```csharp
static void Main()
{
    Split("Allyson Silva", out string a, out string b);
    Console.WriteLine(a); // Allyson
    Console.WriteLine(b); // Silva
}
```

When calling methods with multiple out parameters, sometimes you’re not interested in receiving values from all the parameters. In such cases, you can “discard” the ones you’re uninterested in with an underscore:

```csharp
Split("Allyson Silva", out string a, out _);
Console.WriteLine(a);
```

In this case, the compiler treats the underscore as a special symbol, called a discard. You can include multiple discards in a single call. Assuming `SomeBigMethod` has been defined with seven **out** parameters, we can ignore all but the fourth as follows:

```csharp
SomeBigMethod(out _, out _, out _, out int x, out _, out _, out _);
```

For backward compatibility, this language feature will not take effect if a real underscore variable is in scope:

```csharp
string _;
Split ("Allyson Silva", out string a, _); // Will not compile
```

#### Implications of passing by reference

When you pass an argument by reference, you alias the storage location of an existing variable rather than create a new storage location. In the following example, the variables x and y represent the same instance:

```csharp
class Test
{
    static int x;

    static void Main()
    {
        Foo(out x);
    }

    static void Foo(out int y)
    {
        Console.WriteLine(x);   // x is 0
        y = 1;                  // Mutate y
        Console.WriteLine(x);   // x is 1
    }
}
```

#### The params modifier

The `params` parameter modifier may be specified on the last parameter of a method so that the method accepts any number of arguments of a particular type. The parameter type must be declared as an array. For example:

```csharp
class Test
{
    static int Sum(params int[] ints)
    {
        int sum = 0;
        for (int i = 0; i < ints.Length; i++)
            sum += ints[i]; // Increase sum by ints[i]

        return sum;
    }

    static void Main()
    {
        int total = Sum (1, 2, 3, 4);
        Console.WriteLine(total);   // 10
    }
}
```

You can also supply a params argument as an ordinary array. The first line in Main is semantically equivalent to this:

```csharp
int total = Sum(new int[] { 1, 2, 3, 4 });
```

#### Optional parameters

From C# 4.0, methods, constructors, and indexers can declare optional parameters. A parameter is optional if it specifies a default value in its declaration.

```csharp
void Foo(int x = 23) { Console.WriteLine(x); }
```
**Optional parameters cannot be marked with ref or out.**

Optional parameters may be omitted when calling the method:

```csharp
Foo();  // 23
```

The *default argument* of 23 is actually passed to the optional parameter x—the compiler bakes the value 23 into the compiled code at the *calling* side. The preceding call to Foo is semantically identical to:

```csharp
Foo(23);
```

because the compiler simply substitutes the default value of an optional parameter wherever it is used.

The default value of an optional parameter must be specified by a constant expression, or a parameterless constructor of a value type. Optional parameters cannot be marked with `ref` or `out`.

Mandatory parameters must occur before optional parameters in both the method declaration and the method call (the exception is with params arguments, which still always come last). In the following example, the explicit value of 1 is passed to x, and the default value of 0 is passed to `y`:

```csharp
void Foo(int x = 0, int y = 0)
{
    Console.WriteLine(x + ", " + y);
}

void Test()
{
    Foo(1); // 1, 0
}
```

To do the converse (pass a default value to `x` and an explicit value to `y`) you must combine optional parameters with *named arguments*.

#### Named arguments

Rather than identifying an argument by position, you can identify an argument by name.

```csharp
void Foo(int x, int y)
{
    Console.WriteLine(x + ", " + y);
}

void Test()
{
    Foo(x:1, y:2); // 1, 2
}
```

Named arguments can occur in any order. The following calls to Foo are semantically identical:

```csharp
Foo (x:1, y:2);
Foo (y:2, x:1);
```

You can mix named and positional arguments:

```csharp
Foo (x:1, y:2);
```

However, there is a restriction: positional arguments must come before named arguments. So we couldn’t call Foo like this:

```csharp
Foo(x:1, 2);   // Compile-time error
```

Named arguments are particularly useful in conjunction with optional parameters. For instance, consider the following method:

```csharp
void Bar(int a = 0, int b = 0, int c = 0, int d = 0) { ... }
```

**positional arguments must come before named arguments.**

### Ref Locals (C# 7)

C# 7 adds an esoteric feature, whereby you can define a local variable that *references* an element in an array or field in an object:

```csharp
int[] numbers = { 0, 1, 2, 3, 4 };
ref int numRef = ref numbers [2];
```

In this example, `numRef` is a *reference* to the `numbers[2]`. When we modify `numRef`, we modify the array element:

```csharp
numRef *= 10;
Console.WriteLine(numRef);      // 20
Console.WriteLine(numbers [2]); // 20
```

The target for a ref local must be an array element, field, or local variable; it cannot be a property. Ref locals are intended for specialized micro-optimization scenarios, and are typically used in conjunction with ref returns.

### Ref Returns (C# 7)

You can return a `ref` local from a method. This is called a *ref return*:

```csharp
static string X = "Old Value";

static ref string GetX() => ref X;  // This method returns a ref

static void Main()
{
    ref string xRef = ref GetX();   // Assign result to a ref local
    xRef = "New Value";
    Console.WriteLine(X);           // New Value
}
```

## Null Operators

C# provides two operators to make it easier to work with nulls: the *null-coalescing* operator and the *null-conditional* operator.

### Null-Coalescing Operator

The `??` operator is the **null-coalescing operator**. It says “If the operand is non-null, give it to me.

```csharp
string s1 = null;
string s2 = s1 ?? "nothing"; // s2 evaluates to "nothing"
```

If the left-hand expression is non-null, the right-hand expression is never evaluated. The null-coalescing operator also works with nullable value types.

### Null-conditional operator (C# 6)

The `?.` operator is the null-conditional. It allows you to call methods and access members just like the standard dot operator, except that if the operand on the left is `null`, the expression evaluates to null instead of throwing a `NullReferenceException`:

```csharp
System.Text.StringBuilder sb = null;
string s = sb?.ToString(); // No error; s instead evaluates to null
```

The last line is equivalent to:

```csharp
string s = (sb == null ? null : sb.ToString());
```

Upon encountering a null, the Elvis operator short-circuits the remainder of the expression. In the following example, s evaluates to null, even with a standard dot operator between `ToString()` and `ToUpper()`:

```csharp
System.Text.StringBuilder sb = null;
string s = sb?.ToString().ToUpper();    // s evaluates to null without error
```

Repeated use of Elvis is necessary only if the operand immediately to its left may be null. The following expression is robust to both `x` being null and `x.y` being `null`:

```csharp
x?.y?.z
```

and is equivalent to the following (except that x.y is evaluated only once):

```csharp
x == null ? null : (x.y == null ? null : x.y.z)
```

The final expression must be capable of accepting a null. The following is illegal:

```csharp
System.Text.StringBuilder sb = null;
int length = sb?.ToString().Length;     // Illegal : int cannot be null
```

We can fix this with the use of nullable value types: If you’re already familiar with nullable types, here’s a preview:

```csharp
int? length = sb?.ToString().Length;    // OK : int? can be null
```

You can also use the null-conditional operator to call a void method:

```csharp
someObject?.SomeVoidMethod();
```

If `someObject` is null, this becomes a “no-operation” rather than throwing a `NullReferenceException`.

The null-conditional operator can be used with the commonly used type members, including methods, fields, properties and indexers. It also combines well with the null coalescing operator:

```csharp
System.Text.StringBuilder sb = null;
string s = sb?.ToString() ?? "nothing"; // s evaluates to "nothing"
```

## Statements

Functions comprise statements that execute sequentially in the textual order in which they appear. A statement block is a series of statements appearing between braces (the {} tokens).

### Declaration Statements

A declaration statement declares a new variable, optionally initializing the variable with an expression. A declaration statement ends in a semicolon. You may declare multiple variables of the same type in a comma-separated list. For example:

```csharp
string someWord = "rosebud";
int someNumber = 42;
bool rich = true, famous = false;
```

A constant declaration is like a variable declaration, except that it cannot be changed after it has been declared, and the initialization must occur with the declaration:

```csharp
const double c = 2.99792458E08;
c += 10;    // Compile-time Error
```

#### Local variables

The scope of a local variable or local constant extends throughout the current block. You cannot declare another local variable with the same name in the current block or in any nested blocks. For example:

```csharp
static void Main()
{
    int x;  // Error - x already defined
    {
        int y;
        int x;
    }
    {
        int y;  // OK - y not in scope
    }
    Console.Write(y);   // Error - y is out of scope
}
```

*A variable’s scope extends in both directions throughout its code block. This means that if we moved the initial declaration of x in this example to the bottom of the method, we’d get the same error. This is in contrast to C++ and is somewhat peculiar, given that it’s not legal to refer to a variable or constant before it’s declared.*

## Namespaces

A namespace is a domain for type names. Types are typically organized into hierarchical namespaces, making them easier to find and avoiding conflicts. For example, the RSA type that handles public key encryption is defined within the following namespace:

```csharp
System.Security.Cryptography
```

A namespace forms an integral part of a type’s name. The following code calls RSA’s Create method:

```csharp
System.Security.Cryptography.RSA rsa = System.Security.Cryptography.RSA.Create();
```

*Namespaces are independent of assemblies, which are units of deployment such as an .exe or .dll. Namespaces also have no impact on member visibility—pub lic, internal, private, and so on.*

The `namepace` keyword defines a namespace for types within that block. For example:

```csharp
namespace Outer.Middle.Inner
{
    class Class1 {}
    class Class2 {}
}
```

The dots in the namespace indicate a hierarchy of nested namespaces. The code that follows is semantically identical to the preceding example:

```csharp
namespace Outer
{
    namespace Middle
    {
        namespace Inner
        {
            class Class1 {}
            class Class2 {}
        }
    }
}
```

You can refer to a type with its *fully qualified name*, which includes all namespaces from the outermost to the innermost. For example, we could refer to `Class1` in the preceding example as `Outer.Middle.Inner.Class1`.

Types not defined in any namespace are said to reside in the *global namespace*. The global namespace also includes top-level namespaces, such as Outer in our example.

### The using Directive

The `using` directive *imports* a namespace, allowing you to refer to types without their fully qualified names. The following imports the previous example’s `Outer.Middle.Inner` namespace:

```csharp
using Outer.Middle.Inner;

class Test
{
    static void Main()
    {
        Class1 c; // Don't need fully qualified name
    }
}
```

### using static (C# 6)

From C# 6, you can import not just a namespace, but a specific type, with the using static directive. All static members of that type can then be used without being qualified with the type name. In the following example, we call the Console class’s static WriteLine method:

```csharp
using static System.Console;

class Test
{
    static void Main()
    {
        WriteLine ("Hello");
    }
}
```

The `using static` directive imports all accessible static members of the type, including fields, properties, and nested types. You can also apply this directive to enum types, in which case their members are imported. So, if we import the following enum type:

```csharp
using static System.Windows.Visibility;
```

we can specify Hidden instead of Visibility.Hidden:

```csharp
var textBox = new TextBox { Visibility = Hidden };  // XAML-style
```

Should an ambiguity arise between multiple static imports, the C# compiler is not smart enough to infer the correct type from the context, and will generate an error.

### Rules Within a Namespace

#### Name scoping

Names declared in outer namespaces can be used unqualified within inner namespaces. In this example, Class1 does not need qualification within `Inner`:

```csharp
namespace Outer
{
    class Class1 {}

    namespace Inner
    {
        class Class2 : Class1
    }
}
```

If you want to refer to a type in a different branch of your namespace hierarchy, you can use a partially qualified name. In the following example, we base `SalesReport` on `Common.ReportBase`:

```csharp
namespace MyTradingCompany
{
    namespace Common
    {
        class ReportBase {}
    }

    namespace ManagementReporting
    {
        class SalesReport : Common.ReportBase {}
    }
}
```

#### Name hiding

If the same type name appears in both an inner and an outer namespace, the inner name wins. To refer to the type in the outer namespace, you must qualify its name. For example:

```csharp
namespace Outer
{
    class Foo { }

    namespace Inner
    {
        class Foo { }
        class Test
        {
            Foo f1;         // = Outer.Inner.Foo
            Outer.Foo f2;   // = Outer.Foo
        }
    }
}
```

*All type names are converted to fully qualified names at compile time. Intermediate Language (IL) code contains no unqualified or partially qualified names.*

#### Repeated namespaces

You can repeat a namespace declaration, as long as the type names within the namespaces don’t conflict:

```csharp
namespace Outer.Middle.Inner
{
    class Class1 {}
}

namespace Outer.Middle.Inner
{
    class Class2 {}
}
```

We can even break the example into two source files such that we could compile each class into a different assembly.

Source file 1:

```csharp
namespace Outer.Middle.Inner
{
    class Class1 {}
}
```
Source file 2:

```csharp
namespace Outer.Middle.Inner
{
    class Class2 {}
}
```

#### Nested using directive

You can nest a using directive within a namespace. This allows you to scope the using directive within a namespace declaration. In the following example, Class1 is visible in one scope, but not in another:

```csharp
namespace N1
{
    class Class1 {}
}

namespace N2
{
    using N1;

    class Class2 : Class1 {}
}

namespace N2
{
    class Class3 : Class1 {}    // Compile-time error
}
```

### Aliasing Types and Namespaces

Importing a namespace can result in type-name collision. Rather than importing the whole namespace, you can import just the specific types you need, giving each type an alias. For example:

```csharp
using PropertyInfo2 = System.Reflection.PropertyInfo;
class Program { PropertyInfo2 p; }
```

An entire namespace can be aliased, as follows:

```csharp
using R = System.Reflection;
class Program { R.PropertyInfo p; }
```
