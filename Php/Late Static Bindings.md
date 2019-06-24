# [Late Static Bindings](http://php.net/manual/en/language.oop5.late-static-bindings.php)

PHP lets you access properties or methods without having to create an instance of the class. The keyword used for this purpose is static.

```php
class Model
{
    protected static function validateArgs($args)
    {
        var_dump("Model");
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
        var_dump("Bicycle");
    }
}

Bicycle::find(['foo' => 'bar']);

// [static function find == static::]
// "Bicycle" - Porque é a classe que chama o estático.

// [static function find == self::]
// "Model" - Porque é a classe onde ela é declarada.
```

```php
class Test2
{
    public static $test = 'TEST2';

    public static function getEarlyTest()
    {
        return self::$test;
    }

    public static function getLateTest()
    {
        return static::$test;
    }
}

class Child extends Test2
{
    public static $test = 'CHILD';
}

var_dump(Child::getEarlyTest()); // "TEST2"
var_dump(Child::getLateTest());  // "CHILD"
```

```php
class A
{
    const MY_CONST = false;

    public function my_const_self()
    {
        return self::MY_CONST;
    }

    public function my_const_static()
    {
        return static::MY_CONST;
    }
}

class B extends A
{
   const MY_CONST = true;
}

$b = new B();

echo $b->my_const_self ? 'yes' : 'no';   // output: no
echo $b->my_const_static ? 'yes' : 'no'; // output: yes
```

--------------------------

**Late Static Bindings: The static Keyword**

```php
abstract class DomainObject
{
    public static function create()
    {
        return new self();
    }
}

class User extends DomainObject
{
}

class Document extends DomainObject
{
}

Document::create(); // Error: Cannot instantiate abstract class DomainObject
```

Well, that looks neat. I now have common code in one place, and I've used self as a reference to the class. But I have made an assumption about the `self` keyword. In fact, it does not act for classes exactly the same way that $this does for objects. self does not refer to the calling context; it refers to the context of resolution.

So `self` resolves to `DomainObject`, the place where `create()` is defined, and not to `Document`, the class on which it was called. Until PHP 5.3 this was a serious limitation, which spawned many rather clumsy workarounds. PHP 5.3 introduced a concept called _late static bindings_. The most obvious manifestation of this feature is the keyword: `static`. static is similar to `self`, except that it refers to the invoked rather than the containing class. In this case, it means that calling `Document::create()` results in a new `Document` object and not a doomed attempt to instantiate a `DomainObject` object.

```php
abstract class DomainObject
{
    public static function create(): DomainObject
    {
        return new static();
    }
}

class User extends DomainObject
{
}

class Document extends DomainObject
{
}

print_r(Document::create()); // Document Object()
```

The `static` keyword can be used for more than just instantiation. Like self and `parent`, `static` can be used as an identifier for `static` method calls, even from a non-static context. Let’s say I want to include the concept of a group for my `DomainObject` classes. By default in my new classification, all classes fall into category "default", but I’d like to be able override this for some branches of my inheritance hierarchy:

```php
abstract class DomainObject
{
    private $group;

    public function __construct()
    {
        $this->group = static::getGroup();
    }

    public static function create(): DomainObject
    {
        return new static();
    }

    public static function getGroup(): string
    {
        return "default";
    }
}

class User extends DomainObject
{
}

class Document extends DomainObject
{
    public static function getGroup(): string
    {
        return "document";
    }
}

class SpreadSheet extends Document
{
}

print_r(User::create());        // User Object([group:DomainObject:private] => default)
print_r(SpreadSheet::create()); // SpreadSheet Object([group:DomainObject:private] => document)
```

For the `User` class, not much clever needs to happen. The `DomainObject` constructor calls `getGroup()` and finds it locally. In the case of `SpreadSheet`, though, the search begins at the invoked class, `SpreadSheet` itself. It provides no implementation, so the `getGroup()` method in the `Document` class is invoked. Before PHP 5.3 and late static binding, I would have been stuck with the `self` keyword here, which would only look for `getGroup()` in the `DomainObject` class.

--------------------------

Static methods do not operate on a specific instance of the class where they’re defined.

PHP does not “construct” a temporary object for you to use while you’re inside the method. Therefore, you cannot refer to $this inside a static method, because there’s no $this on which to operate. Calling a static method is just like calling a regular function.

There’s a potential complication from using `self::`. It doesn’t follow the same inheritance rules as nonstatic methods. In this case, `self::` always attaches the reference to the class it’s defined in, regardless whether it’s invoked from that class or from a child.

Use `static::` to change this behavior:

```php
<?php

class Model
{
    protected static function method()
    {
        return __CLASS__;
    }

    public static function getClass()
    {
        return static::method();
    }
}

class Bicycle extends Model
{
    protected static function method()
    {
        return __CLASS__;
    }
}

print_r(Bicycle::getClass()); // Bicycle
```
