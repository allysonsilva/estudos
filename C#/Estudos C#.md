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

# Creating Types in C#

## Classes

A more complex class optionally has the following:

*Preceding the keyword class*: **Attributes and class modifiers. The non-nested class modifiers are `public`, `internal`, `abstract`, `sealed`, `static`, `unsafe`, and `partial`**
*Within the braces*: **Class members (these are methods, properties, indexers, events, fields, constructors, overloaded operators, nested types, and a finalizer)**

### Fields

> A field is a variable that is a member of a class or struct.

**Modifiers**: `static`, `public`, `internal`, `private`, `protected`, `new`, `unsafe`, `readonly`, `volatile`

Fields allow the following modifiers:

|                      |                                           |
|----------------------|-------------------------------------------|
| Static modifier      | `static`                                  |
| Access modifiers     | `public` `internal` `private` `protected` |
| Inheritance modifier | `new`                                     |
| Unsafe code modifier | `unsafe`                                  |
| Read-only modifier   | `readonly`                                |
| Threading modifier   | `volatile`                                |

#### The readonly modifier

The readonly modifier prevents a field from being modified after construction. A read-only field can be assigned only in its declaration or within the enclosing type’s constructor.

#### Field initialization

Field initialization is optional. An uninitialized field has a default value (0, \0, null, false). Field initializers run before constructors.

```csharp
public int Age = 10;
```

Field initializations occur before the constructor is executed and in the declaration order of the fields.

#### Declaring multiple fields together

For convenience, you may declare multiple fields of the same type in a comma separated list. This is a convenient way for all the fields to share the same attributes and field modifiers. For example:

```csharp
static readonly int foo = 8,
                    bar = 2;
```

### Methods

> A method performs an action in a series of statements. A method can receive input data from the caller by specifying parameters and output data back to the caller by specifying a return type. A method can specify a void return type, indicating that it doesn’t return any value to its caller. A method can also output data back to the caller via `ref`/`out` parameters.

**Modifiers**: `static`, `public`, `internal`, `private`, `protected`, `new`, `virtual`, `abstract`, `override`, `sealed`, `partial`, `unsafe`, `extern`, `async`

A method’s *signature* must be unique within the type. A method’s signature comprises its *name* and *parameter types* (but not the parameter *names*, nor the return type).

Methods allow the following modifiers:

|                            |                                                |
|----------------------------|------------------------------------------------|
| Static modifier            | `static`                                       |
| Access modifiers           | `public` `internal` `private` `protected`      |
| Inheritance modifiers      | `new` `virtual` `abstract` `override` `sealed` |
| Partial method modifier    | `partial`                                      |
| Unmanaged code modifiers   | `unsafe` `extern`                              |
| Asynchronous code modifier | `async`                                        |

#### Expression-bodied methods (C# 6)

A method that comprises a single expression, such as the following:

```csharp
int Foo(int x) { return x * 2; }
```

can be written more tersely as an **expression-bodied** method. A fat arrow replaces the braces and `return` keyword:

```csharp
int Foo(int x) => x * 2;
```

Expression-bodied functions can also have a void return type:

```csharp
void Foo(int x) => Console.WriteLine(x);
```

#### Overloading methods

A type may overload methods (have multiple methods with the same name), as long as the signatures are different. For example, the following methods can all coexist in the same type:

```csharp
void Foo(int x) {...}
void Foo(double x) {...}
void Foo(int x, float y) {...}
void Foo(float x, int y) {...}
```

However, the following pairs of methods cannot coexist in the same type, since the return type and the params modifier are not part of a method’s signature:

```csharp
void Foo(int x) {...}
float Foo(int x) {...} // Compile-time error

void Goo(int[] x) {...}
void Goo(params int[] x) {...} // Compile-time error
```

#### Pass-by-value versus pass-by-reference

Whether a parameter is pass-by-value or pass-by-reference is also part of the signature. For example, `Foo(int)` can coexist with either `Foo(ref int)` or `Foo(out int)`. However, `Foo(ref int)` and `Foo(out int)` cannot coexist:

```csharp
void Foo (int x) {...}
void Foo (ref int x) {...} // OK so far
void Foo (out int x) {...} // Compile-time error
```

#### Local methods (C# 7)

From C# 7, you can define a method inside another method:

```csharp
void WriteCubes()
{
    Console.WriteLine(Cube(3));
    Console.WriteLine(Cube(4));
    Console.WriteLine(Cube(5));

    int Cube(int value) => value * value * value;
}
```

The local method (`Cube`, in this case) is visible only to the enclosing method (Write Cubes). This simplifies the containing type and instantly signals to anyone looking at the code that Cube is used nowhere else. Another benefit of local methods is that they can access the local variables and parameters of the enclosing method.

Local methods can appear inside other function kinds, such as property accessors, constructors, and so on. You can even put local methods inside other local methods, and inside lambda expressions that use a statement block. Local methods can be iterators or asynchronous.

*The `static` modifier is invalid for local methods. They are implicitly static if the enclosing method is static.*

### Instance Constructors

> Constructors run initialization code on a class or struct. A constructor is defined like a method, except that the method name and return type are reduced to the name of the enclosing type.

```csharp
public class Panda
{
    string name;                // Define field

    public Panda(string n)      // Define constructor
    {
        name = n;               // Initialization code (set up field)
    }
}
...
Panda p = new Panda("Petey");   // Call constructor
```

**Modifiers**: `public` `internal` `private` `protected` `unsafe` `extern`

|                            |                                                |
|----------------------------|------------------------------------------------|
| Access modifiers           | `public` `internal` `private` `protected`      |
| Unmanaged code modifiers   | `unsafe` `extern`                              |

From C# 7, single-statement constructors can also be written as *expression-bodied* members:

```csharp
public Panda(string n) => name = n;
```

#### Overloading constructors

A class or struct may overload constructors. To avoid code duplication, one constructor may call another, using the `this` keyword.

```csharp
using System;

public class Wine
{
    public decimal Price;
    public int Year;

    public Wine(decimal price) { Price = price; }
    public Wine(decimal price, int year) : this(price) { Year = year; }
}
```

When one constructor calls another, the called constructor executes first.

You can pass an expression into another constructor as follows:

```csharp
public Wine(decimal price, DateTime year) : this(price, year.Year) { }
```

The expression itself cannot make use of the this reference, for example, to call an instance method. (This is enforced because the object has not been initialized by the constructor at this stage, so any methods that you call on it are likely to fail.) It can, however, call static methods.

#### Implicit parameterless constructors

For classes, the C# compiler automatically generates a parameterless public con‐ structor if and only if you do not define any constructors. However, as soon as you define at least one constructor, the parameterless constructor is no longer automatically generated.

#### Constructor and field initialization order

We saw previously that fields can be initialized with default values in their declaration:

```csharp
class Player
{
    int shields = 50;    // Initialized first
    int health = 100;   // Initialized second
}
```

*Field initializations occur before the constructor is executed, and in the declaration order of the fields.*

### Object Initializers

To simplify object initialization, any accessible fields or properties of an object can be set via an *object initializer* directly after construction.

```csharp
public class Bunny
{
    public string Name;
    public bool LikesCarrots;
    public bool LikesHumans;

    public Bunny() {}
    public Bunny(string n) { Name = n; }
}
```

Using object initializers, you can instantiate `Bunny` objects as follows:

```csharp
// Note parameterless constructors can omit empty parentheses
Bunny b1 = new Bunny { Name="Bo", LikesCarrots=true, LikesHumans=false };
Bunny b2 = new Bunny("Bo") { LikesCarrots=true, LikesHumans=false };
```

The code to construct `b1` and `b2` is precisely equivalent to:

```csharp
Bunny temp1 = new Bunny();  // temp1 is a compiler-generated name
temp1.Name = "Bo";
temp1.LikesCarrots = true;
temp1.LikesHumans = false;
Bunny b1 = temp1;

Bunny temp2 = new Bunny("Bo");
temp2.LikesCarrots = true;
temp2.LikesHumans = false;
Bunny b2 = temp2;
```

The temporary variables are to ensure that if an exception is thrown during initialization, you can’t end up with a half-initialized object.

[Object Initializers Versus Optional Parameters](Object%20Initializers%20Versus%20Optional%20Parameters.md)

### The this Reference

> The `this` reference refers to the instance itself.

In the following example, the Marry method uses this to set the partner’s mate field:

```csharp
public class Panda
{
    public Panda Mate;

    public void Marry(Panda partner)
    {
        Mate = partner;
        partner.Mate = this;
    }
}
```

The `this` reference also disambiguates a local variable or parameter from a field. For example:

```csharp
public class Test
{
    string name;

    public Test(string name)
    {
        this.name = name;
    }
}
```

*The `this` reference is valid only within nonstatic members of a class or struct.*

### Properties

> Properties look like fields from the outside, but internally they contain logic, like methods do.

