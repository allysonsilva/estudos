# iOS 11 Programming Fundamentals with Swift

## The Architecture of Swift

### Everything Is an Object?

In Swift, “everything is an object.” That’s a boast common to various modern object-oriented languages, but what does it mean? Well, that depends on what you mean by “object” — and what you mean by “everything.”

Let’s start by stipulating that an object, roughly speaking, is something you can send a message to. A message, roughly speaking, is an imperative instruction. For example, you can give commands to a dog: “Bark!” “Sit!” In this analogy, those phrases are messages, and the dog is the object to which you are sending those messages.

In Swift, the syntax of message-sending is *dot-notation*. We start with the object; then there’s a dot (a period); then there’s the message. (Some messages are also followed by parentheses, but ignore them for now; the full syntax of message-sending is one of those details we’ll be filling in later.) This is valid Swift syntax:

```swift
fido.bark() rover.sit()
```

The idea of *everything* being an object is a way of suggesting that even “primitive” linguistic entities can be sent messages. Take, for example, 1. It appears to be a literal digit and no more. It will not surprise you, if you’ve ever used any programming language, that you can say things like this in Swift:

```swift
let sum = 1 + 2
```

But it is surprising to find that 1 can be followed by a dot and a message. This is legal and meaningful in Swift (don’t worry about what it actually means):

```swift
let s = 1.description
```

But we can go further. Return to that innocent-looking `1 + 2` from our earlier code. It turns out that this is actually a kind of syntactic trickery, a convenient way of expressing and hiding what’s really going on. Just as 1 is actually an object, + is actually a message; but it’s a message with special syntax (*operator syntax*). In Swift, every noun is an object, and every verb is a message.

Perhaps the ultimate acid test for whether something is an object in Swift is whether you can modify it. An object type can be *extended* in Swift, meaning that you can define your own messages on that type. For example, you can’t normally send the `sayHello` message to a number. But you can change a number type so that you can:

```swift
extension Int
{
    func sayHello()
    {
        print("Hello, I'm \(self)")
    }
}

1.sayHello() // outputs: "Hello, I'm 1"
```

In Swift, then, `1` is an object. In some languages, such as Objective-C, it clearly is not; it is a “primitive” or *scalar* built-in data type. So the distinction being drawn here is between object types on the one hand and scalars on the other. In Swift, there are no scalars; *all* types are ultimately object types. That’s what “everything is an object” really means.

## Functions

### Function Parameters and Return Value

![Function Parameters and Return Value](./Images/Function%20Parameters%20and%20Return%20Value.png "Function Parameters and Return Value")

1. The declaration starts with the keyword `func`, followed by the name of this function; here, it’s `sum`. This is the name that must be used in order to call the function — that is, in order to run the code that the function contains.

2. The name of the function is followed by its _parameter_ list. It consists, minimally, of parentheses. If this function takes parameters (input), they are listed inside the parentheses, separated by comma. Each parameter has a strict format: the _name_ of the parameter, a colon, and the _type_ of the parameter.

3. This particular function declaration also has an underscore (`_`) and a space before each parameter name in the parameter list. _I’m not going to explain that underscore yet_. I need it for the example, so just trust me for now.

4. If the function is to return a value, then after the parentheses is an arrow operator (->) followed by the type of value that this function will return.

5. Then we have curly braces enclosing the _body_ of the function — its actual code.

6. Within the curly braces, in the function body, the variables defined as the parameter names have sprung to life, with the types specified in the parameter list.

7. If the function is to return a value, it must do so with the keyword `return` followed by that value. And, not surprisingly, the type of that value must match the type declared earlier for the return value (after the arrow operator).

Here are some further points to note about the parameters and return type of our function:

**Parameters**

Our `sum` function expects two parameters — an `Int`, to which it gives the name `x`, and another `Int`, to which it gives the name `y`. The function body code won’t run unless code elsewhere calls this function and actually passes values of the specified types for its parameters. (In fact, if I were to try to call this function _without_ providing a value for each of these two parameters, or if either of the values I provide were not an `Int`, the compiler would stop me with an error.) In the body of the function, therefore, we can confidently use those values, referring to them by those names, secure in the knowledge that such values will exist and that they will be `Int` values, as specified by our parameter list. This provides certainty for not only the programmer, but also the compiler. Observe that these names, `x` and `y`, are arbitrary and purely local (_internal_) to this function. They are different from any other `x` and `y` that may be used in other functions or at a higher level of scope. These names are defined purely so that the parameter values will have names by which they can be referred to in the code within the function body. The parameter declaration is, indeed, a kind of variable declaration: we are declaring variables `x` and `y` for use inside this function.

**Return type**

The last statement of our `sum` function’s body returns the value of a variable called `result`; this variable was created by adding two Int values together, so it is an Int, which is what this function is supposed to produce. If I tried to return a String (`return "howdy"`), or if I were to omit the return statement altogether, the compiler would stop me with an error.

Note that the keyword `return` actually does two things. It returns the accompanying value, and it also halts execution of the function. It is permitted for more lines of code to follow a return statement, but the compiler will warn if this means that those lines of code can never be executed.

#### Void Return Type and Parameters

**A function without a return type**

No law says that a function must return a value. A function may be declared to return no value. In that case, there are three ways to write the declaration: you can write it as returning Void; you can write it as returning `()`, an empty pair of parentheses; or you can omit the arrow operator and the return type entirely. These are all legal:

```swift
func say1(_ s:String) -> Void { print(s) }
func say2(_ s:String) -> () { print(s) }
func say3(_ s:String) { print(s) }
```

If a function returns no value, then its body need not contain a `return` statement. If it does contain a `return` statement, its purpose will be purely to end execution of the function at that point.

A call to a function that returns no value is made purely for the function’s side effects; it has no useful return value that can be made part of a larger expression, so the statement that calls the function will usually consist of the function call and nothing else.

**A function without any parameters**

No law says that a function must take any parameters. If it doesn’t, the parameter list in the function declaration can be completely empty. But you can’t omit the parameter list parentheses themselves! They will be present in the function declaration, after the function’s name:

```swift
func greet1() -> String { return "howdy" }
```

#### Function Signature

If we ignore for a moment the parameter names in the function declaration, we can completely characterize a function by the types of its inputs and its output, using an expression like this:

```
(Int, Int) -> Int
```

That in fact is a legal expression in Swift. It is the _signature_ of a function. In this case, it’s the signature of our `sum` function. Of course, there can be other functions that take two Int parameters and return an Int — and that’s just the point. This signature characterizes all functions that have this number of parameters, of these types, and that return a result of this type. A function’s signature is, in effect, its type — the type of the function. The fact that functions have types will be of great importance later on.

> The signature of a function must include both the parameter list (without parameter names) and the return type, even if one or both of those is empty; so, the signature of a function that takes no parameters and returns no value may be written `() -> Void` or `() -> ()`.

The signature of a Swift function may be specified explicitly by appending it to its bare name or full name with the keyword as.

For example, a function declared `func say(_ s:String, times:Int)` may be referred to as say as `(String,Int) -> ()`.

### External Parameter Names

A function can _externalize_ the names of its parameters. The external names must then appear in a call to the function as labels to the arguments. There are several reasons why this is a good thing:

- It clarifies the purpose of each argument; each argument label can give a clue as to how that argument contributes to the behavior of the function.
- It distinguishes one function from another; two functions with the same name (before the parentheses) and the same signature but different externalized parameter names are two distinct functions.
- It helps Swift to interface with Objective-C and Cocoa, where method parameters nearly always have externalized names.

> Externalized parameter names are so standard in Swift that there’s a rule: by default, all parameter names are externalized automatically, using the internal name as the externalized name. Thus, if you want a parameter name to be externalized, and if you want the externalized name to be the same as the internal name, do nothing — that will happen all by itself.

If you want to depart from the default behavior, you can do either of the following in your function declaration:

_Change the name of an external parameter_ <br>
If you want to change the external name of a parameter to be different from its internal name, precede the internal name with the external name and a space.

_Suppress the externalization of a parameter_ <br>
To suppress a parameter’s external name, precede the internal name with an underscore and a space.

Here’s the declaration for a function that concatenates a string with itself a given number of times:

```swift
func echoString(_ s:String, times:Int) -> String
{
    var result = ""

    for _ in 1...times {
        result += s
    }

    return result
}
```

That function’s first parameter has an internal name only, but its second parameter has an external name, which will be the same as its internal name, namely `times`. And here’s how to call it:

```swift
let s = echoString("hi", times:3)
```

In the call, as you can see, the external name precedes the argument as a label, separated by a colon.

Now let’s say that in our `echoString` function we prefer to use `times` purely as an external name for the second parameter, with a different name — say, `n` — as the internal name. And let’s strip the `string` off the function’s name (before the parentheses) and make it the external name of the first parameter. Then the declaration would look like this:

```swift
func echo(string s:String, times n:Int) -> String
{
    var result = ""

    for _ in 1...n {
        result += s
    }

    return result
}
```

In the body of that function, there is now no `times` variable available; `times` is purely an external name, for use in the call. The internal name is `n`, and that’s the name the code refers to. And here’s how to call it:

```swift
let s = echo(string:"hi", times:3)
```

_The existence of external names doesn’t mean that the call can use a different parameter order from the declaration. For example, our `echo(string:times:)` expects a String parameter and an Int parameter, in that order. The order can’t be different in the call, even though the label might appear to disambiguate which argument goes with which parameter._

### Overloading

In Swift, function overloading is legal (and common). This means that two functions with exactly the same name, including their external parameter names, can coexist as long as they have different signatures.

Thus, for example, these two functions can coexist:

```swift
func say (_ what:String) { }
func say (_ what:Int) { }
```

The reason overloading works is that Swift has strict typing. A String is not an Int. Swift can tell them apart in the declaration, and Swift can tell them apart in a function call. Thus, Swift knows unambiguously that `say("what")` is different from `say(1)`.

Overloading works for the return type as well. Two functions with the same name and parameter types can have different return types. But the context of the call must disambiguate; that is, it must be clear what return type the caller is expecting.

For example, these two functions can coexist:

```swift
func say() -> String { return "one" }
func say() -> Int { return 1 }
```

But now you can’t call say like this:

```swift
let result = say() // compile error
```

The call is ambiguous, and the compiler tells you so. The call must be used in a context where the expected return type is clear. For example, suppose we have another function that is not overloaded, and that expects a String parameter:

```swift
func giveMeAString(_ s:String) {
    print("thanks!")
}
```

Then `giveMeAString(say())` is legal, because only a String can go in this spot, so we must be calling the say that returns a String. Similarly:

```swift
let result = say() + "two"
```

Only a String can be “added” to a String, so this must be the `say()` that returns a String.

