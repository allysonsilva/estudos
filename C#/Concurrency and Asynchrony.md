# Concurrency and Asynchrony

## Threading

> Uma thread é um caminho de execução que pode prosseguir independentemente dos outros.

Cada thread é executado dentro de um processo do sistema operacional, que fornece um ambiente isolado no qual um programa é executado. Com um programa _single-threaded_, apenas uma thread é executado em ambiente isolado do processo, e assim essa thread tem acesso exclusivo a ele. Com um programa _multithreaded_, várias threads são executados em um único processo, compartilhando o mesmo ambiente de execução(memória, em particular). Isso, em parte, é por que _multithreading_ é útil: Uma thread pode buscar dados em segundo plano, por exemplo, enquanto outra thread exibe os dados à medida que ele chega. Este dado é referido como estado compartilhado.

### Creating a Thread

Um programa client(Console, WPF, Windows Store ou Windows Forms) começa em uma single thread criada automaticamente pelo sistema operacional(the “main” thread).

Você pode criar e iniciar uma nova thread instanciando um objeto `Thread` e chamando seu Método `start()`. O construtor mais simples para o `Thread` leva um delegate `ThreadStart`: um Método sem parâmetros que indica onde a execução deve começar. Por exemplo:

```csharp
// NB: All samples in this chapter assume the following namespace imports:
using System;
using System.Threading;

class ThreadTest
{
    static void Main()
    {
        Thread t = new Thread(WriteY); // Kick off a new thread
        t.Start();                      // running WriteY()

        // Simultaneously, do something on the main thread.
        for (int i = 0; i < 1000; i++)
            Console.Write("x");
    }

    static void WriteY()
    {
        for (int i = 0; i < 1000; i++)
            Console.Write("y");
    }
}

// Typical Output:
// xxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
// xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyy
// yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxx
// xxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
// yyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
// ...
```

The main thread creates a new thread t on which it runs a method that repeatedly prints the character y. Simultaneously, the main thread repeatedly prints the character `x`, as shown in Figure. On a single-core computer, the operating system must allocate “slices” of time to each thread(typically 20 ms in Windows) to simulate concurrency, resulting in repeated blocks of `x` and `y`. On a multicore or multiprocessor machine, the two threads can genuinely execute in parallel(subject to competition by other active processes on the computer), although you still get repeated blocks of `x` and `y` in this example because of subtleties in the mechanism by which `Console` handles concurrent requests.

![Starting a new thread](./Images/Starting%20a%20new%20thread.png)

Once started, a thread’s `IsAlive` property returns true, until the point where the thread ends. A thread ends when the delegate passed to the Thread’s constructor finishes executing. Once ended, a thread cannot restart.

Each thread has a `Name` property that you can set for the benefit of debugging. This is particularly useful in Visual Studio, since the thread’s name is displayed in the Threads Window and Debug Location toolbar. You can set a thread’s name just once; attempts to change it later will throw an exception.

A propriedade estática `Thread.CurrentThread` fornece a thread atualmente sendo executada:

```csharp
Console.WriteLine(Thread.CurrentThread.Name);
```

### Join and Sleep

Você pode aguardar o encerramento de outra thread ao chamar seu método `Join`:

```csharp
static void Main()
{
    Thread t = new Thread(Go);

    t.Start();
    t.Join();

    Console.WriteLine("Thread t has ended!");
}

static void Go()
{
    for (int i = 0; i < 1000; i++)
        Console.Write("y");
}
```

This prints “`y`” 1,000 times, followed by “Thread t has ended!” immediately afterward. You can include a timeout when calling `Join`, either in milliseconds or as a `TimeSpan`. It then returns `true` if the thread ended or `false` if it timed `out`.

`Thread.Sleep` faz uma pausa na Thread atual por um período especificado:

```csharp
Thread.Sleep(TimeSpan.FromHours(1));    // Sleep for 1 hour
Thread.Sleep(500);                      // Sleep for 500 milliseconds
```

`Thread.Sleep(0)` relinquishes the thread’s current time slice immediately, voluntarily handing over the CPU to other threads. `Thread.Yield()` does the same thing —except that it relinquishes only to threads running on the same processor.

_`Sleep(0)` or `Yield` is occasionally useful in production code for advanced performance tweaks. It’s also an excellent diagnostic tool for helping to uncover thread safety issues: if inserting `Thread.Yield()` anywhere in your code breaks the program, you almost certainly have a bug._

While waiting on a `Sleep` or `Join`, a thread is blocked.

### Blocking

A thread is deemed _blocked_ when its execution is paused for some reason, such as when `Sleeping` or waiting for another to end via `Join`. A blocked thread immediately yields its processor time slice, and from then on consumes no processor time until its blocking condition is satisfied. You can test for a thread being blocked via its `ThreadState` property:

```csharp
bool blocked = (someThread.ThreadState & ThreadState.WaitSleepJoin) != 0;
```

---

`ThreadState` is a flags enum, combining three “layers” of data in a bitwise fashion. Most values, however, are redundant, unused, or deprecated. The following extension method strips a ThreadState to one of four useful values: Unstarted, Run ning, WaitSleepJoin, and Stopped:

ThreadState is a flags enum, combining three “layers” of data
in a bitwise fashion. Most values, however, are redundant, unused, or deprecated. The following extension method strips a `ThreadState` to one of four useful values: `Unstarted`, `Running`, `WaitSleepJoin`, and `Stopped`:

```csharp
public static ThreadState Simplify(this ThreadState ts)
{
    return ts & (ThreadState.Unstarted | ThreadState.WaitSleepJoin | ThreadState.Stopped);
}
```

The `ThreadState` property is useful for diagnostic purposes, but unsuitable for synchronization, because a thread’s state may change in between testing `ThreadState` and acting on that information.

---

_When a thread blocks or unblocks, the operating system performs a context switch. This incurs a small overhead, typically one or two microseconds._

#### I/O-bound versus compute-bound

An operation that spends most of its time _waiting_ for something to happen is called I/O-bound—an example is downloading a web page or calling `Console.ReadLine`. (I/O-bound operations typically involve input or output, but this is not a hard requirement: `Thread.Sleep` is also deemed I/O-bound.) In contrast, an operation that spends most of its time performing CPU intensive work is called computebound.

### Local Versus Shared State

The CLR assigns each thread its own memory stack so that local variables are kept separate. In the next example, we define a method with a local variable, then call the method simultaneously on the main thread and a newly created thread:

```csharp
static void Main()
{
    new Thread(Go).Start();     // Call Go() on a new thread
    Go();                       // Call Go() on the main thread
}

static void Go()
{
    // Declare and use a local variable - 'cycles'
    for (int cycles = 0; cycles < 5; cycles++)
        Console.Write('?');
}
```

A separate copy of the `cycles` variable is created on each thread’s memory stack, and so the output is, predictably, ten question marks.

Threads share data if they have a common reference to the same object instance:

```csharp
class ThreadTest
{
    bool _done;

    static void Main()
    {
        ThreadTest tt = new ThreadTest(); // Create a common instance
        new Thread(tt.Go).Start();
        tt.Go();
    }

    void Go() // Note that this is an instance method
    {
        if (!_done) {
            _done = true;
            Console.WriteLine("Done");
        }
    }
}
```

Because both threads call `Go()` on the same `ThreadTest` instance, they share the `_done` field. This results in “Done” being printed once instead of twice.

Local variables captured by a lambda expression or anonymous delegate are converted by the compiler into fields, and so can also be shared:

```csharp
class ThreadTest
{
    static void Main()
    {
        bool done = false;

        ThreadStart action = () =>
        {
            if (!done) {
                done = true;
                Console.WriteLine("Done");
            }
        };

        new Thread(action).Start();
        action();
    }
}
```

Campos estáticos oferecem outra maneira de compartilhar dados entre threads:

```csharp
class ThreadTest
{
    static bool _done; // Static fields are shared between all threads
                       // in the same application domain.
    static void Main()
    {
        new Thread(Go).Start();
        Go();
    }

    static void Go()
    {
        if (!_done) {
            _done = true;
            Console.WriteLine("Done");
        }
    }
}
```

All three examples illustrate another key concept: that of thread safety(or rather, lack of it!). The output is actually indeterminate: it’s possible(though unlikely) that “Done” could be printed twice. If, however, we swap the order of statements in the `Go` method, the odds of “Done” being printed twice go up dramatically:

```csharp
static void Go()
{
    if (!_done) {
        Console.WriteLine("Done");
        _done = true;
    }
}
```

The problem is that one thread can be evaluating the if statement right as the other thread is executing the `WriteLine` statement—before it’s had a chance to set `done` to `true`.

_Our example illustrates one of many ways that shared writable state can introduce the kind of intermittent errors for which multithreading is notorious. We’ll see next how to fix our program with locking; however it’s better to avoid shared state altogether where possible. We’ll see later how asynchronous programming patterns help with this._

### Locking and Thread Safety

We can fix the previous example by obtaining an exclusive lock while reading and writing to the shared field. C# provides the lock statement for just this purpose:

```csharp
class ThreadSafe
{
    static bool _done;
    static readonly object _locker = new object();

    static void Main()
    {
        new Thread(Go).Start();
        Go();
    }

    static void Go()
    {
        lock(_locker)
        {
            if (!_done) {
                Console.WriteLine("Done");
                _done = true;
            }
        }
    }
}
```

When two threads simultaneously contend a lock(which can be upon any reference-type object, in this case, `_locker`), one thread waits, or blocks, until the lock becomes available. In this case, it ensures only one thread can enter its code block at a time, and “Done” will be printed just once. Code that’s protected in such a manner—from indeterminacy in a multithreaded context—is called thread-safe.

Locking is not a silver bullet for thread safety—it’s easy to forget to lock around accessing a field, and locking can create problems of its own(such as deadlocking).

A good example of when you might use locking is around accessing a shared inmemory cache for frequently accessed database objects in an ASP.NET application. This kind of application is simple to get right, and there’s no chance of deadlocking.

### Passing Data to a Thread

Sometimes you’ll want to pass arguments to the thread’s startup method. The easiest way to do this is with a lambda expression that calls the method with the desired arguments:

```csharp
static void Main()
{
    Thread t = new Thread(() => Print("Hello from t!"));
    t.Start();
}

static void Print(string message)
{
    Console.WriteLine(message);
}
```

With this approach, you can pass in any number of arguments to the method. You can even wrap the entire implementation in a multistatement lambda:

```csharp
new Thread(() =>
{
    Console.WriteLine("I'm running on another thread!");
    Console.WriteLine("This is so easy!");
}).Start();
```

Lambda expressions didn’t exist prior to C# 3.0. So you might also come across an old-school technique, which is to pass an argument into Thread’s `Start` method:

```csharp
static void Main()
{
    Thread t = new Thread(Print);
    t.Start("Hello from t!");
}

static void Print(object messageObj)
{
    string message = (string) messageObj; // We need to cast here
    Console.WriteLine(message);
}
```

This works because Thread’s constructor is overloaded to accept either of two delegates:

```csharp
public delegate void ThreadStart();
public delegate void ParameterizedThreadStart(object obj);
```

The limitation of `ParameterizedThreadStart` is that it accepts only one argument. And because it’s of type `object`, it usually needs to be cast.

#### Lambda expressions and captured variables

As we saw, a lambda expression is the most convenient and powerful way to pass data to a thread. However, you must be careful about accidentally modifying captured variables after starting the thread. For instance, consider the following:

```csharp
for (int i = 0; i < 10; i++)
    new Thread(() => Console.Write(i)).Start();
```

The output is nondeterministic! Here’s a typical result:

```
0223557799
```

The problem is that the i variable refers to the same memory location throughout the loop’s lifetime. Therefore, each thread calls `Console.Write` on a variable whose value may change as it is running! The solution is to use a temporary variable as follows:

```csharp
for (int i = 0; i < 10; i++)
{
    int temp = i;
    new Thread(() => Console.Write(temp)).Start();
}
```

Each of the digits 0 to 9 is then written exactly once. (The ordering is still undefined because threads may start at indeterminate times.)

Variable `temp` is now local to each loop iteration. Therefore, each thread captures a different memory location and there’s no problem. We can illustrate the problem in the earlier code more simply with the following example:

```csharp
string text = "t1";
Thread t1 = new Thread(() => Console.WriteLine(text));

text = "t2";
Thread t2 = new Thread(() => Console.WriteLine(text));

t1.Start();
t2.Start();
```

Because both lambda expressions capture the same text variable, `t2` is printed twice.

### Exception Handling

Any `try`/`catch`/`finally` blocks in effect when a thread is created are of no relevance to the thread when it starts executing. Consider the following program:

```csharp
public static void Main()
{
    try
    {
        new Thread(Go).Start();
    }
    catch (Exception ex)
    {
        // We'll never get here!
        Console.WriteLine("Exception!");
    }
}

static void Go() { throw null; } // Throws a NullReferenceException
```

The `try`/`catch` statement in this example is ineffective, and the newly created thread will be encumbered with an unhandled `NullReferenceException`. This behavior makes sense when you consider that each thread has an independent execution path.

A solução é mover o manipulador de exceção para o método `Go`:

```csharp
public static void Main()
{
    new Thread(Go).Start();
}

static void Go()
{
    try
    {
        // ...
        throw null; // The NullReferenceException will get caught below
        // ...
    }
    catch (Exception ex)
    {
        // Typically log the exception, and/or signal another thread
        // that we've come unstuck
        // ...
    }
}
```

You need an exception handler on all thread entry methods in production applications—just as you do(usually at a higher level, in the execution stack) on your main thread. An unhandled exception causes the whole application to shut down. With an ugly dialog box!

### Foreground Versus Background Threads

By default, threads you create explicitly are _foreground threads_. Foreground threads keep the application alive for as long as any one of them is running, whereas _background threads_ do not. Once all foreground threads finish, the application ends, and any background threads still running abruptly terminate.

_A thread’s foreground/background status has no relation to its priority(allocation of execution time)._

You can query or change a thread’s background status using its `IsBackground` property:

```csharp
static void Main(string[] args)
{
    Thread worker = new Thread(() => Console.ReadLine());

    if (args.Length > 0)
        worker.IsBackground = true;

    worker.Start();
}
```

If this program is called with no arguments, the worker thread assumes foreground status and will wait on the `ReadLine` statement for the user to press Enter. Meanwhile, the main thread exits, but the application keeps running because a foreground thread is still alive. On the other hand, if an argument is passed to `Main()`, the worker is assigned background status, and the program exits almost immediately as the main thread ends(terminating the `ReadLine`).

When a process terminates in this manner, any `finally` blocks in the execution stack of background threads are circumvented. If your program employs `finally`(or `using`) blocks to perform cleanup work such as deleting temporary files, you can avoid this by explicitly waiting out such background threads upon exiting an application, either by joining the thread, or with a signaling construct. In either case, you should specify a timeout, so you can abandon a renegade thread should it refuse to finish, otherwise your application will fail to close without the user having to enlist help from the Task Manager.

Foreground threads don’t require this treatment, but you must take care to avoid bugs that could cause the thread not to end. A common cause for applications failing to exit properly is the presence of active foreground threads.

## Tasks

Uma thread é uma ferramenta de baixo nível para criar Concorrência e, como tal, possui limitações. Em particular:

-   Embora seja fácil passar dados em uma thread que você começa, não há uma maneira fácil de obter um "valor de retorno" de uma thread `Join`. Você precisa configurar algum tipo de campo compartilhado. E se a operação lança uma exceção, capturar e propagar essa exceção é igualmente dolorosa.
-   You can’t tell a thread to start something else when it’s finished; instead you must `Join` it(blocking your own thread in the process).

These limitations discourage fine-grained concurrency; in other words, they make it hard to compose larger concurrent operations by combining smaller ones(something essential for the asynchronous programming that we’ll look at in following sections). This in turn leads to greater reliance on manual synchronization(locking, signaling, and so on) and the problems that go with it.

The direct use of threads also has performance implications that we discussed in “The Thread Pool”. And should you need to run hundreds or thousands of concurrent I/O-bound operations, a thread-based approach consumes hundreds or thousands of MB of memory purely in thread overhead.

The `Task` class helps with all of these problems. Compared to a thread, a `Task` is higher-level abstraction—it represents a concurrent operation that may or may not be backed by a thread. Tasks are _compositional_(you can chain them together through the use of _continuations_). They can use the _thread pool_ to lessen startup latency, and with a `TaskCompletionSource`, they can leverage a callback approach that avoids threads altogether while waiting on I/O-bound operations.

The `Task` types were introduced in Framework 4.0 as part of the parallel programming library. However, they have since been enhanced(through the use of _awaiters_) to play equally well in more general concurrency scenarios, and are backing types for C#’s asynchronous functions.

### Starting a Task

From Framework 4.5, the easiest way to start a `Task` backed by a thread is with the static method `Task.Run`(the Task class is in the `System.Threading.Tasks` namespace). Simply pass in an Action delegate:

```csharp
static void Main()
{
    Task.Run(() => Console.WriteLine("Foo"));
    Console.ReadLine();
}
```

_Tasks use pooled threads by default, which are background threads._

The `Task.Run` method was introduced in Framework 4.5. In Framework 4.0, you can accomplish the same thing by calling `Task.Factory.StartNew`. (The former is mostly a shortcut for the latter.)

Chamando `Task.Run` desta maneira é semelhante ao iniciar uma thread da seguinte maneira:

```csharp
new Thread(() => Console.WriteLine("Foo")).Start();
```

`Task.Run` returns a Task object that we can use to monitor its progress, rather like a Thread object. (Notice, however, that we didn’t call `Start` after calling `Task.Run` because this method creates “hot” tasks; you can instead use `Task’s` constructor to create “cold” tasks, although this is rarely done in practice.)

#### Wait

Chamando `Wait` em uma tarefa bloqueia até completar e é o equivalente a `Join` em uma `Thread`:

```csharp
Task task = Task.Run(() =>
{
    Thread.Sleep(2000);
    Console.WriteLine("Foo");
});

Console.WriteLine(task.IsCompleted);    // False
task.Wait();                            // Blocks until task is complete
```

#### Long-running tasks

Por padrão, o CLR executa tarefas em pooled threads(threads agrupadas), o que é ideal para o trabalho limitado em computação. Para operações mais longas e de bloqueio(como o nosso exemplo anterior), você pode impedir o uso de uma thread agrupado da seguinte maneira:

```csharp
Task task = Task.Factory.StartNew(() => {...}, TaskCreationOptions.LongRunning);
```

A execução de uma tarefa de longa duração em um pooled thread não causará problemas; é quando você executa várias tarefas longas em paralelo(particularmente aquelas que bloqueiam) que o desempenho pode sofrer.

### Returning values

`Task` has a generic subclass called `Task<TResult>` that allows a task to emit a return value. You can obtain a `Task<TResult>` by calling `Task.Run` with a `Func<TResult>` delegate(or a compatible lambda expression) instead of an `Action`:

```csharp
Task<int> task = Task.Run(() => { Console.WriteLine("Foo"); return 3; });
// ...
```

You can obtain the result later by querying the `Result` property. If the task hasn’t yet finished, accessing this property will block the current thread until the task finishes:

```csharp
int result = task.Result;   // Blocks if not already finished
Console.WriteLine(result); // 3
```

In the following example, we create a task that uses LINQ to count the number of prime numbers in the first three million (+2) integers:

```csharp
Task<int> primeNumberTask = Task.Run(() => Enumerable.Range(2, 3000000).Count(n => Enumerable.Range(2, (int)Math.Sqrt(n)-1).All(i => n % i > 0)));

Console.WriteLine("Task running...");
Console.WriteLine("The answer is " + primeNumberTask.Result);
```

This writes “Task running...”, and then a few seconds later, writes the answer of 216815.

_`Task<TResult>` can be thought of as a “future,” in that it encapsulates a `Result` that becomes available later in time._

### Exceptions

Unlike with threads, tasks conveniently propagate exceptions. So, if the code in your task throws an unhandled exception(in other words, if your task _faults_), that exception is automatically re-thrown to whoever calls `Wait()`—or accesses the Result property of a `Task<TResult>`:

```csharp
// Start a Task that throws a NullReferenceException:
Task task = Task.Run(() => { throw null; });
try
{
    task.Wait();
}
catch (AggregateException aex)
{
    if (aex.InnerException is NullReferenceException)
        Console.WriteLine("Null!");
    else
        throw;
}
```

(O CLR envolve a exceção em uma `AggregateException` para jogar bem com cenários de programação paralela.)

You can test for a faulted task without re-throwing the exception via the `IsFaulted` and `IsCanceled` properties of the `Task`. If both properties return false, no error occurred; if `IsCanceled` is `true`, an `OperationCanceledException` was thrown for that task; if `IsFaulted` is `true`, another type of exception was thrown and the `Exception` property will indicate the error.

### Continuations

A continuation says to a task, “when you’ve finished, continue by doing something else.” A continuation is usually implemented by a callback that executes once upon completion of an operation. There are two ways to attach a continuation to a task. The first was introduced in Framework 4.5 and is particularly significant because it’s used by C#’s asynchronous functions, as we’ll see soon.

```csharp
Task<int> primeNumberTask = Task.Run(() =>
    Enumerable.Range(2, 3000000).Count(n =>
    Enumerable.Range(2, (int)Math.Sqrt(n)-1).All(i => n % i > 0)));

var awaiter = primeNumberTask.GetAwaiter();

awaiter.OnCompleted(() =>
{
    int result = awaiter.GetResult();
    Console.WriteLine(result); // Writes result
});
```

