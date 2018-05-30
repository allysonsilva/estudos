# Upgrading to PHP 7

## [Sintaxe Uniforme de Variáveis (Uniform Variable Syntax)](https://wiki.php.net/rfc/uniform_variable_syntax)

**`PHP 7.0+`**

* Uma nova sintaxe de variável internamente consistente e completa.
* **Todas as variáveis passam a ser avaliadas da esquerda para a direita**.

A sintaxe de variáveis do PHP é, de certa forma, inconsistente, particularmente o que diz respeito a variáveis-variáveis e variáveis propriedades.

Por exemplo, veja a sintaxe `$objeto->$array[chave]`; estamos sempre esperando que primeiro seja resolvido o `$array[chave]` para depois acessar a propriedade do objeto. Com a sintaxe uniforme de variáveis, toda essa inconsistência foi resolvida.

*A partir do PHP 7, todas as variáveis passam a ser avaliadas da esquerda para a direita, sendo um “problema” somente em caso de variáveis dinâmicas complexas e propriedades.*

Veja no código a seguir que é possível obter o mesmo comportamento do PHP 5.x no PHP 7, ou vice-versa, especificando de forma explícita a ordem das operações com o uso de parênteses () e chaves {}.

| **Expression**        | **PHP 5 Interpretation** | **PHP 7 Interpretation** |
|-----------------------|--------------------------|--------------------------|
| `$$foo['bar']['baz']` | `${$foo['bar']['baz']}`  | `($$foo)['bar']['baz']`  |
| `$foo->$bar['baz']`   | `$foo->{$bar['baz']}`    | `($foo->$bar)['baz']`    |
| `$foo->$bar['baz']()` | `$foo->{$bar['baz']}()`  | `($foo->$bar)['baz']()`  |
| `Foo::$bar['baz']()`  | `Foo::{$bar['baz']}()`   | `(Foo::$bar)['baz']()`   |

### Exemplo de expressões e suas interpretações

| **Expression**        | **PHP 5 Interpretation** | **PHP 7 Interpretation** |
|-----------------------|--------------------------|--------------------------|
| `$$foo['bar']['baz']` | `${$foo['bar']['baz']}`  | `($$foo)['bar']['baz']`  |

```php
// Sintaxe
$$foo['bar']['baz'];

// PHP 5.x:
// Usando valor de um array multidimensional como nome da variável
${$foo['bar']['baz']};

// PHP 7:
// Acesssando um array multidimensional dentro de uma variável-variável
$baz = "foo";
($$foo)['bar']['baz'];

// Exemplo - PHP 7:
$bar = "foo";
$foo = ['bar' => ['baz' => "\u{1F603}"]];

echo ($$bar)['bar']['baz']; // 😃
```

| **Expression**        | **PHP 5 Interpretation** | **PHP 7 Interpretation** |
|-----------------------|--------------------------|--------------------------|
| `$foo->$bar['baz']`   | `$foo->{$bar['baz']}`    | `($foo->$bar)['baz']`    |

```php
// Sintaxe
$foo->$bar['baz'];

// PHP 5.x:
// Usando valor de um array como nome de propriedade
$foo->{$bar['baz']};

// PHP 7:
// Acessando um array dentro de uma propriedade-variável
($foo->$bar)['baz'];

// Exemplo - PHP 7:
$bar = 'bar';
$foo = new class {
    public $bar = ['baz' => "\u{1F603}"];
};

echo ($foo->$bar)['baz']; // 😃
```

| **Expression**        | **PHP 5 Interpretation** | **PHP 7 Interpretation** |
|-----------------------|--------------------------|--------------------------|
| `$foo->$bar['baz']()` | `$foo->{$bar['baz']}()`  | `($foo->$bar)['baz']()`  |

```php
// Sintaxe
$foo->$bar['baz']();

// PHP 5.x:
// Usando valor de um array como nome de método
$foo->{$bar['baz']}();

// PHP 7:
// Chamando uma closure dentro de um array em um propriedade-variável
($foo->$bar)['baz']();

// Exemplo - PHP 7:
$bar = 'bar';
$foo = new class {
    public $bar;

    public function __construct()
    {
        $this->bar = [
            'baz' => function() {
                return "\u{1F603}";
            }
        ];
    }
};

echo ($foo->$bar)['baz'](); // 😃
```

| **Expression**        | **PHP 5 Interpretation** | **PHP 7 Interpretation** |
|-----------------------|--------------------------|--------------------------|
| `Foo::$bar['baz']()`  | `Foo::{$bar['baz']}()`   | `(Foo::$bar)['baz']()`   |

```php
// Sintaxe
Foo::$bar['baz']();

// PHP 5.x:
// Usando um valor de array como nome do método estático
Foo::{$bar['baz']}();

// PHP 7:
// Chamando uma closure dentro de um array em uma variável estática
(Foo::$bar)['baz']();

// Exemplo - PHP 7:
$bar = 'bar';
class Foo {
    public static $bar = [];

    public static function init()
    {
        self::$bar = [
            'baz' => function() {
                return "\u{1F603}";
            }
        ];
    }
};

Foo::init();

echo (Foo::$bar)['baz'](); // 😃
```

### Nova sintaxe

**`PHP 7.0+`**

Um dos principais benefícios da *sintaxe uniforme de variáveis* além da consistência é que ela permite o uso de novas combinações de sintaxe, e muitas dessas novas combinações estão disponíveis para uso.

**Novas combinações de sintaxe**

```php
// Chama uma closure dentro de um array retornado por outra closure
$foo()['bar']();

// Ex.:
$foo = function() {
    return [
        'bar' => function() {
            return "\u{1F60E}";
        }
    ];
};

echo $foo()['bar'](); // 😎

// Chama uma propriedade referenciando um array literal
[$obj1,$obj2][0]->prop;

// Ex.:
$obj1 = new class {
    public $bar = "obj1";
};

$obj2 = new class {
    public $bar = "obj2";
};

echo [$obj1,$obj2][0]->bar; // obj1

// Acessa um caractere pelo índice na string retornada
getStr(){0};

// Ex.:
function getStr() {
    return "ABCD";
}

echo getStr(){0}; // A
```

Ainda existe uma série de casos ambíguos que não podem ser resolvido, mesmo com a nova sintaxe de variáveis, e mesmo quando a adição de parênteses e chaves.

```php
$foo = 'Foo';
$class = 'CLASS';
$constant = 'BAR';

echo $foo::$class::$constant;
echo $foo::{$class}::$constant;
echo $foo::{"$class"}::$constant;
echo $foo::{"$class"}::{"$constant"};
echo $foo::CLASS::$constant;
echo $foo::CLASS::{"$constant"};
echo $foo::($class)::($constant);
```

### Chamada de métodos e funções aninhadas

Além disso, agora você pode fazer chamadas de métodos e funções aninhados usando os parênteses.

```php
// Call a callable returned by a function
foo()();

// Ex.:
function foo() {
    return function() {
        return "Foo";
    };
}

echo foo()(); // Foo

// Call a callable returned by an instance method
$foo->bar()();

// Ex.:
$foo = new class {
    public function bar()
    {
        return function() {
            return "Foo";
        };
    }
};

echo $foo->bar()(); // Foo

// Call a callable returned by a static method
Foo::bar()();

// Ex.:
class Foo {
    public static function bar()
    {
        return function() {
            return "Foo";
        };
    }
};

echo Foo::bar()(); // Foo

// Call a callable return another callable
$foo()();

// Ex.:
$foo = function() {
    return function() {
        return "Foo";
    };
};

echo $foo()(); // Foo
```

### Arbitrary expression dereferencing

Desde o PHP 5.4 era possível dereferencing arrays retornados por métodos e funções, agora com o PHP 7 é possível fazer isso com qualquer expressão válida colocada entre parênteses.

```php
// Access an array key
(expression)['foo'];

// Access a property
(expression)->foo;

// Call a method
(expression)->foo();

// Access a static property
(expression)::$foo;

// Call a static method
(expression)::foo();

// Call a callable
(expression)();

// Access a character
(expression){0};
```

Isso permite, finalmente, chamar uma `closure` quando ele for definido, e chamar um `callable` dentro da propriedade de um objeto.

*Dereferencing callables*

```php
// Define and immediately call a closure without assignment
(function() { /* ... */ })();

// Call a callable within an object property
($obj->callable)();
```

O PHP 7 também dá a possibilidade de chamar métodos usando *array-notation*.

O PHP 7 também possui alguns novos dereferencing para scalar types, em particular, a capacidade de chamar métodos usando callable de arrays.

*Dereferencing scalars*

```php
// Call a dynamic static method
["className", "staticMethod"]();

// Call a dynamic instance method
[$object, "method"]();

// Use a string scalar as a class name
'className'::staticMethod();
```

## Alterações básicas da linguagem

O **PHP 7** apresenta inúmeras pequenas mudanças na linguagem, novos operadores, funções, e mudanças nas funções e construções existentes.

### Novos operadores

