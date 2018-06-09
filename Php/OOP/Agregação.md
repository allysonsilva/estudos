# Agregação Em Php

> Agregação associam classes e objetos, é quando uma classe `Todo` é uma coleção ou recipientes de outras classes. Este estudo aborda conceitos acerca de Agregação na orientação a objetos em php.

*É uma relação conhecida como Todo/Parte. Um objeto agrega o outro, assim quando o objeto agregador deixa de existir, de maneira nenhuma o objeto agregado deixará de existir. Um objeto não depende do outro para existir.*

* Um objeto poderá agregar uma ou mais instâncias de um outro objeto.
* Toda vez que temos agregação, significa que a *Parte* pode ser compartilhada entre vários objetos.
* *Todo/Parte* significa que um dos lados da associação (um classe) é chamado de *Todo* e o outro lado é chamado de *Parte*, já que a *Parte* nos permite pensar que: A *Parte* está contida no *Todo*.

## Implementando Agregação

* `Carrinho` é a classe que representa o [*Todo*].
* `Produtos` são as classes que representam [*Parte*] que compoem a cesta de compras.
* Perceba que *produto* não depende de *carrinho de compras* para existir, então sua função no carrinho é de *agregação*. Se o carrinho de compras não existir mais, os produto continuaram existindo pelo motivo de que os produtos só estam agregando ao carrinho de compras e não compondo-o(composição).

### Exemplo

```php
class Product
{
    private $cod;
    private $name;
    private $value;

    public function __construct(int $cod, string $name, float $value)
    {
        $this->cod = $cod;
        $this->name = $name;
        $this->value = $value;
    }

    public function __call(string $method, array $arguments)
    {
        /** @var string */
        $prefix = substr($method, 0, 3);

        $property = strtolower(substr(str_replace($prefix, '', $method), 0, 1)) . substr($method, 4, strlen($method));
        $property = (function () use ($property) {
            return strtolower(preg_replace('/([a-z])([A-Z])/', '$1_$2', $property));
        })();

        if (property_exists($this, $property)) {
            if (strcasecmp($prefix, 'set') === 0) {
                $this->$property = current($arguments);
            } elseif (strcasecmp($prefix, 'get') === 0) {
                return $this->$property;
            }
        } else {
            throw new BadFunctionCallException("Propriedade $property inexistente.");
        }
    }
}

class ShoppingCart
{
    private $itens;

    public function addProduct(Product $item)
    {
        $this->itens[] = $item;
    }

    public function listItens()
    {
        foreach ($this->itens as $item) {
            yield $item->getName();
        }
    }

    public function total()
    {
        $total = 0;
        foreach ($this->itens as $item) {
            $total += $item->getValue();
        }

        return number_format($total, 2, ',', '.');
    }
}

$produto1 = new Product(1, 'Produto 01', 10.10);
$produto2 = new Product(2, 'Produto 02', 20.10);
$produto3 = new Product(3, 'Produto 03', 30.10);

var_dump($produto1->getCod()); /* int(1) */
var_dump($produto2->getCod()); /* int(2) */
var_dump($produto3->getCod()); /* int(3) */

$produto1->setCod(4);
$produto2->setCod(5);
$produto3->setCod(6);

var_dump($produto1->getCod()); /* int(4) */
var_dump($produto2->getCod()); /* int(5) */
var_dump($produto3->getCod()); /* int(6) */

$shoppingCart = new ShoppingCart();
$shoppingCart->addProduct($produto1); /** Agregando produto ao carrinho */
$shoppingCart->addProduct($produto2); /** Agregando produto ao carrinho */
$shoppingCart->addProduct($produto3); /** Agregando produto ao carrinho */

echo PHP_EOL;
echo "R$ {$shoppingCart->total()}";
echo PHP_EOL;
foreach ($shoppingCart->listItens() as $productName) {
    echo PHP_EOL . $productName;
}
```