Calling `GetAwaiter` on the task returns an awaiter object whose `OnCompleted` method tells the antecedent task(`primeNumberTask`) to execute a delegate when it finishes(or faults). It’s valid to attach a continuation to an already-completed task, in which case the continuation will be scheduled to execute right away.

If an antecedent task faults, the exception is re-thrown when the continuation code calls `awaiter.GetResult()`. Rather than calling `GetResult`, we could simply access the Result property of the antecedent. The benefit of calling `GetResult` is that if the antecedent faults, the exception is thrown directly without being wrapped in Aggre gateException, allowing for simpler and cleaner `catch` blocks.

For nongeneric tasks, `GetResult()` has a void return value. Its useful function is then solely to rethrow exceptions.

If a synchronization context is present, `OnCompleted` automatically captures it and posts the continuation to that context. This is very useful in rich-client applications, as it bounces the continuation back to the UI thread. In writing libraries, however, it’s not usually desirable because the relatively expensive UI-thread-bounce should occur just once upon leaving the library, rather than between method calls. Hence you can defeat it the `ConfigureAwait` method:

```csharp
var awaiter = primeNumberTask.ConfigureAwait(false).GetAwaiter();
```

If no synchronization context is present—or you use `ConfigureAwait(false)`—the continuation will(in general) execute on the same thread as the antecedent, avoiding unnecessary overhead.

The other way to attach a continuation is by calling the task’s `ContinueWith` method:

```csharp
primeNumberTask.ContinueWith(antecedent =>
{
    int result = antecedent.Result;
    Console.WriteLine(result); // Writes 123
});
```

`ContinueWith` itself returns a `Task`, which is useful if you want to attach further continuations. However, you must deal directly with `AggregateException` if the task faults, and write extra code to marshal the continuation in UI applications. And in non-UI contexts, you must specify `TaskContinuationOptions.ExecuteSynchronously` if you want the continuation to execute on the same thread; otherwise it will bounce to the thread pool. `ContinueWith` is particularly useful in parallel programming scenarios;

### TaskCompletionSource

We’ve seen how `Task.Run` creates a task that runs a delegate on a pooled(or nonpooled) thread. Another way to create a task is with `TaskCompletionSource`.

`TaskCompletionSource` lets you create a task out of any operation that starts and finishes some time later. It works by giving you a “slave” task that you manually drive—by indicating when the operation finishes or faults. This is ideal for I/Obound work: you get all the benefits of tasks(with their ability to propagate return values, exceptions, and continuations) without blocking a thread for the duration of the operation.

To use `TaskCompletionSource`, you simply instantiate the class. It exposes a `Task` property that returns a task upon which you can wait and attach continuationsjust as with any other task. The task, however, is controlled entirely by the `TaskCompletionSource` object via the following methods:

```csharp
public class TaskCompletionSource<TResult>
{
    public void SetResult(TResult result);
    public void SetException(Exception exception);
    public void SetCanceled();
    public bool TrySetResult(TResult result);
    public bool TrySetException(Exception exception);
    public bool TrySetCanceled();
    public bool TrysetCanceled(CancellationToken cancellationToken);
    ...
}
```

Calling any of these methods signals the task, putting it into a completed, faulted, or canceled state. You’re supposed to call one of these methods exactly once: if called again, `SetResult`, `SetException`, or `SetCanceled` will throw an exception, whereas the `Try*` methods return `false`.

O exemplo a seguir imprime 42 depois de esperar cinco segundos:

```csharp
var tcs = new TaskCompletionSource<int>();

new Thread(() => {
        Thread.Sleep(5000);
        tcs.SetResult(42);
    }){ IsBackground = true }.Start();

Task<int> task = tcs.Task;      // Our "slave" task.
Console.WriteLine(task.Result); // 42
```

With `TaskCompletionSource`, we can write our own Run method:

```csharp
Task<TResult> Run<TResult>(Func<TResult> function)
{
    var tcs = new TaskCompletionSource<TResult>();

    new Thread(() => {
        try {
            tcs.SetResult(function());
        } catch (Exception ex)
        {
            tcs.SetException(ex);
        }
    }).Start();

    return tcs.Task;
}
...
Task<int> task = Run(() => { Thread.Sleep(5000); return 42; });
```

Calling this method is equivalent to calling `Task.Factory.StartNew` with the Task `CreationOptions.LongRunning` option to request a nonpooled thread.

The real power of `TaskCompletionSource` is in creating tasks that don’t tie up threads. For instance, consider a task that waits for five seconds and then returns the number 42. We can write this without a thread by using the `Timer` class, which with the help of the CLR(and in turn, the operating system) fires an event in x milliseconds.

```csharp
Task<int> GetAnswerToLife()
{
    var tcs = new TaskCompletionSource<int>();

    // Create a timer that fires once in 5000 ms:
    var timer = new System.Timers.Timer(5000) { AutoReset = false };

    timer.Elapsed += delegate { timer.Dispose(); tcs.SetResult(42); };

    timer.Start();

    return tcs.Task;
}
```

Hence our method returns a task that completes five seconds later, with a result of 42. By attaching a continuation to the task, we can write its result without blocking any thread:

```csharp
var awaiter = GetAnswerToLife().GetAwaiter();
awaiter.OnCompleted(() => Console.WriteLine(awaiter.GetResult()));
```

We could make this more useful and turn it into a general-purpose `Delay` method by parameterizing the delay time and getting rid of the return value. This means having it return a `Task` instead of a `Task<int>`. However, there’s no nongeneric version of `TaskCompletionSource`, which means we can’t directly create a nongeneric `Task`. The workaround is simple: since `Task<TResult>` derives from `Task`, we create a `TaskCompletionSource<anything>` and then implicitly convert the `Task<any thing>` that it gives you into a `Task`, like this:

```csharp
var tcs = new TaskCompletionSource<object>();
Task task = tcs.Task;
```

Now we can write our general-purpose `Delay` method:

```csharp
Task Delay(int milliseconds)
{
    var tcs = new TaskCompletionSource<object>();
    var timer = new System.Timers.Timer(milliseconds) { AutoReset = false };

    timer.Elapsed += delegate { timer.Dispose(); tcs.SetResult(null); };

    timer.Start();

    return tcs.Task;
}
```

Here’s how we can use it to write “42” after five seconds:

```csharp
Delay(5000).GetAwaiter().OnCompleted(() => Console.WriteLine(42));
```

Our use of `TaskCompletionSource` without a thread means that a thread is engaged only when the continuation starts, five seconds later. We can demonstrate this by starting 10,000 of these operations at once without error or excessive resource consumption:

```csharp
for (int i = 0; i < 10000; i++)
    Delay(5000).GetAwaiter().OnCompleted(() => Console.WriteLine(42));
```

### Task.Delay

The `Delay` method that we just wrote is sufficiently useful that it’s available as a static method on the `Task` class:

```csharp
Task.Delay(5000).GetAwaiter().OnCompleted(() => Console.WriteLine(42));
```

Or

```csharp
Task.Delay(5000).ContinueWith(ant => Console.WriteLine(42));
```

`Task.Delay` is the asynchronous equivalent of `Thread.Sleep`.

## Using the Task Parallel Library

Using a thread pool allows us to save operating system resources at the cost of reducing a parallelism degree. We can think of a thread pool as an abstraction layer that hides details of thread usage from a programmer, allowing us to concentrate on a program's logic rather than on threading issues.

Para resolver todos esses problemas, uma nova API para operações assíncronas foi introduzida em _.NetFramework 4.0_. Foi chamado de Task Parallel Library(TPL). O _TPL_ pode ser considerado como uma abstração sobre um pool de threads, escondendo o código de nível inferior que funcionará com o pool de threads, fornecendo uma API mais conveniente, fina e simples.

O conceito central do TPL é uma tarefa. Uma tarefa representa uma operação assíncrona que pode ser executada de uma variedade de maneiras, usando uma thread separada ou não.

### Creating a task

```csharp
using System;
using System.Threading.Tasks;
using static System.Console;
using static System.Threading.Thread;

namespace ConsoleApp
{
    class Program
    {
        static void TaskMethod(string name)
        {
            WriteLine(
                $"Task {name} is running on a thread id " +
                $"{CurrentThread.ManagedThreadId}. Is thread pool thread:" +
                $"{CurrentThread.IsThreadPoolThread}");
        }

        static void Main(string[] args)
        {
            var t1 = new Task(() => TaskMethod("Task 1"));
            var t2 = new Task(() => TaskMethod("Task 2"));

            t2.Start();
            t1.Start();

            Task.Run(() => TaskMethod("Task 3"));
            Task.Factory.StartNew(() => TaskMethod("Task 4"));
            Task.Factory.StartNew(() => TaskMethod("Task 5"), TaskCreationOptions.LongRunning);

            Sleep(TimeSpan.FromSeconds(1));
        }
    }
}
```

Quando o programa é executado, ele cria duas tarefas com seu construtor. Passamos a expressão lambda como o delegate da ação; Isso nos permite fornecer um parâmetro de string para `TaskMethod`. Então, executamos essas tarefas usando o método `Start`.

_Observe que, até chamarmos o método `Start` nessas tarefas, eles não irão Iniciar a execução. É muito fácil esquecer de começar a tarefa._

Em seguida, executamos mais duas tarefas usando os métodos `Task.Run` e `Task.Factory.Start`. A diferença é que ambas as tarefas criadas começam imediatamente a funcionar, por isso não precisamos chamar o método `Start` das tarefas explicitamente. Todas as tarefas, numeradas `Tarefa 1` para a `Tarefa 4`, são colocadas nos threads do trabalhador do pool threads e são executadas em uma ordem não especificada. Se você executar o programa várias vezes, você achará que a ordem de execução da tarefa não está definida.

O método `Task.Run` é apenas um atalho para `Task.Factory.StartNew`, mas o último método possui opções adicionais. Em geral, use o método anterior, a menos que você precise fazer algo especial, como no caso da `Tarefa 5`. Marcamos essa tarefa como de longa duração e, como resultado, essa tarefa será executada em um segmento separado que não usa um thread pool. No entanto, esse comportamento pode mudar, dependendo do task scheduler(agendador de tarefas) atual que executa a tarefa.

### Performing basic operations with a task

