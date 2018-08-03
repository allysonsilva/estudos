# Object Basics PHP / Classes e objetos

## Resumo

- Uma classe é um modelo de código usado para gerar um ou mais objetos.
- Se uma classe é um modelo para gerar objetos, segue-se que um objeto é um dado que foi estruturado de acordo com o modelo definido em uma classe. Um objeto é dito ser uma instância da sua classe. É do tipo definido pela classe.
- Propriedades de classes devem ser declarados como públicas, privadas ou protegidas.
- Ao declarar uma propriedade, deve ser precedida pelo seu modificador de visibilidade (`public`, `protected` ou `private`), determinando o escopo pela qual a propriedade deve ser acessada.
- Métodos de classe podem ser definidos como público, privado ou protegido. Métodos sem qualquer declaração explícita serão definidos como público implicitamente.
- Assim como as propriedades permitem que seus objetos armazenem dados, os métodos permitem que seus objetos executem tarefas.
- Um método de construtor é invocado quando um objeto é criado. Você pode usá-lo para configurar as coisas, garantindo que as propriedades essenciais sejam atribuídas valores e qualquer trabalho preliminar necessário seja concluído.

### Working with Interceptors

- `__get($property)` => Invoked when an undefined property is accessed
- `__set($property, $value)` => Invoked when a value is assigned to an undefined property
- `__isset($property)` => Invoked when isset() is called on an undefined property
- `__unset($property)` => Invoked when unset() is called on an undefined property
- `__call($method, $arg_array)` => Invoked when an undefined non-static method is called
- `__callStatic($method, $arg_array)` => Invoked when an undefined static method is called

The `__get()` and `__set()` methods are designed for working with properties that have not been declared in a class (or its parents).

The `__set()` method is invoked when client code attempts to assign to an undefined property. It is passed two arguments: the name of the property and the value the client is attempting to set. You can then decide how to work with these arguments.

The `__isset()` method works in a similar way to `__get()`. It is invoked after the client calls `isset()` on an undefined property.

As you might expect, `__unset()` mirrors `__set()`. When `unset()` is called on an undefined property, `__unset()` is invoked with the name of the property. You can then do what you like with the information.

The `__call()` method is probably the most useful of all the interceptor methods. It is invoked when an undefined method is called by client code. `__call()` is invoked with the method name and an array holding all arguments passed by the client. Any value that you return from the `__call()` method is returned to the client as if it were returned by the method invoked.

### Overriding Property Accesses

_Problem_

You want handler functions to execute whenever you read and write object properties. This lets you write generalized code to handle property access in your class.

_Solution_

Use the magic methods `__get()` and `__set()` to intercept property requests.

To improve this abstraction, also implement `__isset()` and `__unset()` methods to make the class behave correctly when you check a property using `isset()` or delete it using `unset()`.

_Discussion_

Property overloading allows you to seamlessly obscure from the user the actual location of your object’s properties and the data structure you use to store them.

For example, the Person class stores variables in an array, `$__data`. (The name of the variable doesn’t need begin with two underscores, that’s just to indicate to you that it’s used by a magic method.)

```php
class Person
{
    private $__data = array();

    public function __get($property)
    {
        if (isset($this->__data[$property])) {
            return $this->__data[$property];
        }

        return false;
    }

    public function __set($property, $value)
    {
        $this->__data[$property] = $value;
    }
}

$allyson = new Person;
$allyson->email = 'allyson@mail.com';   // sets $user->__data['email']
print $allyson->email;                  // reads $user->__data['email']
```

Using these methods and an array as the alternate variable storage source makes it less painful to implement object encapsulation. Instead of writing a pair of accessor methods for every class property, you use `__get()` and `__set()`.

With `__get()` and `__set()`, you can use what appear to be public properties, such as $johnwood->name, without violating encapsulation. This is because the programmer isn’t reading from and writing to those properties directly, but is instead being routed through accessor methods.

The `__get()` method takes the property name as its single parameter. Within the method, you check to see whether that property has a value inside `$__data`. If it does, the method returns that value; otherwise, it returns `false`.

The `__set()` method takes two arguments: the property name and the new value. Otherwise, the logic inside the method is similar to `__get()`.

Besides reducing the number of methods in your classes, these magical methods also make it easy to implement a centralized set of input and output validation.