For example, you can’t tell by looking at the following code whether CurrentPrice is a field or a property:

```csharp
Stock msft = new Stock();
msft.CurrentPrice = 30;
msft.CurrentPrice -= 3;
Console.WriteLine(msft.CurrentPrice);
```

A property is declared like a field, but with a `get`/`set` block added. Here’s how to implement CurrentPrice as a property:

```csharp
public class Stock
{
    decimal currentPrice;       // The private "backing" field

    public decimal CurrentPrice // The public property
    {
        get { return currentPrice; }
        set { currentPrice = value; }
    }
}
```

`get` and `set` denote property *accessors*. The `get` accessor runs when the property is read. It must return a value of the property’s type. The `set` accessor runs when the property is assigned. It has an implicit parameter named `value` of the property’s type that you typically assign to a private field (in this case, `currentPrice`).

Although properties are accessed in the same way as fields, they differ in that they give the implementer complete control over getting and setting its value. This con‐ trol enables the implementer to choose whatever internal representation is needed, without exposing the internal details to the user of the property.

**Modifiers**: `static`, `public`, `internal`, `private`, `protected`, `new`, `virtual`, `abstract`, `override`, `sealed`, `unsafe`, `extern`

|                            |                                                |
|----------------------------|------------------------------------------------|
| Static modifier            | `static`                                       |
| Access modifiers           | `public` `internal` `private` `protected`      |
| Inheritance modifiers      | `new` `virtual` `abstract` `override` `sealed` |
| Unmanaged code modifiers   | `unsafe` `extern`                              |

#### Read-only and calculated properties

A property is read-only if it specifies only a `get` accessor, and it is write-only if it specifies only a `set` accessor. Write-only properties are rarely used.

Just as with read-only fields, read-only automatic properties can also be assigned in the type’s constructor. This is useful in creating immutable (read-only) types.

A property typically has a dedicated backing field to store the underlying data. However, a property can also be computed from other data. For example:

```csharp
decimal currentPrice,sharesOwned;

public decimal Worth
{
    get { return currentPrice * sharesOwned; }
}
```

#### Expression-bodied properties (C# 6, C# 7)

From C# 6, you can declare a read-only property, such as the preceding example, more tersely as an **expression-bodied property**. A fat arrow replaces all the braces and the get and `return` keywords:

```csharp
public decimal Worth => currentPrice * sharesOwned;
```

C# 7 extends this further by allowing set accessors to be expression-bodied, with a little extra syntax:

```csharp
public decimal Worth
{
    get => currentPrice * sharesOwned;
    set => sharesOwned = value / currentPrice;
}
```

#### Property initializers (C# 6)

From C# 6, you can add a property *initializer* to automatic properties, just as with fields:

```csharp
public decimal CurrentPrice { get; set; } = 123;
```

This gives `CurrentPrice` an initial value of 123. Properties with an initializer can be *read-only*:

```csharp
public int Maximum { get; } = 999;
```

#### get and set accessibility

The `get` and `set` accessors can have different access levels. The typical use case for this is to have a public property with an `internal` or `private` access modifier on the setter:

```csharp
public class Foo
{
    private decimal x;
    public decimal X
    {
        get { return x; }
        private set { x = Math.Round (value, 2); }
    }
}
```

Notice that you declare the property itself with the more permissive access level (`public`, in this case), and add the modifier to the accessor you want to be less accessible.

#### CLR property implementation

C# property accessors internally compile to methods called get_XXX and set_XXX:

```csharp
public decimal get_CurrentPrice {...}
public void set_CurrentPrice (decimal value) {...}
```

Simple nonvirtual property accessors are *inlined* by the JIT (Just-In-Time) compiler, eliminating any performance difference between accessing a property and a field. Inlining is an optimization in which a method call is replaced with the body of that method.

### Indexers

> Indexers provide a natural syntax for accessing elements in a class or struct that encapsulate a list or dictionary of values. Indexers are similar to properties but are accessed via an index argument rather than a property name.

The `string` class has an indexer that lets you access each of its char values via an `int` index:

```csharp
string s = "hello";
Console.WriteLine(s[0]); // 'h'
Console.WriteLine(s[3]); // 'l'
```

The syntax for using indexers is like that for using arrays, except that the index argument(s) can be of any type(s).

Indexers have the same modifiers as properties, and can be called null-conditionally by inserting a question mark before the square bracket.

```csharp
string s = null;
Console.WriteLine(s?[0]); // Writes nothing; no error.
```

#### Implementing an indexer

To write an indexer, define a property called this, specifying the arguments in square brackets.

```csharp
class Sentence
{
    string[] words = "The quick brown fox".Split();

    public string this[int wordNum] // indexer
    {
        get { return words[wordNum]; }
        set { words[wordNum] = value; }
    }
}
```

Here’s how we could use this indexer:

```csharp
Sentence s = new Sentence();
Console.WriteLine(s[3]);    // fox
s[3] = "kangaroo";
Console.WriteLine(s[3]);    // kangaroo
```

A type may declare multiple indexers, each with parameters of different types. An indexer can also take more than one parameter:

```csharp
public string this[int arg1, string arg2]
{
    get { ... } set { ... }
}
```

If you omit the set accessor, an indexer becomes read-only, and expression-bodied syntax may be used in C# 6 to shorten its definition:

```csharp
public string this[int wordNum] => words[wordNum];
```

### Constants

> A constant is a static field whose value can never change. A constant is evaluated statically at compile time, and the compiler literally substitutes its value whenever used (rather like a macro in C++). A constant can be any of the built-in numeric types, bool, char, string, or an enum type.

**A constant is declared with the const keyword and must be initialized with a value.**

A constant is declared with the `const` keyword and must be initialized with a value. For example:

```csharp
public class Test
{
    public const string Message = "Hello World";
}
```

A constant is much more restrictive than a `static readonly` field—both in the types you can use and in field initialization semantics. A constant also differs from a `static readonly` field in that the evaluation of the constant occurs at compile time. For example:

```csharp
public static double Circumference(double radius)
{
    return 2 * System.Math.PI * radius;
}
```

is compiled to:

```csharp
public static double Circumference(double radius)
{
    return 6.2831853071795862 * radius;
}
```

---------

A `static readonly` field is also advantageous when exposing to other assemblies a value that might change in a later version. For instance, suppose assembly X exposes a constant as follows:

```csharp
public const decimal ProgramVersion = 2.3;
```

If assembly Y references X and uses this constant, the value 2.3 will be baked into assembly Y when compiled. This means that if X is later recompiled with the constant set to 2.4, Y will still use the old value of 2.3 until Y is recompiled. A `static readonly` field avoids this problem.

Another way of looking at this is that any value that might change in the future is not constant by definition, and so should not be represented as one.

---------

Constants can also be declared local to a method. For example:

```csharp
static void Main()
{
    const double twoPI  = 2 * System.Math.PI;
    ...
}
```

Nonlocal constants allow the following modifiers:

**Modifiers**: `public`, `internal`, `private`, `protected`, `new`

|                      |                                          |
|----------------------|------------------------------------------|
| Access modifiers     | `public` `internal` `private` `protected` |
| Inheritance modifier | `new`                                     |

### Static Classes

A class can be marked `static`, indicating that it must be composed solely of static members and cannot be subclassed. The `System.Console` and `System.Math` classes are good examples of static classes.

### Finalizers

Finalizers are class-only methods that execute before the garbage collector reclaims the memory for an unreferenced object. The syntax for a finalizer is the name of the class prefixed with the ~ symbol:

```csharp
class Class1
{
    ~Class1()
    {
        ...
    }
}
```

This is actually C# syntax for overriding Object’s Finalize method, and the compiler expands it into the following method declaration:

```csharp
protected override void Finalize()
{
    ...
    base.Finalize();
}
```

Finalizers allow the following modifier: `unsafe`

From C# 7, single-statement finalizers can be written with expression-bodied syntax:

```csharp
~Class1() => Console.WriteLine("Finalizing");
```

### Partial Types and Methods

Partial types allow a type definition to be split—typically across multiple files. A common scenario is for a partial class to be auto-generated from some other source (such as a Visual Studio template or designer) and for that class to be augmented with additional hand-authored methods.

*A constructor with the same parameters, for instance, cannot be repeated*. Partial types are resolved entirely by the compiler, which means that each participant must be available at compile time and must reside in the same assembly.

#### Partial methods

A partial type may contain *partial methods*. These let an auto-generated partial type provide customizable hooks for manual authoring.

```csharp
partial class PaymentForm   // In auto-generated file
{
    ...
    partial void ValidatePayment (decimal amount);
}

partial class PaymentForm   // In hand-authored file
{
    ...
    partial void ValidatePayment (decimal amount)
    {
        if (amount > 100)
            ...
    }
}
```