```csharp
using System;
using System.Threading.Tasks;
using static System.Console;
using static System.Threading.Thread;

namespace ConsoleApp
{
    class Program
    {
        static Task<int> CreateTask(string name)
        {
            return new Task<int>(() => TaskMethod(name));
        }

        static int TaskMethod(string name)
        {
            WriteLine(
                $"Task {name} is running on a thread id " +
                $"{CurrentThread.ManagedThreadId}. Is thread pool thread: " +
                $"{CurrentThread.IsThreadPoolThread}");

            Sleep(TimeSpan.FromSeconds(2));

            return 42;
        }

        static void Main(string[] args)
        {
            TaskMethod("Main Thread Task");

            Task<int> task = CreateTask("Task 1");
            task.Start();

            int result = task.Result;
            WriteLine($"Result is: {result}");

            task = CreateTask("Task 2");
            task.RunSynchronously();

            result = task.Result;
            WriteLine($"Result is: {result}");

            task = CreateTask("Task 3");
            WriteLine(task.Status);
            task.Start();

            while (!task.IsCompleted) {
                WriteLine(task.Status);
                Sleep(TimeSpan.FromSeconds(0.5));
            }

            WriteLine(task.Status);
            result = task.Result;
            WriteLine($"Result is: {result}");
        }
    }
}
```

Em primeiro lugar, executamos o `TaskMethod` sem envolvê-lo em uma tarefa. Como resultado, é executado de forma síncrona, fornecendo-nos informações sobre o segmento principal. Obviamente, não é um thread de pool de threads.

Em seguida, executamos a `Tarefa 1`, começando com o método `Start` e aguardando o resultado. Esta tarefa será colocada em um grupo de threads e o segmento principal aguarda e é bloqueado até a tarefa retornar.

Fazemos o mesmo com a `Tarefa 2`, exceto que o executamos usando o método `RunSynchronously()`. Esta tarefa será executada no tópico principal, e obteremos exatamente a mesma saída que no primeiro caso quando chamamos `TaskMethod` de forma síncrona. Esta é uma otimização muito útil que nos permite evitar o uso do thread pool para operações de vida muito curta.

Executamos a `Tarefa 3` da mesma forma que fizemos a `Tarefa 1`, mas em vez de bloquear o segmento principal, simplesmente rolaremos, imprimindo o status da tarefa até que a tarefa seja concluída.

### Combining tasks

Irá mostrar-lhe como configurar tarefas que dependem umas das outras. Aprenderemos a criar uma tarefa que será executada após a conclusão da tarefa principal. Além disso, descobriremos uma maneira de salvar o uso da thread para tarefas de muito curta duração.

```csharp
using System;
using System.Threading.Tasks;
using static System.Console;
using static System.Threading.Thread;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var firstTask = new Task<int>(() => TaskMethod("First Task 1", 3));
            var secondTask = new Task<int>(() => TaskMethod("Second Task 1", 2));

            firstTask.ContinueWith(
                t => WriteLine(
                    $"The first answer is {t.Result}. Thread id " +
                    $"{CurrentThread.ManagedThreadId}, is thread pool thread: " +
                    $"{CurrentThread.IsThreadPoolThread}"), TaskContinuationOptions.OnlyOnRanToCompletion);

            firstTask.Start();
            secondTask.Start();

            Sleep(TimeSpan.FromSeconds(4));

            Task continuation = secondTask.ContinueWith(
                t => WriteLine(
                    $"The second answer is {t.Result}. Thread id " +
                    $"{CurrentThread.ManagedThreadId}, is thread pool thread: " +
                    $"{CurrentThread.IsThreadPoolThread}"),
                TaskContinuationOptions.OnlyOnRanToCompletion
                | TaskContinuationOptions.ExecuteSynchronously);

            continuation.GetAwaiter().OnCompleted(
                () => WriteLine(
                    $"Continuation Task Completed! Thread id " +
                    $"{CurrentThread.ManagedThreadId}, is thread pool thread: " +
                    $"{CurrentThread.IsThreadPoolThread}"));

            Sleep(TimeSpan.FromSeconds(2));
            WriteLine();

            firstTask = new Task<int>(() => {
                var innerTask = Task.Factory.StartNew(() => TaskMethod("Second Task 2", 5), TaskCreationOptions.AttachedToParent);

                innerTask.ContinueWith(t => TaskMethod("Third Task", 2), TaskContinuationOptions.AttachedToParent);

                return TaskMethod("First Task 2", 2);
            });

            firstTask.Start();

            while (!firstTask.IsCompleted) {
                WriteLine(firstTask.Status);
                Sleep(TimeSpan.FromSeconds(0.5));
            }

            WriteLine(firstTask.Status);

            Sleep(TimeSpan.FromSeconds(10));
        }

        static int TaskMethod(string name, int seconds)
        {
            WriteLine(
                $"Task {name} is running on a thread id " +
                $"{CurrentThread.ManagedThreadId}. Is thread pool thread: " +
                $"{CurrentThread.IsThreadPoolThread}");

            Sleep(TimeSpan.FromSeconds(seconds));

            return 42 * seconds;
        }
    }
}
```

Quando o programa principal é iniciado, criamos duas tarefas e, para a primeira tarefa, configuramos uma continuação(um bloco de código que é executado após a conclusão da tarefa antecedente). Em seguida, começamos as duas tarefas e aguardamos 4 segundos, o que é suficiente para que ambas as tarefas estejam completas. Em seguida, executamos outra continuação para a segunda tarefa e tentamos executá-la de forma síncrona, especificando uma opção `TaskContinuationOptions.ExecuteSynchronously`. Esta é uma técnica útil quando a continuação é de vida muito curta, e será mais rápido executá-la na thread principal do que colocá-la em um pool de threads. Podemos conseguir isso porque a segunda tarefa é completada por esse momento. Se comentarmos o método `Thread.Sleep` de 4 segundos, veremos que esse código será colocado em um conjunto de threads porque ainda não temos o resultado da tarefa antecedente.

Finalmente, definimos uma continuação para a continuação anterior, mas de uma maneira ligeiramente diferente, usando os novos métodos `GetAwaiter` e `OnCompleted`. Esses métodos são destinados a serem usados juntamente com a mecânica assíncrona de linguagem C#.

A última parte da demonstração é sobre os relacionamentos de tarefa pai-filho. Criamos uma nova tarefa, e ao executar esta tarefa, executamos uma chamada tarefa infantil fornecendo uma opção `TaskCreationOptions.AttachedToParent`.

_A tarefa filho deve ser criada durante a execução de uma tarefa pai, de modo que ela seja anexada ao pai corretamente._

Isso significa que a tarefa principal não será completa até que todas as tarefas filho terminem seu trabalho. Também podemos executar continuações nas tarefas filho que fornecem uma opção `TaskContinuationOptions.AttachedToParent`. Essas tarefas de continuação afetarão a tarefa principal também, e ela não será completa até que a última tarefa filho termine.

### Implementing a cancelation option

Sobre a implementação do processo de cancelamento para operações assíncronas baseadas em tarefas. Você aprenderá a usar o token de cancelamento corretamente para tarefas e como descobrir se uma tarefa foi cancelada antes que ela realmente fosse executada.

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using static System.Console;
using static System.Threading.Thread;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var cts = new CancellationTokenSource();
            var longTask = new Task<int>(() => TaskMethod("Task 1", 10, cts.Token), cts.Token);

            WriteLine(longTask.Status);
            cts.Cancel();

            WriteLine(longTask.Status);
            WriteLine("First task has been cancelled before execution");

            cts = new CancellationTokenSource();
            longTask = new Task<int>(() => TaskMethod("Task 2", 10, cts.Token), cts.Token);
            longTask.Start();

            for (int i = 0; i < 5; i++) {
                Sleep(TimeSpan.FromSeconds(0.5));
                WriteLine(longTask.Status);
            }

            cts.Cancel();

            for (int i = 0; i < 5; i++) {
                Sleep(TimeSpan.FromSeconds(0.5));
                WriteLine(longTask.Status);
            }

            WriteLine($"A task has been completed with result {longTask.Result}.");
        }

        static int TaskMethod(string name, int seconds, CancellationToken token)
        {
            WriteLine(
                $"Task {name} is running on a thread id " +
                $"{CurrentThread.ManagedThreadId}. Is thread pool thread: " +
                $"{CurrentThread.IsThreadPoolThread}");

            for (int i = 0; i < seconds; i++) {
                Sleep(TimeSpan.FromSeconds(1));
                if (token.IsCancellationRequested) return -1;
            }

            return 42 * seconds;
        }
    }
}
```

Primeiro, vejamos atentamente o código de criação `longTask`. Estamos fornecendo um token de cancelamento para a tarefa subjacente uma vez e depois para o construtor de tarefas pela segunda vez. Por que precisamos fornecer este token duas vezes?

A resposta é que se cancelarmos a tarefa antes que ela realmente tenha sido iniciada, sua infraestrutura TPL é responsável por lidar com o cancelamento porque nosso código não será executado. Sabemos que a primeira tarefa foi cancelada ao obter seu status. Se tentarmos chamar o método `Start` nesta tarefa, obteremos `InvalidOperationException`.

Então, lidamos com o processo de cancelamento de nosso próprio código. Isso significa que agora somos totalmente responsáveis pelo processo de cancelamento, e depois de cancelarmos a tarefa, seu status ainda era `OnlyOnRanToCompletion` porque, da perspectiva do TPL, a tarefa terminava seu trabalho normalmente. É muito importante distinguir essas duas situações e compreender a diferença de responsabilidade em cada caso.

### Handling exceptions in tasks

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using static System.Console;
using static System.Threading.Thread;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Task<int> task;

            try {

                task = Task.Run(() => TaskMethod("Task 1", 2));
                int result = task.Result;
                WriteLine($"Result: {result}");

            } catch (Exception ex) {
                WriteLine($"Exception caught 1: {ex}");
            }

            WriteLine("----------------------------------------------");
            WriteLine();

            try {

                task = Task.Run(() => TaskMethod("Task 2", 2));
                int result = task.GetAwaiter().GetResult();
                WriteLine($"Result: {result}");

            } catch (Exception ex) {
                WriteLine($"Exception caught 2: {ex}");
            }

            WriteLine("----------------------------------------------");
            WriteLine();

            var t1 = new Task<int>(() => TaskMethod("Task 3", 3));
            var t2 = new Task<int>(() => TaskMethod("Task 4", 2));

            var complexTask = Task.WhenAll(t1, t2);

            var exceptionHandler = complexTask.ContinueWith(t => WriteLine($"Exception caught 3: {t.Exception}"), TaskContinuationOptions.OnlyOnFaulted);

            t1.Start();
            t2.Start();

            Sleep(TimeSpan.FromSeconds(5));
        }

        static int TaskMethod(string name, int seconds)
        {
            WriteLine(
                $"Task {name} is running on a thread id " +
                $"{CurrentThread.ManagedThreadId}. Is thread pool thread: " +
                $"{CurrentThread.IsThreadPoolThread}");

            Sleep(TimeSpan.FromSeconds(seconds));

            throw new Exception("Boom!");

            return 42 * seconds;
        }
    }
}
```

