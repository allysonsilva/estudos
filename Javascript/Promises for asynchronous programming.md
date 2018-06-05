# Promises for asynchronous programming

## Visão geral(Overview)

Promises are an alternative to callbacks for delivering the results of an asynchronous computation. They require more effort from implementors of asynchronous functions, but provide several benefits for users of those functions.

A seguinte função retorna um resultado de forma assíncrona, através de uma Promise:

```js
function asyncFunc() {
    return new Promise(
        function (resolve, reject) {
            ···
            resolve(result);
            ···
            reject(error);
        }
    );
}
```

Você chama `asyncFunc()` da seguinte maneira:

```js
asyncFunc().then(result => { ··· }).catch(error => { ··· });
```

### Chaining then() calls

`then()` sempre retorna uma *Promise*, que permite encadear chamadas de método:

```js
asyncFunc1()
.then(result1 => {
    // Use result1
    return asyncFunction2(); // (A)
})
.then(result2 => { // (B)
    // Use result2
})
.catch(error => {
    // Handle errors of asyncFunc1() and asyncFunc2()
});
```

How the Promise `P` returned by `then()` is settled depends on what its callback does:

* If it returns a Promise (as in line A), the settlement of that Promise is forwarded to P. That’s why the callback from line B can pick up the settlement of `asyncFunction2`’s Promise.
* If it returns a different value, that value is used to settle P.
* If throws an exception then P is rejected with that exception.

Além disso, observe como `catch()` lida com os erros de duas chamadas de função assíncronas (`asyncFunction1()` e `asyncFunction2()`). Ou seja, os erros não detectados são capturados até que haja um manipulador de erro.

### Executing asynchronous functions in parallel

If you chain asynchronous function calls via `then()`, they are executed sequentially, one at a time:

```js
asyncFunc1().then(() => asyncFunc2());
```

If you don’t do that and call all of them immediately, they are basically executed in parallel (a *fork* in Unix process terminology):

```js
asyncFunc1();
asyncFunc2();
```

`Promise.all()` enables you to be notified once all results are in (a *join* in Unix process terminology). Its input is an Array of Promises, its output a single Promise that is fulfilled with an Array of the results.

```js
Promise.all([
    asyncFunc1(),
    asyncFunc2(),
])
.then(([result1, result2]) => {
    ···
})
.catch(err => {
    // Receives first rejection among the Promises
    ···
});
```

### Glossary: Promises

The Promise API is about delivering results asynchronously. A Promise object (short: Promise) is a stand-in for the result, which is delivered via that object.

States:

* A Promise is always in one of three mutually exclusive states:
    * Before the result is ready, the Promise is *pending*.
    * If a result is available, the Promise is *fulfilled*.
    * If an error happened, the Promise is *rejected*.
* A Promise is *settled* if “things are done” (if it is either *fulfilled* or *rejected*).
* A Promise is settled exactly once and then remains unchanged.

Reacting to state changes:

* Promise reactions are callbacks that you register with the Promise method `then()`, to be notified of a fulfillment or a rejection.
* A *thenable* is an object that has a Promise-style `then()` method. Whenever the API is only interested in being notified of settlements, it only demands thenables (e.g. the values returned from `then()` and `catch()`; or the values handed to `Promise.all()` and `Promise.race()`).

*Changing states:* There are two operations for changing the state of a Promise. After you have invoked either one of them once, further invocations have no effect.

* *Rejecting* a Promise means that the Promise becomes rejected.
* *Resolving* a Promise has different effects, depending on what value you are resolving with:
    * *Resolving* with a normal (non-thenable) value fulfills the Promise.
    * *Resolving* a Promise `P` with a *thenable* `T` means that `P` can’t be resolved anymore and will now follow T’s state, including its fulfillment or rejection value. The appropriate `P` reactions will get called once `T` settles (or are called immediately if `T` is already settled).

## Introduction: Promises

Promises are a pattern that helps with one particular kind of asynchronous programming: a function (or method) that returns a single result asynchronously. One popular way of receiving such a result is via a callback (“callbacks as continuations”):

```js
asyncFunction(arg1, arg2,
    result => {
        console.log(result);
    });
```

Promises provide a better way of working with callbacks: Now an asynchronous function returns a *Promise*, an object that serves as a placeholder and container for the final result. Callbacks registered via the Promise method `then()` are notified of the result:

```js
asyncFunction(arg1, arg2).then(result => {
    console.log(result);
});
```

Compared to callbacks as continuations, Promises have the following advantages:

* No inversion of control: similarly to synchronous code, Promise-based functions return results, they don’t (directly) continue – and control – execution via callbacks. That is, the caller stays in control.
* Chaining is simpler: If the callback of `then()` returns a Promise (e.g. the result of calling another Promise-based function) then `then()` returns that Promise (how this really works is more complicated and explained later). As a consequence, you can chain `then()` method calls:

```js
asyncFunction1(a, b).then(result1 => {
    console.log(result1);

    return asyncFunction2(x, y);
}).then(result2 => {
    console.log(result2);
});
```