A partial method consists of two parts: a definition and an implementation. The definition is typically written by a code generator, and the implementation is typically manually authored. If an implementation is not provided, the definition of the partial method is compiled away (as is the code that calls it). This allows auto-generated code to be liberal in providing hooks, without having to worry about bloat. Partial methods must be void and are implicitly private.

### The nameof operator (C# 6)

The `nameof` operator returns the name of any symbol (type, member, variable, and so on) as a string:

```csharp
int count = 123;
string name = nameof(count); // name is "count"
```

Its advantage over simply specifying a string is that of static type checking. Tools such as Visual Studio can understand the symbol reference, so if you rename the symbol in question, all its references will be renamed, too.

## Inheritance

A class can inherit from another class to extend or customize the original class. Inheriting from a class lets you reuse the functionality in that class instead of building it from scratch. A class can inherit from only a single class, but can itself be inherited by many classes, thus forming a class hierarchy. In this example, we start by defining a class called `Asset`:

```csharp
public class Asset
{
    public string Name;
}
```

Next, we define classes called `Stock` and `House`, which will inherit from Asset. Stock and House get everything an Asset has, plus any additional members that they define:

```csharp
public class Stock : Asset
{
    public long SharesOwned;
}

public class House : Asset
{
    public decimal Mortgage;
}
```

Here’s how we can use these classes:

```csharp
Stock msft = new Stock { Name="MSFT", SharesOwned=1000 };

Console.WriteLine(msft.Name);          // MSFT
Console.WriteLine(msft.SharesOwned);   // 1000

House mansion = new House { Name="Mansion", Mortgage=250000 };

Console.WriteLine(mansion.Name);        // Mansion
Console.WriteLine(mansion.Mortgage);    // 250000
```

The derived classes, Stock and House, inherit the Name property from the *base class*, Asset.

*A derived class is also called a subclass.*
*A base class is also called a superclass.*

### Casting and Reference Conversions

An object reference can be:

* **Implicitly `upcast` to a base class reference**
* **Explicitly `downcast` to a subclass reference**

O **upcasting** e o **downcasting** entre tipos de referência compatíveis executam conversões de referência: uma nova referência é (logicamente) criada que aponta para o mesmo objeto. Um upcast sempre é bem sucedido; um downcast só é bem sucedido se o objeto for apropriadamente typed.

#### Upcasting

Uma operação de upcast cria uma referência de classe base a partir de uma referência de subclasse. Cria uma nova referência para a superclasse(classe pai) a partir de um objeto subclasse, herdado da mesma subclasse.

```csharp
Subclass sub = new Subclass();
BaseClass base = sub;           // Upcast

Console.WriteLine(base == sub); // True
```

An upcast operation creates a base class reference from a subclass reference. For example:

```csharp
Stock msft = new Stock();
Asset a = msft; // Upcast
```

After the upcast, variable a still references the same Stock object as variable `msft`. The object being referenced is not itself altered or converted:

```csharp
Console.WriteLine(a == msft);  // True
```
Although a and msft refer to the identical object, a has a more restrictive view on that object:

```csharp
Console.WriteLine(a.Name);          // OK
Console.WriteLine(a.SharesOwned);   // Error: SharesOwned undefined
```

The last line generates a compile-time error because the variable a is of type Asset, even though it refers to an object of type Stock. To get to its `SharesOwned` field, you must downcast the `Asset` to a `Stock`.

#### Downcasting

Uma operação de downcast cria uma referência de subclasse a partir de uma referência de classe base. Processo inverso do upcast, ou seja, quando uma nova referência é criada da classe base para a classe herdade, quando a subclasse cria uma nova referência a partir de um objeto que é sua superclasse, sua classe base.

```csharp
Subclass a = new Subclass();
BaseClass b = a; // Upcast always succeeds
Subclass c = (Subclass)b; // Downcast
```

If a downcast fails, an `InvalidCastException` is thrown.

A downcast operation creates a subclass reference from a base class reference. For example:

```csharp
Stock msft = new Stock();
Asset a = msft;                     // Upcast
Stock s = (Stock)a;                 // Downcast
Console.WriteLine(s.SharesOwned);   // <No error>
Console.WriteLine(s == a);          // True
Console.WriteLine(s == msft);       // True
```

As with an upcast, only references are affected—not the underlying object. A downcast requires an explicit cast because it can potentially fail at runtime:

```csharp
House h = new House();
Asset a = h;        // Upcast always succeeds
Stock s = (Stock)a; // Downcast fails: a is not a Stock
```

If a downcast fails, an InvalidCastException is thrown.

#### The `as` operator

The `as` operator performs a downcast that evaluates to `null` (rather than throwing an exception) if the downcast fails.

```csharp
Asset a = new Asset();
Stock s = a as Stock;   // s is null; no exception thrown
```

This is useful when you’re going to subsequently test whether the result is `null`:

```csharp
if (s != null)
    Console.WriteLine(s.SharesOwned);
```

-----------------

Without such a test, a cast is advantageous, because if it fails, a more helpful exception is `thrown`. We can illustrate by comparing the following two lines of code:

```csharp
int shares = ((Stock)a).SharesOwned;    // Approach #1
int shares = (a as Stock).SharesOwned;  // Approach #2
```

If a is not a `Stock`, the first line throws an `InvalidCastException`, which is an accurate description of what went wrong. The second line throws a `NullReferenceException`, which is ambiguous. Was a not a `Stock` or was a `null`?

Another way of looking at it is that with the cast operator, you’re saying to the compiler: “I’m certain of a value’s type; if I’m wrong, there’s a bug in my code, so throw an exception!” Whereas with the as operator, you’re uncertain of its type and want to branch according to the outcome at runtime.

-----------------

#### The `is` operator

The `is` operator tests whether a reference conversion would succeed; in other words, whether an object derives from a specified class (or implements an interface). It is often used to test before downcasting.

```csharp
if (a is Stock)
    Console.WriteLine(((Stock)a).SharesOwned);
```

The is operator also evaluates to true if an unboxing conversion would succeed. However, it does not consider custom or numeric conversions.

### Virtual Function Members

A function marked as `virtual` can be overridden by subclasses wanting to provide a specialized implementation. Methods, properties, indexers, and events can all be declared virtual.

```csharp
public class Asset
{
    public string Name;
    public virtual decimal Liability => 0;  // Expression-bodied property
}
```

*`Liability => 0` is a shortcut for `{ get { return 0; } }`.*

A subclass overrides a virtual method by applying the `override` modifier:

```csharp
public class Stock : Asset
{
    public long SharesOwned;
}

public class House : Asset
{
    public decimal Mortgage;
    public override decimal Liability => Mortgage;
}
```

By default, the `Liability` of an Asset is 0. A Stock does not need to specialize this behavior. However, the House specializes the `Liability` property to return the value of the `Mortgage`:

```csharp
House mansion = new House { Name="McMansion", Mortgage=250000 };
Asset a = mansion;
Console.WriteLine(mansion.Liability);   // 250000
Console.WriteLine(a.Liability);         // 250000
```

The signatures, return types, and accessibility of the virtual and overridden methods must be identical. An overridden method can call its base class implementation via the `base` keyword.

*Calling virtual methods from a constructor is potentially dangerous because authors of subclasses are unlikely to know, when overriding the method, that they are working with a partially initialized object. In other words, the overriding method may end up accessing methods or properties that rely on fields not yet initialized by the constructor.*

### Abstract Classes and Abstract Members

A class declared as `abstract` can never be instantiated. Instead, only its concrete subclasses can be instantiated.

Abstract classes are able to define `abstract` members. Abstract members are like `virtual` members, except they don’t provide a default implementation. That implementation must be provided by the subclass, unless that subclass is also declared abstract.

```csharp
public abstract class Asset
{
    // Note empty implementation
    public abstract decimal NetValue { get; }
}

public class Stock : Asset
{
    public long SharesOwned;
    public decimal CurrentPrice;

    // Override like a virtual method.
    public override decimal NetValue => CurrentPrice * SharesOwned;
}
```

### Hiding Inherited Members

A base class and a subclass may define identical members.

```csharp
public class A { public int Counter = 1; }
public class B : A { public int Counter = 2; }
```

The `Counter` field in class `B` is said to hide the Counter field in class `A`. Usually, this happens by accident, when a member is added to the base type after an identical member was added to the subtype. For this reason, the compiler generates a warning, and then resolves the ambiguity as follows:

* References to A (at compile time) bind to `A.Counter`.
* References to B (at compile time) bind to `B.Counter`.

Occasionally, you want to hide a member deliberately, in which case you can apply the new modifier to the member in the subclass. The `new` modifier does nothing more than suppress the compiler warning that would otherwise result:

```csharp
public class A { public int Counter = 1; }
public class B : A { public new int Counter = 2; }
```

The `new` modifier communicates your intent to the compiler—and other programmers—that the duplicate member is not an accident.

*C# overloads the `new` keyword to have independent meanings in different contexts. Specifically, the new operator is different from the new member modifier.*