Quando o programa é iniciado, criamos uma tarefa e tentamos obter os resultados da tarefa de forma síncrona. A propriedade `GetResult` faz com que o thread atual aguarde até a conclusão da tarefa e propague a exceção para o thread atual. Nesse caso, capturamos facilmente a exceção em um bloco `catch`, mas esta exceção é uma exceção de wrapper chamada `AggregateException`. Nesse caso, ele contém apenas uma exceção interna porque apenas uma tarefa lançou essa exceção e é possível obter a exceção subjacente ao acessar a propriedade `InnerException`.

O segundo exemplo é principalmente o mesmo, mas para acessar o `result` da tarefa, usamos os métodos `GetAwaiter` e `GetResult`. Nesse caso, não temos uma exceção de wrapper porque é unwrapped pela infra-estrutura TPL. Temos uma exceção original ao mesmo tempo, o que é bastante confortável se tivermos apenas uma tarefa subjacente.

O último exemplo mostra a situação em que temos duas exceções de lançamento de tarefas. Para lidar com exceções, agora usamos uma continuação, que é executada somente no caso de a tarefa antecedente terminar com uma exceção. Esse comportamento é alcançado fornecendo uma opção `TaskContinuationOptions.OnlyOnFaulted` para uma continuação. Como resultado, temos a excepção `AggregateException`, e temos duas exceções internas de ambas as tarefas dentro dela.

### Running tasks in parallel

Mostra como lidar com muitas tarefas assíncronas que estão sendo executadas simultaneamente. Você aprenderá como ser notificado efetivamente quando todas as tarefas estiverem concluídas ou qualquer uma das tarefas em execução tiver que terminar seu trabalho.

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using static System.Console;
using static System.Threading.Thread;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var firstTask = new Task<int>(() => TaskMethod("First Task", 3));
            var secondTask = new Task<int>(() => TaskMethod("Second Task", 2));
            var whenAllTask = Task.WhenAll(firstTask, secondTask);

            whenAllTask.ContinueWith(t =>
                WriteLine($"The first answer is {t.Result[0]}, the second is {t.Result[1]}"),
                TaskContinuationOptions.OnlyOnRanToCompletion);

            firstTask.Start();
            secondTask.Start();

            Sleep(TimeSpan.FromSeconds(4));

            var tasks = new List<Task<int>>();
            for (int i = 1; i < 4; i++) {
                int counter = i;
                var task = new Task<int>(() => TaskMethod($"Task {counter}", counter));
                tasks.Add(task);
                task.Start();
            }

            while (tasks.Count > 0) {
                var completedTask = Task.WhenAny(tasks).Result;
                tasks.Remove(completedTask);
                WriteLine($"A task has been completed with result {completedTask.Result}.");
            }

            Sleep(TimeSpan.FromSeconds(1));
        }

        static int TaskMethod(string name, int seconds)
        {
            WriteLine(
                $"Task {name} is running on a thread id " +
                $"{CurrentThread.ManagedThreadId}. Is thread pool thread: " +
                $"{CurrentThread.IsThreadPoolThread}");

            Sleep(TimeSpan.FromSeconds(seconds));

            return 42 * seconds;
        }
    }
}
```

Quando o programa é iniciado, criamos duas tarefas e, com a ajuda do método `Task.WhenAll`, criamos uma terceira tarefa, que estará completa depois de todas as tarefas iniciais serem concluídas. A tarefa resultante nos fornece uma matriz de respostas, onde o primeiro elemento contém o resultado da primeira tarefa, o segundo elemento contém o segundo resultado, e assim por diante.

Em seguida, criamos outra lista de tarefas e esperamos que qualquer uma dessas tarefas seja completada com o `Task.WhenAny` method. Depois de ter uma tarefa concluída, a remoção da lista e continuamos a aguardar a conclusão das outras tarefas até a lista estar vazia. Este método é útil para obter o progresso da conclusão da tarefa ou para usar um tempo limite ao executar as tarefas. Por exemplo, esperamos uma série de tarefas, e uma dessas tarefas está contando um tempo limite. Se esta tarefa for concluída primeiro, simplesmente cancelamos todas as outras tarefas que ainda não foram concluídas.

## Principles of Asynchrony

In demonstrating `TaskCompletionSource`, we ended up writing asynchronous methods. In this section, we’ll define exactly what asynchronous operations are, and explain how this leads to asynchronous programming.

### Synchronous Versus Asynchronous Operations

_A synchronous operation does its work before returning to the caller._

_An asynchronous operation does(most or all of) its work after returning to the caller._

The majority of methods that you write and call are synchronous. An example is `List<T>.Add`, or `Console.WriteLine`, or `Thread.Sleep`. Asynchronous methods are less common, and initiate _concurrency_, because work continues in parallel to the caller. Asynchronous methods typically return quickly(or immediately) to the caller; hence they are also called _nonblocking methods_.

A maioria dos métodos assíncronos que vimos até agora pode ser descrito como Métodos de uso geral:

-   `Thread.Start`
-   `Task.Run`
-   Methods that attach continuations to tasks

### What is Asynchronous Programming?

The principle of asynchronous programming is that you write long-running(or potentially long-running) functions asynchronously. This is in contrast to the conventional approach of writing long-running functions synchronously, and then calling those functions from a new thread or task to introduce concurrency as required.

The difference with the asynchronous approach is that concurrency is initiated _inside_ the long-running function, rather than from _outside_ the function. This has two benefits:

-   I/O-bound concurrency can be implemented without tying up threads, improving scalability and efficiency.
-   Rich-client applications end up with less code on worker threads, simplifying thread safety.

This, in turn, leads to two distinct uses for asynchronous programming. The first is writing(typically server-side) applications that deal efficiently with a lot of concurrent I/O. The challenge here is not thread safety(as there’s usually minimal shared state) but thread efficiency; in particular, not consuming a thread per network request. Hence in this context, it’s only I/O-bound operations that benefit from asynchrony.

The second use is to simplify thread safety in rich-client applications. This is particularly relevant as a program grows in size, because to deal with complexity, we typically refactor larger methods into smaller ones, resulting in chains of methods that call one another(call graphs).

With a traditional synchronous call graph, if any operation within the graph is long-running, we must run the entire call graph on a worker thread to maintain a responsive UI. Hence, we end up with a single concurrent operation that spans many methods(coarse-grained concurrency), and this requires considering thread safety for every method in the graph.

With an asynchronous call graph, we need not start a thread until it’s actually needed, typically low in the graph(or not at all in the case of I/O-bound operations). All other methods can run entirely on the UI thread, with much-simplified thread safety. This results in fine-grained concurrency—a sequence of small concurrent operations, in between which execution bounces to the UI thread.

### Asynchronous Programming and Continuations

Tasks are ideally suited to asynchronous programming, because they support continuations that are essential for asynchrony(consider the Delay method that we wrote previously in `“TaskCompletionSource”`). In writing Delay, we used `TaskCompletionSource`, which is a standard way to implement “bottom-level” I/O-bound asynchronous methods.

For compute-bound methods, we use `Task.Run` to initiate thread-bound concurrency. Simply by returning the task to the caller, we create an asynchronous method. What distinguishes asynchronous programming is that we aim to do so lower in the call graph, so that in rich-client applications, higher-level methods can remain on the UI thread and access controls and shared state without thread-safety issues. To illustrate, consider the following method, which computes and counts prime numbers, using all available cores:

```csharp
int GetPrimesCount(int start, int count)
{
    return ParallelEnumerable.Range(start, count).Count(n => Enumerable.Range(2, (int)Math.Sqrt(n)-1).All(i => n % i > 0));
}
```

The details of how this works are unimportant; what matters is that it can take a while to run. We can demonstrate this by writing another method to call it:

```csharp
void DisplayPrimeCounts()
{
    for (int i = 0; i < 10; i++)
        Console.WriteLine(GetPrimesCount(i*1000000 + 2, 1000000) + " primes between " + (i*1000000) + " and " + ((i+1)*1000000-1));

    Console.WriteLine("Done!");
}
```

with the following output:

```
78498 primes between 0 and 999999
70435 primes between 1000000 and 1999999
67883 primes between 2000000 and 2999999
66330 primes between 3000000 and 3999999
65367 primes between 4000000 and 4999999
64336 primes between 5000000 and 5999999
63799 primes between 6000000 and 6999999
63129 primes between 7000000 and 7999999
62712 primes between 8000000 and 8999999
62090 primes between 9000000 and 9999999
```

Now we have a call graph, with `DisplayPrimeCounts` calling `GetPrimesCount`. The former uses `Console.WriteLine` for simplicity, although in reality it would more likely be updating UI controls in a rich-client application, as we’ll demonstrate later. We can initiate coarse-grained concurrency for this call graph as follows:

```csharp
Task.Run(() => DisplayPrimeCounts());
```

With a fine-grained asynchronous approach, we instead start by writing an asynchronous version of `GetPrimesCount`:

```csharp
Task<int> GetPrimesCountAsync(int start, int count)
{
    return Task.Run(() => ParallelEnumerable.Range(start, count).Count(n => Enumerable.Range(2, (int) Math.Sqrt(n)-1).All(i => n % i > 0)));
}
```

#### Why Language Support Is Important

Now we must modify DisplayPrimeCounts so that it calls `GetPrimesCountAsync`. This is where C#’s new await and async keywords come into play, because to do so otherwise is trickier than it sounds. If we simply modify the loop as follows:

```csharp
for (int i = 0; i < 10; i++) {
    var awaiter = GetPrimesCountAsync(i*1000000 + 2, 1000000).GetAwaiter();
    awaiter.OnCompleted(() => Console.WriteLine(awaiter.GetResult() + " primes between..."));
}

Console.WriteLine("Done");
```

then the loop will rapidly spin through ten iterations(the methods being nonblocking) and all ten operations will execute in parallel(followed by a premature “Done”).

To get them running sequentially, we must trigger the next loop iteration from the continuation itself. This means eliminating the for loop and resorting to a recursive call in the continuation:

```csharp
void DisplayPrimeCounts()
{
    DisplayPrimeCountsFrom(0);
}

void DisplayPrimeCountsFrom(int i)
{
    var awaiter = GetPrimesCountAsync(i*1000000 + 2, 1000000).GetAwaiter();

    awaiter.OnCompleted(() => {
        Console.WriteLine(awaiter.GetResult() + " primes between...");

        if (++i < 10)
            DisplayPrimeCountsFrom(i);
        else
            Console.WriteLine("Done");
    });
}
```

It gets even worse if we want to make `DisplayPrimesCount` _itself_ asynchronous, returning a task that it signals upon completion. To accomplish this requires creating a `TaskCompletionSource`:

```csharp
Task DisplayPrimeCountsAsync()
{
    var machine = new PrimesStateMachine();
    machine.DisplayPrimeCountsFrom(0);
    return machine.Task;
}

