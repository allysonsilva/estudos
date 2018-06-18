# Metaprogramming with proxies

----------

## Overview

Proxies enable you to intercept and customize operations performed on objects (such as getting properties). They are a _metaprogramming_ feature.

In the following example, `proxy` is the object whose operations we are intercepting and `handler` is the object that handles the interceptions. In this case, we are only intercepting a single operation, `get` (getting properties).

```js
const target = {};

const handler = {
    get(target, propKey, receiver) {
        console.log('get ' + propKey);
        return 123;
    }
};

const proxy = new Proxy(target, handler);
```

When we get the property `proxy.foo`, the handler intercepts that operation:

```js
proxy.foo
// get foo
// 123
```

## Programming versus metaprogramming

Before we can get into what proxies are and why they are useful, we first need to understand what _metaprogramming_ is.

In programming, there are levels:

- At the _base level_ (also called: _application level_), code processes user input.
- At the _meta level_, code processes base level code.

Base and meta level can be different languages. In the following meta program, the metaprogramming language is JavaScript and the base programming language is Java.

```js
const str = 'Hello' + '!'.repeat(3);
console.log('System.out.println("'+str+'")');
```

Metaprogramming can take different forms. In the previous example, we have printed Java code to the console. Let’s use JavaScript as both metaprogramming language and base programming language. The classic example for this is the `eval()` function, which lets you evaluate/compile JavaScript code on the fly. There are not that many actual use cases for `eval()`. In the interaction below, we use it to evaluate the expression `5 + 2`.

```js
eval('5 + 2')
// 7
```

Other JavaScript operations may not look like metaprogramming, but actually are, if you look closer:

```js
// Base level
const obj = {
    hello() {
        console.log('Hello!');
    }
};

// Meta level
for (const key of Object.keys(obj)) {
    console.log(key);
}
```

The program is examining its own structure while running. This doesn’t look like metaprogramming, because the separation between programming constructs and data structures is fuzzy in JavaScript. All of the `Object.*` methods can be considered metaprogramming functionality.

### Kinds of metaprogramming

Reflective metaprogramming means that a program processes itself. Kiczales et al. Distinguish three kinds of reflective metaprogramming:

- **Introspection:** you have read-only access to the structure of a program.
- **Self-modification:** you can change that structure.
- **Intercession:** you can redefine the semantics of some language operations.

Let’s look at examples.

**Example: introspection.** `Object.keys()` performs introspection (see previous example).

**Example: self-modification.** The following function `moveProperty` moves a property from a source to a target. It performs self-modification via the bracket operator for property access, the assignment operator and the `delete` operator. (In production code, you’d probably use [property descriptors] for this task.)

```js
function moveProperty(source, propertyName, target) {
    target[propertyName] = source[propertyName];
    delete source[propertyName];
}
```

Using `moveProperty()`:

```js
const obj1 = { prop: 'abc' };
const obj2 = {};

moveProperty(obj1, 'prop', obj2);

obj1
// {}
obj2
// { prop: 'abc' }
```

ECMAScript 5 doesn’t support intercession; proxies were created to fill that gap.

## Proxies explained

ECMAScript 6 proxies bring intercession to JavaScript. They work as follows. There are many operations that you can perform on an object `obj`. For example:

- Getting the property `prop` of an object `obj` (`obj.prop`)
- Checking whether an object `obj` has a property `prop` (`'prop' in obj`)

Proxies are special objects that allow you customize some of these operations. A proxy is created with two parameters:

- `handler`: For each operation, there is a corresponding handler method that – if present – performs that operation. Such a method _intercepts_ the operation (on its way to the target) and is called a _trap_ (a term borrowed from the domain of operating systems).
- `target`: If the handler doesn’t intercept an operation then it is performed on the target. That is, it acts as a fallback for the handler. In a way, the proxy wraps the target.

In the following example, the handler intercepts the operations `get` and `has`.

```js
const target = {};

const handler = {
    /** Intercepts: getting properties */
    get(target, propKey, receiver) {
        console.log(`GET ${propKey}`);
        return 123;
    },

    /** Intercepts: checking whether properties exist */
    has(target, propKey) {
        console.log(`HAS ${propKey}`);
        return true;
    }
};

const proxy = new Proxy(target, handler);
```

When we get property `foo`, the handler intercepts that operation:

```js
proxy.foo
// GET foo
// 123
```

Similarly, the `in` operator triggers `has`:

```js
'hello' in proxy
// HAS hello
// true
```

The handler doesn’t implement the trap `set` (setting properties). Therefore, setting `proxy.bar` is forwarded to `target` and leads to `target.bar` being set.

```js
proxy.bar = 'abc';
// target.bar
// 'abc'
```

### Function-specific traps

If the target is a function, two additional operations can be intercepted:

- `apply`: Making a function call, triggered via
    - `proxy(···)`
    - `proxy.call(···)`
    - `proxy.apply(···)`
- `construct`: Making a constructor call, triggered via
    - `new proxy(···)`