_Two functions with the same signature but different external parameter names do not constitute a case of overloading; the functions have different external parameter names, so they are simply two different functions with two different names._

### Variadic Parameters

> A parameter can be variadic. This means that the caller can supply as many values of this parameter’s type as desired, separated by comma; the function body will receive these values as an array.

To indicate in a function declaration that a parameter is variadic, follow it by three dots, like this:

```swift
func sayStrings(_ arrayOfStrings:String ...) {
    for s in arrayOfStrings {
        print(s)
    }
}
```

And here’s how to call it:

```swift
sayStrings("hey", "ho", "nonny nonny no")
```

The global `print` function takes a variadic first parameter, so you can output multiple values with a single command:

```swift
print("Manny", 3, true) // Manny 3 true
```

The `print` function’s default parameters dictate further details of the output. The default `separator:` (for when you provide multiple values) is a space, and the default `terminator:` is a newline; you can change either or both:

```swift
print("Manny", "Moe", separator:", ", terminator:", ")
print("Jack")
// output is "Manny, Moe, Jack" on one line
```

A function can declare a maximum of one variadic parameter (because otherwise it might be impossible to determine where the list of values ends).

### Modifiable Parameters

> In the body of a function, a parameter is essentially a local variable. By default, it’s a variable implicitly declared with `let`.

You can’t assign to it:

```swift
func say(_ s:String, times:Int, loudly:Bool) {
    loudly = true // compile error
}
```

If we want our function to alter the original value of an argument passed to it, we must do the following:

- The type of the parameter we intend to modify must be declared `inout`.
- When we call the function, the variable holding the value we intend to tell it to modify must be declared with `var`, not `let`.
- Instead of passing the variable as an argument, we must pass its address. This is done by preceding its name with an ampersand (`&`).

Our `removeCharacter(_:from:)` now looks like this:

```swift
func removeCharacter(_ c:Character, from s: inout String) -> Int {
    var howMany = 0

    while let ix = s.index(of:c) {
        s.remove(at:ix)
        howMany += 1
    }

    return howMany
}
```

And our call to `removeCharacter(_:from:)` now looks like this:

```swift
var s = "hello"
let result = removeCharacter("l", from:&s)
```

After the call, `result` is 2 and `s` is "heo". Notice the ampersand before the name `s` when we pass it as the `from:` argument in our call to `removeCharacter(_:from:)`. I like this requirement, because it forces us to acknowledge explicitly to the compiler, and to ourselves, that we’re about to do something potentially dangerous: we’re letting this function, as a side effect, modify a value outside of itself.

_When a function with an `inout` parameter is called, the variable whose address was passed as argument to that parameter is always set, even if the function makes no changes to that parameter._

### Function As Value

If you’ve never used a programming language where functions are first-class citizens, perhaps you’d better sit down now, because what I’m about to tell you might make you feel a little faint: In Swift, a function is a first-class citizen. This means that a function can be used wherever a value can be used. For example, a function can be assigned to a variable; a function can be passed as an argument in a function call; a function can be returned as the result of a function.

Swift has strict typing. You can only assign a value to a variable or pass a value into or out of a function if it is the right _type_ of value. In order for a function to be used as a value, it needs to have a type. And indeed it does! Have you guessed what it is? A function’s _signature_ is its type.

The chief purpose of using a function as a value is so that this function can later be called without a definite knowledge of what function it is. Here’s the world’s simplest (and silliest) example, just to show the syntax and structure:

```swift
func doThis(_ f:() -> ()) {
    f()
}
```

That is a function `doThis` that takes one parameter (and returns no value). The parameter, `f`, is itself a function; we know this because the type of the parameter is given as a function signature, `() -> ()`, meaning (as you know) a function that takes no parameters and returns no value. The function `doThis` then _calls_ the function `f` that it received as its parameter, by saying `f()`.

Having declared the function `doThis`, how would you call it? To do so, you’d need to pass it a function as argument. Here’s one way to do that:

```swift
func doThis(_ f:() -> ()) {
    f()
}

func whatToDo() { (1)
    print("I did it")
}

doThis(whatToDo) (2)
```

1. First, we declare a function (`whatToDo`) _of the proper type_ — a function that takes no parameters and returns no value.
2. Then, we call `doThis`, passing as argument a _function reference_ — in effect, the bare name of the function. Notice that we are not calling `whatToDo` here; we are passing it.

Sure enough, this works: we pass `whatToDo` as argument to `doThis`; `doThis` calls the function that it receives as its parameter; and the string `"I did it"` appears in the console.

#### Type Aliases Can Clarify Function Types

To make function type specifiers clearer, we can take advantage of Swift’s `typealias` feature to give a function type a name. The name can be descriptive, and the possibly confusing arrow operator notation is avoided. For example, if we say `typealias VoidVoidFunction = () -> ()`, we can then say `VoidVoidFunction` wherever we need to specify a function type with that signature.

Thus, earlier we declared a function like this:

```swift
func doThis(_ f:() -> ()) {
    f()
}
```

We could have declared it like this:

```swift
typealias VoidVoidFunction = () -> ()

func dothis(_ f:VoidVoidFunction) {
    f()
}
```

### Anonymous Functions

That’s called an anonymous function, and it’s legal and common in Swift. To form an anonymous function, you do two things:

1. Create the function body itself, including the surrounding curly braces, but with no function declaration.
2. If necessary, express the function’s parameter list and return type as the first thing inside the curly braces, followed by the keyword in.

Anonymous functions are very commonly used in Swift, so make sure you can read and write that code! Anonymous functions, in fact, are so common and so important, that some shortcuts for writing them are provided:

#### Omission of the return type

If the anonymous function’s return type is already known to the compiler, you can omit the arrow operator and the specification of the return type:

```swift
UIView.animate(withDuration:0.4,
    animations: { () in
        self.myButton.frame.origin.y += 20
    },
    completion: { (finished:Bool) in
        print("finished: \(finished)")
    }
)
```

#### Omission of the `in` expression when there are no parameters

If the anonymous function takes no parameters, and if the return type can be omitted, the `in` expression itself can be omitted entirely:

```swift
UIView.animate(withDuration:0.4,
    animations: {
        self.myButton.frame.origin.y += 20
    },
    completion: { (finished:Bool) in
        print("finished: \(finished)")
    }
)
```

#### Omission of the parameter types

If the anonymous function takes parameters and their types are already known to the compiler, the types can be omitted:

```swift
UIView.animate(withDuration:0.4,
    animations: {
        self.myButton.frame.origin.y += 20
    },
    completion: { (finished) in
        print("finished: \(finished)")
    }
)
```

#### Omission of the parentheses

If the parameter types are omitted, the parentheses around the parameter list can be omitted:

```swift
UIView.animate(withDuration:0.4,
    animations: {
        self.myButton.frame.origin.y += 20
    },
    completion: { finished in
        print("finished: \(finished)")
    }
)
```

#### Omission of the `in` expression even when there are parameters

If the return type can be omitted, and if the parameter types are already known to the compiler, you can omit the in expression and refer to the parameters directly within the body of the anonymous function by using the magic names `$0`, `$1`, and so on, in order:

```swift
UIView.animate(withDuration:0.4,
    animations: {
        self.myButton.frame.origin.y += 20
    },
    completion: {
        print("finished: \($0)")
    }
)
```

#### Omission of the parameter names

If the anonymous function body doesn’t need to refer to a parameter, you can substitute an underscore for its name in the parameter list in the in expression:

```swift
UIView.animate(withDuration:0.4,
    animations: {
        self.myButton.frame.origin.y += 20
    },
    completion: { _ in
        print("finished")
    }
)
```

But note that if the anonymous function takes parameters, you must acknowledge them somehow. You can omit the in expression and use the parameters by the magic names `$0` and so on, or you can keep the `in` expression and ignore the parameters with an underscore, but you can’t omit the in expression altogether and not use the parameters by their magic names! If you do, your code won’t compile.

#### Omission of the function argument label

If, as will just about always be the case, your anonymous function is the last argument being passed in this function call, you can close the function call with a right parenthesis before this last argument, and then put just the anonymous function body without a label (this is called a trailing function):

```swift
UIView.animate(withDuration:0.4,
    animations: {
        self.myButton.frame.origin.y += 20
    }) { _ in
        print("finished")
    }
)
```

#### Omission of the calling function parentheses

If you use the trailing function syntax, and if the function you are calling takes no parameters other than the function you are passing to it, you can omit the empty parentheses from the call. This is the only situation in which you can omit the parentheses from a function call! To illustrate, I’ll declare and call a different function:

```swift
func doThis(_ f:() -> ()) {
    f()
}

doThis { // no parentheses!
    print("Howdy")
}
```

### Define-and-Call

A pattern that’s surprisingly common in Swift is to define an anonymous function and call it, all in one move:

```swift
{
    // ... code goes here
}()
```

> Notice the parentheses after the curly braces! The curly braces define an anonymous function body; the parentheses call that anonymous function.

## Variables and Simple Types

### Variable Declaration

A variable is declared with `let` or `var`:

- With `let`, the variable becomes a constant — its value can never be changed after the first assignment of a value.
- With `var`, the variable is a true variable, and its value can be changed by subsequent assignment.

A variable declaration is usually accompanied by initialization — you use an equal sign to assign the variable a value, as part of the declaration. That, however, is not a requirement; it is legal to declare a variable without immediately initializing it.

What is not legal is to declare a variable without giving it a type. A variable must have a type from the outset, and that type can never be changed. A variable declared with var can have its value changed by subsequent assignment, but the new value must conform to the variable’s type.

_When a variable’s address is to be passed as argument to a function, the variable must be declared and initialized beforehand, even if the initial value is fake._

### Computed Variables

Alternatively, a variable can be computed. This means that the variable, instead of having a value, has functions. One function, the `setter`, is called when the variable is assigned to. The other function, the `getter`, is called when the variable is referred to. Here’s some code illustrating schematically the syntax for declaring a computed variable:

```swift
var now : String { // (1)
    get { // (2)
        return Date().description // (3)
    }
    set { // (4)
        print(newValue) // (5)
    }
}
```

1. The variable must be declared with `var` (not `let`). Its type must be declared explicitly. The type is followed immediately by curly braces.

2. The getter function is called `get`. There is no formal function declaration; the word `get` is simply followed immediately by a function body in curly braces.

3. The getter function must return a value of the same type as the variable.

4. The setter function is called `set`. There is no formal function declaration; the word `set` is simply followed immediately by a function body in curly braces.

5. The setter behaves like a function taking one parameter. By default, this parameter arrives into the setter function body with the local name `newValue`.

There are a couple of variants on the basic syntax:

- The name of the `set` function parameter doesn’t have to be newValue. To specify a different name, put it in parentheses after the word `set`, like this:

```
set (val) { // now you can use "val" inside the setter function body
```

- There doesn’t have to be a setter. If the setter is omitted, this becomes a _read-only_ variable. This is the computed variable equivalent of a `let` variable: attempting to assign to it is a compile error.

- There must always be a getter! However, if there is no setter, the word `get` and the curly braces that follow it can be omitted. Thus, this is a legal declaration of a _read-only_ variable:

```swift
var now : String {
    return Date().description
}
```

### Setter Observers

Computed variables are not needed as a stored variable façade as often as you might suppose. That’s because Swift has another feature, which lets you inject functionality into the setter of a stored variable — setter observers. These are functions that are called just before and just after other code sets a stored variable.

The syntax for declaring a variable with a setter observer is very similar to the syntax for declaring a computed variable; you can write a `willSet` function, a `didSet` function, or both:

```swift
var s = "whatever" { // (1)
    willSet { // (2)
        print(newValue) // (3)
    }
    didSet { // (4)
        print(oldValue) // (5)
        // self.s = "something else"
    }
}
```

1. The variable must be declared with `var` (not `let`). It can be assigned an initial value. It is then followed immediately by curly braces.

2. The `willSet` function, if there is one, is the word `willSet` followed immediately by a function body in curly braces. It is called when other code sets this variable, just _before_ the variable actually receives its new value.

3. By default, the `willSet` function receives the incoming new value as `newValue`. You can change this name by writing a different name in parentheses after the word `willSet`. The old value is still sitting in the stored variable, and the `willSet` function can access it there.

4. The `didSet` function, if there is one, is the word `didSet` followed immediately by a function body in curly braces. It is called when other code sets this variable, just _after_ the variable actually receives its new value.

5. By default, the `didSet` function receives the old value, which has already been replaced as the value of the variable, as `oldValue`. You can change this name by writing a different name in parentheses after the word `didSet`. The new value is already sitting in the stored variable, and the `didSet` function can access it there. Moreover, it is legal for the `didSet` function to _set the stored variable to a different value_.

_Setter observer functions are not called when the stored variable is initialized or when the didSet function changes the stored variable’s value. That would be circular!_

### Lazy Initialization

> The term lazy is not a pejorative puritanical judgment; it’s a formal description of an important behavior. If a stored variable is assigned an initial value as part of its declaration, and if it uses lazy initialization, then the initial value is not actually evaluated and assigned until running code accesses the variable’s value.

There are three types of variable that can be initialized lazily in Swift:

**Global variables**s

Global variables are automatically lazy. This makes sense if you ask yourself when they should be initialized. As the app launches, files and their top-level code are encountered. It would make no sense to initialize globals now, because the app isn’t even running yet. Thus global initialization must be postponed to some moment that does make sense. Therefore, a global variable’s initialization doesn’t happen until other code first refers to that global. Under the hood, this behavior is implemented in such a way as to make initialization both singular (it can happen only once) and thread-safe.

**Static properties**

_Static properties are automatically lazy_. They behave exactly like global variables, and for basically the same reason. (There are no stored class properties in Swift, so class properties can’t be initialized and thus can’t have lazy initialization.)

**Instance properties**

_An instance property is not lazy by default, but it may be made lazy by marking its declaration with the keyword `lazy`_. This property must be declared with `var`, not `let`. The initializer for such a property might never be evaluated, namely if code assigns the property a value before any code fetches the property’s value.

--------------

_Unlike automatically lazy global and static variables, an instance property marked `lazy` does not initialize itself in a thread-safe way. When used in a multithreaded context, `lazy` instance properties can cause multiple initialization and even crashes. Also, lazy instance properties can’t have setter observers; and there’s no `lazy let` for instance properties, so you can’t readily make a lazy instance property read-only._

### Built-In Simple Types

#### Tuples

> A tuple is a lightweight custom ordered collection of multiple values. As a type, it is expressed by surrounding the types of the contained values with parentheses and separating them by commas.

For example, here’s a declaration for a variable whose type is a tuple of an Int and a String:

```swift
var pair : (Int, String)
```

The literal value of a tuple is expressed in the same way — the contained values, surrounded with parentheses and separated by commas:

```swift
var pair : (Int, String) = (1, "Two")
```

Those types can be inferred, so there’s no need for the explicit type in the declaration:

```swift
var pair = (1, "Two")
```

Tuples are a pure Swift language feature; they are not compatible with Cocoa and Objective-C, so you’ll use them only for values that Cocoa never sees. Within Swift, however, they have many uses. For example, a tuple is an obvious solution to the problem that a function can return only one value; a tuple is one value, but it contains multiple values, so using a tuple as the return type of a function permits that function to return multiple values.

Tuples come with numerous linguistic conveniences. You can assign to a tuple of variable names as a way of assigning to multiple variables simultaneously:

```swift
let ix: Int
let s: String
(ix, s) = (1, "Two")
```

That’s such a convenient thing to do that Swift lets you do it in one line, declaring and initializing multiple variables simultaneously:

```swift
let (ix, s) = (1, "Two")
```

To ignore one of the assigned values, use an underscore to represent it in the receiving tuple:

```swift
let pair = (1, "Two")
let (_, s) = pair // now s is "Two"
```

Assigning variable values to one another through a tuple swaps them safely:

```swift
var s1 = "hello"
var s2 = "world"
(s1, s2) = (s2, s1) // now s1 is "world" and s2 is "hello"
```

The `enumerated` method lets you walk a sequence with `for...in` and receive, on each iteration, each successive element’s index number along with the element itself; this double result comes to you as — you guessed it — a tuple:

```swift
let s = "hello"
for (ix,c) in s.enumerated() {
    print("character \(ix) is \(c)")
}
```

You can refer to the individual elements of a tuple directly, in two ways. The first way is by index number, using a literal number (not a variable value) as the name of a message sent to the tuple with dot-notation:

```swift
let pair = (1, "Two")
let ix = pair.0 // now ix is 1
```

If you have a var reference to a tuple, you can assign into it by the same means:

```swift
var pair = (1, "Two")
pair.0 = 2 // now pair is (2, "Two")
```

The second way to access tuple elements is to give them labels. The notation is like that of function parameters, and must appear as part of the explicit or implicit type declaration. Thus, here’s one way to establish tuple element labels:

```swift
let pair : (first:Int, second:String) = (1, "Two")
```

And here’s another way:

```swift
let pair = (first:1, second:"Two")
```

The labels are now part of the type of this value, and travel with it through subsequent assignments. You can then use them as literal messages, just like (and together with) the numeric literals:

```swift
var pair = (first:1, second:"Two")
let x = pair.first // 1
pair.first = 2
let y = pair.0 // 2
```

You can assign from a tuple without labels into a corresponding tuple with labels (and vice versa):

```swift
let pair = (1, "Two")
let pairWithNames : (first:Int, second:String) = pair
let ix = pairWithNames.first // 1
```

You can also pass, or return from a function, a tuple without labels where a corresponding tuple with labels is expected:

```swift
func tupleMaker() -> (first:Int, second:String) {
    return (1, "Two") // no labels here
}

let ix = tupleMaker().first // 1
```

If you’re going to be using a certain type of tuple consistently throughout your program, it might be useful to give it a type name. To do so, use Swift’s `typealias` keyword. For example, in my LinkSame app I have a Board class describing and manipulating the game layout. The board is a grid of Piece objects. I need a way to describe positions of the grid. That’s a pair of integers, so I define my own type as a tuple:

```swift
class Board
{
    typealias Point = (x:Int, y:Int) // ...
}
```

The advantage of that notation is that it now becomes easy to use Points throughout my code. For example, given a Point, I can fetch the corresponding Piece:

```swift
func piece(at p:Point) -> Piece? {
    let (i,j) = p
    // ... error-checking goes here ...
    return self.grid[i][j]
}
```

_Void, the type of value returned by a function that doesn’t return a value, is actually a type alias for an empty tuple. That’s why it is also notated as `()`._

## Object Types

### Object Type Declarations and Features

Object types are declared with the flavor of the object type (`enum`, `struct`, or `class`), the name of the object type (which should start with a capital letter), and curly braces:

```swift
class Manny { }
struct Moe { }
enum Jack { }
```

The visibility (scope), and hence the usability, of an object type by other code depends upon where its declaration appears:

- Object types declared at the _top level of a file will_, by default, be visible to all files in the same module. This is the usual place for object type declarations.
- Sometimes it’s useful to declare a type _inside the declaration of another type_, thus giving it a namespace. This is called a _nested type_.
- An object type declared within the _body of a function_ will exist only inside the scope of the curly braces that surround it; such declarations are legal but rare.

Declarations for any object type may contain within their curly braces the following things:

**Initializers**

An object type is merely the type of an object. The purpose of declaring an object type will usually (though not always) be so that you can make an actual object an instance — that has this type. An initializer is a function, declared and called in a special way, allowing you to do that.

**Properties**

A variable declared at the top level of an object type declaration is a property. By default, it is an instance property. An instance property is scoped to an instance: it is accessed through a particular instance of this type, and its value can be different for every instance of this type.

Alternatively, a property can be a _static_/_class_ property. For an `enum` or `struct`, it is declared with the keyword `static`; for a class, it may instead be declared with the keyword `class`. Such a property belongs to the object type itself: it is accessed through the type, and it has just one value, associated with the type.

**Methods**

A function declared at the top level of an object type declaration is a method. By default, it is an instance method: it is called by sending a message to a particular instance of this type. Inside an instance method, self is the instance.

Alternatively, a method can be a _static_/_class_ method. For an `enum` or `struct`, it is declared with the keyword `static`; for a class, it may be declared instead with the keyword `class`. It is called by sending a message to the type. Inside a static/class method, `self` is the type.

**Subscripts**

A subscript is a special kind of instance method. It is called by appending square brackets to an instance reference.

**Object type declarations**

An object type declaration can contain an object type declaration — a nested type. From inside the containing object type, the nested type is in scope; from outside the containing object type, the nested type must be referred to through the containing object type. Thus, the containing object type is a namespace for the nested type.

#### Inicializadores

##### Como escrever um inicializador

Um inicializador é um tipo de função e sua sintaxe de declaração é semelhante à de uma função. Para declarar um inicializador, você usa a palavra-chave `init` seguida por uma lista de parâmetros, seguida por chaves contendo o código. Um tipo de objeto pode ter vários inicializadores, diferenciados por seus parâmetros. Um uso freqüente dos parâmetros é definir os valores das propriedades da instância.

```swift
class Dog
{
    var name = ""
    var license = 0

    init(name:String)
    {
        self.name = name
    }

    init(license:Int)
    {
        self.license = license
    }

    init(name:String, license:Int)
    {
        self.name = name
        self.license = license
    }
}
```