#### new versus override

Consider the following class hierarchy:

```csharp
public class BaseClass
{
    public virtual void Foo()
    {
        Console.WriteLine("BaseClass.Foo");
    }
}

public class Overrider : BaseClass
{
    public override void Foo()
    {
        Console.WriteLine("Overrider.Foo");
    }
}

public class Hider : BaseClass
{
    public new void Foo()
    {
        Console.WriteLine("Hider.Foo");
    }
}
```

The differences in behavior between `Overrider` and Hider are demonstrated in the following code:

```csharp
Overrider over = new Overrider();
BaseClass b1 = over;
over.Foo(); // Overrider.Foo
b1.Foo();   // Overrider.Foo

Hider h = new Hider();
BaseClass b2 = h;
h.Foo();    // Hider.Foo
b2.Foo();   // BaseClass.Foo
```

### Sealing Functions and Classes

> An overridden function member may seal its implementation with the `sealed` keyword to prevent it from being overridden by further subclasses.

In our earlier virtual function member example, we could have sealed House’s implementation of `Liability`, preventing a class that derives from House from overriding `Liability`, as follows:

```csharp
public sealed override decimal Liability { get { return Mortgage; } }
```

You can also seal the class itself, implicitly sealing all the virtual functions, by applying the sealed modifier to the class itself. Sealing a class is more common than seal‐ ing a function member.

Although you can seal against overriding, you can’t seal a member against being `hidden`.

### The base Keyword

The `base` keyword is similar to the this keyword. It serves two essential purposes:

* Accessing an overridden function member from the subclass
* Calling a base-class constructor

In this example, House uses the base keyword to access Asset’s implementation of `Liability`:

```csharp
public class House : Asset
{
    ...
    public override decimal Liability => base.Liability + Mortgage;
}
```

With the base keyword, we access Asset’s `Liability` property *nonvirtually*. This means we will always access Asset’s version of this property—regardless of the instance’s actual runtime type.

The same approach works if `Liability` is *hidden* rather than overridden. (You can also access hidden members by casting to the base class before invoking the function.)

### Constructors and Inheritance

A subclass must declare its own constructors. The base class’s constructors are accessible to the derived class but are never automatically *inherited*.

For example, if we define Baseclass and Subclass as follows:

```csharp
public class Baseclass
{
    public int X;
    public Baseclass() { }
    public Baseclass(int x) { this.X = x; }
}

public class Subclass : Baseclass { }
```

the following is illegal:

```csharp
Subclass s = new Subclass(123);
```

Subclass must hence "redefine" any constructors it wants to expose. In doing so, however, it can call any of the base class’s constructors with the `base` keyword:

```csharp
public class Subclass : Baseclass
{
    public Subclass(int x) : base(x) { }
}
```

The base keyword works rather like the `this` keyword, except that it calls a constructor in the base class.

Base-class constructors always execute first; this ensures that base initialization occurs before specialized initialization.

#### Implicit calling of the parameterless base-class constructor

If a constructor in a subclass omits the base keyword, the base type’s parameterless constructor is implicitly called.

```csharp
public class BaseClass
{
    public int X;
    public BaseClass() { X = 1; }
}

public class Subclass : BaseClass
{
    public Subclass()
    {
        Console.WriteLine(X); // 1
    }
}
```

If the base class has no accessible parameterless constructor, subclasses are forced to use the base keyword in their constructors.

#### Constructor and field initialization order

When an object is instantiated, initialization takes place in the following order:

1. From subclass to base class:
    1. Fields are initialized.
    2. Arguments to base-class constructor calls are evaluated.
2. From base class to subclass:
    1. Constructor bodies execute.

The following code demonstrates:

```csharp
public class B
{
    int x = 1;          // Executes 3rd
    public B(int x)
    {
        ...             // Executes 4th
    }
}

public class D : B
{
    int y = 1;          // Executes 1st
    public D(int x)
        : base(x + 1) // Executes 2nd
    {
        ...             // Executes 5th
    }
}
```

### Overloading and Resolution

nheritance has an interesting impact on method overloading. Consider the following two overloads:

```csharp
static void Foo(Asset a) { }
static void Foo(House h) { }
```

When an overload is called, the most specific type has precedence:

```csharp
House h = new House(...);
Foo(h); // Calls Foo(House)
```

The particular overload to call is determined statically (at compile time) rather than at runtime. The following code calls Foo(Asset), even though the runtime type of a is House:

```csharp
Asset a = new House(...);
Foo(a); // Calls Foo(Asset)
```

## The object Type

> `object` (`System.Object`) is the ultimate base class for all types. Any type can be *upcast* to object.

**object is a reference type**, by virtue of being a class. Despite this, value types, such as int, can also be cast to and from object.

When you cast between a value type and object, the CLR must perform some special work to bridge the difference in semantics between value and reference types. This process is called **boxing** and **unboxing**.

### Boxing and Unboxing

> Boxing is the act of converting a **value-type instance to a reference-type** instance. The reference type may be either the `object` class or an interface.

```csharp
int x = 9;
object obj = x; // Box the int
```

Unboxing reverses the operation by casting the object back to the original value type:

```csharp
int y = (int)obj; // Unbox the int
```

*Unboxing requires an explicit cast*. The runtime checks that the stated value type matches the actual object type and throws an `InvalidCastException` if the check fails.

Boxing *copies* the value-type instance into the new object, and unboxing copies the contents of the object back into a value-type instance.

```csharp
object obj = 9;     // 9 is inferred to be of type int
long x = (long)obj; // InvalidCastException

The following succeeds, however:

```csharp
object obj = 9;
long x = (int) obj;
```

As does this:

```csharp
object obj = 3.5;           // 3.5 is inferred to be of type double
int x = (int) (double) obj; // x is now 3
```

In the last example, (`double`) performs an *unboxing* and then (`int`) performs a numeric conversion.

#### Copying semantics of boxing and unboxing

Boxing *copies* the value-type instance into the new object, and unboxing copies the contents of the object back into a value-type instance. In the following example, changing the value of `i` doesn’t change its previously boxed copy:

```csharp
int i = 3;
object boxed = i;
i = 5;
Console.WriteLine(boxed); // 3
```

### Static and Runtime Type Checking

C# programs are type-checked both statically (at compile time) and at runtime (by the CLR).

Static type checking enables the compiler to verify the correctness of your program without running it. The following code will fail because the compiler enforces static typing:

```csharp
int x = "5";
```

Runtime type checking is performed by the CLR when you downcast via a reference conversion or unboxing. For example:

```csharp
object y = "5";
int z = (int) y;    // Runtime error, downcast failed
```

Runtime type checking is possible because each object on the heap internally stores a little type token. This token can be retrieved by calling the `GetType` method of `object`.

### The GetType Method and typeof Operator

All types in C# are represented at runtime with an instance of `System.Type`. There are two basic ways to get a `System.Type` object:

* Call `GetType` on the instance.
* Use the `typeof` operator on a type name.

`GetType` is evaluated at runtime; `typeof` is evaluated statically at compile time (when generic type parameters are involved, it’s resolved by the just-in-time compiler).

System.Type has properties for such things as the type’s name, assembly, base type, and so on. For example:

```csharp
using System;

public class Point { public int X, Y; }

class Test
{
    static void Main()
    {
        Point p = new Point();

        Console.WriteLine(p.GetType().Name);      // Point
        Console.WriteLine(typeof(Point).Name);    // Point
        Console.WriteLine(p.GetType() == typeof(Point));   // True
        Console.WriteLine(p.X.GetType().Name);     // Int32
        Console.WriteLine(p.Y.GetType().FullName); // System.Int32
    }
}
```

`System.Type` also has methods that act as a gateway to the runtime’s reflection model.

## Structs

A `struct` is similar to a class, with the following key differences:

* A struct is a value type, whereas a class is a reference type.
* A struct does not support inheritance (other than implicitly deriving from object, or more precisely, `System.ValueType`).

A struct can have all the members a class can, except the following:

* A parameterless constructor.
* Field initializers.
* A finalizer.
* Virtual or protected members.

A struct is appropriate when value-type semantics are desirable. Good examples of structs are numeric types, where it is more natural for assignment to copy a value rather than a reference. Because a struct is a value type, each instance does not require instantiation of an object on the heap; this incurs a useful saving when creating many instances of a type. For instance, creating an array of value type requires only a single heap allocation.

### Struct Construction Semantics

The construction semantics of a struct are as follows:

* A parameterless constructor that you can’t override implicitly exists. This per‐ forms a bitwise-zeroing of its fields.
* When you define a struct constructor, you must explicitly assign every field.

(And you can’t have field initializers.) Here is an example of declaring and calling struct constructors:

```csharp
public struct Point
{
    int x, y;
    public Point(int x, int y) { this.x = x; this.y = y; }
}

...