The reason for only enabling these traps for function targets is simple: You wouldn’t be able to forward the operations `apply` and `construct`, otherwise.

### Intercepting method calls

If you want to intercept method calls via a proxy, there is one challenge: you can intercept the operation `get` (getting property values) and you can intercept the operation `apply` (calling a function), but there is no single operation for method calls that you could intercept. That’s because method calls are viewed as two separate operations: First a `get` to retrieve a function, then an `apply` to call that function.

Therefore, you must intercept `get` and return a function that intercepts the function call. The following code demonstrates how that is done.

```js
function traceMethodCalls(obj) {
    const handler = {
        get(target, propKey, receiver) {
            const origMethod = target[propKey];
            return function (...args) {
                const result = origMethod.apply(this, args);
                console.log(propKey + JSON.stringify(args)
                    + ' -> ' + JSON.stringify(result));
                return result;
            };
        }
    };
    return new Proxy(obj, handler);
}
```

I’m not using a Proxy for the latter task, I’m simply wrapping the original method with a function.

Let’s use the following object to try out `traceMethodCalls()`:

```js
const obj = {
    multiply(x, y) {
        return x * y;
    },
    squared(x) {
        return this.multiply(x, x);
    },
};
```

`tracedObj` is a traced version of `obj`. The first line after each method call is the output of `console.log()`, the second line is the result of the method call.

```js
const tracedObj = traceMethodCalls(obj);
tracedObj.multiply(2,7)
multiply[2,7] -> 14
// 14
tracedObj.squared(9)
multiply[9,9] -> 81
squared[9] -> 81
// 81
```

The nice thing is that even the call `this.multiply()` that is made inside `obj.squared()` is traced. That’s because `this` keeps referring to the proxy.

This is not the most efficient solution. One could, for example, cache methods. Furthermore, Proxies themselves have an impact on performance.

### Revocable proxies

ECMAScript 6 lets you create proxies that can be _revoked_ (switched off):

```js
const {proxy, revoke} = Proxy.revocable(target, handler);
```

On the left hand side of the assignment operator (`=`), we are using destructuring to access the properties `proxy` and `revoke` of the object returned by `Proxy.revocable()`.

After you call the function `revoke` for the first time, any operation you apply to `proxy`causes a `TypeError`. Subsequent calls of `revoke` have no further effect.

```js
const target = {}; // Start with an empty object
const handler = {}; // Don’t intercept anything
const {proxy, revoke} = Proxy.revocable(target, handler);

proxy.foo = 123;
console.log(proxy.foo); // 123

revoke();

console.log(proxy.foo); // TypeError: Revoked
```

### Proxies as prototypes

A proxy `proto` can become the prototype of an object `obj`. Some operations that begin in `obj` may continue in `proto`. One such operation is `get`.

```js
const proto = new Proxy({}, {
    get(target, propertyKey, receiver) {
        console.log('GET '+propertyKey);
        return target[propertyKey];
    }
});

const obj = Object.create(proto);
obj.bla;

// Output:
// GET bla
```

The property `bla` can’t be found in `obj`, which is why the search continues in `proto`and the trap `get` is triggered there. There are more operations that affect prototypes; they are listed at the end of this chapter.

### Forwarding intercepted operations

Operations whose traps the handler doesn’t implement are automatically forwarded to the target. Sometimes there is some task you want to perform in addition to forwarding the operation. For example, a handler that intercepts all operations and logs them, but doesn’t prevent them from reaching the target:

```js
const handler = {
    deleteProperty(target, propKey) {
        console.log('DELETE ' + propKey);
        return delete target[propKey];
    },
    has(target, propKey) {
        console.log('HAS ' + propKey);
        return propKey in target;
    },
    // Other traps: similar
}
```

For each trap, we first log the name of the operation and then forward it by performing it manually. ECMAScript 6 has the module-like object `Reflect` that helps with forwarding: for each trap

```js
handler.trap(target, arg_1, ···, arg_n)
```

`Reflect` has a method

```js
Reflect.trap(target, arg_1, ···, arg_n)
```

If we use `Reflect`, the previous example looks as follows.

```js
const handler = {
    deleteProperty(target, propKey) {
        console.log('DELETE ' + propKey);
        return Reflect.deleteProperty(target, propKey);
    },
    has(target, propKey) {
        console.log('HAS ' + propKey);
        return Reflect.has(target, propKey);
    },
    // Other traps: similar
}
```

Now what each of the traps does is so similar that we can implement the handler via a proxy:

```js
const handler = new Proxy({}, {
    get(target, trapName, receiver) {
        // Return the handler method named trapName
        return function (...args) {
            // Don’t log args[0]
            console.log(trapName.toUpperCase()+' '+args.slice(1));
            // Forward the operation
            return Reflect[trapName](...args);
        }
    }
});
```

For each trap, the proxy asks for a handler method via the `get` operation and we give it one. That is, all of the handler methods can be implemented via the single meta method `get`. It was one of the goals for the proxy API to make this kind of virtualization simple.

Let’s use this proxy-based handler:

```js
const target = {};
const proxy = new Proxy(target, handler);
proxy.foo = 123;
// SET foo,123,[object Object]
proxy.foo
// GET foo,[object Object]
// 123
```

The following interaction confirms that the `set` operation was correctly forwarded to the target:

```js
target.foo
// 123
```

### Pitfall: not all objects can be wrapped transparently by proxies

A proxy object can be seen as intercepting operations performed on its target object – the proxy wraps the target. The proxy’s handler object is like an observer or listener for the proxy. It specifies which operations should be intercepted by implementing corresponding methods (`get` for reading a property, etc.). If the handler method for an operation is missing then that operation is not intercepted. It is simply forwarded to the target.

Therefore, if the handler is the empty object, the proxy should transparently wrap the target. Alas, that doesn’t always work.

#### Wrapping an object affects `this`

Before we dig deeper, let’s quickly review how wrapping a target affects `this`:

```js
const target = {
    foo() {
        return {
            thisIsTarget: this === target,
            thisIsProxy: this === proxy,
        };
    }
};

const handler = {};
const proxy = new Proxy(target, handler);
```

If you call `target.foo()` directly, `this` points to `target`:

```js
target.foo()
// { thisIsTarget: true, thisIsProxy: false }
```

If you invoke that method via the proxy, `this` points to `proxy`:

```js
proxy.foo()
// { thisIsTarget: false, thisIsProxy: true }
```

That’s done so that the proxy continues to be in the loop if, e.g., the target invokes methods on `this`.

#### Objects that can’t be wrapped transparently

Normally, proxies with an empty handler wrap targets transparently: you don’t notice that they are there and they don’t change the behavior of the targets.

If, however, a target associates information with `this` via a mechanism that is not controlled by proxies, you have a problem: things fail, because different information is associated depending on whether the target is wrapped or not.

For example, the following class `Person` stores private information in the WeakMap `_name`:

```js
const _name = new WeakMap();

class Person {
    constructor(name) {
        _name.set(this, name);
    }
    get name() {
        return _name.get(this);
    }
}
```

Instances of `Person` can’t be wrapped transparently:

```js
const jane = new Person('Jane');
jane.name
// 'Jane'

const proxy = new Proxy(jane, {});
proxy.name
// undefined
```

`jane.name` is different from the wrapped `proxy.name`. The following implementation does not have this problem:

```js
class Person2 {
    constructor(name) {
        this._name = name;
    }
    get name() {
        return this._name;
    }
}

const jane = new Person2('Jane');
console.log(jane.name); // Jane

const proxy = new Proxy(jane, {});
console.log(proxy.name); // Jane
```

#### Wrapping instances of built-in constructors

Instances of most built-in constructors also have a mechanism that is not intercepted by proxies. They therefore can’t be wrapped transparently, either. I’ll demonstrate the problem for an instance of `Date`:

```js
const target = new Date();
const handler = {};
const proxy = new Proxy(target, handler);

proxy.getDate(); // TypeError: this is not a Date object.
```

The mechanism that is unaffected by proxies is called _internal slots_. These slots are property-like storage associated with instances. The specification handles these slots as if they were properties with names in square brackets. For example, the following method is internal and can be invoked on all objects `O`:

```js
O.[[GetPrototypeOf]]()
```

However, access to internal slots does not happen via normal “get” and “set” operations. If `getDate()` is invoked via a proxy, it can’t find the internal slot it needs on `this` and complains via a `TypeError`.

For `Date` methods, [the language specification states](http://www.ecma-international.org/ecma-262/6.0/#sec-properties-of-the-number-prototype-object):

> Unless explicitly stated otherwise, the methods of the Number prototype object defined below are not generic and the `this` value passed to them must be either a Number value or an object that has a `[[NumberData]]` internal slot that has been initialized to a Number value.

#### Arrays can be wrapped transparently

In contrast to other built-ins, Arrays can be wrapped transparently:

```js
const p = new Proxy(new Array(), {});
p.push('a');
p.length
// 1
p.length = 0;
p.length
// 0
```

The reason for Arrays being wrappable is that, even though property access is customized to make `length` work, Array methods don’t rely on internal slots – they are generic.

#### A work-around

As a work-around, you can change how the handler forwards method calls and selectively set `this` to the target and not the proxy:

```js
const handler = {
    get(target, propKey, receiver) {
        if (propKey === 'getDate') {
            return target.getDate.bind(target);
        }
        return Reflect.get(target, propKey, receiver);
    },
};

const proxy = new Proxy(new Date('2020-12-24'), handler);
proxy.getDate(); // 24
```

The drawback of this approach is that none of the operations that the method performs on `this` go through the proxy.

**Acknowlegement:** Thanks to Allen Wirfs-Brock for pointing out the pitfall explained in this section.

## Use cases for proxies

This section demonstrates what proxies can be used for. That will give you the opportunity to see the API in action.

### Tracing property accesses (`get`, `set`)

Let’s assume we have a function `tracePropAccess(obj, propKeys)` that logs whenever a property of `obj`, whose key is in the Array `propKeys`, is set or got. In the following code, we apply that function to an instance of the class `Point`:

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return `Point(${this.x}, ${this.y})`;
    }
}