The `__isset()` method checks inside the $data element and returns true or false depending on the status of the property you’re checking.

Likewise, `__unset()` passes back the value of `unset()` applied to the real property, or false if it’s not set.

Implementing these two methods isn’t required when using `__get()` and `__set()`, but it’s best to do so because it’s hard to predict how you may use object properties. Failing to code these methods will lead to confusion when someone (perhaps even yourself) doesn’t know (or forgets) that this class is using magic accessor methods.

Other reasons to consider not using magical accessors are:

- They’re relatively slow. They’re both slower than direct property access and explicitly writing accessor methods for all your properties.
- They make it impossible for the Reflection classes and tools such as phpDocumentor to automatically document your code.
- You cannot use them with static properties.

### Defining Static Properties and Methods

Static methods do not operate on a specific instance of the class where they’re defined. PHP does not “construct” a temporary object for you to use while you’re inside the method. Therefore, you cannot refer to $this inside a static method, because there’s no $this on which to operate. Calling a static method is just like calling a regular function.

There’s a potential complication from using `self::`. It doesn’t follow the same inheritance rules as nonstatic methods. In this case, `self::` always attaches the reference to the class it’s defined in, regardless whether it’s invoked from that class or from a child.

Use `static::` to change this behavior, such as in this ORM example:

```php
class Model
{
    protected static function validateArgs($args)
    {
        throw new Exception("You need to override this in a subclass!");
    }

    public static function find($args)
    {
        static::validateArgs($args);
    }
}

class Bicycle extends Model
{
    protected static function validateArgs($args)
    {
        return true;
    }
}

Bicycle::find(['owner' => 'peewee']);
```

With `self::`, PHP binds to `Model::validateArgs()`, which doesn’t allow for model- specific validation. However, with `static::`, PHP will defer until it knows which class the method is actually called from. This is known as late static binding.

Inside of `find()`, to generate your SQL, you need the name of the calling class. You cannot use the `Reflection` classes and the `__CLASS__` constant because they return Model, so use `get_called_class()` to pull this at runtime.

PHP also has a feature known as static properties. Every instance of a class shares these properties in common. Thus, static properties act as class-namespaced global variables.

One reason for using a static property is to share a database connection among multiple Database objects. For efficiency, you shouldn’t create a new connection to your database every time you instantiate Database.

### Primitive Types

| **Type Checking Function** | **Type**   | **Description**                                                                         |
|----------------------------|------------|-----------------------------------------------------------------------------------------|
| `is_bool()`                | *Boolean*  | One of the two special values, `true` or `false`                                        |
| `is_integer()`             | *Integer*  | A whole number. Alias of `is_int()` and `is_long()`                                     |
| `is_double()`              | *Double*   | A floating point number (a number with a decimal point). Alias of `is_float()`            |
| `is_string()`              | *String*   | Character data                                                                          |
| `is_object()`              | *Object*   | An object                                                                               |
| `is_array()`               | *Array*    | An array                                                                                |
| `is_resource()`            | *Resource* | A handle for identifying and working with external resources such as databases or files  |
| `is_null()`                | *Null*     | An unassigned value                                                                     |

### Taking the Hint (Type hinting): Object Types

Os tipo de declarações são uma maneira de expressar restrições nos valores dos argumentos. Os tipos dizem ao mecanismo PHP que tipo de valor é permitido para um argumento para que ele possa avisá-lo quando o tipo errado for fornecido.

| **Type declaration** | **Minimum PHP version** | **Description**                                                                                                                                                        |
|----------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `array`              | *5.1*                   | Must be an array. Can default to `null` or an `array`                                                                                                                  |
| `int`                | *7.0*                   | An integer. Can default to `null` or an `integer`                                                                                                                      |
| `float`               | *7.0*                   | A floating point number (a number with a decimal point). An integer will be accepted—even with strict mode enabled. Can default to `null`, a `float`, or an `integer`    |
| `callable`           | *5.4*                   | Callable code (such as an anonymous function). Can default to `null`                                                                                                   |
| `bool`               | *7.0*                   | Must be boolean: `true` or `false`. Can default to `null` or a Boolean                                                                                                 |
| `string`             | *5.0*                   | Character data. Can default to `null` or a `string`                                                                                                                    |
| `self`               | *5.0*                   | A reference to the containing class                                                                                                                                    |
| `[a class type]`     | *5.0*                   | The type of a class or interface. Can default to `null`                                                                                                                |