* Composing asynchronous calls (loops, mapping, etc.): is a little easier, because you have data (Promise objects) you can work with.
* Error handling: As we shall see later, error handling is simpler with Promises, because, once again, there isn’t an inversion of control. Furthermore, both exceptions and asynchronous errors are managed the same way.
* Cleaner signatures: With callbacks, the parameters of a function are mixed; some are input for the function, others are responsible for delivering its output. With Promises, function signatures become cleaner; all parameters are input.
* Standardized: Prior to Promises, there were several incompatible ways of handling asynchronous results (Node.js callbacks, XMLHttpRequest, IndexedDB, etc.). With Promises, there is a clearly defined standard: *ECMAScript 6*. *ES6* follows the standard Promises. Since *ES6*, an increasing number of APIs is based on Promises.

## A first example

Vejamos um primeiro exemplo, para lhe dar uma ideia do que é trabalhar com Promises.

With Node.js-style callbacks, reading a file asynchronously looks like this:

```js
fs.readFile('config.json',
    function (error, text) {
        if (error) {
            console.error('Error while reading config file');
        } else {
            try {
                const obj = JSON.parse(text);
                console.log(JSON.stringify(obj, null, 4));
            } catch (e) {
                console.error('Invalid JSON in file');
            }
        }
    }
);
```

Com Promises, a mesma funcionalidade é usada assim:

```js
readFilePromisified('config.json')
.then(function (text) { // (A)
    const obj = JSON.parse(text);
    console.log(JSON.stringify(obj, null, 4));
})
.catch(function (error) { // (B)
    // File read error or JSON SyntaxError
    console.error('An error occurred', error);
});
```

There are still callbacks, but they are provided via methods that are invoked on the result (`then()` and `catch()`). The error callback in line `B` is convenient in two ways: First, it’s a single style of handling errors (versus if (`error`) and `try-catch` in the previous example). Second, you can handle the errors of both `readFilePromisified()` and the callback in line `A` from a single location.

## Three ways of understanding Promises

Vejamos três maneiras de entender Promises.

The following code contains a Promise-based function `asyncFunc()` and its invocation.

```js
function asyncFunc() {
    return new Promise((resolve, reject) => { // (A)
        setTimeout(() => resolve('DONE'), 100); // (B)
    });
}

asyncFunc().then(x => console.log('Result: '+x));

// Output:
// Result: DONE
```

`asyncFunc()` returns a Promise. Once the actual result `'DONE'` of the asynchronous computation is ready, it is delivered via `resolve()` (line B), which is a parameter of the callback that starts in line A.

So what is a Promise?

* Conceptually, invoking `asyncFunc()` is a blocking function call.
* A Promise is both a container for a value and an event emitter.

### Conceptually: calling a Promise-based function is blocking

The following code invokes `asyncFunc()` from the *async* function `main()`. *Async functions are a feature of ECMAScript 2017.*

```js
async function main() {
    const x = await asyncFunc();    // (A)
    console.log('Result: ' + x);    // (B)

    // Same as:
    // asyncFunc().then(x => console.log('Result: '+x));
}

main();
```

The body of `main()` expresses well what’s going on conceptually, how we usually think about asynchronous computations. Namely, `asyncFunc()` is a blocking function call:

* Line A: Wait until `asyncFunc()` is finished.
* Line B: Then log its result x.

Prior to *ECMAScript 6* and generators, you couldn’t suspend and resume code. That’s why, for Promises, you put everything that happens after the code is resumed into a callback. Invoking that callback is the same as resuming the code.

### A Promise is a container for an asynchronously delivered value

If a function returns a Promise then that Promise is like a blank into which the function will (usually) fill in its result, once it has computed it. You can simulate a simple version of this process via an Array:

```js
function asyncFunc() {
    const blank = [];

    setTimeout(() => blank.push('DONE'), 100);

    return blank;
}

const blank = asyncFunc();

// Wait until the value has been filled in

setTimeout(() => {
    const x = blank[0]; // (A)
    console.log('Result: ' + x);
}, 200);
```

With Promises, you don’t access the eventual value via `[0]` (as in line A), you use method `then()` and a callback.

### A Promise is an event emitter

Another way to view a Promise is as an object that *emits events*.

```js
function asyncFunc() {
    const eventEmitter = { success: [] };

    setTimeout(() => { // (A)
        for (const handler of eventEmitter.success) {
            handler('DONE');
        }
    }, 100);

    return eventEmitter;
}

asyncFunc().success.push(x => console.log('Result: '+x)); // (B)
```

Registering the event listener (line B) can be done after calling `asyncFunc()`, because the callback handed to `setTimeout()` (line A) is executed asynchronously (after this piece of code is finished).

Normal event emitters specialize in delivering multiple events, starting as soon as you register.

In contrast, Promises specialize in delivering exactly one value and come with built-in protection against registering too late: the result of a Promise is cached and passed to event listeners that are registered after the Promise was settled.

## Creating and using Promises

Let’s look at how Promises are operated from the producer and the consumer side.

### Producing a Promise

