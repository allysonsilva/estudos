# Factory Method

![Factory Method](https://yuml.me/diagram/scruffy/class/%5BCreator%5D%5E-%5BConcreteCreator%5D,%20%5BProduct%5D%5E-%5BConcreteProduct%5D,%20%5BConcreteCreator%5Dcreates-.-%3E%5BConcreteProduct%5D,%20%5BCreator%7CfactoryMethod%28%29;businessMethod%28%29;],%20[ConcreteCreator%7CfactoryMethod%28%29;],%20[Creator]-[note:%20abstract%20factoryMethod%28%29;%7Bbg:cornsilk%7D],%20[ConcreteCreator]-[note:%20return%20new%20ConcreteProduct%28%29;%7Bbg:cornsilk%7D%20]. "Factory Method")

## Introdução

- Permite que você crie objetos sem saber da sua real implementação, como o mesmo esta sendo implementado. O cliente não sabe e não conhece a real implementação do código, qual é a classe real que está realizando todo o processo.

- Usado para criar instâncias de diferentes classes do mesmo tipo.

- É um padrão de design que permite a criação de objetos sem especificar o tipo de objeto a ser criado no código. Uma classe de Factory contém um método que permite a determinação do tipo criado em tempo de execução.

- Defina uma interface para criar um objeto, mas as subclasses decidem quais classes instanciar. Permite que uma classe adie a instanciação para subclasses.

- No padrão de Factory Method, criamos um objeto sem expor a lógica de criação ao cliente e referimo-nos ao objeto recém-criado usando uma interface comum.

- Factory Method: criação através da herança. Prototype: criação através da delegação.

## Participantes

**FactoryBase**: Esta é uma classe abstrata para as classes concretas de Factory que retornará novos objetos. Em alguns casos, pode ser uma interface simples que contenha a assinatura para o Factory Method. Esta classe contém o `FactoryMethod` no qual retorna um objeto de `ProductBase`.

**ConcreteFactory**: Representa a implementação concreta da Factory. Geralmente, esta classe substitui a `super` `FactoryMethod` e retorna um objeto `ConcreteProduct`.

**ProductBase**: Esta é uma classe base para todos os produtos concreto criados por Factory. Em alguns casos, pode ser uma interface simples.

**ConcreteProduct**: Esta é uma implementação concreta de `ProductBase`. As classes concretas de produtos podem incluir funcionalidades específicas. Esses objetos são criados por `ConcreteFactory`.

## Vantagens do Factory Method

- O Padrão de Método de Fábrica permite que as sub-classes escolham o tipo de objetos a serem criados.
- Ele promove o baixo acoplamento, eliminando a necessidade de vincular classes específicas do aplicativo ao código. Isso significa que o código interage unicamente com a interface resultante ou a classe abstrata, para que ele funcione com qualquer classe que implemente essa interface ou que estenda essa classe abstrata.

## Uso do Padrão de Design de Fábrica

O padrão de Factory é mais adequado quando há algumas etapas complexas da criação de objetos que estão envolvidas. Para garantir que essas etapas sejam centralizadas e não expostas às classes de composição, o padrão de Factory deve ser usado.

- Quando uma classe não sabe quais sub-classes serão necessárias para criar.
- Quando uma classe quer que suas sub-classes especifiquem os objetos a serem criados.
- Quando as classes pai escolhem a criação de objetos para suas sub-classes.

_O padrão de design do método Factory resolve problemas como:_

- Como um objeto pode ser criado para que as subclasses possam redefinir qual classe instanciar?
- Como uma classe pode adiar a instanciação para subclasses?

_O padrão de Factory Method depende da herança, uma vez que a criação de objeto é delegada em subclasses que implementam o Factory Method para criar objetos._

## Exemplos

### Código Estrutural

_Código apenas como modelo da implementação do padrão._

```csharp
using System;

namespace FactoryMethod
{
    static class Program
    {
        static void Main()
        {
            FactoryBase factory = new ConcreteFactory();

            ProductBase product = factory.FactoryMethod(1);
            product.ShowInfo();

            product = factory.FactoryMethod(2);
            product.ShowInfo();
        }
    }

    public abstract class FactoryBase
    {
        public abstract ProductBase FactoryMethod(int type);
    }

    public class ConcreteFactory : FactoryBase
    {
        public override ProductBase FactoryMethod(int type)
        {
            switch (type)
            {
                case 1:
                    return new ConcreteProduct1();
                case 2:
                    return new ConcreteProduct2();
                default:
                    throw new ArgumentException("Invalid type.", "type");
            }
        }
    }

    public abstract class ProductBase
    {
        public abstract void ShowInfo();
    }

    public class ConcreteProduct1 : ProductBase
    {
        public override void ShowInfo()
        {
            Console.WriteLine("Product 1");
        }
    }

    public class ConcreteProduct2 : ProductBase
    {
        public override void ShowInfo()
        {
            Console.WriteLine("Product 2");
        }
    }
}
```

**Output**

```
Product 1
Product 2
```

### Modelo de distribuidora de livros com base na localização

**EXEMPLO**

![Uml Factory Method Exemplo Distribuidora de Livros](./Images/Factory%20Method/FactoryMethodExemploDistribuidoraLivros.png "Uml Factory Method Exemplo Distribuidora de Livros")

**UML**

![Uml Factory Method](./Images/Factory%20Method/FactoryMethod_1.png "Uml Factory Method")

Digamos que você possui uma livraria online e as pessoas chegam ao seu site para comprar livros. Quando um cliente compra um livro, você simplesmente possui uma distribuidora de livros para enviar diretamente ao cliente. Existem muitas distribuidoras que você pode escolher para enviar os livros diretamente para seus clientes.

Como você tem muitos distribuidoras, você tem a seguinte lógica para determinar qual distribuidor escolher para enviar os livros aos seus clientes:

- Se o cliente estiver na Costa-Leste, você usará `EastCoastDistributor`.
- Se o cliente estiver no Centro-Oeste, você usará `MidWestDistributor`.
- Se o cliente estiver na Costa-Oeste, você usará o `WestCoastDistributor`.

O ponto chave é que seu cliente não deve se importar com a distribuidora que você escolher, porque eles terão seus livros independentemente disso. Está completamente escondido do ponto de vista do cliente e eles não devem se preocupar com isso. Você, a livraria online, é a que determina o uso da distribuidora.

A lógica que decide qual distribuidor usar é no método `BookStore.GetDistributor`. O método retorna `IDistributor`.

Este é o Factory Method que retorna um produto (um distribuidor). Cada um dos distribuidores implementa a interface `IDistributor`, que possui o método `ShipBook`. O código do cliente apenas diz "me dê um distribuidor que possa fazer o ShipBook" sem ter que saber qual o distribuidor que você vai retornar.

```csharp
IDistributor distributor = bookStore.GetDistributor();   // The client gets the distributor without having to know which distributor is being used
```

Tomando mais um passo adiante, podemos resumir o `BookStore` como uma interface e ter mais tipos de livrarias, Você pode ter o código do cliente, como o seguinte, que não precisa ser alterado se a lógica de escolha do distribuidor mudou:

```csharp
// This client code will not need to be changed even if the logic for choosing the distributor changed inside the bookstores
public void ShipBook(IBookStore bookStore)
{
    IDistributor distributor = bookStore.GetDistributor();
    distributor.ShipBook();
}
```

Você pode passar em qualquer livraria que implemente a interface `IBookStore` para este método e usará o distribuidor correto automaticamente. O benefício é que o código do cliente não precisa ser alterado quando você altera a lógica do método `GetDistributor` das livrarias.

Abaixo estão o código de implementação e a saída do padrão de _Factory Method_ usando nosso exemplo. Observe que você pode alterar o código na Factory(`ConcreteFactory`) para produzir diferentes produtos(distribuidores) sem alterar o código do cliente:

```csharp
using System;

namespace FactoryMethod
{
    public enum CustomerLocation { EastCoast, MidWest, WestCoast }

    class Program
    {
        static void Main(string[] args)
        {
            IBookStore bookstore;

            Console.WriteLine("\nEast Coast Customer:");
            bookstore = new BookStoreA(CustomerLocation.EastCoast);
            ShipBook(bookstore);

            Console.WriteLine("\nMid West Customer:");
            bookstore = new BookStoreA(CustomerLocation.MidWest);
            ShipBook(bookstore);

            Console.WriteLine("\nWest Coast Customer:");
            bookstore = new BookStoreA(CustomerLocation.WestCoast);
            ShipBook(bookstore);
        }

        //**** Client code does not need to be changed ***
        private static void ShipBook(IBookStore bookStore)
        {
            IDistributor distributor = bookStore.GetDistributor();
            distributor.ShipBook();
        }
    }

    // The Factory
    public interface IBookStore
    {
        IDistributor GetDistributor();
    }

    // Concrete Factory
    public class BookStoreA : IBookStore
    {
        private CustomerLocation location;

        public BookStoreA(CustomerLocation location)
        {
            this.location = location;
        }

        IDistributor IBookStore.GetDistributor()
        {
            // Internal logic on which distributor to return
            //*** Logic can be changed without changing the client code ****
            switch (location)
            {
                case CustomerLocation.EastCoast:
                    return new EastCoastDistributor();
                case CustomerLocation.MidWest:
                    return new MidWestDistributor();
                case CustomerLocation.WestCoast:
                    return new WestCoastDistributor();
            }

            return null;
        }
    }

    // The Product
    public interface IDistributor
    {
        void ShipBook();
    }

    // Concrete Product
    public class EastCoastDistributor : IDistributor
    {
        void IDistributor.ShipBook()
        {
            Console.WriteLine("Book shipped by East Coast Distributor");
        }
    }

    // Concrete Product
    public class MidWestDistributor : IDistributor
    {
        void IDistributor.ShipBook()
        {
            Console.WriteLine("Book shipped by Mid West Distributor");
        }
    }

    // Conceret Product
    public class WestCoastDistributor : IDistributor
    {
        void IDistributor.ShipBook()
        {
            Console.WriteLine("Book shipped by West Coast Distributor");
        }
    }
}
```

**Output**

```
East Coast Customer:
Book shipped by East Coast Distributor

Mid West Customer:
Book shipped by Mid West Distributor

West Coast Customer:
Book shipped by West Coast Distributor
```

### Exemplo de fabricação de carros

Temos uma interface `IVehicleFactory` com um método `CreateVehicle` que retorna o objeto `Vehicle`. Outras duas classes `FordExplorerFactory` e `LincolnAviatorFactory` são concretas implementam essa interface. No caso de `FordExplorerFactory`, o método `CreateVehicle` retorna o objeto `FordExplorer` que deriva do método da classe abstrata `Vehicle` e substitui `ShowInfo`. Este método apenas exibe informações sobre o veículo. `FordExplored` tem um construtor padrão que preenche as propriedades desse objeto.

No caso de `LincolnAviatorFactory` uma situação é um pouco diferente. O método `CreateVehicle` retorna o objeto `LincolnAviator`, mas esse objeto possui um construtor com parâmetros que os valores são usados ​​para preencher as propriedades do objeto.

Este exemplo demonstra como usar diferentes Factory para criar diferentes tipos de objetos.

```csharp
using System;
using System.Reflection;
using System.Collections.Generic;

namespace FactoryMethod
{
    class Program
    {
        static void Main(string[] args)
        {
            IVehicleFactory factory;

            factory = GetFactory("FactoryMethod.LincolnAviatorFactory");
            var lincolnAviator = factory.CreateVehicle();
            lincolnAviator.ShowInfo();

            factory = GetFactory("FactoryMethod.FordExplorerFactory");
            var fordExplorer = factory.CreateVehicle();
            fordExplorer.ShowInfo();
        }

        static IVehicleFactory GetFactory(string factoryName)
        {
            return Assembly.GetExecutingAssembly().CreateInstance(factoryName) as IVehicleFactory;
        }
    }

    public interface IVehicleFactory
    {
        Vehicle CreateVehicle();
    }

    public abstract class Vehicle
    {
        public string Model { get; set; }
        public string Engine { get; set; }
        public string Transmission { get; set; }
        public string Body { get; set; }
        public int Doors { get; set; }

        public List<string> Accessories = new List<string>();

        public abstract void ShowInfo();
    }

    public class FordExplorerFactory : IVehicleFactory
    {
        public Vehicle CreateVehicle()
        {
            return new FordExplorer();
        }
    }

    public class FordExplorer : Vehicle
    {
        public FordExplorer()
        {
            Model = "Ford Explorer";
            Engine = "4.0 L Cologne V6";
            Transmission = "5-speed M50D-R1 manual";
            Body = "SUV";
            Doors = 5;

            Accessories.Add("Car Cover");
            Accessories.Add("Sun Shade");
        }

        public override void ShowInfo()
        {
            Console.WriteLine();

            Console.WriteLine("Model: {0}", Model);
            Console.WriteLine("Engine: {0}", Engine);
            Console.WriteLine("Body: {0}", Body);
            Console.WriteLine("Doors: {0}", Doors);
            Console.WriteLine("Transmission: {0}", Transmission);
            Console.WriteLine("Accessories:");

            foreach (var accessory in Accessories) {
                Console.WriteLine("\t{0}", accessory);
            }
        }
    }

    public class LincolnAviatorFactory : IVehicleFactory
    {
        public Vehicle CreateVehicle()
        {
            return new LincolnAviator("Lincoln Aviator", "4.6 L DOHC Modular V8", "5-speed automatic", "SUV", 4);
        }
    }

    public class LincolnAviator : Vehicle
    {
        public LincolnAviator(string model, string engine, string transmission, string body, int doors)
        {
            Model = model;
            Engine = engine;
            Transmission = transmission;
            Body = body;
            Doors = doors;

            Accessories.Add("Leather Look Seat Covers");
            Accessories.Add("Chequered Plate Racing Floor");
            Accessories.Add("4x 200 Watt Coaxial Speekers");
            Accessories.Add("500 Watt Bass Subwoofer");
        }

        public override void ShowInfo()
        {
            Console.WriteLine();

            Console.WriteLine("Model: {0}", Model);
            Console.WriteLine("Engine: {0}", Engine);
            Console.WriteLine("Body: {0}", Body);
            Console.WriteLine("Doors: {0}", Doors);
            Console.WriteLine("Transmission: {0}", Transmission);
            Console.WriteLine("Accessories:");

            foreach (var accessory in Accessories) {
                Console.WriteLine("\t{0}", accessory);
            }
        }
    }
}
```

**Output**

```
Model: Lincoln Aviator
Engine: 4.6 L DOHC Modular V8
Body: SUV
Doors: 4
Transmission: 5-speed automatic
Accessories:
    Leather Look Seat Covers
    Chequered Plate Racing Floor
    4x 200 Watt Coaxial Speekers
    500 Watt Bass Subwoofer

Model: Ford Explorer
Engine: 4.0 L Cologne V6
Body: SUV
Doors: 5
Transmission: 5-speed M50D-R1 manual
Accessories:
    Car Cover
    Sun Shade
```

### Exemplo na criação de diferentes tipos de documentos

Este código demonstra o método Factory oferecendo flexibilidade na criação de diferentes documentos. As classes de documentos `Report` e `Resume` instanciam versões estendidas da classe `Document`. Aqui, o método Factory é chamado no construtor da classe Base de `Document`.

```csharp
using System;
using System.Reflection;
using System.Collections.Generic;

namespace FactoryMethod
{
    class Program
    {
        static void Main(string[] args)
        {
            // Note: constructors call Factory Method
            Document[] documents = new Document[2];

            documents[0] = new Resume();
            documents[1] = new Report();

            // Display document pages
            foreach (Document document in documents) {
                Console.WriteLine("\n" + document.GetType().Name + " -- ");

                foreach (Page page in document.Pages) {
                    Console.WriteLine("     " + page.GetType().Name);
                }
            }
        }
    }

    // The 'Product' abstract class
    abstract class Page
    {
    }

    // A 'ConcreteProduct' class
    class SkillsPage : Page
    {
    }

    // A 'ConcreteProduct' class
    class EducationPage : Page
    {
    }

    // A 'ConcreteProduct' class
    class ExperiencePage : Page
    {
    }

    // A 'ConcreteProduct' class
    class IntroductionPage : Page
    {
    }

    // A 'ConcreteProduct' class
    class ResultsPage : Page
    {
    }

    // A 'ConcreteProduct' class
    class ConclusionPage : Page
    {
    }

    // A 'ConcreteProduct' class
    class SummaryPage : Page
    {
    }

    // A 'ConcreteProduct' class
    class BibliographyPage : Page
    {
    }

    // The 'Creator' abstract class
    abstract class Document
    {
        private List<Page> _pages = new List<Page>();

        // Constructor calls abstract Factory method
        public Document()
        {
            this.CreatePages();
        }

        public List<Page> Pages
        {
            get { return _pages; }
        }

        // Factory Method
        public abstract void CreatePages();
    }

    // A 'ConcreteCreator' class
    class Resume : Document
    {
        // Factory Method implementation
        public override void CreatePages()
        {
            Pages.Add(new SkillsPage());
            Pages.Add(new EducationPage());
            Pages.Add(new ExperiencePage());
        }
    }

    // A 'ConcreteCreator' class
    class Report : Document
    {
        // Factory Method implementation
        public override void CreatePages()
        {
            Pages.Add(new IntroductionPage());
            Pages.Add(new ResultsPage());
            Pages.Add(new ConclusionPage());
            Pages.Add(new SummaryPage());
            Pages.Add(new BibliographyPage());
        }
    }
}
```

**Output**

```
Resume --
     SkillsPage
     EducationPage
     ExperiencePage

Report --
     IntroductionPage
     ResultsPage
     ConclusionPage
     SummaryPage
     BibliographyPage
```

### Criando diferentes tipos de botões html

O objetivo principal deste padrão é encapsular o procedimento de criação que pode abranger diferentes classes em uma única função. Ao fornecer o contexto correto ao Factory Method, ele retornará o objeto correto.

O melhor momento para usar o padrão de Factory Method é quando você tem diferentes variações de uma única entidade. Digamos que você tenha uma classe de botão; esta classe tem diferentes variações, como `ImageButton`, `InputButton` e `FlashButton`. Dependendo do local, talvez seja necessário criar botões diferentes - é aqui que você pode usar uma Factory para criar os botões para você!

Let's begin by creating our three classes:

```php
abstract class Button
{
    protected $html;

    public function getHtml()
    {
        return $this->html;
    }
}

class ImageButton extends Button
{
    protected $html = "..."; // This should be whatever HTML you want for your image-based button
}

class InputButton extends Button
{
    protected $html = "..."; // This should be whatever HTML you want for your normal button (<input type="button"... />);
}

class FlashButton extends Button
{
    protected $html = "..."; // This should be whatever HTML you want for your flash-based button
}
```

Now, we can create our factory class:

```php
class ButtonFactory
{
    public static function createButton(string $type): Button
    {
        $baseClass = 'Button';
        $targetClass = ucfirst($type).$baseClass;

        if (class_exists($targetClass) && is_subclass_of($targetClass, $baseClass)) {
            return new $targetClass;
        }

        throw new Exception("The button type '$type' is not recognized.");
    }
}
```

We can use this code like so:

```php
foreach(['image', 'input', 'flash'] as $button) {
    echo ButtonFactory::createButton($button)->getHtml()
}
```

A saída deve ser o HTML de todos os seus tipos de botão. Desta forma, você poderá especificar qual botão criar, dependendo da situação e reutilizar a condição também.

### Modelo de criação de páginas web

```php
/**
 * SECTION 1: A WidgetHelper interface and two different implementations.
 *
 * This class purpose is to generate blinking text in spite of all usability recommendations.
 *
 * This is the `Product`.
 */
interface WidgetHelper
{
    public function generateHtml(string $text): string;
}

/**
 * Implementation that generates html tied to a javascript library.
 *
 * This is one `ConcreteProduct`.
 */
class JavascriptWidgetHelper implements WidgetHelper
{
    public function generateHtml(string $text): string
    {
        return '<div dojoType="...">' . $text . '</div>';
    }
}

/**
 * Implementation that generates html that loads a flash object.
 *
 * This is one `ConcreteProduct`.
 */
class FlashWidgetHelper implements WidgetHelper
{
    public function generateHtml(string $text): string
    {
        return '<object> <param name="text">' . $text . '</param> </object>';
    }
}

/**
 * SECTION 2: A `Creator` class which defines a seam to change the object it creates during execution.
 *
 * Normally we would inject the chosen WidgetHelper to the Client class, but
 * since WidgetHelpers are created during the execution (basing on business
 * logic) we cannot instantiate them in advance and so we need an abstraction
 * on the creation process to allow reusing and overriding: the Factory Method createWidgetHelper().
 */
abstract class LoginPage
{
    public abstract function createWidgetHelper(): WidgetHelper;

    /**
     * A Template Method which uses the Factory Method.
     */
    public function render()
    {
        $userId = uniqid('User ');
        // insert all the logic needed here...
        if (true || $complexBusinessLogicRules) {
            return ($this->createWidgetHelper())->generateHtml("Welcome, $userId");
        }
    }
}

/**
 * First subclass: creates Javascript-based helpers.
 *
 * This is one `ConcreteCreator`.
 */
class JavascriptLoginPage extends LoginPage
{
    public function createWidgetHelper(): WidgetHelper
    {
        return new JavascriptWidgetHelper();
    }
}

/**
 * Second subclass: creates Flash-based helpers.
 *
 * This is one `ConcreteCreator`.
 */
class FlashLoginPage extends LoginPage
{
    public function createWidgetHelper(): WidgetHelper
    {
        return new FlashWidgetHelper();
    }
}

$page = new FlashLoginPage();
echo $page->render(), "\n";

$page = new JavascriptLoginPage();
echo $page->render(), "\n";
```

**Output**

```
<object> <param name="text">Welcome, User 5b676aac99ee7</param> </object>
<div dojoType="...">Welcome, User 5b676aac9a033</div>
```