> A `strict_types` declaration applies to the file from which a call is made, and not to the file in which a function or method is implemented. So it’s up to client code to enforce strictness.

### Herança

- A herança é o meio pelo qual uma ou mais classes podem ser derivadas de uma classe base.
- <subclasse> é uma espécie de <superclasse>
- Uma subclasse herda os métodos e propriedades públicos e protegidos da sua classe base.
- Para criar uma subclasse, deve-se usar a palavra-chave `extends` na declaração de classe.
- Para invocar um método na superclasse utilize o identificador `parent`. Ex.: `parent::__construct()`, `parent::methodName()`.

### Modificador de visibilidade

**Propriedades e métodos públicas podem ser acessados ​​a partir de qualquer contexto.**
**Um membro de uma classe(Métodos ou proprieddes) privado só pode ser acessado a partir da classe em que foi definido. As subclasses não têm acesso a membros privados, somente a públicos ou protegidos.**
**Um método ou propriedade protegida(`protected`) só pode ser acessada a partir da classe em que foi definido ou de uma subclasse. Não sendo possível acessar o membro por meio de instâncias(`new ...`).**

### Propriedades e Métodos Estáticos

Você pode acessar métodos e propriedades no contexto de uma classe e não de um objeto.
Tais métodos e propriedades são estáticas e devem ser declarados usando a palavra-chave `static`:

```php
class StaticExample
{
    static public $staticProperty = 0;

    public static function staticMethod()
    {
        print "Hello";
    }
}
```

- Os métodos estáticos são funções com o escopo da classe, assim, todos os objetos, instâncias, podem acessar esse membro estático(de acordo com o modificador de visibilidade).
- Membros estáticos não podem acessar quaisquer propriedades que não sejam estáticas, porque estas pertencem a um objeto não a classe.
- Membros estáticos podem acessar apenas outros membros estáticos.
- Se você alterar uma propriedade estática, todas as instâncias dessa classe podem acessar o novo valor.
- Fazer uma chamada de método usando o `parent` é a única circunstância na qual você deve usar uma referência estática para um método não estático.
- A menos que você esteja acessando um método substituído, você só deve usar `::` para acessar um método ou propriedade que tenha sido explicitamente declarado estático.

Como você acessa um elemento estático via uma classe e não uma instância, não precisa de uma variável que faça referência a um objeto.
Em vez disso, você usa o nome da classe em conjunto com `::`.

```php
print StaticExample::$aNum;
StaticExample::sayHello();
```

### Constant Properties

- Deve ser definido um valor no momento de sua declaracao.
- São implicitamente estáticas.
- Não podem ser alteradas uma vez que são definidas.
- As propriedades constantes devem conter apenas valores primitivos.
- Por convencao, são nomeados usando apenas caracteres maiusculos tendo suas palavras separadas por '_'(underline).
- Como propriedades estáticas, as propriedades constantes são acessadas através da classe e não uma instância.
- Uma propriedade constante é declarada com a palavra-chave `const`. As constantes não são prefixadas com um sinal de dólar como propriedades comuns.

### Abstract Classes

- Uma classe abstrata não pode ser instanciada. Qualquer tentativa de instânciar a classe gerará um erro.
- Serve de classe base para subclasses.
- Você define uma classe abstrata com a palavra-chave `abstract`.
- Métodos abstratos somente podem estar em classes abstratas.
- Um método abstrato, não tem implementacao na classe em que é declarado, apenas sua "assinatura" para que a subclasse(herda da classe abstrata), implemente obrigatoriamente o método abstrato.
- Para declarar um método como abstrato usa-se a palavra-chave `abstract` assim como a classe.
- Métodos abstratos não tem implementacao na classe em que é declarada, apenas sua declaracao comum comecando com `abstract`.
- Ao criar um método abstrato você garante que a implementacao estará disponível em todas as classes concretas(que podem ser instânciadas).
- Qualquer classe que extenda uma classe abstrata deve implementar todos os métodos abstratos, a não ser que a mesma(subclasse) tambêm seja declarada como abstrata.

### Interfaces