// Trace accesses to properties `x` and `y`
const p = new Point(5, 7);
p = tracePropAccess(p, ['x', 'y']);
```

Getting and setting properties of the traced object `p` has the following effects:

```js
p.x
// GET x
// 5
p.x = 21
// SET x=21
// 21
```

Intriguingly, tracing also works whenever `Point` accesses the properties, because `this` now refers to the traced object, not to an instance of `Point`.

```js
p.toString()
// GET x
// GET y
// 'Point(21, 7)'
```

In ECMAScript 5, you’d implement `tracePropAccess()` as follows. We replace each property with a getter and a setter that traces accesses. The setters and getters use an extra object, `propData`, to store the data of the properties. Note that we are destructively changing the original implementation, which means that we are metaprogramming.

```js
function tracePropAccess(obj, propKeys) {
    // Store the property data here
    const propData = Object.create(null);
    // Replace each property with a getter and a setter
    propKeys.forEach(function (propKey) {
        propData[propKey] = obj[propKey];
        Object.defineProperty(obj, propKey, {
            get: function () {
                console.log('GET '+propKey);
                return propData[propKey];
            },
            set: function (value) {
                console.log('SET '+propKey+'='+value);
                propData[propKey] = value;
            },
        });
    });
    return obj;
}
```

In ECMAScript 6, we can use a simpler, proxy-based solution. We intercept property getting and setting and don’t have to change the implementation.

```js
function tracePropAccess(obj, propKeys) {
    const propKeySet = new Set(propKeys);
    return new Proxy(obj, {
        get(target, propKey, receiver) {
            if (propKeySet.has(propKey)) {
                console.log('GET '+propKey);
            }
            return Reflect.get(target, propKey, receiver);
        },
        set(target, propKey, value, receiver) {
            if (propKeySet.has(propKey)) {
                console.log('SET '+propKey+'='+value);
            }
            return Reflect.set(target, propKey, value, receiver);
        },
    });
}
```

### Warning about unknown properties (`get`, `set`)

When it comes to accessing properties, JavaScript is very forgiving. For example, if you try to read a property and misspell its name, you don’t get an exception, you get the result `undefined`. You can use proxies to get an exception in such a case. This works as follows. We make the proxy a prototype of an object.

If a property isn’t found in the object, the `get` trap of the proxy is triggered. If the property doesn’t even exist in the prototype chain after the proxy, it really is missing and we throw an exception. Otherwise, we return the value of the inherited property. We do so by forwarding the `get` operation to the target (the prototype of the target is also the prototype of the proxy).

```js
const PropertyChecker = new Proxy({}, {
    get(target, propKey, receiver) {
        if (!(propKey in target)) {
            throw new ReferenceError('Unknown property: '+propKey);
        }
        return Reflect.get(target, propKey, receiver);
    }
});
```

Let’s use `PropertyChecker` for an object that we create:

```js
const obj = { __proto__: PropertyChecker, foo: 123 };
obj.foo  // own
// 123
obj.fo
// ReferenceError: Unknown property: fo
obj.toString()  // inherited
// '[object Object]'
```

If we turn `PropertyChecker` into a constructor, we can use it for ECMAScript 6 classes via `extends`:

```js
function PropertyChecker() { }
PropertyChecker.prototype = new Proxy(···);

class Point extends PropertyChecker {
    constructor(x, y) {
        super();
        this.x = x;
        this.y = y;
    }
}

const p = new Point(5, 7);
console.log(p.x); // 5
console.log(p.z); // ReferenceError
```

If you are worried about accidentally _creating_ properties, you have two options: You can either wrap a proxy around objects that traps `set`. Or you can make an object `obj` non-extensible via [`Object.preventExtensions(obj)`], which means that JavaScript doesn’t let you add new (own) properties to `obj`.

### Negative Array indices (`get`)

Some Array methods let you refer to the last element via `-1`, to the second-to-last element via `-2`, etc. For example:

```js
['a', 'b', 'c'].slice(-1)
// [ 'c' ]
```

Alas, that doesn’t work when accessing elements via the bracket operator (`[]`). We can, however, use proxies to add that capability. The following function `createArray()` creates Arrays that support negative indices. It does so by wrapping proxies around Array instances. The proxies intercept the `get` operation that is triggered by the bracket operator.

```js
function createArray(...elements) {
    const handler = {
        get(target, propKey, receiver) {
            // Sloppy way of checking for negative indices
            const index = Number(propKey);
            if (index < 0) {
                propKey = String(target.length + index);
            }
            return Reflect.get(target, propKey, receiver);
        }
    };
    // Wrap a proxy around an Array
    const target = [];
    target.push(...elements);
    return new Proxy(target, handler);
}

