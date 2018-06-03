# Classes

## Visão geral

Uma classe e uma subclasse:

```js
class Point
{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString() {
        return `(${this.x}, ${this.y})`;
    }
}

class ColorPoint extends Point
{
    constructor(x, y, color) {
        super(x, y);

        this.color = color;
    }

    toString() {
        return super.toString() + ' in ' + this.color;
    }
}
```

Usando as classes:

```js
const cp = new ColorPoint(25, 8, 'green');

cp.toString();    // '(25, 8) in green'

cp instanceof ColorPoint // true
cp instanceof Point     // true
```

Under the hood, ES6 classes are not something that is radically new: They mainly provide more convenient syntax to create old-school constructor functions. You can see that if you use `typeof`:

```js
typeof Point // 'function'
```

## O essencial

### Classes base

Uma classe é definida da seguinte forma no *ECMAScript 6*:

```js
class Point
{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString() {
        return `(${this.x}, ${this.y})`;
    }
}
```

Você usa essa classe como uma função construtora do *ES5*:

```js
var p = new Point(25, 8);
p.toString()    // '(25, 8)'
```

De fato, o resultado de uma definição de classe é uma função:

```js
typeof Point // 'function'
```

However, you can only invoke a class via `new`, *not via a function call*:

```js
Point()   // TypeError: Classes can’t be function-called
```

*In the spec, function-calling classes is prevented in the internal method `[[Call]]` of function objects.*

#### No separators between members of class definitions

There is no separating punctuation between the members of a class definition. For example, the members of an object literal are separated by commas, which are illegal at the top levels of class definitions. Semicolons are allowed, but ignored:

```js
class MyClass
{
    foo() {}
    ; // OK, ignored
    , // SyntaxError
    bar() {}
}
```

Semicolons are allowed in preparation for future syntax which may include semicolon-terminated members. Commas are forbidden to emphasize that class definitions are different from object literals.

#### Class declarations are not hoisted

Function declarations are *hoisted*: When entering a scope, the functions that are declared in it are immediately available – independently of where the declarations happen. That means that you can call a function that is declared later:

```js
foo(); // works, because `foo` is hoisted

function foo() {}
```

In contrast, class declarations are not *hoisted*. Therefore, a class only exists after execution reached its definition and it was evaluated. Accessing it beforehand leads to a `ReferenceError`:

```js
new Foo(); // ReferenceError

class Foo {}
```

The reason for this limitation is that classes can have an `extends` clause whose value is an arbitrary expression. That expression must be evaluated in the proper “location”, its evaluation can’t be hoisted.

Not having hoisting is less limiting than you may think. For example, a function that comes before a class declaration can still refer to that class, but you have to wait until the class declaration has been evaluated before you can call the function.

```js
function functionThatUsesBar() {
    new Bar();
}

functionThatUsesBar(); // ReferenceError
class Bar {}
functionThatUsesBar(); // OK
```

#### Class expressions

Da mesma forma que as funções, existem dois tipos de definições de classe, duas maneiras de definir uma classe: *class declarations* e *class expressions*.

Similarly to function expressions, class expressions can be anonymous:

```js
const MyClass = class {
    ···
};

const inst = new MyClass();
```

Also similarly to function expressions, class expressions can have names that are only visible inside them:

```js
const MyClass = class Me {
    getClassName() {
        return Me.name;
    }
};

const inst = new MyClass();

console.log(inst.getClassName()); // Me
console.log(Me.name); // ReferenceError: Me is not defined
```

As últimas duas linhas demonstram que `Me` não se torna uma variável fora da classe, mas pode ser usada dentro dela.

### Inside the body of a class definition

A class body can only contain methods, but not data properties. Prototypes having data properties is generally considered an anti-pattern, so this just enforces a best practice.

#### `constructor`, static methods, prototype methods

Vamos examinar três tipos de métodos que você encontra frequentemente nas definições de classe.

```js
class Foo
{
    constructor(prop) {
        this.prop = prop;
    }

    static staticMethod() {
        return 'classy';
    }

    prototypeMethod() {
        return 'prototypical';
    }
}

const foo = new Foo(123);
```

