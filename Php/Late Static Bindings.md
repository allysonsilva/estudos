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