class PrimesStateMachine
{
    TaskCompletionSource<object> _tcs = new TaskCompletionSource<object>(); public Task Task { get { return _tcs.Task; } }

    public void DisplayPrimeCountsFrom(int i)
    {
        var awaiter = GetPrimesCountAsync(i*1000000+2, 1000000).GetAwaiter();

        awaiter.OnCompleted(() => {
            Console.WriteLine(awaiter.GetResult());

            if (++i < 10)
                DisplayPrimeCountsFrom(i);
            else {
                Console.WriteLine("Done");
                _tcs.SetResult(null);
            }
        });
    }
}
```

Fortunately, C#’s _asynchronous_ functions do all of this work for us. With the `async` and `await` keywords, we need only write this:

```csharp
async Task DisplayPrimeCountsAsync()
{
    for (int i = 0; i < 10; i++)
        Console.WriteLine(await GetPrimesCountAsync(i*1000000 + 2, 1000000) + " primes between " + (i*1000000) + " and " + ((i+1)*1000000-1));

    Console.WriteLine("Done!");
}
```

## Asynchronous Functions in C#

C# 5.0 introduced the `async` and `await` keywords. These keywords let you write asynchronous code that has the same structure and simplicity as synchronous code, as well as eliminating the “plumbing” of asynchronous programming.

### Awaiting

The `await` keyword simplifies the attaching of continuations. Starting with a basic scenario, the compiler expands:

```csharp
var result = await expression;
statement(s)
```

into something functionally similar to:

```csharp
var awaiter = expression.GetAwaiter();

awaiter.OnCompleted(() =>
{
    var result = awaiter.GetResult();
    statement(s);
});
```

To demonstrate, let’s revisit the asynchronous method that we wrote previously that computes and counts prime numbers:

```csharp
Task<int> GetPrimesCountAsync(int start, int count)
{
    return Task.Run(() => ParallelEnumerable.Range(start, count).Count(n => Enumerable.Range(2, (int)Math.Sqrt(n) - 1).All(i => n % i > 0)));
}
```

With the `await` keyword, we can call it as follows:

```csharp
int result = await GetPrimesCountAsync(2, 1000000);
Console.WriteLine(result);
```

In order to compile, we need to add the `async` modifier to the containing method:

```csharp
async void DisplayPrimesCount()
{
    int result = await GetPrimesCountAsync(2, 1000000);
    Console.WriteLine(result);
}
```

The `async` modifier tells the compiler to treat `await` as a keyword rather than an identifier should an ambiguity arise within that method(this ensures that code written prior to C# 5 that might use `await` as an identifier will still compile without error). The `async` modifier can be applied only to methods(and lambda expressions) that return void or(as we’ll see later) a Task or `Task<TResult>.`

_The `async` modifier is similar to the `unsafe` modifier in that it has no effect on a method’s signature or public metadata; it affects only what happens inside the method. For this reason, it makes no sense to use `async` in an interface. However it is legal, for instance, to introduce `async` when overriding a `nonasync` virtual method, as long as you keep the signature the same._

Methods with the `async` modifier are called _asynchronous functions_, because they themselves are typically asynchronous. To see why, let’s look at how execution proceeds through an asynchronous function.

Upon encountering an `await` expression, execution(normally) returns to the caller —rather like with `yield` return in an iterator. But before returning, the runtime attaches a continuation to the awaited task, ensuring that when the task completes, execution jumps back into the method and continues where it left off. If the task faults, its exception is re-thrown, otherwise its return value is assigned to the `await` expression. We can summarize everything we just said by looking at the logical expansion of the preceding asynchronous method:

```csharp
void DisplayPrimesCount()
{
    var awaiter = GetPrimesCountAsync(2, 1000000).GetAwaiter();

    awaiter.OnCompleted(() => {
        int result = awaiter.GetResult();
        Console.WriteLine(result);
    });
}
```

The expression upon which you await is typically a task; however, any object with a `GetAwaiter` method that returns an awaitable object(implementing `INotifyCompletion.OnCompleted` and with an appropriately typed `GetResult` method and a bool `IsCompleted` property) will satisfy the compiler.

Notice that our await expression evaluates to an `int` type; this is because the expression that we awaited was a `Task<int>`(whose `GetAwaiter().GetResult()` method returns an `int`).

Awaiting a nongeneric task is legal and generates a void expression:

```csharp
await Task.Delay(5000);
Console.WriteLine("Five seconds passed!");
```

### Writing Asynchronous Functions

With any asynchronous function, you can replace the void return type with a `Task` to make the method itself _usefully_ asynchronous(and `awaitable`). No further changes are required:

```csharp
async Task PrintAnswerToLife() // We can return Task instead of void
{
    await Task.Delay(5000);
    int answer = 21 * 2;
    Console.WriteLine(answer);
}
```

Notice that we don’t explicitly return a task in the method body. The compiler manufactures the task, which it signals upon completion of the method(or upon an unhandled exception). This makes it easy to create asynchronous call chains:

```csharp
async Task Go()
{
    await PrintAnswerToLife();
    Console.WriteLine("Done");
}
```

And because we’ve declared `Go` with a `Task` return type, `Go` itself is awaitable.

The compiler expands asynchronous functions that return tasks into code that leverages `TaskCompletionSource` to create a task that it then signals or faults.

Nuances aside, we can expand `PrintAnswerToLife` into the following functional equivalent:

```csharp
Task PrintAnswerToLife()
{
    var tcs = new TaskCompletionSource<object>();
    var awaiter = Task.Delay(5000).GetAwaiter();

    awaiter.OnCompleted(() =>
    {
        try
        {
            awaiter.GetResult(); // Rethrow any exceptions
            int answer = 21 * 2;
            Console.WriteLine(answer);
            tcs.SetResult(null);
        } catch(Exception ex) { tcs.SetException(ex); }
    });

    return tcs.Task;
}
```

Hence, whenever a task-returning asynchronous method finishes, execution jumps back to whoever awaited it(by virtue of a continuation).

#### Returning `Task<TResult>`

You can return a `Task<TResult>` if the method body returns `TResult`:

```csharp
async Task<int> GetAnswerToLife()
{
    await Task.Delay(5000);
    int answer = 21 * 2;
    return answer; // Method has return type Task<int> we return int
}
```

Internally, this results in the `TaskCompletionSource` being signaled with a value rather than `null`. We can demonstrate `GetAnswerToLife` by calling it from `PrintAnswerToLife`(which is turn, called from `Go`):

```csharp
async Task Go()
{
    await PrintAnswerToLife();
    Console.WriteLine("Done");
}

async Task PrintAnswerToLife()
{
    int answer = await GetAnswerToLife();
    Console.WriteLine(answer);
}

async Task<int> GetAnswerToLife()
{
    await Task.Delay(5000);
    int answer = 21 * 2;
    return answer;
}
```

In effect, we’ve refactored our original `PrintAnswerToLife` into two methods—with the same ease as if we were programming synchronously. The similarity to synchronous programming is intentional; here’s the synchronous equivalent of our call graph, for which calling `Go()` gives the same result after blocking for five seconds:

```csharp
void Go()
{
    PrintAnswerToLife();
    Console.WriteLine("Done");
}

void PrintAnswerToLife()
{
    int answer = GetAnswerToLife();
    Console.WriteLine(answer);
}

int GetAnswerToLife()
{
    Thread.Sleep(5000);
    int answer = 21 * 2;
    return answer;
}
```

This also illustrates the basic principle of how to design with asynchronous functions in C#:

1.  Write your methods synchronously.
2.  Replace _synchronous_ method calls with _asynchronous_ method calls, and await them.
3.  Except for “top-level” methods(typically event handlers for UI controls), upgrade your asynchronous methods’ return types to `Task` or `Task<TResult>` so that they’re awaitable.

The compiler’s ability to manufacture tasks for asynchronous functions means that for the most part, you need to explicitly instantiate a `TaskCompletionSource` only in bottom-level methods that initiate I/O-bound concurrency. (And for methods that initiate compute-bound currency, you create the task with `Task.Run`.)

#### Parallelism

Calling an asynchronous method without awaiting it allows the code that follows to execute in parallel. You might have noticed in earlier examples that we had a button whose event handler called `Go` as follows:

```csharp
_button.Click += (sender, args) => Go();
```

Despite `Go` being an asynchronous method, we didn’t await it, and this is indeed what facilitates the concurrency needed to maintain a responsive UI.

We can use this same principle to run two asynchronous operations in parallel:

```csharp
var task1 = PrintAnswerToLife();
var task2 = PrintAnswerToLife();
await task1;
await task2;
```

(By awaiting both operations afterward, we “end” the parallelism at that point. Later, we’ll describe how the `WhenAll` task combinator helps with this pattern.)

Concurrency created in this manner occurs whether or not the operations are initiated on a UI thread, although there’s a difference in how it occurs. In both cases, we get the same “true” concurrency occurring in the bottom-level operations that initiate it(such as `Task.Delay`, or code farmed to `Task.Run`). Methods above this in the call stack will be subject to `true` concurrency only if the operation was initiated without a synchronization context present; otherwise they will be subject to the pseudoconcurrency(and simplified thread safety) that we talked about earlier, whereby the only places at which we can be preempted is at an `await` statement. This lets us, for instance, define a shared field, `_x`, and increment it in `GetAnswerToLife` without locking:

```csharp
async Task<int> GetAnswerToLife()
{
    _x++;
    await Task.Delay(5000);
    return 21 * 2;
}
```

(We would, though, be unable to assume that `_x` had the same value before and after the `await`.)

### Asynchronous Lambda Expressions

Just as ordinary _named_ methods can be asynchronous:

```csharp
async Task NamedMethod()
{
    await Task.Delay(1000);
    Console.WriteLine("Foo");
}
```

so can _unnamed_ methods(lambda expressions and anonymous methods), if preceded by the async keyword:

```csharp
Func<Task> unnamed = async() =>
{
    await Task.Delay(1000);
    Console.WriteLine("Foo");
};
```

We can call and await these in the same way:

```csharp
await NamedMethod();
await unnamed();
```

Asynchronous lambda expressions can be used when attaching event handlers:

```csharp
myButton.Click += async(sender, args) =>
{
    await Task.Delay(1000);
    myButton.Content = "Done";
};
```

This is more succinct than the following, which has the same effect:

```csharp
myButton.Click += ButtonHandler;
...
async void ButtonHander(object sender, EventArgs args)
{
    await Task.Delay(1000);
    myButton.Content = "Done";
};
```

Asynchronous lambda expressions can also return `Task<TResult>`:

```csharp
Func<Task<int>> unnamed = async () =>
{
    await Task.Delay(1000);
    return 123;
};