The object diagram for this class declaration looks as follows. Tip for understanding it: `[[Prototype]]` is an inheritance relationship between objects, while `prototype` is a normal property whose value is an object. The property `prototype` is only special w.r.t. the `new` operator using its value as the `prototype` for instances it creates.

![Classes and methods foo](./Images/Classes%20and%20methods%20foo.png "Classes and methods foo")

**First, the pseudo-method** `constructor`. This method is special, as it defines the function that represents the class:

```js
Foo === Foo.prototype.constructor // true
typeof Foo // 'function'
```

It is sometimes called a `class constructor`. It has features that normal constructor functions don’t have (mainly the ability to constructor-call its superconstructor via `super()`).

**Second, static methods**. *Static properties* (or *class properties*) are properties of `Foo` itself. If you prefix a method definition with `static`, you create a class method:

```js
typeof Foo.staticMethod   // 'function'
Foo.staticMethod()        // 'classy'
```

**Third, prototype methods**. The *prototype properties* of `Foo` are the properties of `Foo.prototype`. They are usually methods and inherited by instances of `Foo`.

```js
typeof Foo.prototype.prototypeMethod  // 'function'
foo.prototypeMethod()                 // 'prototypical'
```

#### Static data properties

For the sake of finishing ES6 classes in time, they were deliberately designed to be “maximally minimal”. That’s why you can currently only create static methods, getters, and setters, but not static data properties. There is a proposal for adding them to the language. Until that proposal is accepted, there are two work-arounds that you can use.

First, you can manually add a *static* property:

```js
class Point
{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
}

Point.ZERO = new Point(0, 0);
```

You could use `Object.defineProperty()` to create a read-only property, but I like the simplicity of an assignment.

Second, you can create a `static getter`:

```js
class Point
{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    static get ZERO() {
        return new Point(0, 0);
    }
}
```

In both cases, you get a property `Point.ZERO` that you can read. In the first case, the same instance is returned every time. In the second case, a new instance is returned every time.

#### Getters and setters

The syntax for getters and setters is just like in *ECMAScript 5* object literals:

```js
class MyClass
{
    get prop() {
        return 'getter';
    }

    set prop(value) {
        console.log('setter: '+value);
    }
}
```

You use `MyClass` as follows.

```js
const inst = new MyClass();
inst.prop = 123;    // setter: 123
inst.prop           // 'getter'
```

#### Computed method names

Você pode definir o nome de um método através de uma expressão, se você colocá-lo entre colchetes. Por exemplo, as seguintes formas de definição `Foo` são todas equivalentes.

```js
class Foo {
    myMethod() {}
}

class Foo {
    ['my' + 'Method']() {}
}

const m = 'myMethod';
class Foo {
    [m]() {}
}
```

Several special methods in *ECMAScript 6* have keys that are symbols. Computed method names allow you to define such methods. For example, if an object has a method whose key is `Symbol.iterator`, it is *iterable*. That means that its contents can be iterated over by the `for-of` loop and other language mechanisms.

```js
class IterableClass {
    [Symbol.iterator]() {
        ···
    }
}
```

#### Generator methods

Se você prefixar uma definição de método com um asterisco (*), ele se torna um *generator method*. Entre outras coisas, um generator é útil para definir o método cuja chave é `Symbol.iterator`. O código a seguir demonstra como isso funciona.

```js
class IterableArguments {
    constructor(...args) {
        this.args = args;
    }

    * [Symbol.iterator]() {
        for (const arg of this.args) {
            yield arg;
        }
    }
}

for (const x of new IterableArguments('hello', 'world')) {
    console.log(x);
}

// Output:
// hello
// world
```

### Subclassing

The `extends` clause lets you create a subclass of an existing constructor (which may or may not have been defined via a class):

```js
class Point
{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString() {
        return `(${this.x}, ${this.y})`;
    }
}

class ColorPoint extends Point
{
    constructor(x, y, color) {
        super(x, y); // (A)
        this.color = color;
    }

    toString() {
        return super.toString() + ' in ' + this.color; // (B)
    }
}
```

