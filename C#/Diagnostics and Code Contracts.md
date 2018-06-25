# Diagnostics and Code Contracts

Para atender a este requisito(reunir e registrar informações de diagnósticos do aplicativo), o .NET Framework fornece um conjunto de instalações para registrar informações de diagnóstico, monitorar o comportamento do aplicativo, detectar erros de tempo de execução e integrar com ferramentas de depuração, se disponível.

O .NET Framework também permite que você aplique contratos de código. Introduzido no Framework 4.0, os contratos de código permitem que os métodos interajam através de um conjunto de obrigações mútuas e fail early se essas obrigações forem violadas.

Os tipos nesse artigo são definidos nos seguintes namespaces `System.Diagnostics` e `System.Diagnostics.Contracts`.

## Conditional Compilation

Você pode compactar condicionalmente qualquer seção de código em C# com as _diretivas do pré-processador_.

As diretivas do pré-processador são instruções especiais para o compilador que começam com o símbolo `#`(e, ao contrário de outras construções do C#, devem aparecer em uma linha própria). Logicamente, eles executam antes da compilação principal acontecer(embora, na prática, o compilador os processe durante a fase de análise lexical). As diretivas do pré-processador para a compilação condicional são `#if`, `#else`, `#endif` e `#elif`.

A diretiva `#if` instrui o compilador a ignorar uma seção de código, a menos que um símbolo especificado tenha sido definido. Você pode definir um símbolo com a diretiva `#define` ou uma opção de compilação. Diretiva `#define` se aplica a um arquivo específico; Um parâmetro de compilação aplica-se a uma montagem completa(_assembly_):

```csharp
#define TESTMODE // #define directives must be at top of file
                 // Symbol names are uppercase by convention.
using System;

class Program
{
    static void Main()
    {
#if TESTMODE
        Console.WriteLine("in test mode!"); // OUTPUT: in test mode!
#endif
    }
}
```

Se excluíssemos a primeira linha, o programa compilaria com a instrução `Console.WriteLine` completamente eliminada do executável, como se fosse comentada.

A instrução `#else` é análoga à declaração `else` do C#, e `#elif` é equivalente a `#else` seguido de `#if`. Os operadores `||`, `&&`, e `!` podem ser usados para representar respectivamente as seguintes operações `or`, `and`, e `not`:

```csharp
#if TESTMODE && !PLAYMODE // if TESTMODE and not PLAYMODE
...
```

Para definir um conjunto de símbolos em uma assembly, especifique o parâmetro `/define` ao compilar:

```shell
csc Program.cs /define:TESTMODE,PLAYMODE
```

If you’ve defined a symbol at the assembly level and then want to “undefine” it for a particular file, you can do so with the `#undef` directive.

### Conditional Compilation Versus Static Variable Flags

O exemplo anterior poderia, em vez disso, ser implementado com um campo estático simples:

```csharp
static internal bool TestMode = true;

static void Main()
{
    if (TestMode)
        Console.WriteLine("in test mode!");
}
```

This has the advantage of allowing runtime configuration. So, why choose conditional compilation? The reason is that conditional compilation can take you places variable flags cannot, such as:

-   Conditionally including an attribute
-   Changing the declared type of variable
-   Switching between different namespaces or type aliases in a `using` directive - for example:

    ```csharp
    using TestType =
        #if V2
            MyCompany.Widgets.GadgetV2;
        #else
            MyCompany.Widgets.Gadget;
        #endif
    ```

You can even perform major refactoring under a conditional compilation directive, so you can instantly switch between old and new versions, and write libraries that can compile against multiple Framework versions, leveraging the latest Framework features where available.

Another advantage of conditional compilation is that debugging code can refer to types in assemblies that are not included in deployment.

### The Conditional Attribute

O _atributo condicional_ instrui o compilador a ignorar quaisquer chamadas para uma determinada classe ou método, se o símbolo especificado não tiver sido definido.

Para ver como isso é útil, suponha que você escreva um método para registrar informações de status da seguinte maneira:

```csharp
static void LogStatus(string msg)
{
    string logFilePath = ...
    System.IO.File.AppendAllText(logFilePath, msg + "\r\n");
}
```

Now imagine you wanted this to execute only if the `LOGGINGMODE` symbol is defined. The first solution is to wrap all calls to LogStatus around an `#if` directive:

```csharp
#if LOGGINGMODE
LogStatus("Message Headers: " + GetMsgHeaders());
#endif
```

This gives an ideal result, but it is tedious. The second solution is to put the `#if` directive inside the `LogStatus` method. This, however, is problematic should `LogStatus` be called as follows:

```csharp
LogStatus("Message Headers: " + GetComplexMessageHeaders());
```

`GetComplexMessageHeaders` would always get called—which might incur a performance hit.

We can combine the functionality of the first solution with the convenience of the second by attaching the _Conditional_ attribute(defined in `System.Diagnostics`) to the `LogStatus` method:

```csharp
[Conditional("LOGGINGMODE")]
static void LogStatus(string msg)
{
    ...
}
```

Isso instrui o compilador a tratar as chamadas para `LogStatus` como se estivessem envolvidas em uma diretiva `#if` `LOGGINGMODE`. Se o símbolo não estiver definido, todas as chamadas para `LogStatus` são eliminadas inteiramente na compilação, incluindo suas expressões de avaliação de argumento. (Portanto, quaisquer expressões secundárias serão ignoradas.) Isso funciona mesmo se `LogStatus` e o chamador estiverem em assemblies diferentes.

_Another benefit of [Conditional] is that the conditionality check is performed when the caller is compiled, rather than when the called method is compiled. This is beneficial because it allows you to write a library containing methods such as Log Status—and build just one version of that library._

The Conditional attribute is ignored at runtime—it’s purely an instruction to the compiler.

#### Alternatives to the Conditional attribute

The Conditional attribute is useless if you need to dynamically enable or disable functionality at runtime: instead, you must use a variable-based approach. This leaves the question of how to elegantly circumvent the evaluation of arguments when calling conditional logging methods. A functional approach solves this:

```csharp
using System;
using System.Linq;

class Program
{
    public static bool EnableLogging;

    static void LogStatus(Func<string> message)
    {
        string logFilePath = ...
        if (EnableLogging)
            System.IO.File.AppendAllText(logFilePath, message() + "\r\n");
    }
}
```

A lambda expression lets you call this method without syntax bloat:

```csharp
LogStatus(() => "Message Headers: " + GetComplexMessageHeaders());
```

Se `EnableLogging` for falso , `GetComplexMessageHeaders` nunca será avaliado.

## Debug and Trace Classes

`Debug` and `Trace` are static classes that provide basic logging and assertion capabilities. The two classes are very similar; the main differentiator is their intended use. The `Debug` class is intended for debug builds; the `Trace` class is intended for both debug and release builds. To this effect:

-   All methods of the `Debug` class are defined with [Conditional("DEBUG")].
-   All methods of the `Trace` class are defined with [Conditional("TRACE")].

Isso significa que todas as chamadas que você faz para `Debug` ou `Trace` são eliminadas pelo compilador, a menos que você defina símbolos `DEBUG` ou `TRACE`. Por padrão, o Visual Studio define os símbolos `DEBUG` e `TRACE` na configuração de depuração(debug) de um projeto - e apenas o símbolo `TRACE` na configuração de lançamento(release).

Both the `Debug` and `Trace` classes provide `Write`, `WriteLine`, and `WriteIf` methods. By default, these send messages to the debugger’s output window:

```csharp
Debug.Write("Data");
Debug.WriteLine(23 * 34);

int x = 5, y = 3;

Debug.WriteIf(x > y, "x is greater than y");
```

_The `Trace` class also provides the methods `TraceInformation`, `TraceWarning`, and `TraceError`. The difference in behavior between these and the `Write` methods depends on the active `TraceListeners`._

### Fail and Assert

As classes `Debug` e `Trace` fornecem os métodos `Fail` e `Assert`.

The `Debug` and `Trace` classes both provide `Fail` and `Assert` methods. `Fail` sends the message to each `TraceListener` in the `Debug` or `Trace` class’s Listeners collection, which by default writes the message to the debug output as well as displaying it in a dialog:

```csharp
Debug.Fail("File data.txt does not exist!");
```

`Assert` simplesmente chama `Fail` se o argumento `bool` é falso - isto é chamado de fazer uma afirmação e indica um erro no código, se violado. Especificar uma mensagem de falha é opcional:

```csharp
Debug.Assert(File.Exists("data.txt"), "File data.txt does not exist!");

var result = ...

Debug.Assert(result != null);
```

The `Write`, `Fail`, and `Assert` methods are also overloaded to accept a string category in addition to the message, which can be useful in processing the output.

An alternative to assertion is to throw an exception if the opposite condition is true. This is a common practice when validating method arguments:

```csharp
public void ShowMessage(string message)
{
    if (message == null) throw new ArgumentNullException("message");
    ...
}
```

Such “assertions” are compiled unconditionally and are less flexible in that you can’t control the outcome of a failed assertion via `TraceListeners`. And technically, they’re not assertions. An assertion is something that, if violated, indicates a bug in the current method’s code. Throwing an exception based on argument validation indicates a bug in the caller’s code.

### TraceListener

The `Debug` and `Trace` classes each have a `Listeners` property, comprising a static collection of `TraceListener` instances. These are responsible for processing the content emitted by the `Write`, `Fail`, and `Trace` methods.

Por padrão, a coleção `Listeners` de cada um inclui um único listener(ouvinte) (`DefaultTraceListener`). O listener padrão possui dois recursos principais:

-   When connected to a debugger such as Visual Studio, messages are written to the debug output window; otherwise, message content is ignored.
-   When the `Fail` method is called(or an assertion fails), a dialog appears asking the user whether to continue, abort, or retry(attach/debug)—regardless of whether a debugger is attached.

You can change this behavior by(optionally) removing the default listener, and then adding one or more of your own. You can write trace listeners from scratch(by subclassing `TraceListener`) or use one of the predefined types:

-   `TextWriterTraceListener` writes to a Stream or TextWriter or appends to a file.
-   `EventLogTraceListener` writes to the Windows event log.
-   `EventProviderTraceListener` writes to the Event Tracing for Windows(ETW) subsystem in Windows Vista and later.
-   `WebPageTraceListener` writes to an ASP.NET web page.

`TextWriterTraceListener` is further subclassed to `ConsoleTraceListener`, `DelimitedListTraceListener`, `XmlWriterTraceListener`, and `EventSchemaTraceListener`.

_Nenhum desses ouvintes exibe uma caixa de diálogo quando o `Fail` é chamado - apenas o `DefaultTraceListener` possui esse comportamento_.

The following example clears Trace’s default listener, then adds three listeners—one that appends to a file, one that writes to the console, and one that writes to the Windows event log:

```csharp
// Clear the default listener:
Trace.Listeners.Clear();

// Add a writer that appends to the trace.txt file:
Trace.Listeners.Add(new TextWriterTraceListener("trace.txt"));

// Obtain the Console's output stream, then add that as a listener:
System.IO.TextWriter tw = Console.Out;
Trace.Listeners.Add(new TextWriterTraceListener(tw));

// Set up a Windows Event log source and then create/add listener.
// CreateEventSource requires administrative elevation, so this would
// typically be done in application setup.
if (!EventLog.SourceExists("DemoApp"))
    EventLog.CreateEventSource("DemoApp", "Application");

Trace.Listeners.Add(new EventLogTraceListener("DemoApp"));
```

In the case of the Windows event log, messages that you write with the `Write`, `Fail`, or `Assert` method always display as “Information” messages in the Windows event viewer. Messages that you write via the `TraceWarning` and `TraceError` methods, however, show up as warnings or errors.

`TraceListener` also has a `Filter` of type `TraceFilter` that you can set to control whether a message gets written to that listener. To do this, you either instantiate one of the predefined subclasses(`EventTypeFilter` or `SourceFilter`), or subclass `TraceFilter` and override the `ShouldTrace` method. You could use this to filter by category, for instance.

`TraceListener` also defines `IndentLevel` and `IndentSize` properties for controlling indentation, and the `TraceOutputOptions` property for writing extra data:

```csharp
TextWriterTraceListener tl = new TextWriterTraceListener(Console.Out);
tl.TraceOutputOptions = TraceOptions.DateTime | TraceOptions.Callstack;
```

`TraceOutputOptions` are applied when using the Trace methods:

```csharp
Trace.TraceWarning("Orange alert");

/*
DiagTest.vshost.exe Warning: 0 : Orange alert
    DateTime=2007-03-08T05:57:13.6250000Z
    Callstack= at System.Environment.GetStackTrace(Exception e, Boolean needFileInfo)
    at System.Environment.get_StackTrace() at ...
*/
```

### Flushing and Closing Listeners

Some listeners, such as `TextWriterTraceListener`, ultimately write to a stream that is subject to caching. This has two implications:

-   A message may not appear in the output stream or file immediately.
-   You must close—or at least flush—the listener before your application ends; otherwise, you lose what’s in the cache(up to 4 KB, by default, if you’re writing to a file).

The `Trace` and `Debug` classes provide static `Close` and `Flush` methods that call `Close` or `Flush` on all listeners(which in turn calls `Close` or `Flush` on any underlying writers and streams). `Close` implicitly calls `Flush`, closes file handles, and prevents further data from being written.

As a general rule, call `Close` before an application ends and call `Flush` anytime you want to ensure that current message data is written. This applies if you’re using stream- or file-based listeners.

`Trace` and `Debug` also provide an `AutoFlush` property, which, if true, forces a `Flush` after every message.

_It’s a good policy to set `AutoFlush` to true on `Debug` and `Trace` if you’re using any file- or stream-based listeners. Otherwise, if an unhandled exception or critical error occurs, the last 4 KB of diagnostic information may be lost._

## Processes and Process Threads

We described in the last section how to launch a new process with `Process.Start`. The Process class also allows you to query and interact with other processes running on the same, or another, computer. The Process class is part of .NET Standard 2.0, although its features are restricted for the UWP platform.

### Examining Running Processes

The `Process.GetProcessXXX` methods retrieve a specific process by name or process ID, or all processes running on the current or nominated computer. This includes both managed and unmanaged processes. Each `Process` instance has a wealth of properties mapping statistics such as name, ID, priority, memory and processor utilization, window handles, and so on. The following sample enumerates all the running processes on the current computer:

```csharp
foreach (Process p in Process.GetProcesses())
using (p)
{
    Console.WriteLine(p.ProcessName);
    Console.WriteLine("PID: " + p.Id);
    Console.WriteLine("Memory: " + p.WorkingSet64);
    Console.WriteLine("Threads: " + p.Threads.Count);
}
```

`Process.GetCurrentProcess` returns the current process. If you’ve created additional application domains, all will share the same process.

You can terminate a process by calling its `Kill` method.

### Examining Threads in a Process

You can also enumerate over the threads of other processes, with the `Process` .`Threads` property. The objects that you get, however, are not `System.Thread` ing.Thread objects, but rather `ProcessThread` objects, and are intended for administrative rather than synchronization tasks. A `ProcessThread` object provides diagnostic information about the underlying thread and allows you to control some aspects of it such as its priority and processor affinity:

```csharp
public void EnumerateThreads(Process p)
{
    foreach (ProcessThread pt in p.Threads)
    {
        Console.WriteLine(pt.Id);
        Console.WriteLine("State: " + pt.ThreadState);
        Console.WriteLine("Priority: " + pt.PriorityLevel);
        Console.WriteLine("Started: " + pt.StartTime);
        Console.WriteLine("CPU time: " + pt.TotalProcessorTime);
    }
}
```

## StackTrace and StackFrame

The `StackTrace` and `StackFrame` classes provide a read-only view of an execution call stack and are part of the standard desktop .NET Framework. You can obtain stack traces for the current thread, another thread in the same process, or an `Exception` object. Such information is useful mostly for diagnostic purposes, though it can also be used in programming(hacks). `StackTrace` represents a complete call stack; `StackFrame` represents a single method call within that stack.

If you instantiate a `StackTrace` object with no arguments—or with a bool argument —you get a snapshot of the current thread’s call stack. The bool argument, if `true`, instructs `StackTrace` to read the assembly .pdb(project debug) files if they are present, giving you access to filename, line number, and column offset data. Project debug files are generated when you compile with the `/debug` switch. (Visual Studio compiles with this switch unless you request otherwise via Advanced Build Settings.)

Once you’ve obtained a `StackTrace`, you can examine a particular frame by calling `GetFrame—or` obtain the whole lot with `GetFrames`:

```csharp
static void Main() { A(); }
static void A() { B(); }
static void B() { C(); }
static void C()
{
    StackTrace s = new StackTrace(true);
    Console.WriteLine("Total frames: " + s.FrameCount);
    Console.WriteLine("Current method: " + s.GetFrame(0).GetMethod().Name);
    Console.WriteLine("Calling method: " + s.GetFrame(1).GetMethod().Name);
    Console.WriteLine("Entry method: " + s.GetFrame(s.FrameCount-1).GetMethod().Name);
    Console.WriteLine("Call Stack:");

    foreach(StackFrame f in s.GetFrames())
        Console.WriteLine(
            " File: " + f.GetFileName() +
            " Line: " + f.GetFileLineNumber() +
            " Col: " + f.GetFileColumnNumber() +
            " Offset: " + f.GetILOffset() +
            " Method: " + f.GetMethod().Name);
}

/*
Total frames: 4
Current method: C
Calling method: B
Entry method: Main
Call stack:
    File: C:\Test\Program.cs Line: 15 Col: 4 Offset: 7 Method: C
    File: C:\Test\Program.cs Line: 12 Col: 22 Offset: 6 Method: B
    File: C:\Test\Program.cs Line: 11 Col: 22 Offset: 6 Method: A
    File: C:\Test\Program.cs Line: 10 Col: 25 Offset: 6 Method: Main
*/
```

_The IL offset indicates the offset of the instruction that will execute next—not the instruction that’s currently executing. Peculiarly, though, the line and column number(if a .pdb file is present) usually indicate the actual execution point._

_This happens because the CLR does its best to infer the actual execution point when calculating the line and column from the IL offset. The compiler emits IL in such a way as to make this possible—including inserting nop(no-operation) instructions into the IL stream._

_Compiling with optimizations enabled, however, disables the insertion of nop instructions and so the stack trace may show the line and column number of the next statement to execute. Obtaining a useful stack trace is further hampered by the fact that optimization can pull other tricks, including collapsing entire methods._

A shortcut to obtaining the essential information for an entire `StackTrace` is to call `ToString` on it. Here’s what the result looks like:

```
at DebugTest.Program.C() in C:\Test\Program.cs:line 16
at DebugTest.Program.B() in C:\Test\Program.cs:line 12
at DebugTest.Program.A() in C:\Test\Program.cs:line 11
at DebugTest.Program.Main() in C:\Test\Program.cs:line 10
```

To obtain the stack trace for another thread, pass the other `Thread` into `StackTrace’s` constructor. This can be a useful strategy for profiling a program, although you must suspend the thread while obtaining the stack trace. This is actually quite tricky to do without risking a deadlock.

You can also obtain the stack trace for an `Exception` object(showing what led up to the exception being thrown) by passing the `Exception` into `StackTrace’s` constructor.

_`Exception` already has a `StackTrace` property; however, this property returns a simple string—not a `StackTrace` object. A `StackTrace` object is far more useful in logging exceptions that occur after deployment—where no .pdb files are available —because you can log the IL offset in lieu of line and column numbers. With an IL offset and ildasm, you can pinpoint where within a method an error occurred._

## Code Contracts Overview

Abordagem para os princípios de falha e erros, resultados incorretos, efeitos colaterais indesejados:

-   Ao chamar o método `Assert`, `Debug` ou `Trace`.
-   Ao lançar exceções(como `ArgumentNullException`)

O Framework 4.0 introduziu um novo recurso chamado contratos de código , que substitui essas duas abordagens por um sistema unificado. Esse sistema permite que você faça asserções não apenas simples, mas também asserções baseadas em contratos mais poderosas.

Os contratos de código derivam do princípio de "Design by Contract" da linguagem de programação de _Eiffel_, onde as funções interagem uns com os outros através de um sistema de obrigações e benefícios mútuos. Essencialmente, uma função especifica condições prévias que devem ser atendidas pelo cliente(chamador) e, em contrapartida, garante condições pós que o cliente pode depender quando a função retornar.

The types for code contracts live in the `System.Diagnostics.Contracts` namespace.

### Why Use Code Contracts?

Para ilustrar, escreveremos um método que adicione um item a uma lista somente se ainda não estiver presente - com duas pré-condições e uma condição posterior:

```csharp
public static bool AddIfNotPresent<T>(IList<T> list, T item)
{
    Contract.Requires(list != null);        // Precondition
    Contract.Requires(!list.IsReadOnly);    // Precondition
    Contract.Ensures(list.Contains(item));  // Postcondition

    if (list.Contains(item))
        return false;

    list.Add(item);

    return true;
}
```

As condições prévias são definidas por `Contract.Requires` e são verificadas quando o método é iniciado. A pós-condição é definido por `Contract.Ensures` e é verificado não onde ele aparece no código, mas when the method exits.

_Condições prévias e pós-condições devem aparecer no início do método. Isto é propício ao bom design: se você não conseguir Cumprir o contrato em subsequentemente escrever o método, o erro será detectado_.

`AddIfNotPresent` anuncia aos consumidores:

-   Você deve me chamar com uma lista não nula para gravação.
-   Quando eu retornar, essa lista conterá o item que você especificou.

Esses fatos podem ser emitidos no arquivo de documentação XML da assembly(você pode fazer isso no Visual Studio, indo para a guia Contratos do código da janela Propriedades do Projeto, permitindo a construção de uma assembly de referência de contratos e verificando "Emitir Contratos em arquivo de documento XML"). Ferramentas como _SandCastle_ podem então incorporar detalhes do contrato em arquivos de documentação.

Os contratos também permitem que seu programa seja analisado quanto à correção por ferramentas estáticas de validação de contratos. Se você tentar chamar `AddIfNotPresent` com uma lista cujo valor pode ser nulo, por exemplo, uma ferramenta de validação estática pode avisá-lo antes mesmo de executar o programa.

Os contratos também suportam invariantes de objetos - que reduzem ainda mais a codificação repetitiva e garantem uma aplicação mais confiável.

A desvantagem de usar contratos de código é que a implementação .NET depende de um reescritor binário - uma ferramenta que altera o assembly após a compilação. Isso retarda o processo de compilação, bem como a complicação de serviços que dependem de chamar o compilador C#(seja explicitamente ou através da classe `CSharpCodeProvider`).

### Contract Principles

-   As condições prévias são verificadas quando uma função é iniciada.
-   Pós-condições são verificadas antes de uma função terminar.
-   Asserções são verificadas onde quer que aparecem no código.
-   Os invariantes de objetos são verificados após cada função pública em uma classe.

Os contratos podem aparecer não apenas em métodos, mas também em outras funções, como construtores, propriedades, indexadores e operadores.

#### Compilation

Almost all methods in the Contract class are defined with the [Conditional("CONTRACTS_FULL")] attribute. This means that unless you define the CONTRACTS_FULL symbol, (most) contract code is stripped out. Visual Studio defines the CONTRACTS_FULL symbol automatically if you enable contract checking in the Code Contracts tab of the Project Properties page.

#### The binary rewriter

After compiling code that contains contracts, you must call the binary rewriter tool, ccrewrite.exe(Visual Studio does this automatically if contract checking is enabled). The binary rewriter moves postconditions(and object invariants) into the right place, calls any conditions and object invariants in overridden methods, and replaces calls to Contract with calls to a contracts runtime class.

#### Asserting versus throwing on failure

The binary rewriter also lets you choose between displaying a dialog and throwing a ContractException upon contract failure. The former is typically used for debug builds; the latter for release builds. To enable the latter, specify `/throwonfailure` when calling the binary rewriter, or uncheck the “Assert on contract failure” checkbox in Visual Studio’s Code Contracts tab in Project Properties.

### Preconditions

You can define code contract preconditions by calling `Contract.Requires`, `Contract.Requires<TException>` or `Contract.EndContractBlock`.

#### Contract.Requires

Calling `Contract.Requires` at the start of a function enforces a precondition:

```csharp
static string ToProperCase(string s)
{
    Contract.Requires(!string.IsNullOrEmpty(s));
    ...
}
```

Isto é como fazer uma afirmação, exceto que o pré-requisito forma um descoberta Fato sobre sua função que pode ser extraída do código compilado e consumida por documentação ou ferramentas de verificação estática(para que eles possam avisá-lo se eles vejam algum código em outro lugar do seu programa que tente chamar o `ToProperCase` com uma string vazia ou vazia).

Um outro benefício de pré-condições é que subclasses que substituem métodos virtuais Com pré-condições não pode impedir que as condições prévias do método de classe base sejam verificadas. E as pré-condições definidas nos membros da interface serão implicitamente contidas nas implementações concretas.

Você pode chamar `Contract.Requires` quantas vezes for necessário no início do método para impor condições diferentes.

#### `Contract.Requires<TException>`

The introduction of code contracts challenges the following deeply entrenched pattern established in the .NET Framework from version 1.0:

```csharp
static void SetProgress(string message, int percent) // Classic approach
{
    if (message == null)
        throw new ArgumentNullException("message");

    if (percent < 0 || percent > 100)
        throw new ArgumentOutOfRangeException("percent");
    ...
}

static void SetProgress(string message, int percent) // Modern approach
{
    Contract.Requires(message != null);
    Contract.Requires(percent >= 0 && percent <= 100);
    ...
}
```

Se você tiver uma montagem grande que imponha a verificação clássica de argumentos, a escrita de novos métodos com pré-condições criará uma biblioteca inconsistente: alguns métodos lançarão exceções de argumento, enquanto outros lançarão um `ContractException`. Uma solução é atualizar todos os métodos existentes para usar contratos, mas esta tem duas problemas:

-   É demorado.
-   Os chamadores podem vir a depender de um tipo de exceção, como `ArgumentNullException` sendo acionada. (Isso quase certamente indica um design ruim, mas pode ser a realidade, no entanto.)

A solução é chamar a versão genérica de `Contract.Requires`. Isso permite que você especifique um tipo de exceção a ser lançado após a falha:

```csharp
Contract.Requires<ArgumentNullException>(message != null, "message");
Contract.Requires<ArgumentOutOfRangeException>(percent >= 0 && percent <= 100, "percent");
```

O segundo argumento é passado para o construtor da classe de exceção.

Isso resulta no mesmo comportamento que na verificação de argumentos antigos, ao mesmo tempo que oferece os benefícios dos contratos(concisão, suporte para interfaces, documentação implícita, verificação estática e personalização de tempo de execução).

Chamadas para o contrato genérico `Contract.Requires<TException>`, permaneça no local, enquanto todas as outras verificações são removidas: isso resulta em uma montagem que se comporta como no passado.

#### `Contract.EndContractBlock`

O método `Contract.EndContractBlock` permite que você obtenha o benefício de contratos de código com o código tradicional de verificação de argumentos, evitando a necessidade de refatorar o código escrito - antes do Framework 4.0. Tudo o que você faz é chamar este método depois de realizar verificações de argumento manuais:

```csharp
static void Foo(string name)
{
    if (name == null)
        throw new ArgumentNullException("name");

    Contract.EndContractBlock();
    ...
}
```

The binary rewriter then converts this code into something equivalent to:

```csharp
static void Foo(string name)
{
    Contract.Requires<ArgumentNullException>(name != null, "name");
    ...
}
```

The code that precedes `EndContractBlock` must comprise simple statements of the form: `if <condition> throw <expression>`;

#### Preconditions and Overridden Methods

Ao substituir um método virtual, você não pode adicionar pré-condições, pois assim mudaria o contrato(tornando-o mais restritivo) - rompendo os princípios do polimorfismo.

O reescritor binário garante que as condições prévias de um método base sejam sempre aplicadas em subclasses - independentemente de o método substituído chamar o método base.

### Postconditions

#### `Contract.Ensures`

`Contract.Ensures` aplicam uma pós-condição: algo que deve ser verdadeiro quando o método sai. Vimos no exemplo anteriormente:

```csharp
static bool AddIfNotPresent<T>(IList<T> list, T item)
{
    Contract.Requires(list != null);        // Precondition
    Contract.Ensures(list.Contains(item));  // Postcondition

    if (list.Contains(item))
        return false;

    list.Add(item);

    return true;
}
```

Ao contrário das pré-condições, que detectam o uso indevido pelo chamador, as pós-condições detectam um erro na própria função(em vez de asserções). Portanto, as condições pós-publicação podem acessar o estado privado.

#### `Contract.EnsuresOnThrow<TException>`

Ocasionalmente, é útil garantir que uma determinada condição seja verdadeira se um tipo particular de exceção foi executado. O método `EnsuresOnThrow` faz exatamente isso:

```csharp
Contract.EnsuresOnThrow<WebException>(this.ErrorMessage != null);
```

#### `Contract.Result<T>` and `Contract.ValueAtReturn<T>`

Como as condições pós-avaliação não são avaliadas até que uma função termina, é razoável querer acessar o valor de retorno de um método. O método `Contract.Result<T>` faz exatamente isso:

```csharp
Random _random = new Random();

int GetOddRandomNumber()
{
    Contract.Ensures(Contract.Result<int>() % 2 == 1);

    return _random.Next(100) * 2 + 1;
}
```

O método `Contract.ValueAtReturn<T>` cumpre a mesma função, mas para os parâmetros de `ref` e `out`.

#### `Contract.OldValue<T>`

`Contract.OldValue<T>` retorna o valor original de um parâmetro de método. Isso é útil com as pós-condições, porque estas são verificadas no final de uma função. Portanto, quaisquer expressões nas pós-condições que incorporam parâmetros lerão os valores dos parâmetros modificados.

Por exemplo, a condição posterior no seguinte método sempre falhará:

```csharp
static string Middle(string s)
{
    Contract.Requires(s != null && s.Length >= 2);
    Contract.Ensures(Contract.Result<string>().Length < s.Length);

    s = s.Substring(1, s.Length - 2);

    return s.Trim();
}
```

Veja como podemos corrigi-lo:

```csharp
static string Middle(string s)
{
    Contract.Requires(s != null && s.Length >= 2);
    Contract.Ensures(Contract.Result<string>().Length < Contract.OldValue(s).Length);

    s = s.Substring(1, s.Length - 2);

    return s.Trim();
}
```

#### Postconditions and Overridden Methods

Um método substituído não pode contornar as condições pós definidas por sua base, mas pode adicionar novas. The binary rewriter garante que postconditions um método base sempre são verificados, mesmo se o método substituído não chama os implementação base.

### Assertions and Object Invariants

Além de condições prévias e pós-condições, o código contratos API permite que você faça asserções e defina invariantes de objetos.

#### Assertions

##### `Contract.Assert`

Você pode fazer asserções em qualquer lugar em uma função chamando `Contract.Assert`. Você pode, opcionalmente, especificar uma mensagem de erro se a afirmação falhar:

```csharp
...
int x = 3;
...
Contract.Assert(x == 3); // Fail unless x is 3
Contract.Assert(x == 3, "x must be 3");
...
```

O reescritor binário não move asserções ao redor.

##### `Contract.Assume`

`Contract.Assume` se comporta exatamente como `Contract.Assert` em tempo de execução, mas tem implicações ligeiramente diferentes para ferramentas de verificação estática. Essencialmente, as ferramentas de verificação estática não desafiarão uma hipótese, enquanto elas podem desafiar uma afirmação. Isso é útil em que sempre haverá coisas que um verificador estático é incapaz de provar, e isso pode levar a isso a "crying wolf" em uma afirmação válida. Alterar a asserção para uma suposição mantém o verificador estático calado.

#### Object Invariants

Para uma classe, você pode especificar um ou mais métodos invariantes do objeto . Esses métodos são executados automaticamente após cada função pública na classe e permitem que você afirme que o objeto está em um estado internamente consistente.

_O suporte para múltiplos métodos invariantes de objetos foi incluído para tornar os invariantes de objetos funcionando bem com classes parciais._

Para definir um método invariante de objeto, escreva um método de vazio sem parâmetros e anote-o com o atributo `[ContractInvariantMethod]`. Nesse método, chame Contract.Invariant para impor cada condição que deve ser verdadeira:

```csharp
class Test
{
    int _x, _y;

    [ContractInvariantMethod]
    void ObjectInvariant()
    {
        Contract.Invariant(_x >= 0);
        Contract.Invariant(_y >= _x);
    }

    public int X { get { return _x; } set { _x = value; } }
    public void Test1() { _x = -3; }
    void Test2() { _x = -3; }
}
```

The binary rewriter translates the `X` property, `Test1` method and `Test2` method to something equivalent to this:

```csharp
public void X { get { return _x; } set { _x = value; ObjectInvariant(); } }
public void Test1() { _x = -3; ObjectInvariant(); }
void Test2() { _x = -3; } // No change because it's private
```

_Os objetos invariantes não impedem que um objeto entre em um estado inválido: eles apenas detectam quando essa condição ocorreu._

`Contract.Invariant` é bastante semelhante como `Contract.Assert`, exceto que ele pode aparecer apenas em um método marcado com o atributo `[ContractInvariantMethod]`. E, inversamente, um método invariante de contrato só pode conter chamadas para `Contract.Invariant`.