**`PHP 7.0+`**

O *PHP 7.0* acrescenta dois novos operadores, o *Null coalescing operator* e o *Spaceship Operator*.

#### Null coalescing operator (??)

> O operador de *Null coalescing operator* ( _??_ ) foi adicionado como *syntactic sugar* para o caso comum de necessidade de usar um ternário em conjunto com [isset()](http://php.net/manual/en/function.isset.php). Retorna o valor de seu primeiro operando (operador esquerdo) se ele existe e não é nulo. Caso contrário, ele retorna seu segundo operando (operando direito).

```php
$post = $_POST['title'] ?? NULL;
```

O operador verifica se `$_POST['title']` existe. Se existir, retorna seu valor; Caso contrário, retorna *NULL*.

Outra característica excelente deste operador é que ele pode ser encadeado.

```php
$title = $_POST['title'] ?? $_GET['title'] ?? 'No POST or GET';
```

Primeiro verificará se o primeiro operando existe e devolvê-lo; Se não existir, retornará o segundo operando. Agora, se houver outro operador coalesce usado no segundo operando, a mesma regra será aplicada e o valor no operando esquerdo será retornado se ele existir. Caso contrário, o valor do operando direito será retornado.

Assim, o código anterior é o mesmo que o seguinte:

```php
if (isset($_POST['title']))
    $title = $_POST['title'];
elseif (isset($_GET['title']))
    $title = $_GET['title'];
else
    $title = 'No POST or GET';
```

**O operador coalesce pode ajudar a escrever códigos limpos e concisos.**

* Avalia primeiro o operando a esquerda de `??`, retornando-o se o mesmo não for nulo. Caso contrário retornará o operando a direita de `??`.
* Não emitirá um "aviso"(*NOTICE*) no caso de o operando a esquerda for uma variável inexistente, como no uso de `isset()`.
* Um atalho para `isset()` combinado com um ternário para atribuição.

```php
$foo = isset($bar) ? $bar : $baz; // Sintaxe antiga
$foo = $bar ?? $baz; // Null coalesce operator "??"
```

As duas linhas do exemplo anterior são funcionalmente idênticas.

Pode tambêm aninhar o operador e retornará o primeiro não-nulo (ou o último argumento).

*Nested null coalesce operators*
```php
$config = $config ?? $this->config ?? static::$defaultConfig;
```

#### [Spaceship operator)](https://wiki.php.net/rfc/combined-comparison-operator)

> Usado para comparar duas expressões. Este operador é apenas um invólucro e executa as mesmas tarefas que os três operadores de comparação `==`, `<` e `>`.

Ao invés de retornar um binário simples `true` ou `false`, ele pode retornar três valores distintos:

* `-1`: Se o operando direito for maior que o operando esquerdo.
* `0`: Se ambos os operandos forem iguais.
* `+1`: Se o operando esquerdo for maior que o operando direito.

Frequentemente usado para classificar items por exemplo `sort`, com um *callback* de retorno.

*Sorting with the combined comparison operator*

```php
// PHP 5
function orderFuncTraditional($a, $b)
{
    return ($a < $b) ? -1 : (($a > $b) ? 1 : 0);
}

// PHP 7.0
function orderFuncSpaceship($a, $b)
{
    return $a <=> $b;
}
```

```php
$int1 = 1;
$int2 = 2;
$int3 = 1;

echo $int1 <=> $int3; // Returns 0
echo $int1 <=> $int2; // Returns -1
echo $int2 <=> $int3; // Returns 1
```

Podemos verificar cadeias de caracteres, objetos e matrizes da mesma maneira, e eles são comparados com a mesma maneira padrão do PHP.

```php
// Integers
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// Floats
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1

// Strings
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1

echo "a" <=> "aa"; // -1
echo "zz" <=> "aa"; // 1

// Arrays
echo [] <=> []; // 0
echo [1, 2, 3] <=> [1, 2, 3]; // 0
echo [1, 2, 3] <=> []; // 1
echo [1, 2, 3] <=> [1, 2, 1]; // 1
echo [1, 2, 3] <=> [1, 2, 4]; // -1

// Objects
$a = (object) ["a" => "b"];
$b = (object) ["a" => "b"];
echo $a <=> $b; // 0

$a = (object) ["a" => "b"];
$b = (object) ["a" => "c"];
echo $a <=> $b; // -1

$a = (object) ["a" => "c"];
$b = (object) ["a" => "b"];
echo $a <=> $b; // 1

$a = (object) ["a" => "b"];
$b = (object) ["b" => "b"];
echo $a <=> $b; // 0
```

### Constant arrays using `define()`

**`PHP 7.0+`**

Até agora, as constantes definidas com `define()` só podem conter *scalar types*. Com o *PHP 7.0*, eles foram atualizados para corresponder as constantes definidas por `const`, permitindo que você as configure como matrizes.

*Constant arrays using `define()`*

```php
define('FOO', [
    'bar' => 'baz',
    'bat' => 'qux'
]);

echo FOO['bar']; // baz
```

Começando com o PHP 5.6, as constantes de arrays podem ser inicializados usando a palavra-chave `const` , da seguinte maneira:

```php
const STORES = ['en', 'fr', 'ar'];
```

Agora, a partir do *PHP 7.0*, constantes de arrays podem ser inicializados usando `define`, da seguinte maneira:

```php
define('STORES', ['en', 'fr', 'ar']);
```

### Multiple default cases in the switch statement

**`PHP 7.0+`**

Antes do PHP 7.0, foram permitidos vários `cases` `default` em uma declaração de `switch`.

```php
switch (true) {
    default:
        echo 'I am first one';
        break;
    default:
        echo 'I am second one';
}
```

Antes do PHP 7.0, o código anterior era permitido, mas no PHP 7.0, isso resultaria em um fatal erro semelhante ao seguinte:

```
Fatal error: Switch statements may only contain one default clause in…
```

### The options array for session_start function

**`PHP 7.0+`**

Antes do *PHP 7.0*, sempre que precisávamos iniciar uma sessão, usamos a função `session_start()`. Esta função não teve nenhum argumento, e todas as configurações definidas no *php.ini* foram usadas. Agora, a partir do PHP 7.0, uma matriz opcional para opções pode ser passada, o que irá substituir as configurações da sessão no arquivo *php.ini*.

```php
session_start([ 'cookie_lifetime' => 3600, 'read_and_close' => true ]);
```

Como pode ser visto no exemplo anterior, é possível substituir facilmente as configurações do *php.ini* de uma sessão.

## [Manipulação de Erros](http://php.net/manual/en/language.errors.php7.php)

**`PHP 7.0+`**

### The Throwable Interface

O *PHP 7.0* apresentou uma interface que pode ser base para cada objeto que pode usar a instrução `throw`. Em PHP, podem ocorrer exceções e erros. Anteriormente, as exceções poderiam ser tratadas, mas não era possível lidar com erros da linguagem e, portanto, qualquer erro fatal causaria a aplicação uma parada parcial ou total. O PHP 7.0 apresentou a interface `throwable`, que é implementada tanto pela exceção quanto pelo erro.

Com `\Throwable` e a nova hierarquia de exceções, seria sensato que possamos criar nossos próprios separação na hierarquia de exceções para exceções completamente personalizadas simplesmente implementando a interface `\Throwable`.

Infelizmente, devido ao fato de que as exceções são mágicas sob o capô, para poder fazer coisas como capturar informações de rastreamento de linha/arquivo e pilha, você ainda deve estender `\Exception` ou `\Error` e não pode implementar diretamente `\Throwable`.

*Throwable Interface*
```php
interface Throwable
{
   public function getMessage(): string;
   public function getCode(): int;
   public function getFile(): string;
   public function getLine(): int;
   public function getTrace(): array;
   public function getTraceAsString(): string;
   public function getPrevious(): Throwable;
   public function __toString(): string;
}
```

Se você trabalhou com exceções, essa interface deve parecer familiar. `Throwable` especifica métodos quase idênticos aos da exceção. A única diferença é que `Throwable::getPrevious()` pode retornar qualquer instância do `Throwable` em vez de apenas uma exceção.

Here’s what a simple catch-all block looks like:

```php
try {
   // Code that may throw an Exception or Error.
} catch (Throwable $t) {
   // Executed only in PHP 7, will not match in PHP 5
} catch (Exception $e) {
   // Executed only in PHP 5, will not be reached in PHP 7
}
```

### Engine exceptions (Exceções da linguagem)

Com o *PHP 7.0*, quase todos os erros fatais e erros *catchable fatal errors* são agora exceções do motor do PHP. Isso é possível porque uma exceção não tratada ainda resulta em um erro fatal tradicional, garantindo que a alteração seja principalmente compatível com versões anteriores.

Com esta mudança, obtemos uma série de benefícios, o mais óbvio é que agora podemos lidar com erros fatais em nosso código, usando blocos `try...catch`. No entanto, existem vários outros benefícios:

* O bloco `finally` é chamado.
* Objeto destrutor (`__destruct()`) é chamado.
* *Catchable fatal errors* são muito mais fáceis de controlar.
* Como todas as exceções, `fatal errors` terá um rastreamento de pilha, tornando mais fácil a depuracão.

Anteriormente, *Catchable Fatal Errors* poderiam ter sido capturados e manipulados usando `set_error_handler()` - manipula todos os erros não tratados. No entanto, com o PHP 7.0, eles são agora exceções de `\Error`, que, uma vez que uma exceção não detectada ainda é um erro fatal real, não será mais possível capturar em `set_error_handler()`.

Esta é uma ruptura compatível com versões anteriores e significa que para trabalhar em PHP 5.x e 7, você precisa usar `set_error_handler()` e blocos `try...catch`.

**Consistent constructor failure**

Antes do PHP 7.0, se uma classe interna não for instanciada corretamente, um objeto nulo ou inutilizável seria retornado.

Com o PHP 7.0, todas as classes internas lançarão uma exceção quando `__construct()` falhar. O tipo de exceção será diferente no objeto que está sendo instanciado e o motivo da falha.

### Hierarquia das exceções

Para a compatibilidade com versões anteriores, devemos garantir que os blocos de capturas (`catch (\Exception $e) { }`) não capturem as novas exceções do motor(*fatal errors*) e, portanto, serão um erro fatal como antes.

Para resolver isso, as novas exceções não estendem a classe base básica `\Exception`. As novas exceções são, em vez disso, instâncias de `\Error`, que é um irmão para a classe original `\Exception`. Todas as outras exceções do motor se estendem a partir da nova classe de exceção `\Error`.

Além disso, uma nova interface `\Throwable` foi adicionada para que ambos `\Exception` e `\Error` implementem e mantenham uma classe base comum(Superclasse de todas as exceptions).

Com essas novas mudanças, temos uma nova hierarquia de exceções:

*New exception hierarchy for PHP 7.0+*
```
\Throwable (1)
    ├── \Exception (implements \Throwable) (2)
    │   ├── \LogicException
    │   │   ├── \BadFunctionCallException
    │   │   │   └── \BadMethodCallException
    │   │   ├── \DomainException
    │   │   ├── \InvalidArgumentException
    │   │   ├── \LengthException
    │   │   └── \OutOfRangeException
    │   └── \RuntimeException
    │       ├── \OutOfBoundsException
    │       ├── \OverflowException
    │       ├── \RangeException
    │       ├── \UnderflowException
    │       └── \UnexpectedValueException
    └── \Error (implements \Throwable) (3)
        ├── \AssertionError
        ├── \ArithmeticError
        ├── \DivisionByZeroError
        ├── \ParseError
        └── \TypeError
```

1. A interface `\Throwable` é a Superclasse base para todas as exceções.
2. A exceção básica original `\Exception` agora implementa `\Throwable`.
3. Exceções do motor usam a nova exceção de `\Error`.

Com esta mudança, todas as exceções serão capturadas, tratadas via `\Throwable`: (`catch (\Throwable $e) {}`).

### Exceções de `Error`

Como você pode ver na hierarquia de exceções, existem quatro novas exceções de *error*, cada uma usada para uma finalidade diferente.

#### `\Error`

Exceções padrão do motor do PHP e *catchable-fatal* são agora lançados com exception de `\Error`. Continuaram a causar um erro fatal "tradicional" se não forem tratados.

Quase todos os erros fatais agora podem lançar uma instância de `Error`, e de forma semelhante às exceções, as instâncias de `Error` podem ser capturadas usando o bloco `try/catch`.

*Error exceptions*
```php
try {
    non_existent_function();
} catch (\Error $e) {
    // handle error
}
```

#### `\AssertionError`

Com os aprimoramentos das asserções, se você definir `assert.exception` para `1` no seu *php.ini* (ou via `ini_set()`), uma exceção é lançada quando a afirmação falhar.

Essas exceções são exceções `\AssertionError`.

*AssertionError exceptions*
```php
try {
    ini_set('assert.exception', 1);
    assert('true === false', 'Assertion failed');
} catch (\AssertionError $e) {
    // handle error
}
```

#### `\ArithmeticError` e `\DivisionByZeroError`

Duas exceções foram adicionadas para lidar com problemas ao executar a aritmética. O primeiro, `\ArithmeticError`, será lançado sempre que ocorrer um erro ao executar operações matemáticas.

*ArithmeticError exceptions*
```php
try {
    1 >> -1;
} catch (\ArithmeticError $e) {
    // handle error
}
```

Um segundo, mais específico `\DivisionByZeroError` também foi adicionado que é lançado sempre que você tenta dividir por zero - isso inclui o uso dos operadores `/` e `%` e da função `intdiv()`.

```php
try {
    10 % 0;
} catch (\DivisionByZeroError $e) {
    // handle error
}
```

#### `\ParseError`

Agora você pode lidar com erros de análise nas instruções `include` e `require`, e usando `eval()` nas análises de erros, pois ambos agora lançam exceções de `\ParseError`.

*ParseError exceptions*
```php
try {
    include 'parse-error.php';
} catch (\ParseError $e) {
    // handle error
}
```

#### `\TypeError`

Com a introdução de scalar types e (especialmente) tipos `strict` no PHP 7.0, estes lançarão exceções quando ocorrer uma incompatibilidade de tipo.

*TypeError exceptions*
```php
function example(callable $callback) {
    return $callback();
}

try {
    example(new stdClass);
} catch (\TypeError $e) {
    // handle error
}
```

## Closure Enhancements

**`PHP 7.0+`**

As Closures existem desde o PHP 5.3, e com o PHP 5.4, foi adicionada a capacidade de acessar `$this` de dentro da closure. Além disso, a classe `Closure` passou de ser um detalhe de implementação para ser provavelmente a maneira mais fácil de quebrar o modelo de objeto do PHP.

O *PHP 7.0* torna isso ainda mais fácil.

### Bind Closure On Call

> [Closure :: call()](http://php.net/manual/en/closure.call.php) é uma maneira abreviada e mais prática de vincular temporariamente um escopo de objeto a uma closure e invocá-lo.

O PHP introduziu a classe `Closure` na versão 5.3. A classe de `Closure` é usada para representar funções anônimas. Funções anônimas, implementadas no PHP 5.3, produzem objetos do tipo `Closure` . A partir do PHP 5.4, a classe `Closure` obteve vários métodos (`bind`, `bindTo`) que permitem o controle adicional da função anônima após sua criação. Esses métodos basicamente duplicam a closure com um objeto vinculado específico ao escopo da classe. O *PHP 7.0* introduziu o método de `call` em uma classe de `Closure`. O método de `call` não duplica a closure, vincula temporariamente ao novo (`$newThis`) e o chama com alguns parâmetros. Em seguida, retorna o valor de retorno da closure.

The call function signature looks like the following:
```php
function call ($newThis, ...$parameters) {}
```

`$newThis` é o objeto para vincular com a closure durante a duração da chamada. Os parâmetros, que serão dados como `$parameters` para a closure são opcionais, ou seja, zero ou mais.

Let's take a look at the following example of a simple `Customer` class and a `$greeting` closure:

```php
class Customer
{
    private $firstname;
    private $lastname;

    public function __construct($firstname, $lastname)
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
    }
}

$customer = new Customer('John', 'Doe');

$greeting = function ($message) {
    return "$message $this->firstname $this->lastname!";
};

echo $greeting->call($customer, 'Hello'); // Hello John Doe!
```

Dentro da closure original de `$greeting`, não há `$this`, ele não existe até que a ligação(`call`) real ocorra. Nós poderíamos facilmente confirmar isso chamando diretamente uma closure como `$greeting('Hello')`; No entanto, assumimos que `$this` existirá quando ligamos a closure a uma instância de objeto através da sua função `call`. Nesse caso, `$this` dentro da closure se torna `$this` da instância do objeto de `$customer`. O exemplo anterior mostra a vinculação de `$customer` a closure usando uma chamada de método - `call`. A saída resultante exibe `Hello John Doe!`.

------------------------------

Antes do PHP 7.0 tinhas o método de instância `Closure->bindTo()` e o método estático `Closure::bind()`, ambas as formas possui o mesmo objetivo e fazem a mesma coisa(Duplicar a closure com um novo objeto vinculado e um novo escopo de classe). A primeira forma é chamada na própria instância da closure, enquanto a versão estática deve ser passado a closure como primeiro argumento. Ambos em seguida levam dois argumentos: o primeiro é o `$newthis` que é o objeto que a closure irá apontar, e o segundo é o `$newscope` que é o escopo da nova classe a ser associada a closure. Membros privados/protegidos são acessíveis apenas se for definido o argumento `$newscope`.

*As duas funções retornão um clone da closure com um novo `$this` e um novo escopo, em vez de modificar o original.*

Com o PHP 7.0, a classe `Closure` tem um novo método de instância, `Closure->call()`, que leva um objeto como seu primeiro argumento(`$newthis`), para o qual `$this` é vinculado, e para o qual o escopo está definido, e então a closure é chamada passando por quaisquer argumentos adicionais passados para `Closure->call()`.

```php
class HelloWorld
{
    private $greeting = "Hello";
}

$closure = function ($whom) {
    echo $this->greeting. ' ' .$whom;
};

$obj = new HelloWorld();
$closure->call($obj, 'World'); // Hello World
```

## [Generator] Enhancements

**`PHP 7.0+`**

Definidos no PHP 5.5 e aprimorados no *PHP 7.0*.

> Os generators fornecem uma maneira fácil de implementar simples [iterators](http://php.net/manual/pt_BR/language.oop5.iterations.php) sem a sobrecarga ou complexidade de criar uma classe que implemente a interface [Iterator](http://php.net/manual/pt_BR/class.iterator.php).

Um generator permite que você escreva código que use [foreach](http://php.net/manual/pt_BR/control-structures.foreach.php) para iterar em um conjunto de dados sem precisar construir um array em memória, o que pode fazer com que o limite de memória seja ultrapassado ou exigir uma quantidade considerável de tempo de processamento para a geração. Em vez disso, você pode escrever uma função generator, que é o mesmo que uma [função](http://php.net/manual/pt_BR/functions.user-defined.php) normal, exceto que ao invés do [retorno](http://php.net/manual/pt_BR/functions.returning-values.php) ocorrer única vez, um generator pode entregar o resultado quantas vezes forem necessárias para permitir que os valores sejam iterados. Eles não eram novos para o PHP, pois foram adicionados no PHP 5.5.

### [Generator Return Expressions]

No PHP 5.5, se uma função de `generator` tiver uma declaração de retorno seguida por uma expressão (Ex.: `return true;`), isso resultaria em um erro de análise.

Antes do *PHP 7.0*, as funções de generator não conseguiam retornar expressões. A incapacidade das funções de generator para especificar valores de retorno limitou sua utilidade para a multitarefa em contextos de co-rotina.

O *PHP 7.0* possibilitou que generators retornassem expressões. Agora podemos chamar `Generator->getReturn()` para recuperar a expressão de retorno.

Com a nova RFC de "[Generator Return Expressions]", isso agora mudou. Tal como acontece com a classe `Closure` entre 5.3 e 5.4, a classe `Generator` que fornece a magia para generators passou de um detalhe de implementação para uma implementação concreta com a adição do método `Generator->getReturn()`.

> O método `Generator->getReturn()` permitirá que você recupere o valor retornado por qualquer declaração de retorno dentro do generator.

[Generator Return Expressions]: https://wiki.php.net/rfc/generator-return-expressions
[Generator]: https://secure.php.net/manual/pt_BR/language.generators.overview.php

*Retrieving generator return values*
```php
function helloGoodbye()
{
    yield "Hello";
    yield " ";
    yield "World!";

    return "Goodbye Moon!";
}

$gen = helloGoodbye();

foreach ($gen as $value) {
    echo $value; // Hello World!
}

echo $gen->getReturn(); // Goodbye Moon!
```

*Chamar `$generator->getReturn()` quando o generator ainda não retornou, ou lançou uma exceção não detectada, lançará uma exceção.*

*Se o generator não tiver uma expressão de retorno definida e tiver concluído o loop, `null` será retornado.*

```php
function gen()
{
    yield 'A';
    yield 'B';
    yield 'C';

    return 'gen-return';
}

$generator = gen();

// Output of the below class Generator#1 (0) { }
var_dump($generator);

// Output of the below code: PHP Fatal error: Uncaught Exception: Cannot get return value of a generator that hasn't returned ... //
var_dump($generator->getReturn());

// Output of the below code: ABC
foreach ($generator as $letter) {
    echo $letter;
}

var_dump($generator->getReturn()); // Output: string(10) "gen-return"
```

Olhando para a definição de função `gen()` e sua expressão de retorno, pode-se esperar que o valor da variável `$generator` seja igual à string `gen-return`. No entanto, este não é o caso, pois a variável `$generator` se torna a instância da classe `\Generator`. Chamar o método `getReturn()` no generator enquanto ainda está aberto (não iterado) resultará em um erro fatal.

Se o código estiver estruturado de forma que não seja óbvio se o generator estiver fechado, podemos usar o método `valid` para verificar, antes de obter o valor de retorno:

```php
if ($generator->valid() === false) { var_dump($generator->getReturn()); }
```

### [Generator Delegation](https://wiki.php.net/rfc/generator-delegation)

**`PHP 7.0+`**

> Generators agora podem delegar para outros generators, objetos [Traversable](http://php.net/manual/pt_BR/class.traversable.php) ou [array](http://php.net/manual/pt_BR/language.types.array.php) automaticamente, sem a necessidade de escrever código repetitivo no generator externo utilizando o construtor [_yield from_](http://php.net/manual/pt_BR/language.generators.syntax.php#control-structures.yield.from)

Vamos dar uma olhada no exemplo a seguir com três funções do tipo generator:

```php
function gen1()
{
    yield '1';
    yield '2';
    yield '3';
}

function gen2()
{
    yield '4';
    yield '5';
    yield '6';
}

function gen3()
{
    yield '7';
    yield '8';
    yield from gen1();
    yield '9';
    yield from gen2();
    yield '10';
}

// Output of the below code: 123
foreach (gen1() as $number) {
    echo $number;
}

// Output of the below code: 78123945610
foreach (gen3() as $number) {
    echo $number;
}

// Output: 12378123945610
```

A delegação do generator permite que um generator produza outros generators, arrays ou objetos que implementam a interface [Traversable](http://php.net/manual/pt_BR/class.traversable.php), e eles serão iterados por sua vez. Em outras palavras, podemos dizer que a delegação do generator está produzindo sub-generators.

Isso é feito usando a nova sintaxe `yield from <thing to iterate>` e tem o efeito de nivelar os sub-generators, de modo que o mecanismo de iteração não tenha conhecimento da delegação.

*Using generator delegation*
```php
function hello()
{
    yield "Hello";
    yield " ";
    yield "World!";

    yield from goodbye();
}

function goodbye()
{
    yield "Goodbye";
    yield " ";
    yield "Moon!";
}

$gen = hello();

foreach ($gen as $value) {
    echo $value; // Output: Hello World!Goodbye Moon!
}
```

## Mudanças na programação orientada a objetos

### [Context Sensitive Lexer](https://wiki.php.net/rfc/context_sensitive_lexer)

**`PHP 7.0+`**

O *PHP 7.0* apresenta o context-sensitive lexer, que permite o uso de [palavras-chave][reserved.keywords] como nomes de propriedades, métodos e constantes em classes, interfaces e traits.

O que isso significa é que o PHP passa de ter 64 [palavras-chave][reserved.keywords] reservadas para ter apenas uma classe - e apenas no contexto constante da classe.

Agora você pode usar qualquer uma das seguintes [palavras-chave][reserved.keywords] como propriedade, função e nomes de constantes.

[reserved.keywords]: http://php.net/manual/pt_BR/reserved.keywords.php

### [PHP 4 Constructors Deprecated](http://php.net/manual/en/migration70.deprecated.php#migration70.deprecated.php4-constructors)

**`PHP 7.0+`**

Construtores ao estilo PHP 4 (métodos que têm o mesmo nome que a classe onde estão definidos, o que significa que uma classe `Foo` possui um construtor PHP 4 chamado `Foo()`) estão depreciados, e será removido no futuro. O *PHP 7.0* emitirá **`E_DEPRECATED`** se um construtor do PHP 4 for o único construtor definido na classe. Classes que implementam o método **__construct()** não são afetadas.

```php
class Foo
{
    function Foo()
    {
        echo 'Eu sou um construtor';
    }
}
// Output:
/*
PHP Deprecated:  Methods with the same name as their class will not be constructors in a future version of PHP; Foo has a deprecated constructor in example.php on line 3
*/
```

### [Group Use Declarations - Namespaces]

**`PHP 7.0+`**

*Em PHP, não é necessário dividir classes em subpastas de acordo com seu namespace, como é o caso de outras linguagens de programação. Namespaces apenas fornecem uma separação lógica de classes. No entanto, não estamos limitados a colocar nossas classes em subpastas de acordo com nossos namespaces.*

[Group Use Declarations - Namespaces]: https://wiki.php.net/rfc/group_use_declarations

> As declarações de `use` em grupo nos permitem duplicar os prefixos comuns e simplesmente especificar as partes únicas dentro de um bloco (`{}`).

> Classes, funções e constantes importadas do mesmo [_namespace_](http://php.net/manual/pt_BR/language.namespaces.definition.php), agora podem ser agrupadas em uma única declaração [_use_](http://php.net/manual/pt_BR/language.namespaces.importing.php)

```php
// Original
use Framework\Component\ClassA;
use Framework\Component\ClassB as ClassC;
use Framework\OtherComponent\ClassD;

// With group
use Framework\ {
    Component\ClassA,
    Component\ClassB as ClassC,
    OtherComponent\ClassD
};
```

*Alternative organization of use statements*
```php
use Framework\Component\ {
    Component\ClassA,
    Component\ClassB as ClassC
};
Use Framework\OtherComponent\ClassD;
```

Além disso, se você quiser importar funções ou constantes - um recurso que foi adicionado no PHP 5.6 - você simplesmente prefixa a linha de importação com `function` ou `const`.

*Importing functions and constants with group use statements*
```php
use Framework\Component\ {
    SubComponent\ClassA,
    function OtherComponent\someFunction,
    const OtherComponent\SOME_CONSTANT
};
```

**Existem três tipos de declarações de uso em grupo**:

* *Non mixed group use declarations.*
* *The compound namespace declaration.*
* *Mixed group use declarations.*

**Non mixed group use declarations**

```php
use Publishers\Packt\{
    Book,
    Ebook,
    Video,
    Presentation
};

use function Publishers\Packt\{
    getBook,
    saveBook
};

use const Publishers\Packt\{
    COUNT, KEY
};
```

**The compound namespace declaration**

```php
use Publishers\Packt\{
    Paper\Book,
    Electronic\Ebook,
    Media\Video,
    Media\Presentation
};
```

**Mixed group use declarations**

Nesta declaração, combinamos todos os tipos em uma única declaração de `use`.

```php
use Publishers\Packt\{
    Book,
    Ebook,
    Video,
    Presentation,
    function getBook,
    function saveBook,
    const COUNT,
    const KEY
};
```

### [Anonymous Classes](https://wiki.php.net/rfc/anonymous_classes)

**`PHP 7.0+`**

Com a adição de classes anônimas, os objetos PHP ganharam recursos semelhantes a closure. Agora podemos instanciar objetos através de classes anônimas, o que nos aproxima da sintaxe literal do objeto encontrada em outras linguagens.

> O suporte a classes anônimas foi adicionado utilizando _new class_. Estas podem ser utilizadas no lugar de definições completas de classes para objetos descartáveis. São úteis quando objetos descartáveis precisarem ser criados.

> Uma classe anônima é uma classe que é declarada e instanciada ao mesmo tempo. Não tem um nome e pode ter os recursos completos de uma classe normal. Essas classes são úteis quando uma única pequena tarefa é necessário para ser realizada e não há necessidade de escrever uma classe full-blown para ele.

```php
$object = new class
{
    public function hello($message)
    {
        return "Hello $message";
    }
};

echo $object->hello('PHP'); // Output: Hello PHP
```

Para criar uma classe anônima, você combina simplesmente `new class($constructor, $args)` seguido de uma definição de classe padrão. Uma classe anônima sempre é instanciada durante a criação, dando-lhe um objeto dessa classe.

*Creating an anonymous class*
```php
$object = new class ("bar")
{
    public $foo;

    public function __construct($arg)
    {
        $this->foo = $arg;
    }
};
```

O exemplo anterior criará um objeto construtor com `__construct()`, que teve seu argumento de `$arg` para o valor de "bar" e a propriedade `$foo` que foi atribuído o valor desse argumento pelo construtor.

```
object(class@anonymous)#1 (1) {
    public $foo => string(3) "bar"
}
```

Semelhante a qualquer classe normal, as classes anônimas podem passar argumentos para seus construtores, estender outras classes, implementar interfaces e usar traits:

```php
class TheClass
{
}

interface TheInterface
{
}

trait TheTrait
{
}

$object = new class ('A', 'B', 'C') extends TheClass implements TheInterface
{
    use TheTrait;

    public $a;
    private $b;
    protected $c;

    public function __construct($a, $b, $c)
    {
        $this->a = $a;
        $this->b = $b;
        $this->c = $c;
    }
};

var_dump($object);
// Output:
/*
class class@anonymous#1 (3) {
  public $a => string(1) "A"
  private $b => string(1) "B"
  protected $c => string(1) "C"
}
*/
```

**Alguns dos benefícios do uso de classes anônimas são:**

* *Mocking application tests becomes trivial. We can create on-the-fly implementations for interfaces, avoiding using complex mocking APIs.*
* *Avoid invoking the autoloader every so often for simpler implementations.*
* *Makes it clear to anyone reading the code that this class is used here and nowhere else.*

Classes anônimas, ou melhor, objetos criados de classes anônimas, não podem ser serializados. Tentando serializar resulta em um erro fatal da seguinte maneira:

```
Fatal error: Uncaught Exception: Serialization of 'class@anonymous' is not allowed in example.php:31
Stack trace:
#0 example.php(31): serialize(Object(class@anonymous))
#1 {main} thrown in example.php on line 31
```

Aninhar uma classe anônima não dá acesso a métodos e propriedades `privates` ou `protected` da classe externa. Para usar os métodos e propriedades `protected` da classe externa, a classe anônima pode estender a classe externa. Ignorando métodos, propriedades privadas ou protegidas da classe externa podem ser usadas na classe anônima se passadas através de seu construtor:

```php
class Outer
{
    private $prop = 1;
    protected $prop2 = 2;

    protected function outerFunc1()
    {
        return 3;
    }

    public function outerFunc2()
    {
        return new class ($this->prop) extends Outer
        {
            private $prop3;

            public function __construct($prop)
            {
                $this->prop3 = $prop;
            }

            public function innerFunc1()
            {
                return $this->prop2 + $this->prop3 + $this->outerFunc1();
            }
        };
    }
}

echo (new Outer)->outerFunc2()->innerFunc1(); // Output: 6
```

Embora rotulemos-os como classes anônimas, eles não são realmente anônimos em termos do nome interno que o mecanismo PHP atribui aos objetos instanciados dessas classes. O nome interno de uma classe anônima é gerado com uma referência exclusiva com base em seu endereço. A instrução `get_class(new class{});` resultaria em algo como `class@anonymous/example.php0x7f33c22381c8`, onde `0x7f33c22381c8` é o endereço interno.

*Ao criar uma classe anônima, ela não tem nome por ser anônima, mas é chamado internamente em PHP com uma referência exclusiva com base em seu endereço no bloco de memória. Por exemplo, o nome interno de uma classe anônima pode ser `class@ 0x4f6a8d124`.*

* A sintaxe desta classe é igual à das classes nomeadas, mas sem um nome na frente da palavra chave `class`.

```php
$name = new class ()
{
    public function __construct()
    {
        echo 'Allyson Silva';
    }
};
```

* Argumentos também podem ser passados ​​para o construtor.

```php
$name = new class ('Allyson Silva')
{
    public function __construct(string $name)
    {
        echo $name;
    }
};
```

* As classes anônimas podem estender outras classes e ter as mesmas classes pai-filho funcionando normalmente como classes nomeadas.

```php
class Parent
{
    protected $number;

    public function __construct()
    {
        echo 'I am parent constructor';
    }

    public function getNumber(): float
    {
        return $this->number;
    }
}

$number = new class (5) extends Parent
{
    public function __construct(float $number)
    {
        parent::__construct();

        $this->number = $number;
    }
};

echo $number->getNumber(); // Output: I am parent constructor 5
```

* As classes anônimas também podem implementar interfaces, assim como classes nomeadas.


```php
interface Publishers
{
    public function __construct(string $name, string $address);

    public function getName();
    public function getAddress();
}

class Parent
{
    protected $number;
    protected $name;
    protected $address;
    // public function …
}

$info = new class ('Allyson Silva', 'Fortaleza, Brazil' ) extends Parent implements Publishers
{
    public function __construct(string $name, string $address)
    {
        $this->name = $name;
        $this->address = $address;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getAddress(): string
    {
        return $this->address;
    }
};

echo ($info->getName() . ' ' . $info->getAddress()); // Output: Allyson Silva Fortaleza, Brazil
```

* É possível usar classes anônimas dentro de outra classe.

```php
class Math
{
    public $firstNumber = 10;
    public $secondNumber = 20;

    public function add(): float
    {
        return $this->firstNumber + $this->secondNumber;
    }

    public function multiplySum()
    {
        return new class () extends Math
        {
            public function multiply(float $third_number): float
            {
                return $this->add() * $third_number;
            }
        };
    }
}

$math = new Math();
echo $math->multiplySum()->multiply(2); // Output: 60
```

* Suporta herança, traits e interfaces usando a mesma sintaxe de uma classe regular.

```php
namespace MyProject\Component;

$object = new class ($args) extends Foo implements Bar
{
    use Bat;
};
```

### [Nullable types](https://wiki.php.net/rfc/nullable_types)

**`PHP 7.1+`**

As declarações de tipo para parâmetros e valores de retorno agora podem ser marcadas como anuláveis, prefixando o nome do tipo com um ponto de interrogação. Isso significa que, assim como o tipo especificado, **`NULL`**pode ser passado como um argumento ou retornado como um valor, respectivamente.

Esse recurso permite definir valores nulos como parâmetros ou retornos para funções. Podemos tornar um parâmetro ou um valor de retorno Anulável ao prefixar um ponto de interrogação (**?**) o *type hint*.

```php
function sayHello(?string $name)
{
    echo "Hello " . $name . PHP_EOL;
}

sayHello(null); // Hello
sayHello("Allyson"); // Hello Allyson
```

Da mesma forma, para o tipo de retorno anulável:

```php
class Cookie
{
    protected $jar;

    public function get(string $key): ?string
    {
        if (isset($this->jar[$key])) {
            return $this->jar[$key];
        }

        return null;
    }
}
```

Aqui estão mais alguns exemplos para demonstrar a funcionalidade básica:

```php
function answer(): ?int
{
    return null; // Ok
}

function answer(): ?int
{
    return 42; // Ok
}

function answer(): ?int
{
    return new stdclass(); // Error
}
```

```php
function say(?string $msg)
{
    if ($msg) {
        echo $msg;
    }
}

say('hello'); // Ok -- Output: hello
say(null); // Ok -- Does not print
say(); // Error -- Missing parameter
say(new stdclass); // Error -- Bad type
```

```php
function testReturn(): ?string
{
    return 'elePHPant';
}

var_dump(testReturn()); // Output: string(10) "elePHPant"

function testReturn(): ?string
{
    return null;
}

var_dump(testReturn()); // Output: NULL

function test(?string $name)
{
    var_dump($name);
}

test('elePHPant'); // Output: string(10) "elePHPant"
test(null); // Output: NULL
test(); // Uncaught Error: Too few arguments to function test(), 0 passed in...
```

### [Void functions](https://wiki.php.net/rfc/void_return_type)

**`PHP 7.1+`**

As funções/métodos declarados com `void` como seu tipo de retorno, devem omitir sua instrução de `return` ou usar uma instrução de `return` vazia(`return;`). **`NULL`** não é um valor de retorno válido para uma função de retorno `void`.

```php
function swap(&$left, &$right): void
{
    if ($left === $right) {
        return;
    }

    $tmp = $left;
    $left = $right;
    $right = $tmp;
}

$a = 1;
$b = 2;

var_dump(swap($a, $b), $a, $b);

// Output:
/*
    null
    int(2)
    int(1)
*/
```

Às vezes, podemos querer especificar que a função não deve retornar nenhum valor(`void`), apenas realize determinado processamento.

```php
class Cookie
{
    protected $jar;

    public function set(string $key, $value): void
    {
        $this->jar[$key] = $value;
    }
}
```

Uma função com um tipo de retorno `void`, pode retornar implicitamente ou ter uma instrução de retorno sem um valor:

```php
function lacks_return(): void
{
    // Valid
}

function returns_nothing(): void
{
    return; // Valid
}
```

Uma função `void` não pode retornar um valor:

```php
function returns_one(): void
{
    return 1; // Fatal error: A void function must not return a value
}

function returns_null(): void
{
    return null; // Fatal error: A void function must not return a value
}
```

Observe que `void` é válido apenas como um tipo de retorno, não como um tipo de parâmetro:

```php
function foo(void $foo)
{
    // Fatal error: void cannot be used as a parameter type
}
```

### [Symmetric array destructuring](https://wiki.php.net/rfc/short_list_syntax)

**`PHP 7.1+`**

The shorthand array syntax (_[]_) may now be used to destructure arrays for assignments (including within  _foreach_), as an alternative to the existing  [list()](http://php.net/manual/en/function.list.php)  syntax, which is still supported.

*Agora, a sintaxe da lista curta nos permite usar colchetes ao invés de `list()` em uma construção para atribuir variáveis ​​de uma matriz.*

```php
// Antes do php 7.1
list($firstName, $lastName) = ["John", "Doe"];

// PHP 7.1
[$firstName, $lastName] = ["John", "Doe"];
```

```php
$data = [
    [1, 'Allyson'],
    [2, 'Silva'],
];

// list() Style
list($id1, $name1) = $data[0];

// [] Style
[$id1, $name1] = $data[0];

// list() Style
foreach ($data as list($id, $name)) {
    // Logic here with $id and $name
}

// [] Style
foreach ($data as [$id, $name]) {
    // Logic here with $id and $name
}
```

```php
 // Assigns to $a, $b and $c the values of their respective array elements in $array with keys numbered from zero
[$a, $b, $c] = $array;

// Assigns to $a, $b and $c the values of the array elements in $array with the keys "a", "b" and "c", respectively
["a" => $a, "b" => $b, "c" => $c] = $array;
```

```php
// The two lines in each of the following pairs are equivalent to each other

list($a, $b, $c) = array(1, 2, 3);
[$a, $b, $c] = [1, 2, 3];

list("a" => $a, "b" => $b, "c" => $c) = array("a" => 1, "b" => 2, "c" => 3);
["a" => $a, "b" => $b, "c" => $c] = ["a" => 1, "b" => 2, "c" => 3];

list($a, $b) = array($b, $a);
[$a, $b] = [$b, $a];
```

```php
// This is not allowed:
list([$a, $b], [$c, $d]) = [[1, 2], [3, 4]];

// This is also not allowed:
[list($a, $b), list($c, $d)] = [[1, 2], [3, 4]];

// This, however, is allowed:
[[$a, $b], [$c, $d]] = [[1, 2], [3, 4]];
```

#### [With support for keys in `list()`](https://wiki.php.net/rfc/list_keys)

**`PHP 7.1+`**

You can now specify keys in [list()](http://php.net/manual/en/function.list.php), or its new shorthand _[]_ syntax. This enables destructuring of arrays with non-integer or non-sequential keys.

```php
$data = [
    ["id" => 1, "name" => 'Allyson'],
    ["id" => 2, "name" => 'Silva'],
];

// list() Style
list("id" => $id1, "name" => $name1) = $data[0];

// [] Style
["id" => $id1, "name" => $name1] = $data[0];

// list() Style
foreach ($data as list("id" => $id, "name" => $name)) {
    // Logic here with $id and $name
}

// [] Style
foreach ($data as ["id" => $id, "name" => $name]) {
    // Logic here with $id and $name
}
```

Até o PHP 7, `list()`apenas suportava matrizes numericamente indexadas com índice começando do zero. Não sendo capazes de usá-lo com matrizes associativas.

Agora com PHP 7.1, esse recurso nos permite especificar chaves ao desestruturar arrays.

```php
$user = [ "first_name" => "Allyson", "last_name" => "Silva"];
["first_name" => $firstName, "last_name" => $lastName]= $user;
```

Como um efeito colateral interessante, agora podemos atribuir apenas as chaves necessárias da matriz.

```php
["last_name" => $lastName] = $user;
```

Também é possível desestruturar matrizes multidimensionais usando listas dentro de listas.

```php
$users = [
    ["name" => "John Doe", "age" => 29],
    ["name" => "Sam", "age" => 36]
];

[["name" => $name1, "age" => $age1], ["name" => $name2, "age" => $age2]] = $users;
```

### [Convert callables to  Closures with  `Closure::fromCallable()`](https://wiki.php.net/rfc/closurefromcallable)

**`PHP 7.1+`**

A new static method has been introduced to the [Closure](http://php.net/manual/en/class.closure.php) class to allow for [callables](http://php.net/manual/en/language.types.callable.php) to be easily converted into[Closure](http://php.net/manual/en/class.closure.php) objects.

```php
class Test
{
    public function exposeFunction()
    {
        return Closure::fromCallable([$this, 'privateFunction']);
    }

    private function privateFunction($param)
    {
        var_dump($param);
    }
}

$privFunc = (new Test)->exposeFunction();
$privFunc('some value'); // Output: string(10) "some value"
```

```php
class MyClass
{
    public function getCallback()
    {
        return Closure::fromCallable([$this, 'callback']);
    }

    public function callback($value)
    {
        echo $value . PHP_EOL;
    }
}

$callback = (new MyClass)->getCallback();
$callback('Hello World'); // Output: Hello World
```

### [Class constant visibility](https://wiki.php.net/rfc/class_const_visibility)

**`PHP 7.1+`**

Support for specifying the visibility of class constants has been added.

Agora, podemos especificar os modificadores de visibilidade para constantes de classes, assim como fazemos para métodos e propriedades.

```php
class ConstDemo
{
    const PUBLIC_CONST_A = 1;
    public const PUBLIC_CONST_B = 2;
    protected const PROTECTED_CONST = 3;
    private const PRIVATE_CONST = 4;
}
```

```php
class PostValidator
{
    protected const MAX_LENGTH = 100;

    public function validateTitle($title)
    {
        if (strlen($title) > self::MAX_LENGTH) {
            throw new \Exception("Title is too long");
        }

        return true;
    }
}
```

No exemplo acima à constante `MAX_LENGHT` é interno à classe `PostValidator` e não queremos que o mundo exterior acesse esse valor.

### [Iterable pseudo-type](https://wiki.php.net/rfc/iterable)

**`PHP 7.1+`**

A new pseudo-type (similar to  [callable](http://php.net/manual/en/language.types.callable.php)) called  iterable  has been introduced. It may be used in parameter and return types, where it accepts either arrays or objects that implement the  [Traversable](http://php.net/manual/en/class.traversable.php)  interface. With respect to subtyping, parameter types of child classes may broaden a parent's declaration of  [array](http://php.net/manual/en/language.types.array.php)  or  [Traversable](http://php.net/manual/en/class.traversable.php)  to  iterable. With return types, child classes may narrow a parent's return type of  iterable  to  [array](http://php.net/manual/en/language.types.array.php)  or an object that implements  [Traversable](http://php.net/manual/en/class.traversable.php).

O *PHP 7.1* introduziu um novo `iterable`pseudo tipo, que podemos usar para especificar qualquer type hint que possa ser iterado usando `foreach`.

Agora, essa função pode aceitar matrizes, objetos iteráveis ​​e generators.

`iterable` aceita qualquer array ou objeto implementando `Traversable`. Ambos os tipos são iteráveis usando `foreach` e podem ser usados com `yield from` dentro de um generator.

`iterable` pode ser usado como um tipo de parâmetro para indicar que uma função requer um conjunto de valores, mas não se preocupa com a forma do conjunto de valores (array, Iterator, Generator, etc.), pois ele será usado com `foreach`. Se um valor não for uma matriz ou instância de `Traversable`, um `\TypeError` será lançado.

```php
function iterator(iterable $iterable)
{
    foreach ($iterable as $value) {
        // ...
    }
}
```

`iterable` também pode ser usado como um tipo de retorno para indicar que uma função retornará um valor iterável. Se o valor retornado não for uma matriz ou instância de `Traversable`, um `TypeError` será lançado.

```php
function bar(): iterable
{
    return [1, 2, 3];
}
```

Parâmetros declarados como iteráveis podem usar `null` ou uma matriz como um valor padrão.

```php
function foo(iterable $iterable = [])
{
    // ...
}
```

Functions declaring iterable as a return type may also be generators.

```php
function gen() : iterable
{
    yield 1;
    yield 2;
    yield 3;
}
```

`is_iterable()` retorna um booleano: `true` se um valor for iterável e será aceito pelo pseudo-tipo iterável, false para outros valores.

```php
var_dump(is_iterable([1, 2, 3]));  // bool(true)
var_dump(is_iterable(new ArrayIterator([1, 2, 3])));  // bool(true)
var_dump(is_iterable((function () {
    yield 1;
})()));  // bool(true)
var_dump(is_iterable(1));  // bool(false)
var_dump(is_iterable(new stdClass()));  // bool(false)
```

### [Catching Multiple Exception Types](https://wiki.php.net/rfc/multiple-catch)

**`PHP 7.1+`**

Multiple exceptions per catch block may now be specified using the pipe character (_|_). This is useful for when different exceptions from different class hierarchies are handled the same.

```php
try {
    // some code
} catch (FirstException | SecondException $e) {
    // handle first and second exceptions
}
```

Ao lidar com exceções, às vezes podemos querer lidar com várias exceções da mesma maneira. Normalmente, teremos vários blocos `catch` para lidar com diferentes tipos de exceções.

```php
try {
  // something
} catch (MissingParameterException $e) {
    throw new \InvalidArgumentException($e->getMessage());
} catch (IllegalOptionException $e) {
    throw new \InvalidArgumentException($e->getMessage());
}
```

Várias exceções são separadas usando um único símbolo "pipe". Desta forma, podemos evitar a duplicação do mesmo código para lidar com vários tipos de exceção.

No *PHP 7.1*, podemos manipular múltiplas exceções em um único bloco `catch`:

```php
try {
  // something
} catch (MissingParameterException | IllegalOptionException $e) {
    throw new \InvalidArgumentException($e->getMessage());
}
```

### [New object type hint](https://wiki.php.net/rfc/object-typehint)

**`PHP 7.2+`**

A new type, [object](http://php.net/manual/en/language.types.object.php), has been introduced that can be used for (contravariant) parameter typing and (covariant) return typing of any objects.

Passar um valor que não é um objeto para um parâmetro que é declarado como tipo `object` falha na verificação de tipo e um `\TypeError` será lançado.

Da mesma forma, se uma função é declarada como retorno um `object`mas não retorna um objeto, novamente um `\TypeError` será lançado.

Para métodos de classe que usam o objeto como um parâmetro ou tipo de retorno, as verificações de herança usam *contravariância* para tipos de parâmetro e *covariância* para tipos de retorno.

#### Exemplos

**Parameter type**
```php
function acceptsObject(object $obj)
{
    ...
}

// This code can be statically analyzed to be correct
acceptsObject(json_decode('{}'));

// This code can be statically analyzed to be correct
acceptsObject(new \MyObject());

// This can be statically analysed to contain an error.
// and would throw an TypeError at runtime.
acceptsObject("Ceci n'est pas une object.");
```

**Return type**
```php
// This function can be statically analysed to conform to the
// return type
function correctFunction(): object
{
    $obj = json_decode('{}');

    return $obj;
}

// This function can be statically analysed to contain an error
// and will also fail at runtime.
function errorFunction(): object
{
    return [];
}
```

#### Variance

**Argument typehint contravariance**

`Contravariance`

Permite a você usar um tipo mais genérico (menos derivado) do que aquele especificado originalmente.

Classes que estendem outra classe, ou implementam uma interface, podem ampliar um tipo de parâmetro de uma classe específica para o tipo(typehint) mais genérico, que no caso é `object`.

```php
class Foo
{
}

class Bar
{
    public function foo(Foo $object): object
    {
        return $object;
    }
}

class Baz extends Bar
{
    public function foo(object $object): object
    {
        return $object;
    }
}
```

Isso é normalmente conhecido como [contravariance](https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Contravariant_method_argument_type "https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Contravariant_method_argument_type").

As classes que estendem/implementam não podem restringir os tipos de argumentos do método de `object` para um tipo mais específico.

```php
class Foo
{
}

class Bar
{
    public function foo(object $object): object
    {
        return $object;
    }
}

class Baz extends Bar
{
    public function foo(Foo $object): object
    {
        return $object;
    }
}
```

Nessa situação, o aviso padrão do PHP para assinaturas incompatíveis será gerado.

```php
Declaration of Baz::foo(Foo $object): object should be compatible with Bar::foo(object $object): object
```

**Return type covariance - (Não implementado no PHP 7.2, apenas foi apresentado e colocado em votação)**

`Covariance`

Permite usar um tipo mais derivado que o especificado originalmente.

Classes que estendem/implementam podem restringir os métodos a retornarem tipos de `object` para o nome de classe especificado, retornarem tipos mais derivados. É normalmente chamado de [covariance](https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Covariant_method_return_type "https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Covariant_method_return_type") .

```php
class Foo
{
}

class Bar
{
    public function foo(object $object): object
    {
        return $object;
    }
}

class Baz extends Bar
{
    public function foo(object $object): Foo
    {
        return $object;
    }
}
```

### [Abstract method overriding](https://wiki.php.net/rfc/allow-abstract-function-override)

**`PHP 7.2+`**

Abstract methods can now be overridden when an abstract class extends another abstract class.

```php
abstract class A
{
    abstract function test(string $s);
}

abstract class B extends A
{
    // Overridden - Still maintaining contravariance for parameters and covariance for return
    abstract function test($s): int;
}
```

## Type Hints [(Scalar type hints)](https://wiki.php.net/rfc/scalar_type_hints)

Antes do PHP 7.0, não havia necessidade de declarar o tipo de dados dos argumentos passados ​​para uma função ou método de classe. Além disso, não era necessário mencionar o tipo de dados de retorno. Qualquer tipo de dados pode ser passado e retornado de uma função ou método. Este é um dos enormes problemas no PHP, em que nem sempre é claro quais tipos de dados devem ser passados ​​ou recebidos de uma função ou método. Para corrigir esse problema, o PHP 7 introduziu dicas de tipo(Type Hints). A partir de agora, são introduzidas dois *Type Hints*: *scalar* and *return type hints*. Estas são discutidas nas seções a seguir.

[Declarações de tipos](http://php.net/manual/pt_BR/functions.arguments.php#functions.arguments.type-declaration) escalares vêm de duas formas: coercivo (padrão) e estrito. Para parâmetros os seguintes tipos agora podem ser forçados (tanto coercivamente quanto estritamente): strings ([string](http://php.net/manual/pt_BR/language.types.string.php)), inteiros (_int_), números de ponto flutuante ([float](http://php.net/manual/pt_BR/language.types.float.php)) e booleanos (_bool_). Eles incrementam os outros tipos introduzidos no PHP 5: nomes de classe, interfaces, [array](http://php.net/manual/pt_BR/language.types.array.php) e [callable](http://php.net/manual/pt_BR/language.types.callable.php).

### [Declarações de Tipos Escalares](http://php.net/manual/pt_BR/migration70.new-features.php#migration70.new-features.scalar-type-declarations)

PHP 5.0 apresentou a primeira declaração de tipo, que foi a capacidade de um parâmetro de um método ou função ser uma classe ou interface. PHP 5.1 apresentou a capacidade dos parâmetros poderem aceitar objetos do tipo de arrays, enquanto o PHP 5.4 adicionou o tipo callable(funções anônimas, closure, funções nomeadas) na aceitação dos parâmetros. Finalmente, o PHP 7.0 introduziu *scalar type hints*.

*Type Hints é uma característica tanto no OOP como no PHP processual, pois pode ser usado tanto para funções procedurais quanto para métodos de objeto.*

O PHP 7 acrescenta a capacidade de aceitar novos tipos nos parâmetros, conhecidos como *Tipos escalares*:

* `bool`: Valores booleanos (`true`|`false`)
* `float`: Números de pontos flutuantes
* `int`: Números inteiros
* `string`: sequência de caracteres

Por padrão, o PHP 7 funciona no modo de verificação de tipo fraco(Modo coercivo) e tentará converter para o tipo especificado sem advertência. Podemos controlar esse modo usando a diretiva `strict_types`.

A diretiva `declare(strict_types=1)` deve ser a primeira declaração em um arquivo, ou então gerará um erro no compilador. Isso afeta apenas o arquivo específico em que é usado e não afeta outros arquivos incluídos. The directive is entirely compile-time and cannot be controlled at runtime:

```php
declare(strict_types=0); // weak type-checking
declare(strict_types=1); // strict type-checking
```

Let's assume the following simple function that accepts hinted scalar types.

```php
function hint(int $a, float $b, string $c, bool $d)
{
    var_dump($a, $b, $c, $d);
}
```

As regras de verificação de tipo fracas para as novas declarações de tipo escalar são principalmente as mesmas das extensões e funções PHP incorporadas. Por causa dessa conversão automatizada, podemos perder dados inconscientemente ao passá-lo para uma função. Um exemplo simples é passar um float para uma função que requer um int; caso em que a conversão simplesmente tira decimais.

Supondo que a verificação de tipo fraca esteja ativada, como por padrão, o seguinte pode ser observado:

```php
hint(2, 4.6, 'false', true);
/* int(2) float(4.6) string(5) "false" bool(true) */

hint(2.4, 4, true, 8);
/* int(2) float(4) string(1) "1" bool(true) */
```

Podemos ver que a primeira chamada de função passa parâmetros como eles são sugeridos. A segunda chamada de função não passa os tipos exatos de parâmetros, mas ainda assim a função consegue executar à medida que os parâmetros passam pela conversão.

Assuming the weak type-checking is off, by using the `declare(strict_types=1)`; directive, the following can be observed:

```php
hint(2.4, 4, true, 8);
```

```
PHP Fatal error:  Uncaught TypeError: Argument 1 passed to hint() must be of the type integer, float given, called in example.php on line 11 and defined in example.php:5
Stack trace:
#0 example.php(11): hint(2.4, 4, true, 8)
#1 {main} thrown in example.php on line 5
```

A execução da função quebrou no primeiro argumento resultando na exceção `\TypeError`. A diretiva `strict_types=1` não permite nenhum tipo de malabarismo. O parâmetro deve ser do mesmo tipo, conforme especificado pela definição da função.

#### Tipos Coercivos (Padrão)

Por padrão, o PHP 7.0 usará type hints coercivo, o que significa que ele tentará converter para o tipo especificado e, se possível, o fará sem problemas. Esta é a opção não restrita. tipos coercive vêm com alguns destaques, o mais importante, perda de precisão.

```php
function sendHttpStatus(int $statusCode, string $message)
{
    header('HTTP/1.0 ' . $statusCode . ' ' . $message);
}

sendHttpStatus(404, "File Not Found");
sendHttpStatus("403", "OK");
```

**Precision Loss**

Por causa dessa coerção, você pode, sem saber, perder dados ao passá-lo para uma função. O exemplo mais simples é passar um `float` para uma função que requer um `int`.

```php
function add(int $a, int $b)
{
    return $a + $b;
}

add(897.23481, 361.53);
```

Ambos os floats são transformados em números inteiros, 897 e 361 . Este é o mesmo comportamento de um casting, por exemplo, `(int) 361.53` retornará 361.

*By default, hint coercion will result in the following:*

* `int(1)` ⇒ `function(float $foo)` ⇒ `float(1.0)`
* `float(1.5)` ⇒ `function(int $foo)` ⇒ `int(1)`
* `string("100")` ⇒ `function(int $foo)` ⇒ `int(100)`
* `string("100int")` ⇒ `+function(int $foo)` ⇒ `int(100)`
* `string("1.23")` ⇒ `function(float $foo)` ⇒ `float(1.23)`
* `string("1.23float")` ⇒ `function(float $foo)` `float(1.23)`

#### Tipos Estritos, Rigorosos (Strict Types)

Além do comportamento padrão, o PHP 7.0 também oferece **Strict Types Hints**. Isso significa que em vez de coagir, qualquer tipo de incompatibilidade resultará em uma exceção `\TypeError`.

O comportamento de strict type hint só pode ser ativado pela chamada do sistema. Isso significa que, por exemplo, um autor de biblioteca não pode ditar que sua biblioteca é estritamente digitada: isso só pode ser feito no código que o chama.

Isso pode parecer estranho, porque se você não pode impor um uso rígido, como você pode ter certeza de que sua biblioteca é usada corretamente? Embora esta seja uma preocupação válida, você não precisa estar preocupado com o fato de a entrada resultante ser o tipo errado. Independentemente do modo estrito, independentemente do que um valor seja passado, será sempre o tipo correto dentro da função.

Por padrão, as dicas de tipo escalar não são restritivas. Isso significa que podemos passar números flutuantes para um método que espera um número inteiro.

Para habilitar tipos rigorosos, usamos a construção `declare` com a nova diretiva `strict_types`: `declare(strict_types = 1);`.

Agora, se passarmos um número flutuante para a função de esperar como entrada um número inteiro, obteremos um fatal error `Uncaught Type Error`. Erros semelhantes serão gerados se passarmos uma seqüência de caracteres para um método que não seja do tipo de seqüência de caracteres.

A declaração de construção deve ser a primeira declaração no arquivo. Isso significa que nada pode ser colocado entre a tag PHP aberta e `declare`, exceto comentários. Isso inclui a declaração do namespace, que deve ser colocada imediatamente após qualquer construção de `declare`.

*Enabling strict types*
```php
// Enable strict types
declare(strict_types = 1);

namespace MyProject\Component;

hinted("foo", 123, 4.35, true);
```

Isso resultaria em uma exceção de `\TypeError` para o primeiro argumento incorreto com a mensagem `TypeError: Argument 1 passed to hinted() must be of the type boolean, string given`.

### Return Type Hints

Outra característica importante do PHP 7.0 é a capacidade de definir o tipo de dados de retorno para uma função ou método. Ele se comporta da mesma maneira que *Strict Types Hints* se comportam.

*É preciso saber que você pode usar sugestões de tipo não escalar para dicas de retorno também: classes/interfaces, array e callable. Isso é muitas vezes ignorado, já que os tipos de retorno são vistos como uma extensão do tipo escalar do tipo RFC.*

Return hints seguem as mesmas regras em configurações de sugestão coercitivas ou rígidas. Com sugestões de tipo coercitivo, o retorno será forçado ao tipo sugerido quando possível, enquanto que as dicas de tipo estritas resultarão em uma exceção `\TypeError` na incompatibilidade de tipos.

*Return type hints*
```php
function divide(int $a, int $b): int
{
    return $a / $b;
}

divide(4, 2);
divide(5, 2);
```

*Ambas as chamadas resultarão em `int(2)` no modo coercitivo, ou causará uma exceção `\TypeError` no modo estrito com a mensagem `Return value of divide() must be of the type integer, float returned in strict mode.`.*

Apesar do fato de que todos os argumentos são inteiros, o resultado da divisão pode resultar em um float, você pode ver novamente a perda de precisão no modo coercivo ou exceções de `\TypeError` no modo estrito.

### Reserved Keywords for Future Types

Uma alteração final que foi adicionada para types hints é que a lista de nomes de tipos a seguir não pode mais ser usada como nomes de classe, interface ou traço:

* int
* float
* bool
* string
* true
* false
* null

Isso foi feito para facilitar possíveis mudanças futuras que, caso contrário, precisariam esperar até o PHP 8 para preservar a compatibilidade com versões anteriores no PHP 7.x.

## Recursos

### Leitura adicional - Referências

* http://php.net/manual/en/appendices.php
* http://php.net/manual/en/migration70.new-features.php
* http://php.net/manual/en/migration71.new-features.php
* http://php.net/manual/en/migration72.new-features.php
* https://pt.stackoverflow.com/questions/32880/o-que-s%C3%A3o-covari%C3%A2ncia-e-contravari%C3%A2ncia
* https://docs.microsoft.com/pt-br/dotnet/standard/generics/covariance-and-contravariance