const arr = createArray('a', 'b', 'c');
console.log(arr[-1]); // c
```

Acknowledgement: The idea for this example comes from a [blog post](http://h3manth.com/new/blog/2013/negative-array-index-in-javascript/) by hemanth.hm.

### Data binding (`set`)

Data binding is about syncing data between objects. One popular use case are widgets based on the MVC (Model View Controler) pattern: With data binding, the _view_ (the widget) stays up-to-date if you change the _model_ (the data visualized by the widget).

To implement data binding, you have to observe and react to changes made to an object. In the following code snippet, I sketch how observing changes could work for an Array.

```js
function createObservedArray(callback) {
    const array = [];
    return new Proxy(array, {
        set(target, propertyKey, value, receiver) {
            callback(propertyKey, value);
            return Reflect.set(target, propertyKey, value, receiver);
        }
    });
}
const observedArray = createObservedArray((key, value) => console.log(`${key}=${value}`));
observedArray.push('a');
```

Output:

```
0=a
length=1
```

### Accessing a restful web service (method calls)

A proxy can be used to create an object on which arbitrary methods can be invoked. In the following example, the function `createWebService` creates one such object, `service`. Invoking a method on `service` retrieves the contents of the web service resource with the same name. Retrieval is handled via an ECMAScript 6 Promise.

```js
const service = createWebService('http://example.com/data');
// Read JSON data in http://example.com/data/employees
service.employees().then(json => {
    const employees = JSON.parse(json);
    ···
});
```

The following code is a quick and dirty implementation of `createWebService` in ECMAScript 5. Because we don’t have proxies, we need to know beforehand what methods will be invoked on `service`. The parameter `propKeys` provides us with that information, it holds an Array with method names.

```js
function createWebService(baseUrl, propKeys) {
    const service = {};
    propKeys.forEach(function (propKey) {
        service[propKey] = function () {
            return httpGet(baseUrl+'/'+propKey);
        };
    });
    return service;
}
```

The ECMAScript 6 implementation of `createWebService` can use proxies and is simpler:

```js
function createWebService(baseUrl) {
    return new Proxy({}, {
        get(target, propKey, receiver) {
            // Return the method to be called
            return () => httpGet(baseUrl+'/'+propKey);
        }
    });
}
```

Both implementations use the following function to make HTTP GET requests.

```js
function httpGet(url) {
    return new Promise(
        (resolve, reject) => {
            const request = new XMLHttpRequest();
            Object.assign(request, {
                onload() {
                    if (this.status === 200) {
                        // Success
                        resolve(this.response);
                    } else {
                        // Something went wrong (404 etc.)
                        reject(new Error(this.statusText));
                    }
                },
                onerror() {
                    reject(new Error(
                        'XMLHttpRequest Error: '+this.statusText));
                }
            });
            request.open('GET', url);
            request.send();
        });
}
```

### Revocable references

_Revocable references_ work as follows: A client is not allowed to access an important resource (an object) directly, only via a reference (an intermediate object, a wrapper around the resource). Normally, every operation applied to the reference is forwarded to the resource. After the client is done, the resource is protected by _revoking_ the reference, by switching it off. Henceforth, applying operations to the reference throws exceptions and nothing is forwarded, anymore.

In the following example, we create a revocable reference for a resource. We then read one of the resource’s properties via the reference. That works, because the reference grants us access. Next, we revoke the reference. Now the reference doesn’t let us read the property, anymore.

```js
const resource = { x: 11, y: 8 };
const {reference, revoke} = createRevocableReference(resource);

// Access granted
console.log(reference.x); // 11

revoke();