Again, this class is used like you’d expect:

```js
const cp = new ColorPoint(25, 8, 'green');
cp.toString()   // '(25, 8) in green'

cp instanceof ColorPoint  // true
cp instanceof Point       // true
```

There are two kinds of classes:

* `Point` é uma classe base, porque não possui uma cláusula de `extends`.
* `ColorPoint` é uma classe derivada.

There are two ways of using `super`:

* A *class constructor* (the pseudo-method `constructor` in a class definition) uses it like a function call (`super(···)`), in order to make a *superconstructor* call (*line A*).
* Method definitions (in object literals or classes, with or without `static`) use it like property references (`super.prop`) or method calls (`super.method(···)`), in order to refer to superproperties (*line B*).

#### The prototype of a subclass is the superclass

O protótipo de uma subclasse é a superclasse no *ECMAScript 6*:

```js
Object.getPrototypeOf(ColorPoint) === Point // true
```

Isso significa que as propriedades estáticas são herdadas:

```js
class Foo {
    static classMethod() {
        return 'hello';
    }
}

class Bar extends Foo {
}

Bar.classMethod(); // 'hello'
```

You can even super-call static methods:

```js
class Foo {
    static classMethod() {
        return 'hello';
    }
}

class Bar extends Foo {
    static classMethod() {
        return super.classMethod() + ', too';
    }
}

Bar.classMethod(); // 'hello, too'
```

#### Superconstructor calls

In a derived class, you must call `super()` before you can use `this`:

```js
class Foo {}

class Bar extends Foo
{
    constructor(num) {
        const tmp = num * 2; // OK
        this.num = num; // ReferenceError
        super();
        this.num = num; // OK
    }
}
```

Implicitly leaving a derived constructor without calling `super()` also causes an *error*:

```js
class Foo {}

class Bar extends Foo {
    constructor() {
    }
}

const bar = new Bar(); // ReferenceError
```

#### Overriding the result of a constructor

Assim como no *ES5*, você pode substituir o resultado de um construtor ao retornar explicitamente um objeto:

```js
class Foo {
    constructor() {
        return Object.create(null);
    }
}

console.log(new Foo() instanceof Foo); // false
```

If you do so, it doesn’t matter whether `this` has been initialized or not. In other words: you don’t have to call `super()` in a derived constructor if you override the result in this manner.

#### Default constructors for classes

Se você não especificar um constructor para uma classe base, a seguinte definição é usada:

```js
constructor() {}
```

Para classes derivadas, o seguinte construtor padrão é usado:

```js
constructor(...args) {
    super(...args);
}
```

#### Subclassing built-in constructors

In ECMAScript 6, you can finally subclass all built-in constructors (there are work-arounds for ES5, but these have significant limitations).

For example, you can now create your own exception classes (that will inherit the feature of having a stack trace in most engines):

```js
class MyError extends Error {
}

throw new MyError('Something happened!');
```

You can also create subclasses of Array whose instances properly handle `length`:

```js
class Stack extends Array {
    get top() {
        return this[this.length - 1];
    }
}

var stack = new Stack();

stack.push('world');
stack.push('hello');

console.log(stack.top);     // hello
console.log(stack.length);  // 2
```

Note that subclassing `Array` is usually not the best solution. It’s often better to create your own class (whose interface you control) and to delegate to an Array in a private property.

Subclassing built-in constructors is something that engines have to support natively, you won’t get this feature via transpilers.

## Private data for classes

This section explains four approaches for managing private data for *ES6* classes:

1. Keeping private data in the environment of a class `constructor`
2. Marking private properties via a naming convention (e.g. a prefixed underscore)
3. Keeping private data in `WeakMaps`
4. Using symbols as keys for private properties

Approaches #1 and #2 were already common in ES5, for constructors. Approaches #3 and #4 are new in ES6. Let’s implement the same example four times, via each of the approaches.

### Private data via constructor environments

Our running example is a class `Countdown` that invokes a callback `action` once a counter (whose initial value is `counter`) reaches zero. The two parameters `action` and `counter` should be stored as private data.