As a producer, you create a Promise and send a result via it:

```js
const p = new Promise(
    function (resolve, reject) { // (A)
        ···
        if (···) {
            resolve(value); // success
        } else {
            reject(reason); // failure
        }
    });
```

### The states of Promises

Once a result was delivered via a Promise, the Promise stays locked in to that result. That means each Promise is always in either one of three (mutually exclusive) states:

* *Pending*: the result hasn’t been computed, yet (the initial state of each Promise)
* *Fulfilled*: the result was computed successfully
* *Rejected*: a failure occurred during computation

A Promise is *settled* (the computation it represents has finished) if it is either fulfilled or rejected. A Promise can only be settled once and then stays settled. Subsequent attempts to settle have no effect.

![Promises simple states](./Images/Promises%20simple%20states.jpg "Promises simple states")

The parameter of `new Promise()` (starting in line A) is called an *executor*:

* *Resolving*: If the computation went well, the executor sends the result via `resolve()`. That usually fulfills the Promise `p`. But it may not – resolving with a Promise `q` leads to p tracking `q`: If `q` is still pending then so is p. However `q` is settled, `p` will be settled the same way.
* *Rejecting*: If an error happened, the executor notifies the Promise consumer via `reject()`. That always rejects the Promise.

If an exception is thrown inside the executor, `p` is rejected with that exception.

### Consuming a Promise

As a consumer of `promise`, you are notified of a fulfillment or a rejection via reactions – callbacks that you register with the methods `then()` and `catch():`

```js
promise
.then(value => { /* fulfillment */ })
.catch(error => { /* rejection */ });
```

What makes Promises so useful for asynchronous functions (with one-off results) is that once a Promise is settled, it doesn’t change anymore. Furthermore, there are never any race conditions, because it doesn’t matter whether you invoke `then()` or `catch()` before or after a Promise is settled:

* Reactions that are registered with a Promise before it is settled, are notified of the settlement once it happens.
* Reactions that are registered with a Promise after it is settled, receive the cached settled value “immediately” (their invocations are queued as tasks).

Note that `catch()` is simply a more convenient (and recommended) alternative to calling `then()`. That is, the following two invocations are equivalent:

```js
promise.then(
    null,
    error => { /* rejection */ });
```

```js
promise.catch(
    error => { /* rejection */ });
```

## Examples

### Example: promisifying `fs.readFile()`

The following code is a Promise-based version of the built-in Node.js function `fs.readFile()`.

```js
import {readFile} from 'fs';

function readFilePromisified(filename) {
    return new Promise(
        function (resolve, reject) {
            readFile(filename, { encoding: 'utf8' },
                (error, data) => {
                    if (error) {
                        reject(error);
                    } else {
                        resolve(data);
                    }
                });
        });
}
```

`readFilePromisified()` is used like this:

```js
readFilePromisified(process.argv[2])
.then(text => {
    console.log(text);
})
.catch(error => {
    console.log(error);
});
```

### Example: promisifying `XMLHttpRequest`

The following is a Promise-based function that performs an HTTP GET via the event-based [XMLHttpRequest](https://xhr.spec.whatwg.org/) API:

```js
function httpGet(url) {
    return new Promise(
        function (resolve, reject) {
            const request = new XMLHttpRequest();
            request.onload = function () {
                if (this.status === 200) {
                    // Success
                    resolve(this.response);
                } else {
                    // Something went wrong (404 etc.)
                    reject(new Error(this.statusText));
                }
            };

            request.onerror = function () {
                reject(new Error('XMLHttpRequest Error: '+this.statusText));
            };

            request.open('GET', url);
            request.send();
        });
}
```

This is how you use `httpGet()`:

```js
httpGet('http://example.com/file.txt')
.then(function (value) {
        console.log('Contents: ' + value);
    },
    function (reason) {
        console.error('Something went wrong', reason);
    }
);
```

### Example: delaying an activity

Let’s implement `setTimeout()` as the Promise-based function `delay()` (similar to Q.delay()).

```js
function delay(ms) {
    return new Promise(function (resolve, reject) {
        setTimeout(resolve, ms); // (A)
    });
}

// Using delay():
delay(5000).then(function () { // (B)
    console.log('5 seconds have passed!')
});
```

Note that in line A, we are calling `resolve` with zero parameters, which is the same as calling `resolve(undefined)`. We don’t need the fulfillment value in line B, either and simply ignore it. Just being notified is enough here.

### Example: timing out a Promise

```js
function timeout(ms, promise) {
    return new Promise(function (resolve, reject) {
        promise.then(resolve);
        setTimeout(function () {
            reject(new Error('Timeout after '+ ms +' ms')); // (A)
        }, ms);
    });
}
```

Note that the rejection after the timeout (in line A) does not cancel the request, but it does prevent the Promise being fulfilled with its result.

Using `timeout()` looks like this:

```js
timeout(5000, httpGet('http://example.com/file.txt'))
.then(function (value) {
    console.log('Contents: ' + value);
})
.catch(function (reason) {
    console.error('Error or timeout', reason);
});
```