// Access denied
console.log(reference.x); // TypeError: Revoked
```

Proxies are ideally suited for implementing revocable references, because they can intercept and forward operations. This is a simple proxy-based implementation of `createRevocableReference`:

```js
function createRevocableReference(target) {
    let enabled = true;
    return {
        reference: new Proxy(target, {
            get(target, propKey, receiver) {
                if (!enabled) {
                    throw new TypeError('Revoked');
                }
                return Reflect.get(target, propKey, receiver);
            },
            has(target, propKey) {
                if (!enabled) {
                    throw new TypeError('Revoked');
                }
                return Reflect.has(target, propKey);
            },
            ···
        }),
        revoke() {
            enabled = false;
        },
    };
}
```

The code can be simplified via the proxy-as-handler technique from the previous section. This time, the handler basically is the `Reflect` object. Thus, the `get` trap normally returns the appropriate `Reflect` method. If the reference has been revoked, a `TypeError` is thrown, instead.

```js
function createRevocableReference(target) {
    let enabled = true;
    const handler = new Proxy({}, {
        get(dummyTarget, trapName, receiver) {
            if (!enabled) {
                throw new TypeError('Revoked');
            }
            return Reflect[trapName];
        }
    });
    return {
        reference: new Proxy(target, handler),
        revoke() {
            enabled = false;
        },
    };
}
```

However, you don’t have to implement revocable references yourself, because ECMAScript 6 lets you create proxies that can be revoked. This time, the revoking happens in the proxy, not in the handler. All the handler has to do is forward every operation to the target. As we have seen that happens automatically if the handler doesn’t implement any traps.

```js
function createRevocableReference(target) {
    const handler = {}; // forward everything
    const { proxy, revoke } = Proxy.revocable(target, handler);
    return { reference: proxy, revoke };
}
```

#### Membranes

_Membranes_ build on the idea of revocable references: Environments that are designed to run untrusted code, wrap a membrane around that code to isolate it and keep the rest of the system safe. Objects pass the membrane in two directions:

- The code may receive objects (“dry objects”) from the outside.
- Or it may hand objects (“wet objects”) to the outside.

In both cases, revocable references are wrapped around the objects. Objects returned by wrapped functions or methods are also wrapped. Additionally, if a wrapped wet object is passed back into a membrane, it is unwrapped.

Once the untrusted code is done, all of the revocable references are revoked. As a result, none of its code on the outside can be executed anymore and outside objects that it has cease to work, as well. The [Caja Compiler](https://developers.google.com/caja/) is “a tool for making third party HTML, CSS and JavaScript safe to embed in your website”. It uses membranes to achieve this task.

### Implementing the DOM in JavaScript

The browser Document Object Model (DOM) is usually implemented as a mix of JavaScript and C++. Implementing it in pure JavaScript is useful for:

- Emulating a browser environment, e.g. to manipulate HTML in Node.js. [jsdom](https://github.com/tmpvar/jsdom) is one library that does that.
- Speeding the DOM up (switching between JavaScript and C++ costs time).

Alas, the standard DOM can do things that are not easy to replicate in JavaScript. For example, most DOM collections are live views on the current state of the DOM that change dynamically whenever the DOM changes. As a result, pure JavaScript implementations of the DOM are not very efficient. One of the reasons for adding proxies to JavaScript was to help write more efficient DOM implementations.

### Other use cases

There are more use cases for proxies. For example:

- Remoting: Local placeholder objects forward method invocations to remote objects. This use case is similar to the web service example.
- Data access objects for databases: Reading and writing to the object reads and writes to the database. This use case is similar to the web service example.
- Profiling: Intercept method invocations to track how much time is spent in each method. This use case is similar to the tracing example.
- Type checking: Nicholas Zakas has used proxies to [type-check objects](http://www.nczonline.net/blog/2014/04/29/creating-type-safe-properties-with-ecmascript-6-proxies/).

## FAQ: proxies

### Where is the `enumerate` trap?

ES6 originally had a trap `enumerate` that was triggered by `for-in` loops. But it was recently removed, to simplify proxies. `Reflect.enumerate()` was removed, as well. ([Source: TC39 notes](https://github.com/tc39/tc39-notes/blob/master/es7/2016-01/2016-01-28.md))

## Reference: the proxy API

This section serves as a quick reference for the proxy API: the global objects `Proxy`and `Reflect`.

### Creating proxies

There are two ways to create proxies:

- `const proxy = new Proxy(target, handler)`

    Creates a new proxy object with the given target and the given handler.

- `const {proxy, revoke} = Proxy.revocable(target, handler)`

    Creates a proxy that can be revoked via the function `revoke`. `revoke` can be called multiple times, but only the first call has an effect and switches `proxy` off. Afterwards, any operation performed on `proxy` leads to a `TypeError` being thrown.

### Handler methods

This subsection explains what traps can be implemented by handlers and what operations trigger them. Several traps return boolean values. For the traps `has` and `isExtensible`, the boolean is the result of the operation. For all other traps, the boolean indicates whether the operation succeeded or not.

Traps for all objects:

- `defineProperty(target, propKey, propDesc) : boolean`
    - `Object.defineProperty(proxy, propKey, propDesc)`
- `deleteProperty(target, propKey) : boolean`
    - `delete proxy[propKey]`
    - `delete proxy.foo // propKey = 'foo'`
- `get(target, propKey, receiver) : any`
    - `receiver[propKey]`
    - `receiver.foo // propKey = 'foo'`
- `getOwnPropertyDescriptor(target, propKey) : PropDesc|Undefined`
    - `Object.getOwnPropertyDescriptor(proxy, propKey)`
- `getPrototypeOf(target) : Object|Null`
    - `Object.getPrototypeOf(proxy)`
- `has(target, propKey) : boolean`
    - `propKey in proxy`
- `isExtensible(target) : boolean`
    - `Object.isExtensible(proxy)`
- `ownKeys(target) : Array<PropertyKey>`
    - `Object.getOwnPropertyPropertyNames(proxy)` (only uses string keys)
    - `Object.getOwnPropertyPropertySymbols(proxy)` (only uses symbol keys)
    - `Object.keys(proxy)` (only uses enumerable string keys; enumerability is checked via `Object.getOwnPropertyDescriptor`)
- `preventExtensions(target) : boolean`
    - `Object.preventExtensions(proxy)`
- `set(target, propKey, value, receiver) : boolean`
    - `receiver[propKey] = value`
    - `receiver.foo = value // propKey = 'foo'`
- `setPrototypeOf(target, proto) : boolean`
    - `Object.setPrototypeOf(proxy, proto)`

Traps for functions (available if target is a function):