Point p1 = new Point();     // p1.x and p1.y will be 0
Point p2 = new Point(1, 1); // p1.x and p1.y will be 1
```

The next example generates three compile-time errors:

```csharp
public struct Point
{
    int x = 1;                          // Illegal: field initializer
    int y;
    public Point() {}                   // Illegal: parameterless constructor
    public Point(int x) {this.x = x;}  // Illegal: must assign field y
}
```

Changing `struct` to `class` makes this example legal.

## Access Modifiers

To promote encapsulation, a type or type member may limit its accessibility to other types and other assemblies by adding one of five access modifiers to the declaration:

**public**
> Fully accessible. This is the implicit accessibility for members of an enum or interface.

**internal**
> Accessible only within the containing assembly or friend assemblies. This is the default accessibility for non-nested types.

**private**
> Accessible only within the containing type. This is the default accessibility for members of a class or struct.

**protected**
> Accessible only within the containing type or subclasses.

**protected internal**
> The union of protected and internal accessibility. Eric Lippert explains it as follows: Everything is as private as possible by default, and each modifier makes the thing more accessible. So something that is protected internal is made more accessible in two ways.

### Examples

`Class2` is accessible from outside its assembly; `Class1` is not:

```csharp
class Class1 {} // Class1 is internal (default)
public class Class2 {}
```

`ClassB` exposes field `x` to other types in the same assembly; `ClassA` does not:

```csharp
class ClassA { int x; } // x is private (default)
class ClassB { internal int x; }
```

Functions within Subclass can call Bar but not Foo:

```csharp
class BaseClass
{
    void Foo() {} // Foo is private (default)
    protected void Bar() {}
}

class Subclass : BaseClass
{
    void Test1() { Foo(); } // Error - cannot access Foo
    void Test2() { Bar(); } // OK
}
```

### Accessibility Capping

A type caps the accessibility of its declared members. The most common example of capping is when you have an `internal` type with public members. For example:

```csharp
class C { public void Foo() {} }
```

C’s (default) `internal` accessibility caps Foo’s accessibility, effectively making Foo internal. A common reason Foo would be marked public is to make for easier refactoring, should C later be changed to `public`.

### Restrictions on Access Modifiers

When overriding a base class function, accessibility must be identical on the overridden function. For example:

```csharp
class BaseClass { protected virtual void Foo() {} }
class Subclass1 : BaseClass { protected override void Foo() {} }    // OK
class Subclass2 : BaseClass { public override void Foo() {} }       // Error
```

(An exception is when overriding a `protected internal` method in another assembly, in which case the override must simply be `protected`.)

The compiler prevents any inconsistent use of access modifiers. For example, a subclass itself can be less accessible than a base class, but not more:

```csharp
internal class A {}
public class B : A {}   // Error
```

## Interfaces

> An interface is similar to a class, but it provides a specification rather than an implementation for its members.

An interface is special in the following ways:

* Interface members are all *implicitly abstract*. In contrast, a class can provide both abstract members and concrete members with implementations.
* A class (or struct) can implement multiple interfaces. In contrast, a class can inherit from only a single class, and a struct cannot inherit at all (aside from deriving from `System.ValueType`).

An interface declaration is like a class declaration, but it provides no implementation for its members, since all its members are implicitly abstract. These members will be implemented by the classes and structs that implement the interface. An interface can contain only methods, properties, events, and indexers, which non‐ coincidentally are precisely the members of a class that can be abstract.

Here is the definition of the IEnumerator interface, defined in `System.Collections`:

```csharp
public interface IEnumerator
{
    bool MoveNext();
    object Current { get; }
    void Reset();
}
```

**Interface members are always implicitly public and cannot declare an access modifier. Implementing an interface means providing a public implementation for all its members**.

```csharp
internal class Countdown : IEnumerator
{
    int count = 11;
    public bool MoveNext() => count-- > 0;
    public object Current => count;
    public void Reset() { throw new NotSupportedException(); }
}
```

You can implicitly cast an object to any interface that it implements. For example:

```csharp
IEnumerator e = new Countdown();
while(e.MoveNext())
    Console.Write(e.Current);   // 109876543210
```

-------------------

Even though `Countdown` is an internal class, its members that implement `IEnumerator` can be called publicly by casting an instance of `Countdown` to `IEnumerator`. For instance, if a public type in the same assembly defined a method as follows:

```csharp
public static class Util
{
    public static object GetCountDown() => new CountDown();
}
```

a caller from another assembly could do this:

```csharp
IEnumerator e = (IEnumerator) Util.GetCountDown();
e.MoveNext();
```

If `IEnumerator` was itself defined as internal, this wouldn’t be possible.

-------------------

### Extending an Interface

Interfaces may derive from other interfaces. For instance:

```csharp
public interface IUndoable { void Undo(); }
public interface IRedoable : IUndoable { void Redo(); }
```

`IRedoable` "inherits" all the members of `IUndoable`. In other words, types that implement `IRedoable` must also implement the members of `IUndoable`.

### Explicit Interface Implementation

Implementing multiple interfaces can sometimes result in a collision between member signatures. You can resolve such collisions by *explicitly implementing* an interface member. Consider the following example:

```csharp
interface I1 { void Foo(); }
interface I2 { int Foo(); }

public class Widget : I1, I2
{
    public void Foo()
    {
        Console.WriteLine("Widget's implementation of I1.Foo");
    }

    int I2.Foo()
    {
        Console.WriteLine("Widget's implementation of I2.Foo");
        return 42;
    }
}
```

Because both `I1` and `I2` have conflicting Foo signatures, `Widget` explicitly implements I2’s `Foo` method. This lets the two methods coexist in one class. The only way to call an explicitly implemented member is to cast to its interface:

```csharp
Widget w = new Widget();
w.Foo();        // Widget's implementation of I1.Foo
((I1)w).Foo();  // Widget's implementation of I1.Foo
((I2)w).Foo();  // Widget's implementation of I2.Foo
```

Another reason to explicitly implement interface members is to hide members that are highly specialized and distracting to a type’s normal use case. For example, a type that implements ISerializable would typically want to avoid flaunting its ISerializable members unless explicitly cast to that interface.

### Implementing Interface Members Virtually

**An implicitly implemented interface member is, by default, sealed. It must be marked virtual or abstract in the base class in order to be overridden**.

```csharp
public interface IUndoable { void Undo(); }

public class TextBox : IUndoable
{
    public virtual void Undo() => Console.WriteLine("TextBox.Undo");
}

public class RichTextBox : TextBox
{
    public override void Undo() => Console.WriteLine("RichTextBox.Undo");
}
```

Calling the interface member through either the base class or the interface calls the subclass’s implementation:

```csharp
RichTextBox r = new RichTextBox();
r.Undo();               // RichTextBox.Undo
((IUndoable)r).Undo();  // RichTextBox.Undo
((TextBox)r).Undo();    // RichTextBox.Undo
```

*An explicitly implemented interface member cannot be marked virtual, nor can it be overridden in the usual manner. It can, however, be reimplemented.*

### Reimplementing an Interface in a Subclass

A subclass can reimplement any interface member already implemented by a base class. Reimplementation hijacks a member implementation (when called through the interface) and works whether or not the member is virtual in the base class. It also works whether a member is implemented implicitly or explicitly—although it works best in the latter case, as we will demonstrate.

In the following example, `TextBox` implements `IUndoable.Undo` explicitly, and so it cannot be marked as virtual. In order to “override” it, `RichTextBox` must reimplement IUndoable’s Undo method:

```csharp
public interface IUndoable { void Undo(); }

public class TextBox : IUndoable
{
    void IUndoable.Undo() => Console.WriteLine("TextBox.Undo");
}

public class RichTextBox : TextBox, IUndoable
{
    public void Undo() => Console.WriteLine("RichTextBox.Undo");
}
```

Calling the reimplemented member through the interface calls the subclass’s implementation:

```csharp
RichTextBox r = new RichTextBox();
r.Undo();               // RichTextBox.Undo Case 1
((IUndoable)r).Undo();  // RichTextBox.Undo Case 2
```

Assuming the same `RichTextBox` definition, suppose that `TextBox` implemented Undo implicitly:

```csharp
public class TextBox : IUndoable
{
    public void Undo() => Console.WriteLine("TextBox.Undo");
}
```

This would give us another way to call Undo, which would “break” the system, as shown in Case 3:

```csharp
RichTextBox r = new RichTextBox();
r.Undo();               // RichTextBox.Undo Case 1
((IUndoable)r).Undo();  // RichTextBox.Undo Case 2
((TextBox)r).Undo();    // TextBox.Undo Case 3
```

Case 3 demonstrates that reimplementation hijacking is effective only when a member is called through the interface and not through the base class. This is usually undesirable as it can mean inconsistent semantics. This makes reimplementation most appropriate as a strategy for overriding explicitly implemented interface members.

#### Alternatives to interface reimplementation

Even with explicit member implementation, interface reimplementation is problematic for a couple of reasons:

* The subclass has no way to call the base class method.
* The base class author may not anticipate that a method be reimplemented and may not allow for the potential consequences.

Reimplementation can be a good last resort when subclassing hasn’t been anticipated. A better option, however, is to design a base class such that reimplementation will never be required. There are two ways to achieve this:

* When implicitly implementing a member, mark it virtual if appropriate.
* When explicitly implementing a member, use the following pattern if you anticipate that subclasses might need to override any logic:

```csharp
public class TextBox : IUndoable
{
    void IUndoable.Undo() => Undo(); // Calls method below
    protected virtual void Undo() => Console.WriteLine("TextBox.Undo");
}