int answer = await unnamed();
```

### Asynchrony and Synchronization Contexts

We’ve already seen how the presence of a synchronization context is significant in terms of posting continuations. There are a couple of other more subtle ways in which synchronization contexts come into play with void-returning asynchronous functions. These are not a direct result of C# compiler expansions, but a function of the Async\*MethodBuilder types in the `System.CompilerServices` namespace that the compiler uses in expanding asynchronous functions.

#### Exception posting

It’s common practice in rich-client applications to rely on the central exceptionhandling event(`Application.DispatcherUnhandledException` in WPF) to process unhandled exceptions thrown on the UI thread. And in ASP.NET applications, the `Application_Error` in global.asax does a similar job. Internally, they work by invoking UI events(or in ASP.NET, the pipeline of page processing methods) in their own `try`/`catch` block.

Top-level asynchronous functions complicate this. Consider the following event handler for a button click:

```csharp
async void ButtonClick(object sender, RoutedEventArgs args)
{
    await Task.Delay(1000);
    throw new Exception("Will this be ignored?");
}
```

When the button is clicked and the event handler runs, execution returns normally to the message loop after the `await` statement, and the exception that’s thrown a second later cannot be caught by the catch block in the message loop.

To mitigate this problem, `AsyncVoidMethodBuilder` catches unhandled exceptions(in void-returning asynchronous functions), and posts them to the synchronization context if present, ensuring that global exception-handling events still fire.

_The compiler applies this logic only to void-returning asynchronous functions. So if we changed ButtonClick to return a Task instead of void, the unhandled exception would fault the resultant Task, which would then have nowhere to go(resulting in an unobserved exception)._

An interesting nuance is that it makes no difference whether you throw before or after an `await`. So in the following example, the exception is posted to the synchronization context(if present) and never to the caller:

```csharp
async void Foo() { throw null; await Task.Delay(1000); }
```

If no synchronization context is present, the exception will go unobserved. It might seem odd that the exception isn’t thrown right back to the caller, although it’s not entirely different to what happens with iterators:

```csharp
IEnumerable<int> Foo() { throw null; yield return 123; }
```

In this example, an exception is never thrown straight back to the caller: not until the sequence is enumerated is the exception thrown.

### Optimizations

#### Completing synchronously

An asynchronous function may return _before_ awaiting. Consider the following method that caches the downloading of web pages:

```csharp
static Dictionary<string,string> _cache = new Dictionary<string,string>();

async Task<string> GetWebPageAsync(string uri)
{
    string html;

    if (_cache.TryGetValue(uri, out html))
        return html;

    return _cache [uri] = await new WebClient().DownloadStringTaskAsync(uri);
}
```

Should a URI already exist in the cache, execution returns to the caller with no awaiting having occurred, and the method returns an _already-signaled_ task. This is referred to as synchronous completion.

When you await a synchronously completed task, execution does not return to the caller and bounce back via a continuation—instead, it proceeds immediately to the next statement. The compiler implements this optimization by checking the `IsCompleted` property on the awaiter; in other words, whenever you await:

```csharp
Console.WriteLine(await GetWebPageAsync("http://oreilly.com"));
```

the compiler emits code to _short-circuit_ the continuation in case of synchronization completion:

```csharp
var awaiter = GetWebPageAsync().GetAwaiter();

if (awaiter.IsCompleted)
    Console.WriteLine(awaiter.GetResult());