- `apply(target, thisArgument, argumentsList) : any`
    - `proxy.apply(thisArgument, argumentsList)`
    - `proxy.call(thisArgument, ...argumentsList)`
    - `proxy(...argumentsList)`
- `construct(target, argumentsList, newTarget) : Object`
    - `new proxy(..argumentsList)`

#### Fundamental operations versus derived operations

The following operations are _fundamental_, they don’t use other operations to do their work: `apply`, `defineProperty`, `deleteProperty`, `getOwnPropertyDescriptor`, `getPrototypeOf`, `isExtensible`, `ownKeys`, `preventExtensions`, `setPrototypeOf`

All other operations are _derived_, they can be implemented via fundamental operations. For example, for data properties, `get` can be implemented by iterating over the prototype chain via `getPrototypeOf` and calling `getOwnPropertyDescriptor`for each chain member until either an own property is found or the chain ends.

### Invariants of handler methods

Invariants are safety constraints for handlers. This subsection documents what invariants are enforced by the proxy API and how. Whenever you read “the handler must do X” below, it means that a `TypeError` is thrown if it doesn’t. Some invariants restrict return values, others restrict parameters. The correctness of a trap’s return value is ensured in two ways: Normally, an illegal value means that a `TypeError` is thrown. But whenever a boolean is expected, coercion is used to convert non-booleans to legal values.

This is the complete list of invariants that are enforced:

- `apply(target, thisArgument, argumentsList)`
    - No invariants are enforced.
- `construct(target, argumentsList, newTarget)`
    - The result returned by the handler must be an object (not `null` or a primitive value).
- `defineProperty(target, propKey, propDesc)`
    - If the target is not extensible then you can’t add properties and `propKey`must be one of the own keys of the target.
    - If `propDesc` sets the attribute `configurable` to `false` then the target must have a non-configurable own property whose key is `propKey`.
    - If `propDesc` were to be used to (re)define an own property for the target then that must not cause an exception. An exception is thrown if a change is forbidden by the attributes `writable` and `configurable` (non-extensibility is handled by the first rule).
- `deleteProperty(target, propKey)`
    - Non-configurable own properties of the target can’t be deleted.
- `get(target, propKey, receiver)`
    - If the target has an own, non-writable, non-configurable data property whose key is `propKey` then the handler must return that property’s value.
    - If the target has an own, non-configurable, getter-less accessor property then the handler must return `undefined`.
- `getOwnPropertyDescriptor(target, propKey)`
    - The handler must return either an object or `undefined`.
    - Non-configurable own properties of the target can’t be reported as non-existent by the handler.
    - If the target is non-extensible then exactly the target’s own properties must be reported by the handler as existing.
    - If the handler reports a property as non-configurable then that property must be a non-configurable own property of the target.
    - If the result returned by the handler were used to (re)define an own property for the target then that must not cause an exception. An exception is thrown if the change is not allowed by the attributes `writable`and `configurable` (non-extensibility is handled by the third rule). Therefore, the handler can’t report a non-configurable property as configurable and it can’t report a different value for a non-configurable non-writable property.
- `getPrototypeOf(target)`
    - The result must be either an object or `null`.
    - If the target object is not extensible then the handler must return the prototype of the target object.
- `has(target, propKey)`
    - A handler must not hide (report as non-existent) a non-configurable own property of the target.
    - If the target is non-extensible then no own property of the target may be hidden.
- `isExtensible(target)`
    - The result returned by the handler is coerced to boolean.
    - After coercion to boolean, the value returned by the handler must be the same as `target.isExtensible()`.
- `ownKeys(target)`
    - The handler must return an object, which treated as Array-like and converted into an Array.
    - Each element of the result must be either a string or a symbol.
    - The result must contain the keys of all non-configurable own properties of the target.
    - If the target is not extensible then the result must contain exactly the keys of the own properties of the target (and no other values).
- `preventExtensions(target)`
    - The result returned by the handler is coerced to boolean.
    - If the handler returns a truthy value (indicating a successful change) then `target.isExtensible()` must be `false` afterwards.
- `set(target, propKey, value, receiver)`
    - If the target has an own, non-writable, non-configurable data property whose key is `propKey` then `value` must be the same as the value of that property (i.e., the property can’t be changed).
    - If the target has an own, non-configurable, setter-less accessor property then a `TypeError` is thrown (i.e., such a property can’t be set).
- `setPrototypeOf(target, proto)`
    - The result returned by the handler is coerced to boolean.
    - If the target is not extensible, the prototype can’t be changed. This is enforced as follows: If the target is not extensible and the handler returns a truthy value (indicating a successful change) then `proto` must be the same as the prototype of the target. Otherwise, a `TypeError` is thrown.