public class RichTextBox : TextBox
{
    protected override void Undo() => Console.WriteLine("RichTextBox.Undo");
}
```

If you don’t anticipate any subclassing, you can mark the class as `sealed` to preempt interface reimplementation.

### Interfaces and Boxing

Converting a struct to an interface causes boxing. Calling an implicitly implemented member on a struct does not cause boxing:

```csharp
interface I { void Foo(); }
struct S : I { public void Foo() {} }
...
S s = new S();
s.Foo();        // No boxing.

I i = s;        // Box occurs when casting to interface.
i.Foo();
```

[Writing a Class Versus an Interface](Writing%20a%20Class%20Versus%20an%20Interface.md)

## Enums

> An enum is a special value type that lets you specify a group of named numeric constants.

For example:

```csharp
public enum BorderSide { Left, Right, Top, Bottom }
```

We can use this enum type as follows:

```csharp
BorderSide topSide = BorderSide.Top;
bool isTop = (topSide == BorderSide.Top); // true
```

Each enum member has an underlying integral value. By default:

* Underlying values are of type `int`.
* The constants 0, 1, 2... are automatically assigned, in the declaration order of the enum members.

You may specify an alternative integral type, as follows:

```csharp
public enum BorderSide : byte { Left, Right, Top, Bottom }
```

You may also specify an explicit underlying value for each enum member:

```csharp
public enum BorderSide : byte { Left=1, Right=2, Top=10, Bottom=11 }
```

---------------

The compiler also lets you explicitly assign some of the enum members. The unassigned enum members keep incrementing from the last explicit value. The preceding example is equivalent to the following:

```csharp
public enum BorderSide : byte { Left=1, Right, Top=10, Bottom }
```

---------------

### Enum Conversions

You can convert an `enum` instance to and from its underlying integral value with an explicit cast:

```csharp
int i = (int) BorderSide.Left;
BorderSide side = (BorderSide) i;
bool leftOrRight = (int) side <= 2;
```

You can also explicitly cast one enum type to another. Suppose `HorizontalAlign` ment is defined as follows:

```csharp
public enum HorizontalAlignment
{
    Left = BorderSide.Left,
    Right = BorderSide.Right,
    Center
}
```

A translation between the enum types uses the underlying integral values:

```csharp
HorizontalAlignment h = (HorizontalAlignment) BorderSide.Right;
// same as:
HorizontalAlignment h = (HorizontalAlignment) (int) BorderSide.Right;
```

The numeric literal 0 is treated specially by the compiler in an `enum` expression and does not require an explicit cast:

```csharp
BorderSide b = 0;   // No cast required
if (b == 0) ...
```

There are two reasons for the special treatment of 0:

* The first member of an enum is often used as the “default” value.
* For *combined enum* types, 0 means “no flags.”

### Flags Enums

You can combine enum members. To prevent ambiguities, members of a combinable enum require explicitly assigned values, typically in powers of two.

```csharp
[Flags]
public enum BorderSides { None=0, Left=1, Right=2, Top=4, Bottom=8 }
```

### Type-Safety Issues

Consider the following enum:

```csharp
public enum BorderSide { Left, Right, Top, Bottom }
```

Since an enum can be cast to and from its underlying integral type, the actual value it may have may fall outside the bounds of a legal enum member. For example:

```csharp
BorderSide b = (BorderSide) 12345;
Console.WriteLine(b); // 12345
```

The bitwise and arithmetic operators can produce similarly invalid values:

```csharp
BorderSide b = BorderSide.Bottom;
b++;    // No errors
```

An invalid BorderSide would break the following code:

```csharp
void Draw (BorderSide side)
{
    if (side == BorderSide.Left) {...}
    else if (side == BorderSide.Right) {...}
    else if (side == BorderSide.Top) {...}
    else {...} // Assume BorderSide.Bottom
}
```

One solution is to add another else clause:

```csharp
...
else if (side == BorderSide.Bottom) ...
else throw new ArgumentException ("Invalid BorderSide: " + side, "side");
```

Another workaround is to explicitly check an enum value for validity. The static `Enum.IsDefined` method does this job:

```csharp
BorderSide side = (BorderSide) 12345;
Console.WriteLine(Enum.IsDefined(typeof(BorderSide), side)); // False
```

Unfortunately, `Enum.IsDefined` does not work for flagged enums. However, the following helper method (a trick dependent on the behavior of `Enum.ToString()`) returns true if a given flagged enum is valid:

```csharp
static bool IsFlagDefined(Enum e)
{
    decimal d;
    return !decimal.TryParse(e.ToString(), out d);
}

[Flags]
public enum BorderSides { Left=1, Right=2, Top=4, Bottom=8 }

static void Main()
{
    for(int i = 0; i <= 16; i++)
    {
        BorderSides side =(BorderSides)i;
        Console.WriteLine(IsFlagDefined(side) + " " + side);
    }
}
```

## Nested Types

> A nested type is declared within the scope of another type.

```csharp
public class TopLevel
{
    public class Nested { }                 // Nested class
    public enum Color { Red, Blue, Tan }    // Nested enum
}
```

A nested type has the following features:

* It can access the enclosing type’s private members and everything else the enclosing type can access.
* It can be declared with the full range of access modifiers, rather than just public and `internal`.
* The default accessibility for a nested type is `private` rather than `internal`.
* Accessing a nested type from outside the enclosing type requires qualification with the enclosing type’s name (like when accessing static members).

All types (classes, structs, interfaces, delegates and enums) can be nested inside either a class or a struct.

Here is an example of accessing a private member of a type from a nested type:

```csharp
public class TopLevel
{
    static int x;
    class Nested
    {
        static void Foo()
        {
            Console.WriteLine(TopLevel.x);
        }
    }
}
```

Here is an example of applying the `protected` access modifier to a nested type:

```csharp
public class TopLevel
{
    protected class Nested { }
}

public class SubTopLevel : TopLevel
{
    static void Foo() { new TopLevel.Nested(); }
}
```

Here is an example of referring to a nested type from outside the enclosing type:

```csharp
public class TopLevel
{
    public class Nested { }
}

class Test
{
    TopLevel.Nested n;
}
```

Nested types are used heavily by the compiler itself when it generates private classes that capture state for constructs such as iterators and anonymous methods.

*If the sole reason for using a nested type is to avoid cluttering a namespace with too many types, consider using a nested namespace instead. A nested type should be used because of its stronger access control restrictions, or when the nested class must access private members of the containing class.*

## Generics

> C# has two separate mechanisms for writing code that is reusable across different types: `inheritance` and `generics`. Whereas inheritance expresses reusability with a base type, generics express reusability with a “template” that contains “placeholder” types. Generics, when compared to inheritance, can *increase type safety* and *reduce* *casting* and *boxing*.

*C# generics and C++ templates are similar concepts, but they work differently.*

### Generic Types

A generic type declares *type parameters*—placeholder types to be filled in by the consumer of the generic type, which supplies the *type arguments*.

Here is a generic type `Stack<T>`, designed to stack instances of type `T`. `Stack<T>` declares a single type *parameter* `T`:

```csharp
public class Stack<T>
{
    int position;
    T[] data = new T[100];
    public void Push(T obj) => data[position++] = obj;
    public T Pop()          => data[--position];
}
```

We can use `Stack<T>` as follows:

```csharp
var stack = new Stack<int>();
stack.Push(5);
stack.Push(10);