In the first implementation, we store `action` and `counter` in the environment of the class constructor. An environment is the internal data structure, in which a JavaScript engine stores the parameters and local variables that come into existence whenever a new scope is entered (e.g. via a function call or a constructor call). This is the code:

```js
class Countdown
{
    constructor(counter, action) {
        Object.assign(this, {
            dec() {
                if (counter < 1)
                    return;
                counter--;
                if (counter === 0) {
                    action();
                }
            }
        });
    }
}
```

Using `Countdown` looks like this:

```js
const c = new Countdown(2, () => console.log('DONE'));
c.dec();
c.dec(); // DONE
```

Pros:

* The private data is completely safe
* The names of private properties won’t clash with the names of other private properties (of superclasses or subclasses).

Cons:

* The code becomes less elegant, because you need to add all methods to the instance, inside the constructor (at least those methods that need access to the private data).
* Due to the instance methods, the code wastes memory. If the methods were prototype methods, they would be shared.

### Private data via a naming convention

The following code keeps private data in properties whose names a marked via a prefixed underscore:

```js
class Countdown
{
    constructor(counter, action) {
        this._counter = counter;
        this._action = action;
    }

    dec() {
        if (this._counter < 1)
            return;

        this._counter--;

        if (this._counter === 0) {
            this._action();
        }
    }
}
```

Pros:

* Code looks nice.
* We can use prototype methods.

Cons:

* Not safe, only a guideline for client code.
* The names of private properties can clash.

### Private data via `WeakMaps`

There is a neat technique involving WeakMaps that combines the advantage of the first approach (safety) with the advantage of the second approach (being able to use prototype methods). This technique is demonstrated in the following code: we use the WeakMaps `_counter` and `_action` to store private data.

```js
const _counter = new WeakMap();
const _action = new WeakMap();

class Countdown
{
    constructor(counter, action) {
        _counter.set(this, counter);
        _action.set(this, action);
    }

    dec() {
        let counter = _counter.get(this);

        if (counter < 1)
            return;

        counter--;
        _counter.set(this, counter);

        if (counter === 0) {
            _action.get(this)();
        }
    }
}
```

Each of the two *WeakMaps* `_counter` and `_action` maps objects to their private data. Due to how *WeakMaps* work that won’t prevent objects from being *garbage-collected*. As long as you keep the *WeakMaps* hidden from the outside world, the private data is safe.

If you want to be even safer, you can store `WeakMap.prototype.get` and `WeakMap.prototype.set` in variables and invoke those (instead of the methods, dynamically):

```js
const set = WeakMap.prototype.set;
···
set.call(_counter, this, counter);  // _counter.set(this, counter);
```

Then your code won’t be affected if malicious code replaces those methods with ones that snoop on our private data. However, you are only protected against code that runs after your code. There is nothing you can do if it runs before yours.

Pros:

* We can use prototype methods.
* Safer than a naming convention for property keys.
* The names of private properties can’t clash.
* Relatively elegant.

Cons:

* Code is not as elegant as a naming convention.

### Private data via symbols

Outro local de armazenamento para dados privados são propriedades cujas chaves são símbolos:

```js
const _counter = Symbol('counter');
const _action = Symbol('action');

class Countdown
{
    constructor(counter, action) {
        this[_counter] = counter;
        this[_action] = action;
    }

    dec() {
        if (this[_counter] < 1)
            return;

        this[_counter]--;

        if (this[_counter] === 0) {
            this[_action]();
        }
    }
}
```

Each symbol is unique, which is why a symbol-valued property key will never clash with any other property key. Additionally, symbols are somewhat hidden from the outside world, but not completely:

```js
const c = new Countdown(2, () => console.log('DONE'));

console.log(Object.keys(c));    // []
console.log(Reflect.ownKeys(c));    // [ Symbol(counter), Symbol(action) ]
```

Pros:

* We can use prototype methods.
* The names of private properties can’t clash.

Cons:

* Code is not as elegant as a naming convention.
* Not safe: you can list all property keys (including symbols!) of an object via `Reflect.ownKeys()`.