```swift
let fido = Dog(name:"Fido")
let rover = Dog(license:1234)
let spot = Dog(name:"Spot", license:1357)
```

_O que não consigo fazer é criar um Dog sem parâmetros de inicialização. Eu criei inicializadores, então meu inicializador implícito não existe mais. Este código não é mais legal:_

```swift
let puff = Dog() // compile error
```

Claro, eu poderia tornar legal esse código explicitamente declarando um inicializador sem parâmetros:

```swift
class Dog
{
    var name = ""
    var license = 0

    init()
    {
    }

    init(name:String)
    {
        self.name = name
    }

    init(license:Int)
    {
        self.license = license
    }

    init(name:String, license:Int)
    {
        self.name = name
        self.license = license
    }
}
```

Agora vem a parte realmente interessante. Em minhas declarações de propriedade, posso eliminar a atribuição de valores iniciais padrão (desde que eu declare explicitamente o tipo de cada propriedade):

```swift
class Dog
{
    var name : String // no default value!
    var license : Int // no default value!

    init(name:String = "", license:Int = 0)
    {
        self.name = name
        self.license = license
    }
}
```

Esse código é legal (e comum) - porque um inicializador inicializa! Em outras palavras, eu não tenho que dar meus valores iniciais de propriedades em suas declarações, desde que eu lhes dê valores iniciais em todos os inicializadores. Dessa forma, tenho a garantia de que todas as propriedades da minha instância possuem valores quando a instância passa a existir, o que é o que importa. Por outro lado, uma propriedade de instância sem um valor inicial quando a instância é criada é ilegal. Uma propriedade deve ser inicializada como parte de sua declaração ou por todos os inicializadores, ou o compilador parará de outra forma. A insistência do compilador Swift de que todas as propriedades de instância sejam inicializadas corretamente é um recurso valioso do Swift.

```swift
class Dog
{
    var name : String // no default value!
    var license : Int // no default value!

    init(name:String = "", license:Int = 0)
    {
        self.name = name // compile error
    }
}
```

##### Delegating initializers

Inicializadores dentro de um tipo de objeto podem chamar um ao outro usando a sintaxe `self.init(...)`. Um inicializador que chama outro inicializador é chamada de _delegating initializer_. Quando um inicializador delega, o outro inicializador - aquele para o qual ele delega - deve inicializar completamente a instância primeiro e, em seguida, o inicializador de delegação pode trabalhar com a instância totalmente inicializada, possivelmente definindo novamente uma propriedade `var` que já foi definida pelo inicializador delegou para.

```swift
struct Digit
{
    var number : Int
    var meaningOfLife : Bool

    init(number:Int)
    {
        self.number = number
        self.meaningOfLife = false
    }

    init() // this is a delegating initializer
    {
        self.init(number:42)
        self.meaningOfLife = true
    }
}
```

Além disso, um inicializador de delegação não pode definir uma propriedade imutável (uma variável `let`). Isso ocorre porque ele não pode se referir à propriedade até depois de ter chamado o outro inicializador e, nesse ponto, a instância está totalmente formada - a inicialização propriamente dita terminou e a porta para inicialização das propriedades imutáveis ​​foi fechada. Assim, o código anterior seria ilegal se `meaningOfLife` fosse declarado com `let`, porque o segundo inicializador é um inicializador de delegação e não pode definir uma propriedade imutável.

##### Failable initializers

Um inicializador pode retornar um Optional envolvendo a nova instância. Desta forma, `nil` pode ser retornado para sinalizar falha. Um inicializador que se comporta desta maneira é uma _failable initializer_. Para marcar um inicializador como failable ao declará-lo, coloque um ponto de interrogação após a palavra-chave `init`. Se o failable initializer precisar retornar `nil`, escreva explicitamente `return nil`. Cabe ao chamador testar o Opcional resultante para equivalência com `nil`, desembrulhá-lo e assim por diante, como acontece com qualquer Opcional.

```swift
class Dog
{
    let name : String
    let license : Int

    init?(name:String, license:Int)
    {
        if name.isEmpty {
            return nil
        }

        if license <= 0 {
            return nil
        }

        self.name = name
        self.license = license
    }
}
```

O valor resultante é tipado como um Optional que envolve um `Dog`, e o chamador precisará desembrulhar esse Optional (se não for `nil`) antes de enviar qualquer mensagem para ele.

#### Propriedades

Uma propriedade tem um tipo fixo; pode ser declarado com `var` ou `let`; pode ser armazenado ou computado; pode ter observadores setter. Uma propriedade de instância também pode ser declarada `lazy`.

##### Property initialization

Uma propriedade de instância armazenada deve receber um valor inicial. Mas, como expliquei há pouco, isso não tem que ser feito por meio da declaração; pode ser através de um inicializador. _Observadores Setter não são chamados durante a inicialização de propriedades_.

Uma declaração de propriedade que atribui um valor inicial à propriedade não pode buscar uma propriedade de instância ou chamar um método de instância. Tal comportamento exigiria uma referência, explícita ou implícita, a si mesmo; e durante a inicialização, ainda não há `self` - o `self` é exatamente o que estamos no processo de inicialização. Cometer esse erro pode resultar em algumas das mais desconcertantes mensagens de erro de compilação do Swift. Por exemplo, isso é ilegal (e remover as referências explícitas para si mesmo não o torna legal):

```swift
class Moi
{
    let first = "Allyson"
    let last = "Silva"
    let whole = self.first + " " + self.last // compile error
}
```

Existem duas soluções comuns nessa situação:

_Faça disso uma propriedade computed_

Uma propriedade calculado pode referir-se a `self`, porque o cálculo não vai realmente ser executada até `self` existir:

```swift
class Moi
{
    let first = "Allyson"
    let last = "Silva"

    var whole : String {
        return self.first + " " + self.last
    }
}
```

_Declarar esta propriedade como `lazy`_

Como uma propriedade calculada, um propriedade `lazy` pode referir-se a `self` legalmente porque essa referência não será executada até depois de `self` existir:

```swift
class Moi
{
    let first = "Allyson"
    let last = "Silva"

    lazy var whole = self.first + " " + self.last
}
```

Uma variável pode ser inicializada como parte de sua declaração usando várias linhas de código por meio de uma função anônima _define-and-call_. Se esta variável for uma propriedade de instância, e se esse código se referir a outras propriedades de instância ou métodos de instância, a variável deve ser declarada `lazy`:

```swift
class Moi
{
    let first = "Allyson"
    let last = "Silva"

    lazy var whole : String = {
        var s = self.first

        s.append(" ")
        s.append(self.last)
        return s
    }()
}
```

Ao contrário das propriedades de instância, as propriedades estáticas podem ser inicializadas com referência umas às outras; a razão é que os inicializadores de propriedade estática são `lazy`:

```swift
struct Greeting
{
    static let friendly = "hello there"
    static let hostile = "go away"
    static let ambivalent = friendly + " but " + hostile
}
```

#### Subscripts

Um subscript é um método de instância que é chamado de maneira especial - acrescentando colchetes a uma referência de instância. Os colchetes podem conter argumentos a serem passados ​​para o método subscript. Você pode usar esse recurso para o que quiser, mas é particularmente adequado para situações em que este é um tipo de objeto com elementos que podem ser acessados ​​apropriadamente por chave ou por número de índice. O uso dessa sintaxe com strings, e também é familiar de dicionários e arrays; você pode usar colchetes com strings e dicionários e arrays exatamente porque os tipos de string e dicionários e matriz de Swift declaram métodos de subscripts.

A sintaxe para declarar um método de subscript é algo como uma declaração de função e um pouco como uma declaração de propriedade computed. Isso não é coincidência! Um subscript é como uma função na qual ele pode receber parâmetros: argumentos podem aparecer nos colchetes quando um método subscript é chamado. Um subscript é como uma propriedade computed em que a chamada é usada como uma referência a uma propriedade: você pode buscar seu valor ou pode atribuir a ele.

```swift
struct Digit
{
    var number : Int

    init(_ n:Int)
    {
        self.number = n
    }

    subscript(ix:Int) -> Int { (1) (2)
        get { (3)
            let s = String(self.number)
            return Int(String(s[s.index(s.startIndex, offsetBy:ix)]))!
        }
    }

}
```

1. Após a palavra-chave `subscript`, temos uma lista de parâmetros indicando quais parâmetros devem aparecer dentro dos colchetes; por padrão, os seus nomes não são externalizados.

2. Então, após o operador arrow, temos o tipo de valor que é passado (quando o _getter_ é chamado) ou em (quando o _setter_ é chamado); este é paralelo ao tipo declarado para uma propriedade calculada, embora a sintaxe com o operador arrow é como a sintaxe para o valor retornado em uma declaração de função.

3. Finalmente, temos chaves cujo conteúdo é exatamente igual ao de uma propriedade computed. Você pode obter chaves para o _getter_ e chaves fixas para o _setter_. Se houver um _getter_ e nenhum _setter_, a palavra get e suas chaves podem ser omitidas. O _setter_ recebe o novo valor como `newValue`, mas você pode alterar esse nome fornecendo um nome diferente entre parênteses após a palavra `set`.

Aqui está um exemplo de chamar o _getter_; a instância com colchetes anexados contendo os argumentos é usada como se você estivesse obtendo um valor de propriedade:

```swift
var d = Digit(1234)
let aDigit = d[1] // 2
```

Agora vou expandir minha estrutura de Dígito para que o método `subscript` inclua um _setter_ (e novamente omitirei a verificação de erros):

```swift
struct Digit
{
    var number : Int

    init(_ n:Int) {
        self.number = n
    }

    subscript(ix:Int) -> Int {
        get {
            let s = String(self.number)
            return Int(String(s[s.index(s.startIndex, offsetBy:ix)]))!
        }

        set {
            var s = String(self.number)
            let i = s.index(s.startIndex, offsetBy:ix)
            s.replaceSubrange(i...i, with: String(newValue))
            self.number = Int(s)!
        }
    }
}
```

E aqui está um exemplo de chamar o _setter_; a instância com colchetes contêm os argumentos são usados ​​como se você estivesse configurando um valor de propriedade:

```swift
var d = Digit(1234)
d[0] = 2 // now d.number is 2234
```

### Structs

#### Struct Initializers, Properties, and Methods

Uma estrutura que não tem um inicializador explícito e que não precisa de um inicializador explícito - porque não tem propriedades armazenadas ou porque todas as propriedades armazenadas recebem valores padrão como parte de sua declaração - obtém automaticamente um inicializador implícito sem parâmetros, `init()`. Por exemplo:

```swift
struct Digit {
    var number = 42
}
```

Essa estrutura pode ser inicializada, por meio de `Digit()`. Mas se você adicionar qualquer inicializador explícito, perderá o inicializador implícito:

```swift
struct Digit {
    var number = 42
    init(number:Int) {
        self.number = number
    }
}
```

Agora você pode digitar `Digit(number:42)`, mas não `Digit()`. Claro, você pode adicionar um inicializador explícito que faz a mesma coisa:

```swift
struct Digit {
    var number = 42
    init() {}
    init(number:Int) {
        self.number = number
    }
}
```

Uma estrutura que tenha propriedades armazenadas e que não tenha um inicializador explícito obtém automaticamente um inicializador implícito derivado de suas propriedades de instância. Isso é chamado de **memberwise initializer**. Por exemplo:

```swift
struct Digit {
    var number : Int // could use "let" here instead
}
```

Essa estrutura é legal - na verdade, é legal mesmo que a propriedade numérica seja declarada com `let` em vez de `var` - mesmo que pareça que não tenhamos cumprido o contrato exigindo que inicializemos todas as propriedades armazenadas em sua declaração ou em um inicializador. A razão é que essa estrutura automaticamente possui um inicializador de membros que inicializa todas as suas propriedades. Nesse caso, o inicializador _memberwise_ é `init(number:)` e você pode digitar `Digit(number:42)`.

O inicializador _memberwise_ existe mesmo para `var` propriedades armazenadas que recebem um valor padrão em sua declaração; Portanto, essa estrutura possui um _memberwise_ initializer `init(number:)`, além de seu inicializador `init()` implícito:

```swift
struct Digit {
    var number = 42
}
```

Mas se você adicionar quaisquer inicializadores explícitos, você perderá o inicializador _memberwise_ (embora, é claro, você possa escrever um inicializador explícito que faça a mesma coisa).

Se uma struct tiver quaisquer inicializadores explícitos, eles devem cumprir o contrato de modo que todas as propriedades armazenadas devem ser inicializadas por inicialização direta na declaração ou por todos os inicializadores. Se uma struct tiver vários inicializadores explícitos, eles poderão delegar um ao outro com `self.init(...)`.

### Classes

#### Class Initializers

A inicialização de uma instância de classe é consideravelmente mais complicada do que a inicialização de uma instância de struct ou enum, devido à existência de herança de classe. A principal tarefa de um inicializador é garantir que todas as propriedades tenham um valor inicial, tornando a instância bem formada à medida que ela surge; e um inicializador pode ter outras tarefas para executar que são essenciais para o estado inicial e integridade desta instância. Uma classe, no entanto, pode ter uma superclasse, que pode ter propriedades e inicializadores próprios. Assim, devemos garantir que, quando uma subclasse for inicializada, as propriedades de sua superclasse sejam inicializadas e as tarefas de seus inicializadores sejam executadas em boa ordem, além de inicializar as propriedades e executar as tarefas inicializadoras da própria subclasse.

O Swift resolve esse problema de maneira coerente e confiável - e engenhosamente - aplicando algumas regras claras e bem definidas sobre o que um inicializador de classe deve fazer.

##### Kinds of class initializer

As regras começam com uma distinção entre os tipos de inicializadores que uma classe pode ter:

**Designated initializer:**

Um inicializador de classe, por padrão, é um inicializador designado. Uma classe com quaisquer propriedades armazenadas que não são inicializadas como parte de sua declaração deve ter pelo menos um inicializador designado e, quando a classe é instanciada, exatamente um de seus inicializadores designados deve ser chamado e deve garantir que todas as propriedades armazenadas sejam inicializado. Um inicializador designado não pode delegar para outro inicializador na mesma classe; é ilegal para um inicializador designado usar `self.init(...)`.

**Convenience initializer:**

Um inicializador de conveniência é marcado com a palavra-chave `convenience`. É um inicializador de delegação; deve conter a `self.init(...)`. Além disso, um inicializador de conveniência deve delegar a um inicializador designado: quando `self.init(...)`, ele deve chamar um inicializador designado na mesma classe - ou então deve chamar outro inicializador de conveniência na mesma classe, formando assim uma cadeia de inicializadores de conveniência que termina chamando um inicializador designado na mesma classe.

**Implicit initializer:**

Uma classe sem propriedades armazenadas, ou com propriedades armazenadas, todas inicializadas como parte de sua declaração, e que não possui inicializadores designados explícitos, possui um inicializador `init()` implícito.

Aqui estão alguns exemplos. Esta classe não possui propriedades armazenadas, portanto, possui um inicializador designado `init()` implícito:

```swift
class Dog { }
let d = Dog()
```

As propriedades armazenadas dessa classe têm valores padrão, portanto, ela também tem um inicializador designado `init()` implícito:

```swift
class Dog {
    var name = "Fido"
}
let d = Dog()
```

As propriedades armazenadas desta classe têm valores padrão, mas não possuem inicializador `init()` implícito porque possuem um inicializador designado explícito:

```swift
class Dog {
    var name = "Fido"
    init(name:String) {
        self.name = name
    }
}

let d = Dog(name:"Rover") // ok
let d2 = Dog() // compile error
```

As propriedades armazenadas dessa classe têm valores padrão e possuem um inicializador explícito, mas também possuem um inicializador implícito `init()` porque seu inicializador explícito é um inicializador de conveniência. Além disso, o inicializador `init()` implícito é um inicializador designado, portanto, o inicializador de conveniência pode delegar para ele:

```swift
class Dog {
    var name = "Fido"
    convenience init(name:String) {
        self.init()
        self.name = name
    }
}

let d = Dog(name:"Rover")
let d2 = Dog()
```

Esta classe tem propriedades armazenadas sem valores padrão; ele tem um inicializador designado explícito e todas essas propriedades são inicializadas nesse inicializador designado:

```swift
class Dog {
    var name : String
    var license : Int
    init(name:String, license:Int) {
        self.name = name
        self.license = license
    }
}

let d = Dog(name:"Rover", license:42)
```

Essa classe é semelhante ao exemplo anterior, mas também possui inicializadores de conveniência formando uma cadeia que termina com um inicializador designado:

```swift
class Dog {
    var name : String
    var license : Int

    init(name:String, license:Int) {
        self.name = name
        self.license = license
    }
    convenience init(license:Int) {
        self.init(name:"Fido", license:license)
    }
    convenience init() {
        self.init(license:1)
    }
}

let d = Dog()
```

Note que as regras sobre o que mais um inicializador pode dizer e quando ele pode dizê-lo, ainda estão em vigor:

- Um inicializador designado não pode, exceto para inicializar uma propriedade (ou buscar o valor de uma propriedade que já tenha sido inicializada), declarar `self`, implícita ou explicitamente, até que todas as propriedades desta classe tenham sido inicializadas.
- Um inicializador de conveniência é um inicializador de delegação, portanto, ele não pode declarar `self` para qualquer finalidade até depois de ter chamado, direta ou indiretamente, um inicializador designado (e não pode definir uma propriedade imutável).

##### Subclass initializers

Tendo definido e distinguido entre inicializadores designados e inicializadores de conveniência, estamos prontos para as regras sobre o que acontece com relação aos inicializadores quando uma classe é em si uma subclasse de alguma outra classe:

**No declared initializers**

Se uma subclasse não precisar ter nenhum inicializador próprio e se não declarar inicializadores próprios, seus inicializadores consistirão nos inicializadores herdados de sua superclasse. (Uma subclasse não tem inicializador `init()` implícito, a menos que ela a herde de sua superclasse.).

**Convenience initializers only**

Se uma subclasse não precisa ter nenhum inicializador, ela é elegível para declarar inicializadores de conveniência, e estes funcionam exatamente como os inicializadores de conveniência sempre fazem, porque a herança fornece os inicializadores designados que os inicializadores de conveniência devem chamar `self.init(...)`.

**Designated initializers**

Se uma subclasse declarar qualquer inicializador designado, todo o jogo muda drasticamente. Agora, nenhum inicializador é herdado! A existência de um inicializador designado explícito bloqueia a herança do inicializador. Os únicos inicializadores que a subclasse tem agora são os inicializadores que você escreve explicitamente. (No entanto, há uma exceção, a qual chegarei em breve).

Cada inicializador designado na subclasse agora tem um requisito extra: ele deve chamar um dos inicializadores designados da superclasse, com `super.init(...)`.

Além disso, as regras sobre `self` continuam a se aplicar. Assim, um inicializador designado pela subclasse deve fazer as coisas nesta ordem:

1. Deve assegurar que todas as propriedades desta classe (a subclasse) sejam inicializadas.
2. Ele deve chamar `super.init(...)` e o inicializador que ele chama deve ser um inicializador designado.
3. Somente então, esse inicializador pode chamar `self` para propósitos como chamar um método de instância ou acessar uma propriedade herdada.

**Designated and convenience initializers**

Se uma subclasse declarar os inicializadores designados e de conveniência, os inicializadores de conveniência na subclasse ainda estarão sujeitos às regras que já descrevi. Eles devem chamar `self.init(...)`, chamando um inicializador designado diretamente ou (por meio de uma cadeia de inicializadores de conveniência) indiretamente. Não há inicializadores herdados, portanto, o inicializador designado que um inicializador de conveniência chama deve ser declarado na subclasse.

**Override initializers**

Os inicializadores da superclasse podem ser substituídos na subclasse, de acordo com estas restrições:

- Um inicializador cujos parâmetros correspondam a um inicializador de conveniência da superclasse pode ser um inicializador designado ou um inicializador de conveniência e não é marcado como `override`.
- Um inicializador cujos parâmetros correspondam a um inicializador designado da superclasse pode ser um inicializador designado ou um inicializador de conveniência e deve ser marcado como `override`. Um inicializador designado `override` ainda deve chamar algum inicializador designado pela superclasse (possivelmente até mesmo aquele que ele substitui) com `super.init(...)`.

Geralmente, como já disse, se uma subclasse tiver qualquer inicializador designado, nenhum inicializador será herdado. Mas se uma subclasse substituir todos os inicializadores designados da sua superclasse, a subclasse herdará os inicializadores de conveniência da superclasse.

**Failable initializers**

Se um inicializador chamado por um inicializador failable estiver disponível, a sintaxe de chamada não será alterada e nenhum teste adicional será necessário - se um inicializador failable chamado falhar, todo o processo de inicialização falhará (e será abortado) imediatamente.

Existem algumas restrições adicionais sobre inicializadores failable:

- `init` pode substituir `init?`, mas não vice-versa.
- `init?` pode chamar `init`.
- `init` pode chamar o `init?` declarando `init` e desembrulhando o resultado com um ponto de exclamação (e se o `init?` falhar, o sistema irá dar crash!!).