int x = stack.Pop(); // x is 10
int y = stack.Pop(); // y is 5
```

`Stack<int>` fills in the type parameter `T` with the type argument int, implicitly creating a type on the fly (the synthesis occurs at runtime). Attempting to push a string onto our `Stack<int>` would, however, produce a compile-time error. `Stack<int>` effectively has the following definition (substitutions appear in bold, with the class name hashed out to avoid confusion):

```csharp
public class Stack<T>
{
    int position;
    **int**[] data = new T[100];
    public void Push(**int** obj) => data[position++] = obj;
    public **int** Pop()          => data[--position];
}
```

Technically, we say that `Stack<T>` is an open type, whereas `Stack<int>` is a closed type. At runtime, all generic type instances are closed—with the placeholder types filled in.

This means that the following statement is illegal:

```csharp
var stack = new Stack<T>(); // Illegal: What is T?
```

unless inside a class or method which itself defines `T` as a type parameter:

```csharp
public class Stack<T>
{
    ...
    public Stack<T> Clone()
    {
        Stack<T> clone = new Stack<T>(); // Legal
        ...
    }
}
```

### Why Generics Exist

**Generics exist to write code that is reusable across different types.** Suppose we needed a stack of integers, but we didn’t have generic types. One solution would be to hardcode a separate version of the class for every required element type (e.g., `IntStack`, `StringStack`, etc.). Clearly, this would cause considerable code duplication. Another solution would be to write a stack that is generalized by using object as the element type:

```csharp
public class ObjectStack
{
    int position;
    **object**[] data = new **object**[10];
    public void Push(**object** obj)    => data[position++] = obj;
    public **object** Pop()             => data[--position];
}
```

An `ObjectStack`, however, wouldn’t work as well as a hardcoded `IntStack` for specifically stacking integers. Specifically, an `ObjectStack` would require boxing and downcasting that could not be checked at compile time:

```csharp
// Suppose we just want to store integers here:
ObjectStack stack = new ObjectStack();

stack.Push("s");            // Wrong type, but no error!
int i = (int)stack.Pop();   // Downcast - runtime error
```

What we need is both a general implementation of a stack that works for all element types, and a way to easily specialize that stack to a specific element type for increased type safety and reduced casting and boxing. Generics give us precisely this, by allowing us to parameterize the element type. `Stack<T>` has the benefits of both ObjectStack and `IntStack`. Like `ObjectStack`, `Stack<T>` is written once to work generally across all types. Like `IntStack`, `Stack<T>` is *specialized* for a particular type—the beauty is that this type is T, which we substitute on the fly.

*`ObjectStack` is functionally equivalent to `Stack<object>`.*

### Generic Methods

> A generic method declares type parameters within the signature of a method.

With generic methods, many fundamental algorithms can be implemented in a general-purpose way only. Here is a generic method that swaps the contents of two variables of any type `T`:

```csharp
static void Swap<T>(ref T a, ref T b)
{
    T temp = a;
    a = b;
    b = temp;
}
```

`Swap<T>` can be used as follows:

```csharp
int x = 5;
int y = 10;
Swap(ref x, ref y);
```

The `default` keyword can be used to get the default value for a generic type parameter. The default value for a reference type is `null`, and the default value for a value type is the result of bitwise-zeroing the value type’s fields.

Generally, there is no need to supply type arguments to a generic method, because the compiler can implicitly infer the type. If there is ambiguity, generic methods can be called with the type arguments as follows:

```csharp
Swap<int>(ref x, ref y);
```

**Within a generic *type*, a method is not classed as generic unless it *introduces* type parameters (with the angle bracket syntax)**. The `Pop` method in our generic stack merely uses the type’s existing type parameter, `T`, and is not classed as a generic method.

Methods and types are the only constructs that can introduce type parameters. Properties, indexers, events, fields, constructors, operators, and so on cannot declare type parameters, although they can partake in any type parameters already declared by their enclosing type. In our generic stack example, for instance, we could write an indexer that returns a generic item:

```csharp
public T this[int index] => data[index];
```

Similarly, constructors can partake in existing type parameters, but not introduce them:

```csharp
public Stack<T>() { }   // Illegal
```

### Declaring Type Parameters

> Type parameters can be introduced in the declaration of classes, structs, interfaces, delegates, and methods. Other constructs, such as properties, cannot introduce a type parameter, but can use one.

For example, the property Value uses `T`:

```csharp
public struct Nullable<T>
{
    public T Value { get; }
}
```

A generic type or method can have multiple parameters. For example:

```csharp
class Dictionary<TKey, TValue> {...}
```

To instantiate:

```csharp
Dictionary<int,string> myDic = new Dictionary<int,string>();
// OR
var myDic = new Dictionary<int,string>();
```

**Generic type names and method names can be overloaded as long as the number of type parameters is different**.

For example, the following three type names do not conflict:

```csharp
class A {}
class A<T> {}
class A<T1,T2> {}
```

*By convention, generic types and methods with a single type parameter typically name their parameter T, as long as the intent of the parameter is clear. When using multiple type parameters, each parameter is prefixed with T, but has a more descriptive name.*

### typeof and Unbound Generic Types

Open generic types do not exist at runtime: open generic types are closed as part of compilation. However, it is possible for an unbound generic type to exist at runtime — purely as a Type object. The only way to specify an unbound generic type in C# is with the typeof operator:

```csharp
class A<T> {}
class A<T1,T2> {}
...

Type a1 = typeof(A<>);  // Unbound type (notice no type arguments).
Type a2 = typeof(A<,>); // Use commas to indicate multiple type args.
```

You can also use the typeof operator to specify a closed type:

```csharp
Type a3 = typeof(A<int,int>);
```

or an open type (which is closed at runtime):

```csharp
class B<T>
{
    void X()
    {
        Type t = typeof(T);
    }
}
```

### Generic Constraints

> By default, a type parameter can be substituted with any type whatsoever. *Constraints* can be applied to a type parameter to require more specific type arguments.

These are the possible constraints:

```csharp
where T : base-class    // Base-class constraint
where T : interface     // Interface constraint
where T : class         // Reference-type constraint
where T : struct        // Value-type constraint (excludes Nullable types)
where T : new()         // Parameterless constructor constraint
where U : T             // Naked type constraint
```

In the following example, `GenericClass<T,U>` requires `T` to derive from (or be identical to) `SomeClass` and implement `Interface1`, and requires `U` to provide a parameterless constructor:

```csharp
class SomeClass {}
interface Interface1 {}

class GenericClass<T,U> where T : SomeClass, Interface1
                        where U : new()
{
    ...
}
```

**Constraints can be applied wherever type parameters are defined, in both methods and type definitions**.

A *base-class constraint* specifies that the type parameter must subclass (or match) a particular class; an interface constraint specifies that the type parameter must implement that interface. These constraints allow instances of the type parameter to be implicitly converted to that class or interface.

For example, suppose we want to write a generic `Max` method, which returns the maximum of two values. We can take advantage of the generic interface defined in the framework called `IComparable<T>`:

```csharp
public interface IComparable<T> // Simplified version of interface
{
    int CompareTo(T other);
}
```

`CompareTo` returns a positive number if this is greater than other. Using this interface as a constraint, we can write a `Max` method as follows (to avoid distraction, `null` checking is omitted):

```csharp
static T Max<T>(T a, T b) where T : IComparable<T>
{
    return a.CompareTo(b) > 0 ? a : b;
}
```

The `Max` method can accept arguments of any type implementing `IComparable<T>` (which includes most built-in types such as `int` and `string`):

```csharp
int z = Max(5, 10);                // 10
string last = Max("ant", "zoo");   // zoo
```

The *class constraint* and *struct constraint* specify that `T` must be a reference type or (non-nullable) value type. A great example of the struct constraint is the `System.Nullable<T>` struct:

```csharp
struct Nullable<T> where T : struct {...}
```

The *parameterless constructor* constraint requires `T` to have a public parameterless constructor. If this constraint is defined, you can call `new()` on `T`:

```csharp
static void Initialize<T>(T[] array) where T : new()
{
    for (int i = 0; i < array.Length; i++)
        array[i] = new T();
}
```

The *naked type constraint* requires one type parameter to derive from (or match) another type parameter. In this example, the method `FilteredStack` returns another Stack, containing only the subset of elements where the type parameter `U` is of the type parameter `T`:

```csharp
class Stack<T>
{
    Stack<U> FilteredStack<U>() where U : T
    {
        ...
    }
}
```

### Subclassing Generic Types

> A generic class can be subclassed just like a nongeneric class. The subclass can leave the base class’s type parameters open.

```csharp
class Stack<T> {...}
class SpecialStack<T> : Stack<T> {...}
```

Or the subclass can close the generic type parameters with a concrete type:

```csharp
class IntStack : Stack<int> {...}
```

A subtype can also introduce fresh type arguments:

```csharp
class List<T> {...}
class KeyedList<T,**TKey**> : List<T> {...}
```

------------

*Technically, all type arguments on a subtype are fresh*: you could say that a subtype closes and then reopens the base type arguments. This means that a subclass can give new (and potentially more meaningful) names to the type arguments it reopens:

```csharp
class List<T> {...}
class KeyedList<TElement,TKey> : List<TElement> {...}
```

------------

### Self-Referencing Generic Declarations

A type can name itself as the concrete type when closing a type argument:

```csharp
public interface IEquatable<T> { bool Equals(T obj); }

public class Balloon : IEquatable<Balloon>
{
    public string Color { get; set; }
    public int CC { get; set; }