- Pode conter declaracoes de propriedades e métodos, mais não corpo do método, pois isso é responsabilidade da classe que implementa a interface.
- Um interface é declarada utilizando a palavra-chave `interface`.
- Uma classe pode implementar uma interface usado a palavra-chave `implements`.
- Métodos em uma interface são sempre públicos por padrão.

### Traits

- Permite compartilhar uma implementacão para diferentes classes.
- A mesma implementacao pode ser vista por diferentes classes, tendo um ponto central para determinada funcionalidade, assim, toda alteracao refletira nas classes que estão usando essa `trait`.
- É uma estrutura como uma classe, que não pode ser instânciada, mais pode ser incorporada nas classes, usando seus métodos e atributos.
- Qualquer método ou propriedade definido em uma `trait` se torna disponível como parte de qualquer classe que o use.
- Uma `trait` alterar a estrutura de uma classe mais não seu tipo.
- Resolve o problema de duplicacao de código.
- Uma classe pode ter várias `traits`, separadas por vírgula.
- Pode ser acessado propriedades e métodos da classe em que a trait foi declarada, ou seja, a trait pode usar membros da classe em que a mesma foi declarada.
- Pode ser definido métodos abstratos da mesma maneira como se define em uma classe. Quando uma trait é usada por uma classe, ela assume o compromisso de implementar o método abstrato definido na trait.
- Pode ser declarado métodos como o mesmo nível de visibilidade como em uma classe(public, private e protected).

### Final Classes and Methods

- Uma classe final não pode ser subclassada.
- Métodos finais não podem ser subistituidos.

## Traits

Many of my PHP developer friends are confused by traits, a new concept introduced in PHP 5.4.0. Traits behave like classes but look like interfaces. Which one are they? Neither and both.

A trait is a partial class implementation (i.e., constants, properties, and methods) that can be mixed into one or more existing PHP classes. Traits work double duty: they say what a class can do (like an interface), and they provide a modular implementation (like a class).

### Why We Use Traits

The PHP language uses a classical inheritance model. This means you start with a single generalized root class that provides a base implementation. You extend the root class to create more specialized classes that inherit their immediate parent’s implementation. This is called an inheritance hierarchy, and it is a common pattern used by many programming languages.

*If it helps, picture yourself back in grade school Biology. Remember how you learned about the biological classification system? There are six kingdoms. Each kingdom is extended by phyla. Each phylum is extended by biological classes. Classes are extended by orders, orders by families, families by genera, and genera by species. Each hierarchy extension represents further specialization*.

The classical inheritance model works well most of the time. However, what do we do if two unrelated PHP classes need to exhibit similar behavior? For example, a PHP class RetailStore and another PHP class Car are very different classes and don’t share a common parent in their inheritance hierarchies. However, both classes should be geocodable into lat
## Generators

PHP generators are an underutilized yet remarkably helpful feature introduced in PHP 5.5.0. I think many PHP developers are unaware of generators because their purpose is not immediately obvious. Generators are simple iterators. That’s it.

Unlike your standard PHP iterator, PHP generators don’t require you to implement the Iterator interface in a heavyweight class. Instead, generators compute and yield iteration values on-demand. This has profound implications for application performance. Think about it. A standard PHP iterator often iterates in-memory, precomputed data sets. This is inefficient, especially with large and formulaic data sets that can be computed instead. This is why we use generators to compute and yield subsequent values on the fly without commandeering valuable memory.

*PHP generators are not a panacea for your iteration needs. Because generators never know the next iteration value until asked, it’s impossible to rewind or fast-forward a generator. You can iterate in only one direction—forward. Generators are also a once-and-done deal. You can’t iterate the same generator more than once. However, you are free to rebuild or clone a generator if necessary*.

### Create a Generator

Generators are easy to create because they are just PHP functions that use the yield keyword one or more times. Unlike regular PHP functions, generators never return a value. They only yield values.

```php
function myGenerator()
{
    yield 'value1';
    yield 'value2';
    yield 'value3';
}
```

Pretty simple, huh? When you invoke the generator function, PHP returns an object that belongs to the Generator class. This object can be iterated with the foreach() function. During each iteration, PHP asks the Generator instance to compute and provide the next iteration value. What’s neat is that the generator pauses its internal state whenever it yields a value. The generator resumes internal state when it is asked for the next value. The generator continues pausing and resuming until it reaches the end of its function definition or an empty return; statement. We can invoke and iterate the generator in like this:

```php
foreach (myGenerator() as $yieldedValue)
{
    echo $yieldedValue, PHP_EOL;
}

// value1
// value2
// value3
```

### Use a Generator

I like to demonstrate how a PHP generator saves memory by implementing a simple range() function. First, let’s do it the wrong way.

```php
function makeRange($length)
{
    $dataset = [];

    for ($i = 0; $i < $length; $i++) {
        $dataset[] = $i;
    }

    return $dataset;
}

$customRange = makeRange(1000000);

foreach ($customRange as $i) {
    echo $i, PHP_EOL;
}
```

Example makes poor use of memory. The `makeRange()` method in Example allocates one million integers into a precomputed array. A PHP generator can do the same thing while allocating memory for only one integer at any given time, as shown in Example.

_Range generator (good)_

```php
function makeRange($length)
{
    for ($i = 0; $i < $length; $i++) {
        yield $i;
    }
}

foreach (makeRange(1000000) as $i) {
    echo $i, PHP_EOL;
}
```

This is a contrived example. However, just imagine all of the potential data sets that you can compute. Number sequences (e.g., Fibonacci) are an obvious candidate. You can also iterate a stream resource. Imagine you need to iterate a 4 GB comma- separated value (CSV) file and your virtual private server (VPS) has only 1 GB of memory available to PHP. There’s no way you can pull the entire file into memory.

_CSV generator_

```php
function getRows($file)
{
    $handle = fopen($file, 'rb');

    if ($handle === false) {
        throw new Exception();
    }

    while (feof($handle) === false) {
        yield fgetcsv($handle);
    }

    fclose($handle);
}

foreach (getRows('data.csv') as $row) {
    print_r($row);
}
```

This example allocates memory for only one CSV row at a time instead of reading the entire 4 GB CSV file into memory. It also encapsulates the iteration implementation into a tidy package; this lets us quickly change how we get data (e.g., CSV, XML, JSON) without interrupting our application code that iterates the data.

Generators are a tradeoff between versatility and simplicity. Generators are forward- only iterators. This means you cannot use a generator to rewind, fast-forward, or seek a data set. You can only ask a generator to compute and yield its next value. Generators are most useful for iterating large or numerically sequenced data sets with only a tiny amount of system memory. They are also useful for accomplishing the same simple tasks as larger iterators with less code.

Generators do not add functionality to PHP. You can do what generators do without a generator. However, generators greatly simply certain tasks while using less memory. If you require more versatility to rewind, fast-forward, or seek through a data set, you’re better off writing a custom class that implements the Iterator interface, or using one of PHP’s prebuilt Standard PHP Library (SPL) iterators.
itude and longitude coordinates for display on a map.

Traits were created for exactly this purpose. They enable modular implementations that can be injected into otherwise unrelated classes. Traits also encourage code reuse.

My first (bad) reaction is to create a common parent class Geocodable that both RetailStore and Car extend. This is a bad solution because it forces two otherwise unrelated classes to share a common ancestor that does not naturally belong in either inheritance hierarchy.

My second (better) reaction is to create a Geocodable interface that defines which methods are required to implement the geocoding behavior. The RetailStore and Car classes can both implement the Geocodable interface. This is a good solution that allows each class to retain its natural inheritance hierarchy, but it requires us to duplicate the same geocoding behavior in both classes. This is not a DRY solution.

DRY is an acronym for Do not repeat yourself. It’s considered a good practice never to duplicate the same code in multiple locations. You should not need to change code in one location because you changed code in another location. Read more on Wikipedia.

My third (best) reaction is to create a Geocodable trait that defines and implements the geocodable methods. I can then mix the Geocodable trait into both the Retail Store and Car classes without polluting their natural inheritance hierarchies.

### How to Create a Trait

Here’s how you define a PHP trait:

```php
trait MyTrait
{
    // Trait implementation goes here
}
```

*It is considered a good practice to define only one trait per file, just like class and interface definitions*.

Let’s return to our Geocodable example to better demonstrate traits in practice. We agree both RetailStore and Car classes need to provide geocodable behavior, and we’ve decided inheritance and interfaces are not the best solution. Instead, we create a Geocodable trait that returns latitude and longitude coordinates that we can plot on a map.