In the spec, the invariants are listed in the section “[Proxy Object Internal Methods and Internal Slots](http://www.ecma-international.org/ecma-262/6.0/#sec-proxy-object-internal-methods-and-internal-slots)”.

### Operations that affect the prototype chain

The following operations of normal objects perform operations on objects in the prototype chain. Therefore, if one of the objects in that chain is a proxy, its traps are triggered. The specification implements the operations as internal own methods (that are not visible to JavaScript code). But in this section, we pretend that they are normal methods that have the same names as the traps. The parameter `target`becomes the receiver of the method call.

- `target.get(propertyKey, receiver)`
    If `target` has no own property with the given key, `get` is invoked on the prototype of `target`.
- `target.has(propertyKey)`
    Similarly to `get`, `has` is invoked on the prototype of `target` if `target` has no own property with the given key.
- `target.set(propertyKey, value, receiver)`
    Similarly to `get`, `set` is invoked on the prototype of `target` if `target` has no own property with the given key.

All other operations only affect own properties, they have no effect on the prototype chain.

In the spec, these (and other) operations are described in the section “[Ordinary Object Internal Methods and Internal Slots](http://www.ecma-international.org/ecma-262/6.0/#sec-ordinary-object-internal-methods-and-internal-slots)”.

### Reflect

The global object `Reflect` implements all interceptable operations of the JavaScript meta object protocol as methods. The names of those methods are the same as those of the handler methods, which, helps with forwarding operations from the handler to the target.

- `Reflect.apply(target, thisArgument, argumentsList) : any`

    Same as `Function.prototype.apply()`.

- `Reflect.construct(target, argumentsList, newTarget=target) : Object`

    The `new` operator as a function. `target` is the constructor to invoke, the optional parameter `newTarget` points to the constructor that started the current chain of constructor calls.

- `Reflect.defineProperty(target, propertyKey, propDesc) : boolean`

    Similar to `Object.defineProperty()`.

- `Reflect.deleteProperty(target, propertyKey) : boolean`

    The `delete` operator as a function. It works slightly differently, though: It returns `true` if it successfully deleted the property or if the property never existed. It returns `false` if the property could not be deleted and still exists. The only way to protect properties from deletion is by making them non-configurable. In sloppy mode, the `delete` operator returns the same results. But in strict mode, it throws a `TypeError` instead of returning `false`.

- `Reflect.get(target, propertyKey, receiver=target) : any`

    A function that gets properties. The optional parameter `receiver` is needed when `get` reaches a getter later in the prototype chain. Then it provides the value for `this`.

- `Reflect.getOwnPropertyDescriptor(target, propertyKey) : PropDesc|Undefined`

    Same as `Object.getOwnPropertyDescriptor()`.

- `Reflect.getPrototypeOf(target) : Object|Null`

    Same as `Object.getPrototypeOf()`.

- `Reflect.has(target, propertyKey) : boolean`

    The `in` operator as a function.

- `Reflect.isExtensible(target) : boolean`

    Same as `Object.isExtensible()`.

- `Reflect.ownKeys(target) : Array<PropertyKey>`

    Returns all own property keys (strings and symbols!) in an Array.

- `Reflect.preventExtensions(target) : boolean`

    Similar to `Object.preventExtensions()`.

- `Reflect.set(target, propertyKey, value, receiver=target) : boolean`

    A function that sets properties.

- `Reflect.setPrototypeOf(target, proto) : boolean`

    The new standard way of setting the prototype of an object. The current non-standard way, that works in most engines, is to set the special property `__proto__`.

Several methods have boolean results. For `has` and `isExtensible`, they are the results of the operation. For the remaining methods, they indicate whether the operation succeeded.

#### Use cases for `Reflect` besides forwarding

Apart from forwarding operations, why is `Reflect` useful?

- Different return values: `Reflect` duplicates the following methods of `Object`, but its methods return booleans indicating whether the operation succeeded (where the `Object` methods return the object that was modified).
    - `Object.defineProperty(obj, propKey, propDesc) : Object`
    - `Object.preventExtensions(obj) : Object`
    - `Object.setPrototypeOf(obj, proto) : Object`
- Operators as functions: The following `Reflect` methods implement functionality that is otherwise only available via operators:
    - `Reflect.construct(target, argumentsList, newTarget=target) : Object`
    - `Reflect.deleteProperty(target, propertyKey) : boolean`
    - `Reflect.get(target, propertyKey, receiver=target) : any`
    - `Reflect.has(target, propertyKey) : boolean`
    - `Reflect.set(target, propertyKey, value, receiver=target) : boolean`
- Shorter version of `apply()`: If you want to be completely safe about invoking the method `apply()` on a function, you can’t do so via dynamic dispatch, because the function may have an own property with the key `'apply'`:

    ```js
    func.apply(thisArg, argArray) // not safe
    Function.prototype.apply.call(func, thisArg, argArray) // safe
    ```

    Using `Reflect.apply()` is shorter:

    ```js
    Reflect.apply(func, thisArg, argArray)
    ```

- No exceptions when deleting properties: the `delete` operator throws in strict mode if you try to delete a non-configurable own property. `Reflect.deleteProperty()` returns `false` in that case.

#### `Object.*` versus `Reflect.*`

Going forward, `Object` will host operations that are of interest to normal applications, while `Reflect` will host operations that are more low-level.