else
    awaiter.OnCompleted(() => Console.WriteLine(awaiter.GetResult());
```

_Awaiting an asynchronous function that returns synchronously still incurs a small overhead—maybe 50–100 nanoseconds on a 2015-era PC._

_In contrast, bouncing to the thread pool introduces the cost of a context switch—perhaps one or two microseconds, and bouncing to a UI message loop, at least ten times that(much longer if the UI thread is busy)._

It’s even legal to write asynchronous methods that never await, although the compiler will generate a warning:

```csharp
async Task<string> Foo() { return "abc"; }
```

Such methods can be useful when overriding `virtual`/`abstract` methods, if your implementation doesn’t happen to need asynchrony. (An example is Memory Stream’s `ReadAsync`/`WriteAsync`.) Another way to achieve the same result is to use `Task.FromResult`, which returns an already-signaled task:

```csharp
Task<string> Foo() { return Task.FromResult("abc"); }
```

Our `GetWebPageAsync` method is implicitly thread-safe if called from a UI thread, in that you could invoke it several times in succession(thereby initiating multiple concurrent downloads), and no locking is required to protect the cache. If the series of calls were to the same URI, though, we’d end up initiating multiple redundant downloads, all of which would eventually update the same cache entry(the last one winning). While not erroneous, it would be more efficient if subsequent calls to the same URI could instead(asynchronously) wait upon the result of the in-progress request.

There’s an easy way to accomplish this—without resorting to locks or signaling constructs. Instead of a cache of strings, we create a cache of “futures”(`Task<string>`):

```csharp
static Dictionary<string,Task<string>> _cache = new Dictionary<string,Task<string>>();

Task<string> GetWebPageAsync(string uri)
{
    Task<string> downloadTask;

    if (_cache.TryGetValue(uri, out downloadTask))
        return downloadTask;

    return _cache[uri] = new WebClient().DownloadStringTaskAsync(uri);
}
```

(Notice that we don’t mark the method as `async`, because we’re directly returning the task we obtain from calling `WebClient’s` method.)

If we call `GetWebPageAsync` repeatedly with the same URI, we’re now guaranteed to get the same `Task<string>` object back. (This has the additional benefit of minimizing GC load.) And if the task is complete, awaiting it is cheap, thanks to the compiler optimization that we just discussed.

We could further extend our example to make it thread-safe without the protection of a synchronization context, by locking around the entire method body:

```csharp
lock(_cache)
{
    Task<string> downloadTask;

    if (_cache.TryGetValue(uri, out downloadTask))
        return downloadTask;

    return _cache [uri] = new WebClient().DownloadStringTaskAsync(uri);
}
```

This works because we’re not locking for the duration of downloading a page(which would hurt concurrency); we’re locking for the small duration of checking the cache, starting a new task if necessary, and updating the cache with that task.

## Asynchronous Patterns

### Cancellation

It’s often important to be able to cancel a concurrent operation after it’s started, perhaps in response to a user request. A simple way to implement this is with a cancellation flag, which we could encapsulate by writing a class like this:

```csharp
class CancellationToken
{
    public bool IsCancellationRequested { get; private set; }
    public void Cancel() { IsCancellationRequested = true; }
    public void ThrowIfCancellationRequested()
    {
        if (IsCancellationRequested)
            throw new OperationCanceledException();
    }
}
```

We could then write a cancellable asynchronous method as follows:

```csharp
async Task Foo(CancellationToken cancellationToken)
{
    for(int i = 0; i < 10; i++)
    {
        Console.WriteLine(i);
        await Task.Delay(1000);
        cancellationToken.ThrowIfCancellationRequested();
    }
}
```

When the caller wants to cancel, it calls Cancel on the cancellation token that it passed into `Foo`. This sets `IsCancellationRequested` to true, which causes `Foo` to fault a short time later with an `OperationCanceledException`(a predefined exception in the `System` namespace designed for this purpose).

Thread safety aside(we should be locking around reading/writing `IsCancellationRequested`), this pattern is effective and the CLR provides a type called `CancellationToken` which is very similar to what we’ve just shown. However, it lacks a `Cancel` method; this method is instead exposed on another type called `CancellationTokenSource`. This separation provides some security: a method that has access only to a `CancellationToken` object can check for but not initiate cancellation.

To get a cancellation token, we first instantiate a `CancellationTokenSource`:

```csharp
var cancelSource = new CancellationTokenSource();
```

This exposes a Token property which returns a `CancellationToken`. Hence, we could call our `Foo` method as follows:

```csharp
var cancelSource = new CancellationTokenSource();
Task foo = Foo(cancelSource.Token);
...
... (some time later)
cancelSource.Cancel();
```

Most asynchronous methods in the CLR support cancellation tokens, including `Delay`. If we modify `Foo` such that it passes its token into the `Delay` method, the task will end immediately upon request(rather than up to a second later):

```csharp
async Task Foo(CancellationToken cancellationToken)
{
    for (int i = 0; i < 10; i++)
    {
        Console.WriteLine(i);
        await Task.Delay(1000, cancellationToken);
    }
}
```

Notice that we no longer need to call `ThrowIfCancellationRequested` because `Task.Delay` is doing that for us. `Cancellation` tokens propagate nicely down the call stack(just as cancellation requests cascade up the call stack, by virtue of being exceptions).

Synchronous methods can support cancellation, too(such as `Task’s` `Wait` method). In such cases, the instruction to cancel will have to come asynchronously(e.g., from another task). For example:

```csharp
var cancelSource = new CancellationTokenSource();
Task.Delay(5000).ContinueWith(ant => cancelSource.Cancel());
...
```

In fact, from Framework 4.5, you can specify a time interval when constructing `CancellationTokenSource` to initiate cancellation after a set period of time(just as we demonstrated). It’s useful for implementing timeouts, whether synchronous or asynchronous:

```csharp
var cancelSource = new CancellationTokenSource(5000);
try { await Foo(cancelSource.Token); }
catch(OperationCanceledException ex) { Console.WriteLine("Cancelled"); }
```

The `CancellationToken` struct provides a `Register` method which lets you register a callback delegate that will be fired upon cancellation; it returns an object that can be disposed to undo the registration.

Tasks generated by the compiler’s asynchronous functions automatically enter a “Canceled” state upon an unhandled `OperationCanceledException`(`IsCanceled` returns `true` and `IsFaulted` returns `false`). The same goes for tasks created with Task.Run for which you pass the(same) `CancellationToken` to the constructor. The distinction between a faulted and a canceled task is unimportant in asynchronous scenarios, in that both throw an `OperationCanceledException` when awaited; it matters in advanced parallel programming scenarios(specifically conditional continuations).

### Progress Reporting

Sometimes you’ll want an asynchronous operation to report back progress as it’s running. A simple solution is to pass an `Action` delegate to the asynchronous method, which the method fires whenever progress changes:

```csharp
Task Foo(Action<int> onProgressPercentChanged)
{
    return Task.Run(() =>
    {
        for(int i = 0; i < 1000; i++)
        {
            if (i % 10 == 0)
                onProgressPercentChanged(i / 10);

            // Do something compute-bound...
        }
    });
}
```

Here’s how we could call it:

```csharp
Action<int> progress = i => Console.WriteLine(i + " %");
await Foo(progress);
```

While this works well in a Console application, it’s not ideal in rich-client scenarios because it reports progress from a worker thread, causing potential thread-safety issues for the consumer. (In effect, we’ve allowed a side effect of concurrency to “leak” to the outside world, which is unfortunate as the method is otherwise isolated if called from a UI thread.)

#### `IProgress<T>` and `Progress<T>`

The CLR provides a pair of types to solve this problem: an interface called `IProgress<T>` and a class that implements this interface called `Progress<T>`. Their purpose, in effect, is to “wrap” a delegate, so that UI applications can report progress safely through the synchronization context.

The interface defines just one method:

```csharp
public interface IProgress<in T>
{
    void Report(T value);
}
```

Using `IProgress<T>` is easy; our method hardly changes:

```csharp
Task Foo(IProgress<int> onProgressPercentChanged)
{
    return Task.Run(() =>
    {
        for(int i = 0; i < 1000; i++)
        {
            if (i % 10 == 0)
                onProgressPercentChanged.Report(i / 10);

            // Do something compute-bound...
        }
    });
}
```

The `Progress<T>` class has a constructor that accepts a delegate of type `Action<T>` that it wraps:

```csharp
var progress = new Progress<int>(i => Console.WriteLine(i + " %"));
await Foo(progress);
```

(`Progress<T>` also has a `ProgressChanged` event that you can subscribe to instead of [or in addition to] passing an action delegate to the constructor.) Upon instantiating `Progress<int>`, the class captures the synchronization context, if present. When Foo then calls `Report`, the delegate is invoked through that context.

Asynchronous methods can implement more elaborate progress reporting by replacing int with a custom type that exposes a range of properties.

_If you’re familiar with Reactive Framework, you’ll notice that `IProgress<T>` together with the task returned by the asynchronous function provide a feature set similar to `IObserver<T>`. The difference is that a task can expose a “final” return value in addition to(and differently typed to) the values emitted by `IProgress<T>`._

### The Task-based Asynchronous Pattern(TAP)

Framework 4.5 and later exposes hundreds of task-returning asynchronous methods that you can `await`(mainly related to I/O). Most of these methods(at least partly) follow a pattern called the _Task-based Asynchronous Pattern_(TAP), which is a sensible formalization of what we have described to date. A TAP method:

-   Returns a “hot”(running) `Task` or `Task<TResult>`
-   Has an “Async” suffix(except for special cases such as task combinators)
-   Is overloaded to accept a cancellation token and/or `IProgress<T>` if it supports cancellation and/or progress reporting
-   Returns quickly to the caller(has only a small initial _synchronous phase_)
-   Does not tie up a thread if I/O-bound

As we’ve seen, TAP methods are easy to write with C#’s asynchronous functions.

### Task Combinators

A nice consequence of there being a consistent protocol for asynchronous functions(whereby they consistently return tasks) is that it’s possible to use and write task combinators—functions that usefully combine tasks, without regard for what those specific tasks do.

The CLR includes two task combinators: `Task.WhenAny` and `Task.WhenAll`. In describing them, we’ll assume the following methods are defined:

```csharp
async Task<int> Delay1() { await Task.Delay(1000); return 1; }
async Task<int> Delay2() { await Task.Delay(2000); return 2; }
async Task<int> Delay3() { await Task.Delay(3000); return 3; }
```

#### WhenAny

`Task.WhenAny` returns a task that completes when any one of a set of tasks complete. The following completes in one second:

```csharp
Task<int> winningTask = await Task.WhenAny(Delay1(), Delay2(), Delay3());
Console.WriteLine("Done");
Console.WriteLine(winningTask.Result); // 1
```

Because `Task.WhenAny` itself returns a task, we await it, which returns the task that finished first. Our example is entirely nonblocking—including the last line when we access the `Result` property(because winningTask will already have finished). Nonetheless, it’s usually better to `await` the `winningTask`:

```csharp
Console.WriteLine(await winningTask); // 1
```

because any exceptions are then re-thrown without an `AggregateException` wrapping. In fact, we can perform both awaits in one step:

```csharp
int answer = await await Task.WhenAny(Delay1(), Delay2(), Delay3());
```

If a nonwinning task subsequently faults, the exception will go unobserved unless you subsequently await the task(or query its `Exception` property).

`WhenAny` is useful for applying timeouts or cancellation to operations that don’t otherwise support it:

```csharp
Task<string> task = SomeAsyncFunc();
Task winner = await (Task.WhenAny(task, Task.Delay(5000)));

if (winner != task)
    throw new TimeoutException();

string result = await task; // Unwrap result/rethrow
```

Notice that because in this case we’re calling WhenAny with differently typed tasks, the winner is reported as a plain Task(rather than a `Task<string>`).

#### WhenAll

`Task.WhenAll` returns a task that completes when all of the tasks that you pass to it complete. The following completes after three seconds(and demonstrates the fork/ join pattern):

```csharp
await Task.WhenAll(Delay1(), Delay2(), Delay3());
```

We could get a similar result by awaiting `task1`, `task2` and `task3` in turn rather than using `WhenAll`:

```csharp
Task task1 = Delay1(), task2 = Delay2(), task3 = Delay3();
await task1; await task2; await task3;
```

The difference(apart from it being less efficient by virtue of requiring three awaits rather than one), is that should `task1` fault, we’ll never get to await `task2`/`task3`, and any of their exceptions will go unobserved. In fact, this is why they relaxed the unobserved task exception behavior from CLR 4.5: it would be confusing if, despite an exception handling block around the entire preceding code block, an exception from `task2` or `task3` could crash your application sometime later when garbage collected.

In contrast, `Task.WhenAll` doesn’t complete until all tasks have completed—even when there’s a fault. And if there are multiple faults, their exceptions are combined into the task’s `AggregateException`(this is when `AggregateException` actually becomes useful—should you be interested in all the exceptions, that is). Awaiting the combined task, however, throws only the first exception, so to see all the exceptions you need to do this:

```csharp
Task task1 = Task.Run(() => { throw null; });
Task task2 = Task.Run(() => { throw null; });

Task all = Task.WhenAll(task1, task2);

try { await all; }
catch
{
    Console.WriteLine(all.Exception.InnerExceptions.Count); // 2
}
```

Calling `WhenAll` with tasks of type `Task<TResult>` returns a `Task<TResult[]>`, giving the combined results of all the tasks. This reduces to a `TResult[]` when awaited:

```csharp
Task<int> task1 = Task.Run(() => 1);
Task<int> task2 = Task.Run(() => 2);
int[] results = await Task.WhenAll(task1, task2); // { 1, 2 }
```

To give a practical example, the following downloads URIs in parallel and sums their total length:

```csharp
async Task<int> GetTotalSize(string[] uris)
{
    IEnumerable<Task<byte[]>> downloadTasks = uris.Select(uri => new WebClient().DownloadDataTaskAsync(uri));

    byte[][] contents = await Task.WhenAll(downloadTasks);

    return contents.Sum(c => c.Length);
}
```

There’s a slight inefficiency here, though, in that we’re unnecessarily hanging onto the byte arrays that we download until every task is complete. It would be more efficient if we collapsed byte arrays into their lengths right after downloading them. This is where an asynchronous lambda comes in handy, because we need to feed an `await` expression into LINQ’s Select query operator:

```csharp
async Task<int> GetTotalSize(string[] uris)
{
    IEnumerable<Task<int>> downloadTasks = uris.Select(async uri => (await new WebClient().DownloadDataTaskAsync(uri)).Length);

    int[] contentLengths = await Task.WhenAll(downloadTasks);

    return contentLengths.Sum();
}
```

#### Custom combinators

It can be useful to write your own task combinators. The simplest “combinator” accepts a single task, such as the following, which lets you await any task with a timeout:

```csharp
async static Task<TResult> WithTimeout<TResult>(this Task<TResult> task, TimeSpan timeout)
{
    Task winner = await (Task.WhenAny(task, Task.Delay(timeout)));

    if (winner != task)
        throw new TimeoutException();

    return await task; // Unwrap result/re-throw }
```

The following lets you “abandon” a task via a `CancellationToken`:

```csharp
static Task<TResult> WithCancellation<TResult>(this Task<TResult> task, CancellationToken cancelToken)
{
    var tcs = new TaskCompletionSource<TResult>();
    var reg = cancelToken.Register(() => tcs.TrySetCanceled());

    task.ContinueWith(ant => {
        reg.Dispose();
        if (ant.IsCanceled)
            tcs.TrySetCanceled();
        else if (ant.IsFaulted)
            tcs.TrySetException(ant.Exception.InnerException);
        else
            tcs.TrySetResult(ant.Result);
    });

    return tcs.Task;
}
```

Task combinators can be complex to write, sometimes requiring the use of signaling constructs. This is actually a good thing, because it keeps concurrency-related complexity out of your business logic and into reusable methods that can be tested in isolation.

The next combinator works like `WhenAll`, except that if any of the tasks fault, the resultant task faults immediately:

```csharp
async Task<TResult[]> WhenAllOrError<TResult>(params Task<TResult>[] tasks)
{
    var killJoy = new TaskCompletionSource<TResult[]>();

    foreach (var task in tasks)
        task.ContinueWith(ant => {
            if (ant.IsCanceled)
                killJoy.TrySetCanceled();
            else if (ant.IsFaulted)
                killJoy.TrySetException(ant.Exception.InnerException);
        });

    return await await Task.WhenAny(killJoy.Task, Task.WhenAll(tasks));
}
```

We start by creating a `TaskCompletionSource` whose sole job is to end the party if a task faults. Hence, we never call its `SetResult` method; only its `TrySetCanceled` and `TrySetException` methods. In this case, `ContinueWith` is more convenient than `GetAwaiter()`. `OnCompleted` because we’re not accessing the tasks’ results and wouldn’t want to bounce to a UI thread at that point.

---

**Anotações**

-   Concorrência significa várias tarefas que iniciam, executam e completam em períodos de sobreposição, sem uma ordem específica. O paralelismo é quando várias tarefas ou várias partes de uma tarefa única são executadas literalmente ao mesmo tempo, por exemplo, em um processador multi-core. Lembre-se de que a concorrência e o paralelismo NÃO são a mesma coisa.
-   Concorrência é a composição dos processos de execução independentes, enquanto o paralelismo é a execução simultânea de cálculos(possivelmente relacionados).
-   Concorrência é sobre lidar com muitas coisas ao mesmo tempo. Paralelismo é fazer muitas coisas ao mesmo tempo.
-   Um aplicativo pode ser concorrente - mas não paralelo, o que significa que ele processa mais de uma tarefa ao mesmo tempo, mas nenhuma tarefa está sendo executada no mesmo instante.
-   Um aplicativo pode ser paralelo - mas não concorrente, o que significa que ele processa várias subtarefas de uma tarefa em CPU multi-core ao mesmo tempo.
-   Um aplicativo não pode ser paralelo nem simultâneo, o que significa que ele processa todas as tarefas de uma vez por outra, sequencialmente.
-   Um aplicativo pode ser paralelo e simultâneo, o que significa que ele processa várias tarefas simultaneamente em CPU multi-core ao mesmo tempo.