*Em nenhum momento um inicializador de subclasse pode definir uma propriedade constante (`let`) de uma superclasse. Isso ocorre porque, quando a subclasse tem permissão para fazer qualquer outra coisa além de inicializar suas próprias propriedades e chamar outro inicializador, a superclasse concluiu sua própria inicialização e a porta para inicializar suas constantes foi fechada.*

Aqui estão alguns exemplos básicos. Começamos com uma classe cuja subclasse não possui inicializadores explícitos:

```swift
class Dog
{
    var name : String
    var license : Int

    init(name:String, license:Int)
    {
        self.name = name
        self.license = license
    }

    convenience init(license:Int)
    {
        self.init(name:"Fido", license:license)
    }
}

class NoisyDog : Dog { }
```

Dado esse código, você pode fazer um assim:

```swift
let nd1 = NoisyDog(name:"Fido", license:1)
let nd2 = NoisyDog(license:2)
```

Esse código é legal, porque o `NoisyDog` herda os inicializadores de sua superclasse. No entanto, você não pode fazer um `NoisyDog` assim:

```swift
let nd3 = NoisyDog() // compile error
```

Esse código é ilegal. Mesmo que um NoisyDog não possua propriedades próprias, ele não possui um inicializador `init()` implícito; seus inicializadores são inicializadores herdados, e sua superclasse, `Dog`, não possui um inicializador `init()` implícito para herdar.

Agora, aqui está uma classe cujo único inicializador explícito da subclasse é um inicializador de conveniência:

```swift
class Dog
{
    var name : String
    var license : Int

    init(name:String, license:Int)
    {
        self.name = name
        self.license = license
    }

    convenience init(license:Int)
    {
        self.init(name:"Fido", license:license)
    }
}

class NoisyDog : Dog
{
    convenience init(name:String)
    {
        self.init(name:name, license:1)
    }
}
```

Observe como o inicializador de conveniência do `NoisyDog` cumpre seu contrato chamando `self.init(...)` para chamar um inicializador designado - o que acontece ter herdado. Dado esse código, existem três maneiras de criar um `NoisyDog`, exatamente como você esperaria:

```swift
let nd1 = NoisyDog(name:"Fido", license:1)
let nd2 = NoisyDog(license:2)
let nd3 = NoisyDog(name:"Rover")
```

Em seguida, aqui está uma classe cuja subclasse declara um inicializador designado:

```swift
class Dog
{
    var name : String
    var license : Int

    init(name:String, license:Int)
    {
        self.name = name
        self.license = license
    }

    convenience init(license:Int)
    {
        self.init(name:"Fido", license:license)
    }
}

class NoisyDog : Dog
{
    init(name:String)
    {
        super.init(name:name, license:1)
    }
}
```

O inicializador explícito do `NoisyDog` agora é um inicializador designado. Ele cumpre seu contrato chamando um inicializador designado em `super`. O NoisyDog agora cortou a herança de todos os inicializadores; A única maneira de fazer um NoisyDog é assim:

```swift
let nd1 = NoisyDog(name:"Rover")
```

Finalmente, aqui está uma classe cuja subclasse substitui seus inicializadores designados:

```swift
class Dog
{
    var name : String
    var license : Int

    init(name:String, license:Int)
    {
        self.name = name
        self.license = license
    }

    convenience init(license:Int)
    {
        self.init(name:"Fido", license:license)
    }
}

class NoisyDog : Dog
{
    override init(name:String, license:Int)
    {
        super.init(name:name, license:license)
    }
}
```

O `NoisyDog` substituiu todos os inicializadores designados pela sua superclasse, então ele herda os inicializadores de conveniência da sua superclasse. Existem duas maneiras de criar um `NoisyDog`:

```swift
let nd1 = NoisyDog(name:"Rover", license:1)
let nd2 = NoisyDog(license:2)
```

Esses exemplos ilustram as principais regras que você deve manter na sua cabeça. Você provavelmente não precisa memorizar as regras restantes, porque o compilador irá aplicá-las e continuará batendo até que você as consiga corrigir.

##### Required initializers

Há mais uma coisa a saber sobre inicializadores de classe: um inicializador de classe pode ser precedido pela palavra-chave `required`. Isto significa que uma subclasse pode não ter falta deste inicializador. Isso, por sua vez, significa que, se uma subclasse implementar inicializadores designados, bloqueando assim a herança, ela deverá substituir esse inicializador e marcar a substituição `required`. Aqui está um exemplo (sem sentido):

```swift
class Dog
{
    var name : String
    required init(name:String)
    {
        self.name = name
    }
}

class NoisyDog : Dog
{
    var obedient = false
    init(obedient:Bool)
    {
        self.obedient = obedient
        super.init(name:"Fido")
    }
} // compile error
```

Esse código não será compilado. Dog’s `init(name:)` está marcado como `required`; assim, nosso código não será compilado, a menos que herdemos ou substituamos o `init(name:)` no NoisyDog. Mas não podemos herdá-lo porque, ao implementar inicializador designado `init(obedient:)` em NoisyDog, nós bloqueamos a herança. Portanto, devemos substituí-lo:

```swift
class Dog
{
    var name : String

    required init(name:String)
    {
        self.name = name
    }
}

class NoisyDog : Dog
{
    var obedient = false

    init(obedient:Bool)
    {
        self.obedient = obedient
        super.init(name:"Fido")
    }

    required init(name:String)
    {
        super.init(name:name)
    }
}
```

Observe que o nosso inicializador requerido não é marcado com `override`, mas é marcado com `required`, garantindo assim que o requisito continue descendo abaixo de qualquer subclasse adicional.

Expliquei o que declarar um inicializador como `required`, mas não expliquei por que você precisaria fazer isso. Eu darei exemplos mais adiante.

#### Class Deinitializer

Uma classe e apenas uma classe (não os outros tipos de objeto) podem ter um deinicializador. Esta é uma função declarada com a palavra-chave `deinit` seguida por chaves que contêm o corpo da função. Você nunca chama essa função sozinho; é chamado em tempo de execução quando uma instância dessa classe sai da existência. Se uma classe tiver uma superclasse, o deinicializador da subclasse (se houver) será chamado antes do deinicializador da superclasse (se houver).

A ideia de um deinicializador é que você pode querer executar alguma limpeza, ou apenas logar no console para provar a si mesmo que sua instância está saindo da existência em boa ordem.

#### Class Properties and Methods

Uma subclasse pode substituir suas propriedades herdadas. A substituição deve ter o mesmo nome e tipo da propriedade herdada e deve ser marcada com `override`. (Uma propriedade não pode ter o mesmo nome que uma propriedade herdada, mas um tipo diferente, pois não há como distingui-las.)

A principal restrição é que uma propriedade de `override` não pode ser uma propriedade armazenada. Mais especificamente:

- Se a propriedade da superclasse for gravável (uma propriedade armazenada ou uma propriedade computed com um setter), a substituição na subclasse pode consistir na adição de observadores de setter a essa propriedade.
- Como alternativa, a substituição na subclasse pode ser uma propriedade computed. Nesse caso:
    - Se a propriedade da superclasse for armazenada, a substituição da propriedade computed da subclasse deve ter um getter e um setter.
    - Se a propriedade da superclasse for computed, a propriedade computed da subclasse deve reimplementar todos os acessores que a superclasse implementa. Se a propriedade da superclasse for somente leitura (possui apenas um getter), a substituição pode incluir um setter.

A class can have static members, marked `static`, just like a struct or an enum. It can also have class members, marked `class`. Both static and class members are inherited by subclasses.

The chief difference between static and class methods from the programmer’s point of view is that a static method cannot be overridden; it is as if `static` were a synonym for `class final`.

The difference between static properties and class properties is similar, but with an additional, rather dramatic qualification: a static property can be stored, but a class property can only be computed.

### Type Reference

#### From Instance to Type

Pode ser útil, dada uma instância, referir-se ao seu tipo. Na verdade, pode ser útil para uma instância se referir ao seu próprio tipo - por exemplo, para enviar uma mensagem para esse tipo. Em um exemplo anterior, um método de instância Dog buscou uma propriedade de classe `Dog` enviando uma mensagem para o tipo Dog explicitamente usando a palavra `Dog`:

```swift
class Dog
{
    class var whatDogsSay : String {
        return "woof"
    }

    func bark()
    {
        print(Dog.whatDogsSay)
    }
}
```

A expressão `Dog.whatDogsSay` parece desajeitada e inflexível. Por que deveríamos codificar no `Dog` um conhecimento de qual classe ele é? Tem uma classe; deve apenas saber o que é.

No Swift, você pode acessar o tipo de objeto subjacente de uma referência de objeto por meio da função `type(of:)`. Assim, se você não gosta da noção de uma instância Dog chamando um método de classe Dog dizendo Dog explicitamente, há outra maneira:

```swift
class Dog
{
    class var whatDogsSay : String {
        return "woof"
    }

    func bark()
    {
        print(type(of:self).whatDogsSay)
    }
}
```

Uma coisa importante sobre o uso do `type(of:)` em vez de codificar um nome de classe é que ele obedece ao polimorfismo:

```swift
class Dog
{
    class var whatDogsSay : String {
        return "woof"
    }

    func bark()
    {
        print(type(of:self).whatDogsSay)
    }
}

class NoisyDog : Dog
{
    override class var whatDogsSay : String {
        return "woof woof woof"
    }
}
```

```swift
let nd = NoisyDog()
nd.bark() // woof woof woof
```

#### The Keyword Self

Em um método de classe, `self` representa a classe polimorficamente. Isso significa que, em um método de classe, você pode enviar uma mensagem para si mesmo para chamar um inicializador polimorficamente. Aqui está um exemplo. Digamos que queremos mover nosso método de fábrica de instâncias para o próprio Dog, como um método de classe. Vamos chamar esse método de classe de `makeAndName`. Nós queremos que este método de classe crie e retorne um Dog nomeado de qualquer classe para a qual enviamos a mensagem `makeAndName`. Se executarmos `Dog.makeAndName()`, devemos recuperar um Dog. Se execurtamos `NoisyDog.makeAndName()`, devemos obter um `NoisyDog`. Portanto, nosso método de classe `makeAndName` inicializa o `self` polimórfico:

```swift
class Dog
{
    var name : String required init(name:String) {
        self.name = name
    }

    class func makeAndName() -> Dog
    {
        let d = self.init(name:"Fido")
        return d
    }
}

class NoisyDog : Dog { }
```

Funciona como esperado:

```swift
let d = Dog.makeAndName() // d is a Dog named Fido
let d2 = NoisyDog.makeAndName() // d2 is a NoisyDog named Fido
```