    public bool Equals(Balloon b)
    {
        if (b == null)
            return false;

        return b.Color == Color && b.CC == CC;
    }
```

The following are also legal:

```csharp
class Foo<T> where T : IComparable<T> { ... }
class Bar<T> where T : Bar<T> { ... }
```

### Static Data

**Static data is unique for each closed type.**

```csharp
class Bob<T> { public static int Count; }

class Test
{
    static void Main()
    {
        Console.WriteLine(++Bob<int>.Count);    // 1
        Console.WriteLine(++Bob<int>.Count);    // 2
        Console.WriteLine(++Bob<string>.Count); // 1
        Console.WriteLine(++Bob<object>.Count); // 1
    }
}
```

### Type Parameters and Conversions

C#’s cast operator can perform several kinds of conversion, including:

* Numeric conversion
* Reference conversion
* Boxing/unboxing conversion
* Custom conversion

The decision as to which kind of conversion will take place happens at compile time, based on the known types of the operands. This creates an interesting scenario with generic type parameters, because the precise operand types are unknown at compile time. If this leads to ambiguity, the compiler generates an error.

The most common scenario is when you want to perform a reference conversion:

```csharp
StringBuilder Foo<T> (T arg)
{
    if (arg is StringBuilder)
        return (StringBuilder) arg; // Will not compile
    ...
}
```

Without knowledge of `T`’s actual type, the compiler is concerned that you might have intended this to be a *custom conversion*. The simplest solution is to instead use the as operator, which is unambiguous because it cannot perform custom conversions:

```csharp
StringBuilder Foo<T> (T arg)
{
    StringBuilder sb = arg as StringBuilder;
    if (sb != null)
        return sb;
    ...
}
```

A more general solution is to first *cast* to `object`. This works because conversions to/from `object` are assumed not to be custom conversions, but reference or boxing/ unboxing conversions. In this case, `StringBuilder` is a reference type, so it has to be a reference conversion:

```csharp
return (StringBuilder) (object) arg;
```

Unboxing conversions can also introduce ambiguities. The following could be an unboxing, numeric, or custom conversion:

```csharp
int Foo<T>(T x) => (int) x;    // Compile-time error
```

The solution, again, is to first cast to object and then to int (which then unambiguously signals an unboxing conversion in this case):

```csharp
int Foo<T>(T x) => (int) (object) x;
```

### Covariance

**Covariance**: Especialização de objetos. Ex.: `object -> string`.
```csharp
Action<object> x = ...;
Action<string> y = x;
```

> Assuming `A` is convertible to `B`, `X` has a covariant type parameter if `X<A>` is convertible to `X<B>`.

*With C#’s notion of covariance (and contravariance), “convertible” means convertible via an implicit reference conversion —such as `A` subclassing `B`, or `A` implementing `B`. Numeric conversions, boxing conversions, and custom conversions are not included.*

For instance, type `IFoo<T>` has a covariant `T` if the following is legal:

```csharp
IFoo<string> s = ...;
IFoo<object> b = s;
```

From C# 4.0, interfaces permit covariant type parameters (as do delegates), but classes do not. Arrays also allow covariance (`A[]` can be converted to `B[]` if `A` has an implicit reference conversion to `B`), and are discussed here for comparison.

*Covariance and contravariance (or simply “variance”) are advanced concepts. The motivation behind introducing and enhancing variance in C# was to allow generic interface and generic types (in particular, those defined in the Framework, such as `IEnumerable<T>`) to work more as you’d expect. You can benefit from this without understanding the details behind covariance and contravariance.*

#### Variance is not automatic

To ensure static type safety, type parameters are not automatically variant. Consider the following:

```csharp
class Animal {}
class Bear : Animal {}
class Camel : Animal {}

public class Stack<T>   // A simple Stack implementation
{
    int position;
    T[] data = new T[100];
    public void Push(T obj) => data[position++] = obj;
    public T Pop()          => data[--position];
}
```

The following fails to compile:

```csharp
Stack<Bear> bears = new Stack<Bear>();
Stack<Animal> animals = bears;  // Compile-time error
```

That restriction prevents the possibility of runtime failure with the following code:

```csharp
animals.Push(new Camel()); // Trying to add Camel to bears
```

Lack of covariance, however, can hinder reusability. Suppose, for instance, we wanted to write a method to Wash a stack of animals:

```csharp
public class ZooCleaner
{
    public static void Wash(Stack<Animal> animals) {...}
}
```

Calling `Wash` with a stack of bears would generate a compile-time error. One workaround is to redefine the `Wash` method with a constraint:

```csharp
class ZooCleaner
{
    public static void Wash<T>(Stack<T> animals) where T : Animal { ... }
}
```

We can now call `Wash` as follows:

```csharp
Stack<Bear> bears = new Stack<Bear>();
ZooCleaner.Wash(bears);
```

Another solution is to have `Stack<T>` implement an interface with a covariant type parameter, as we’ll see shortly.

#### Arrays

For historical reasons, array types support covariance. This means that `B[]` can be cast to A`[]` if `B` subclasses `A` (and both are reference types). For example:

```csharp
Bear[] bears = new Bear[3];
Animal[] animals = bears;   // OK
```

The downside of this reusability is that element assignments can fail at runtime:

```csharp
animals[0] = new Camel();   // Runtime error
```

#### Declaring a covariant type parameter

As of C# 4.0, type parameters on interfaces and delegates can be declared covariant by marking them with the out modifier. This modifier ensures that, unlike with arrays, covariant type parameters are fully type-safe.

We can illustrate this with our `Stack<T>` class by having it implement the following interface:

```csharp
public interface IPoppable<out T> { T Pop(); }
```

The `out` modifier on `T` indicates that `T` is used only in output positions (e.g., return types for methods). The out modifier flags the type parameter as *covariant* and allows us to do this:

```csharp
var bears = new Stack<Bear>();
bears.Push (new Bear());
// Bears implements IPoppable<Bear>. We can convert to IPoppable<Animal>:
IPoppable<Animal> animals = bears; // Legal
Animal a = animals.Pop();
```

The conversion from bears to animals is permitted by the compiler—by virtue of the type parameter being covariant. This is type-safe because the case the compiler is trying to avoid—pushing a `Camel` onto the stack—can’t occur as there’s no way to feed a `Camel` into an interface where `T` can appear only in output positions.

*Covariance (and contravariance) in interfaces is something that you typically consume: it’s less common that you need to write variant interfaces.*

We can leverage the ability to cast covariantly to solve the reusability problem described earlier:

```csharp
public class ZooCleaner
{
    public static void Wash(IPoppable<Animal> animals) { ... }
}
```

The compiler will generate an error if you use a covariant type parameter in an input position (e.g., a parameter to a method or a writable property).

*Covariance (and contravariance) works only for elements with reference conversions—not boxing conversions. (This applies both to type parameter variance and array variance.) So, if you wrote a method that accepted a parameter of type `IPoppable<object>`, you could call it with `IPoppable<string>`, but not `IPoppable<int>`.*

### Contravariance

**Contravariance**: Generalização de objetos. Ex.: `string -> object`.
```csharp
Func<string> x = ...;
Func<object> y = x;
```

We previously saw that, assuming that `A` allows an implicit reference conversion to `B`, a type `X` has a covariant type parameter if `X<A>` allows a reference conversion to `X<B>`. Contravariance is when you can convert in the reverse direction—from `X<B>` to `X<A>`. This is supported if the type parameter appears only in input positions, and is designated with the in modifier. Extending our previous example, if the `Stack<T>` class implements the following interface:

```csharp
public interface IPushable<in T>
{
    void Push(T obj);
}
```

we can legally do this:

```csharp
IPushable<Animal> animals = new Stack<Animal>();
IPushable<Bear> bears = animals; // Legal
bears.Push(new Bear());
```

No member in `IPushable` outputs a `T`, so we can’t get into trouble by casting `animals` to `bears` (there’s no way to `Pop`, for instance, through that interface).

*Our `Stack<T>` class can implement both `IPushable<T>` and `IPoppable<T>`—despite `T` having opposing variance annotations in the two interfaces! This works because you must exercise variance through the interface and not the class; therefore, you must commit to the lens of either `IPoppable` or `IPushable` before performing a variant conversion. This lens then restricts you to the operations that are legal under the appropriate variance rules.*

*This also illustrates why classes do not allow variant type parameters: concrete implementations typically require data to flow in both directions.*

To give another example, consider the following interface, defined as part of the .NET Framework:

```csharp
public interface IComparer<in T>
{
    // Returns a value indicating the relative ordering of a and b
    int Compare (T a, T b);
}
```

Because the interface has a contravariant `T`, we can use an `IComparer<object>` to compare two strings:

```csharp
var objectComparer = Comparer<object>.Default;
// objectComparer implements IComparer<object>
IComparer<string> stringComparer = objectComparer;
int result = stringComparer.Compare("Brett", "Jemaine");
```

Mirroring covariance, the compiler will report an error if you try to use a contravariant type parameter in an output position (e.g., as a return value, or in a readable property).