Embora o exemplo anterior funcione, há um problema. Embora `d2` seja de fato um `NoisyDog`, ele é _typed_ como um Dog. Isso ocorre porque nosso método de classe `makeAndName` é declarado como retornando um `Dog`. Isso não é o que queremos declarar. O que queremos declarar é que esse método retorna uma instância do mesmo tipo da classe para a qual a mensagem `makeAndName` foi originalmente enviada. Em outras palavras, precisamos de uma declaração do tipo polimórfica! Esse tipo é `Self` (observe a capitalização). O tipo `Self` é usado como um tipo de retorno em uma declaração de método para significar “uma instância de qualquer tipo que esteja em tempo de execução”. Assim:

```swift
class Dog
{
    var name : String required init(name:String) {
        self.name = name
    }

    class func makeAndName() -> Self
    {
        let d = self.init(name:"Fido")
        return d
    }
}

class NoisyDog : Dog { }
```

Agora, quando chamamos `NoisyDog.makeAndName()`, temos um `NoisyDog` _typed_ como um `NoisyDog`.

(Nosso exemplo anterior, a função global de fábrica `dogMakerAndNamer`, exibe o mesmo problema - ele também retorna um objeto typed como Dog, mesmo que a instância subjacente seja de fato um `NoisyDog`. Não podemos usar `Self` para resolver o problema aqui, porque há não há nenhum tipo de referência. O Swift tem uma solução, no entanto - genéricos.

`Self` também funciona por exemplo, declarações de método. Portanto, podemos escrever uma versão de método de instância do nosso método de fábrica. Aqui, começamos com um Dog ou um NoisyDog e dizemos para ele ter um filhote do mesmo tipo que ele:

```swift
class Dog
{
    var name : String required init(name:String) {
        self.name = name
    }

    func havePuppy(name:String) -> Self
    {
        return type(of:self).init(name:name)
    }
}

class NoisyDog : Dog { }
```

```swift
let d = Dog(name:"Fido")
let d2 = d.havePuppy(name:"Fido Junior")
let nd = NoisyDog(name:"Rover")
let nd2 = nd.havePuppy(name:"Rover Junior")
```

As expected, d2 is a Dog, but nd2 is a NoisyDog typed as a NoisyDog.

### Protocols

> Um protocolo é uma maneira de expressar semelhanças entre tipos não relacionados.

Um protocolo é um tipo de objeto, mas não há objetos de protocolo - você não pode instanciar um protocolo. Um protocolo é muito mais leve que isso. Uma declaração de protocolo é apenas uma lista de propriedades e métodos. As propriedades não têm valores e os métodos não têm código! A ideia é que um tipo de objeto “real” possa declarar formalmente que pertence a um tipo de protocolo; isso é chamado de adoção ou conformidade com o protocolo. Um tipo de objeto que implemente um protocolo está assinando um contrato informando que ele realmente implementa as propriedades e os métodos listados pelo protocolo.

#### Declaring a Protocol

A declaração do protocolo pode ocorrer somente no nível superior de um arquivo. Para declarar um protocolo, use a palavra-chave `protocol` seguido pelo nome do protocolo, que, sendo um tipo de objeto, deve começar com uma letra maiúscula. Então vem chaves que podem conter o seguinte:

**Properties**

Uma declaração de propriedade em um protocolo consiste em `var` (não `let`), o nome da propriedade, dois pontos, seu tipo e chaves, contendo a palavra `get` ou as palavras `get` `set`. No primeiro caso, a implementação do adotante dessa propriedade pode ser gravável, enquanto no último caso, deve ser: o adotante não pode implementar uma propriedade `get` `set` como uma propriedade computada somente leitura ou como uma propriedade armazenada constante (`let`).

Para declarar uma propriedade static/class, preceda-a com a palavra-chave `static`. Um adotante de classe é livre para implementar isso como uma propriedade de classe.

**Methods**

Uma declaração de método em um protocolo é uma declaração de função sem um corpo de função - ou seja, não possui chaves e, portanto, não possui código. Qualquer tipo de função de objeto é legal, incluindo `init` e `subscript`. (A sintaxe para declarar um subscrito em um protocolo é igual à sintaxe para declarar um subscrito em um tipo de objeto, exceto que não haverá corpos de função, portanto, as chaves, como aquelas de uma declaração de propriedade em um protocolo, contêm `get` ou `get` `set`.)

Para declarar um método estático/classe, preceda-o com a palavra-chave `static`. Um adotante de classe é livre para implementar isso como um método de classe.

Para permitir que um enum ou struct adoptante declare um método mutação, declare-o o método no protocolo como `mutating`. Um adoptante não é possível adicionar mutação se o protocolo não tiver, mas o adoptante pode omitir mutação se o protocolo tem.

--------------------

Um protocolo pode implementar um ou mais protocolos; A sintaxe é exatamente como você esperaria - dois pontos após o nome do protocolo na declaração, seguido por uma lista separada dos protocolos que ela implementa. Com efeito, isso permite criar uma hierarquia secundária inteira de tipos!

Um protocolo que implemente outro protocolo pode repetir o conteúdo das chaves do protocolo implementado, para maior clareza; mas não precisa, já que esta repetição é implícita. Um tipo de objeto que implementa um protocolo deve satisfazer os requisitos deste protocolo e todos os protocolos que o protocolo implementa.

#### Composição do Protocolo

Se a única finalidade de um protocolo é combinar outros protocolos implementando todos eles, sem adicionar novos requisitos, você pode evitar formalmente declarar o protocolo em primeiro lugar, especificando o protocolo de combinação em tempo real. Para o fazer, junte os nomes dos protocolos com &. Isso é chamado de composição de protocolo. Por exemplo:

```swift
func f(_ x: CustomStringConvertible & CustomDebugStringConvertible) { }
```

Essa é uma declaração de função com um parâmetro cujo tipo é especificado como sendo algum tipo de objeto que implementa o protocolo `CustomStringConvertible` e o protocolo `CustomDebugStringConvertible`.

A partir do Swift 4, um tipo também pode ser especificado como um composto de uma classe e um protocolo (ou múltiplos protocolos). Um caso típico em questão pode ser algo como isto:

```swift
protocol MyViewProtocol : class
{
    func doSomethingCool()
}

class ViewController: UIViewController
{
    var v: (UIView & MyViewProtocol)?
    // ...
}
```

Nesse código, a propriedade `v` do `ViewController` é typed como um `Optional` que envolve um composto do `UIView` e `MyViewProtocol`. Para ser atribuído à propriedade `v`, um objeto precisaria ser uma instância de uma subclasse UIView que também é um adotante do `MyViewProtocol`. O próprio UIView pertence ao Cocoa e não implementa o MyViewProtocol; mas podemos facilmente subclassificar o UIView e fazer com que essa subclasse o implemente (ou, como explicarei mais adiante, podemos estender uma subclasse interna do UIView para adotá-la).

Desta forma, garantimos ao compilador que tanto as mensagens do UIView quanto as mensagens do MyViewProtocol podem ser enviadas para o `v` de ViewController. Isso, no final das contas, é uma coisa bastante comum que se deseja poder fazer; sem esse recurso, você teria que type `v` como um `MyViewProtocol` e, em seguida, converter para o UIView para enviar mensagens do UIView, mesmo sabendo que `v` seria, de fato, sempre um UIView.

#### Class Protocol

Um protocolo declarado com a palavra-chave `class` após os dois-pontos após seu nome é um protocolo de classe, o que significa que ele pode ser implementado apenas pelos tipos de objeto de classe:

```swift
protocol SecondViewControllerDelegate : class
{
    func accept(data:Any!)
}
```

Um motivo típico para declarar um protocolo de classe é tirar proveito dos recursos de gerenciamento de memória especial que se aplicam somente às classes.

```swift
class SecondViewController : UIViewController
{
    weak var delegate : SecondViewControllerDelegate?
    // ...
}
```

A palavra-chave `weak` marca a propriedade `delegate` como tendo um gerenciamento de memória especial que se aplica apenas às instâncias de classe. A propriedade `delegate` é typed como um protocolo e um protocolo pode ser implementado por um tipo struct ou enum. Então, para satisfazer o compilador que este objeto será de fato uma instância de classe, e não uma instância struct ou enum, o protocolo é declarado como um protocolo de classe.

No Swift 4, você pode, alternativamente, aproveitar a composição do protocolo de classe para realizar a mesma coisa:

```swift
class SecondViewController : UIViewController
{
    weak var delegate : (NSObject & SecondViewControllerDelegate)?
    // ...
}
```

Isso é legal mesmo que `SecondViewControllerDelegate` não tenha `class` em sua designação, porque o compilador sabe que o objeto atribuído à propriedade `delegate` será derivado de `NSObject`, caso em que é uma instância de classe.

#### Implicitly Required Initializers

Suponha que um protocolo declare um inicializador. E suponha que uma classe implemente esse protocolo. Pelos termos deste protocolo, esta classe e qualquer subclasse que ela possa ter, deve implementar este inicializador. Portanto, a classe deve não apenas implementar o inicializador, mas também deve marcá-lo como `required`. Um inicializador declarado em um protocolo é, portanto, implicitamente requerido, e a classe é forçada a tornar esse requisito explícito.

Considere este exemplo simples, que não compilará:

```swift
protocol Flier
{
    init()
}

class Bird : Flier
{
    init() {} // compile error
}
```

Esse código gera uma mensagem de erro de compilação elaborada mas perfeitamente informativa: "Initializer requirement init() can only be satisfied by a required initializer in nonfinal class Bird." Para compilar nosso código, devemos designar nosso inicializador conforme necessário:

```swift
protocol Flier
{
    init()
}

class Bird : Flier
{
    required init() {}
}
```

A alternativa, como nos informa a mensagem de erro de compilação, seria marcar a classe `Bird` como `final`. Isto significaria que não pode ter nenhuma subclasse - garantindo assim que o problema nunca surgirá em primeiro lugar. Se Bird fosse marcado como final, não haveria necessidade de marcar seu `init` conforme necessário.

No código acima, `Bird` não está marcado como final e seu `init` é marcado como `required`. Isso, como já expliquei, significa que qualquer subclasse de `Bird` que implemente quaisquer inicializadores designados - e, portanto, perde a herança do inicializador - deve implementar o inicializador necessário e marcá-lo como `required` também.

Esse fato é responsável por uma característica estranha e irritante da programação iOS na vida real com o Swift. Digamos que você subclasse o `UIViewController` da classe Cocoa integrado, algo que é extremamente provável que você faça. E digamos que você forneça à sua subclasse um inicializador - algo que também é extremamente provável que você faça:

```swift
class ViewController: UIViewController
{
    init()
    {
        super.init(nibName: "ViewController", bundle: nil)
    }
}
```

Esse código não será compilado. O erro de compilação diz: “required initializer init(coder:) must be provided by subclass of UIViewController.”

O que esta acontecendo aqui? Acontece que o `UIViewController` implementa um protocolo, o `NSCoding`. E este protocolo requer um `init` `init(coder:)`. Nada disso é seu feito; UIViewController e NSCoding são declarados pelo Cocoa, não por você. Mas isso não importa! Esta é a mesma situação que eu estava descrevendo. Sua subclasse UIViewController deve herdar o init `init(coder:)` ou explicitamente implementá-lo e marcá-lo como `required`. Bem, sua subclasse implementou um inicializador designado - portanto, cortando a herança do inicializador. Portanto, ele deve implementar `init(coder:)` e marcá-lo como `required`.

Mas isso não faz sentido se você não está esperando que `init(coder:)` seja chamado na sua subclasse UIViewController. Você está sendo forçado a escrever um inicializador para o qual você não pode fornecer nenhuma funcionalidade significativa! Felizmente, o recurso Fix-it do Xcode se oferecerá para escrever o inicializador para você, assim:

```swift
required init?(coder aDecoder: NSCoder)
{
    fatalError("init(coder:) has not been implemented")
}
```

Esse código satisfaz o compilador. Ele também crashes deliberadamente se é chamado - o que é bom, porque ex hypothesi você não espera que ele nunca ser chamado. Se, por outro lado, você tiver funcionalidade para este inicializador, você excluirá a linha `fatalError` e inserirá essa funcionalidade em seu lugar. Uma implementação mínima significativa seria chamar `super.init(coder:aDecoder)`, mas é claro que se sua classe tiver propriedades que precisem de inicialização, será necessário inicializá-las primeiro.

Não apenas o UIViewController, mas muitas classes internas do Cocoa adotam o `NSCoding`. Você encontrará esse problema se subclassificar qualquer uma dessas classes e implementar seu próprio inicializador. É algo com o qual você terá que se acostumar.

#### Literal Convertibles

Uma das coisas maravilhosas sobre Swift é que tantas de suas características, ao invés de ser construído-em e realizado por magia, são expostos à vista no cabeçalho Swift. Os literals são um caso no ponto. A razão que você pode dizer `5` para fazer um Int cujo valor é 5, em vez de formalmente inicializar Int, dizendo `Int(5)`, não é por causa da magia (ou pelo menos, não inteiramente por causa da magia). É porque Int adota um protocolo, `ExpressibleByIntegerLiteral`. Não apenas literais Int, mas todos os literais funcionam dessa maneira. Os seguintes protocolos são declarados no cabeçalho Swift:

- `ExpressibleByNilLiteral`
- `ExpressibleByBooleanLiteral`
- `ExpressibleByIntegerLiteral`
- `ExpressibleByFloatLiteral`
- `ExpressibleByStringLiteral`
- `ExpressibleByExtendedGraphemeClusterLiteral`
- `ExpressibleByUnicodeScalarLiteral`
- `ExpressibleByArrayLiteral`
- `ExpressibleByDictionaryLiteral`

Seu próprio tipo de objeto pode implementar um protocolo conversível literal também. Isso significa que um literal pode aparecer onde uma instância do seu tipo de objeto é esperada! Por exemplo, aqui nós declaramos um tipo de `Nest` que contém algum número de ovos (seu `eggCount`):

```swift
struct Nest : ExpressibleByIntegerLiteral
{
    var eggCount : Int = 0

    init() {}
    init(integerLiteral val: Int)
    {
        self.eggCount = val
    }
}
```

Como o `Nest` adota `ExpressibleByIntegerLiteral`, podemos passar um Int onde um Nest é esperado, e nosso `init(integerLiteral:)` será chamado automaticamente, fazendo com que um novo objeto Nest com o `eggCount` especificado venha a existir naquele momento:

```swift
func reportEggs(_ nest:Nest)
{
    print("this nest contains \(nest.eggCount) eggs")
}

reportEggs(4) // this nest contains 4 eggs
```

### Generics

Um genérico é uma espécie de espaço reservado para um tipo, no qual um tipo real será dividido posteriormente. Em particular, há situações em que você quer dizer que certo mesmo tipo deve ser usado em vários lugares, sem especificar precisamente que tipo de objeto deve ser. Os genéricos Swift permitem que você diga isso sem sacrificar ou evitar a tipagem estrita fundamental do Swift.

### Extensions

Uma extensão é uma maneira de injetar seu próprio código em um tipo de objeto que já foi declarado em outro lugar; você está estendendo um tipo de objeto existente. Você pode estender seus próprios tipos de objetos; você também pode estender um dos tipos de objeto do Swift ou um dos tipos de objeto do Cocoa, caso em que você está adicionando funcionalidade a um tipo que não pertence a você!

Alguma restrições das extensões:

- Uma extensão não pode substituir um membro existente (mas pode sobrecarregar um método existente).
- Uma extensão não pode declarar uma propriedade armazenada (mas pode declarar uma propriedade computada).
- Uma extensão de uma classe não pode declarar um inicializador designado ou um deinitializer (mas pode declarar um convenience initializer).

#### Extending Object Types

Na minha vida real de programação, às vezes eu estendo um tipo Swift ou Cocoa embutido apenas para encapsular algumas funcionalidades ausentes expressando-as como uma propriedade ou método. Aqui estão alguns exemplos de aplicativos reais.

Em um jogo de cartas, eu preciso embaralhar o baralho, que é armazenado em uma matriz. Eu estendo o tipo de Array embutido do Swift para criar um método `shuffle`:

```swift
extension Array
{
    mutating func shuffle ()
    {
        for i in (0..<self.count).reversed() {
            let ix1 = i
            let ix2 = Int(arc4random_uniform(UInt32(i+1)))
            self.swapAt(ix1, ix2)
        }
    }
}
```

#### Extending Protocols

Ao estender um protocolo, você pode adicionar métodos e propriedades ao protocolo, assim como para qualquer tipo de objeto. Ao contrário de uma declaração de protocolo, esses métodos e propriedades não são meras exigências, a serem cumpridas pelo adoptante do protocolo; eles são métodos e propriedades reais, para serem herdados pelo adotante do protocolo! Por exemplo:

```swift
protocol Flier { }

extension Flier
{
    func fly()
    {
        print("flap flap flap")
    }
}

struct Bird : Flier { }
```

Observe que `Bird` agora pode adotar o `Flier` sem implementar o método `fly`. Isso porque a extensão do protocolo `Flier` fornece o método `fly`! Bird thus inherits an implementation of fly:

```swift
let b = Bird()
b.fly() // flap flap flap
```

Naturalmente, um adotante ainda pode fornecer sua própria implementação de um método herdado de uma extensão de protocolo:

```swift
protocol Flier { }

extension Flier
{
    func fly()
    {
        print("flap flap flap")
    }
}

struct Insect : Flier
{
    func fly()
    {
        print("whirr")
    }
}

let i = Insect()
i.fly() // whirr
```

Mas seja advertido: este tipo de herança não é polimorfórico. A implementação do adotante não é uma substituição; é meramente uma outra implementação. A regra de identidade interna não se aplica; importa como uma referência é tipada:

```swift
let f : Flier = Insect()
f.fly() // flap flap flap (!!)
```

Mesmo que `f` é internamente um `Insect` (como podemos descobrir com o operador `is`), a mensagem de `fly` está sendo enviada para uma referência de objeto tipado como `Flier`, por isso é a implementação de `Flier` do método `fly` que é chamado, não a implementação de `Insect`.

Para obter algo que se pareça com a herança pofórico, devemos declarar `fly` como um requisito no protocolo original:

```swift
protocol Flier
{
    func fly() // *
}

extension Flier
{
    func fly()
    {
        print("flap flap flap")
    }
}

struct Insect : Flier
{
    func fly()
    {
        print("whirr")
    }
}
```

Agora `Insect` mantém sua integridade interna:

```swift
let f : Flier = Insect()
f.fly() // whirr
```

### Umbrella Types

O Swift fornece alguns tipos integrados types as general umbrella types, capazes de abranger vários tipos reais sob um único conjunto.

#### Any

O tipo Any é o tipo universal de umbrella Swift. Onde um objeto Any é esperado, absolutamente qualquer objeto ou função pode ser passado, sem conversão:

```swift
func anyExpecter(_ a:Any) {}
anyExpecter("howdy") // a struct instance
anyExpecter(String.self) // a struct type
anyExpecter(Dog()) // a class instance
anyExpecter(Dog.self) // a class type
anyExpecter(anyExpecter) // a function
```

Indo para o outro lado, se você quiser tipar um objeto como um tipo mais específico, você geralmente terá que fazer cast down. Tal conversão é legal para qualquer tipo de objeto específico ou tipo de função. A forced cast não é seguro, mas você pode facilmente torná-lo seguro, porque você também pode testar o objeto contra qualquer tipo de objeto específico ou tipo de função. Aqui, `anything` é tipado como Any:

```swift
if anything is String {
    let s = anything as! String // ...
}
```

#### AnyObject

AnyObject is an empty protocol (requiring no properties or methods) with the special feature that all class types conform to it automatically. Although Objective-C APIs present Objective-C id as Any in Swift, Swift AnyObject is Objective-C id. AnyObject is useful primarily when you want to take advantage of the behavior of ObjectiveC id, as I’ll demonstrate in a moment.

A class type can be assigned directly where an AnyObject is expected; to retrieve it as its original type, you’ll need to cast down:

```swift
class Dog { }

let d = Dog()
let anyo : AnyObject = d
let d2 = anyo as! Dog
```

Assigning a nonclass type to an AnyObject requires casting (with `as`). The bridge to Objective-C is then crossed immediately, as I described for Any in the preceding section:

```swift
let s = "howdy" as AnyObject // String to NSString to AnyObject
let i = 1 as AnyObject // Int to NSNumber to AnyObject
let r = CGRect() as AnyObject // CGRect to NSValue to AnyObject
let d = Date() as AnyObject // Date to NSDate to AnyObject
let b = Bird() as AnyObject // Bird (struct) to boxed type to AnyObject
```

#### AnyClass

AnyClass is the type of AnyObject. It corresponds to the Objective-C Class type. It arises typically in declarations where a Cocoa API wants to say that a class is expected.

For example, the UIView layerClass class property is declared, in its Swift translation, like this:

```swift
class var layerClass : AnyClass {get}
```

That means: if you override this class property, implement your getter to return a class. This will presumably be a CALayer subclass. To return an actual class in your implementation, send the self message to the name of the class:

```swift
override class var layerClass : AnyClass {
    return CATiledLayer.self
}
```

## Life Cycle of a Project
