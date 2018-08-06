# Design Patterns

## Builder Pattern

### GoF Definition

Separate the construction of a complex object from its representation so that the same construction processes can create different representations.

### Concept

The Builder pattern is useful for creating complex objects that have multiple parts.

The creation process of an object should be independent of these parts; in other words, the construction process does not care how these parts are assembled. In addition, you should be able to use the same construction process to create different representations of the objects.

![Builder Pattern](./Images/Model%20Builder%20Pattern.png "Builder Pattern")

### Illustration

This example has the following participants: `IBuilder`, `Car`, `MotorCycle`, `Product`, and `Director`. `IBuilder` is used to create parts of the `Product` object, where `Product` represents the complex object under construction. `Car` and `MotorCycle` are the concrete implementations of the `IBuilder` interface. They implement the `IBuilder` interface. That’s why they need to supply the body for these methods: `StartUpOperations()`, `BuildBody()`, `InsertWheels()`, `AddHeadlights()`, `EndOperations()`, and `GetVehicle()`. The first five methods are straightforward; they are used to perform some operation at the beginning, build the body of the vehicle, insert a number of wheels into it, add headlights to the vehicle, and perform an operation at the end, respectively. `GetVehicle()` returns the ultimate product. Finally, `Director` is responsible for constructing the final representation of these products using the `IBuilder` interface. (See the structure defined by GoF in Figure) Notice that `Director` is calling the same `Construct()` method to create different types of vehicles.

### Class Diagram

![Class Diagram Builder](./Images/Class%20Diagram%20Builder.png "Class Diagram Builder")

### Implementation

```csharp
using System;
using System.Collections.Generic;

namespace BuilderPattern
{
    interface IBuilder
    {
        void StartUpOperations();
        void BuildBody();
        void InsertWheels();
        void AddHeadlights();
        void  EndOperations();
        Product GetVehicle();
    }

    class Car : IBuilder
    {
        private string brandName;
        private Product product;

        public Car(string brand)
        {
            product = new Product();
            this.brandName = brand;
        }

        public void StartUpOperations()
        {
            product.Add(string.Format("Car Model name :{0}",this.brandName));
        }

        public void BuildBody()
        {
            product.Add("This is a body of a Car");
        }

        public void InsertWheels()
        {
            product.Add("4 wheels are added");
        }

        public void AddHeadlights()
        {
            product.Add("2 Headlights are added");
        }

        public void EndOperations()
        {
        }

        public Product GetVehicle()
        {
            return product;
        }
    }

    class MotorCycle : IBuilder
    {
        private string brandName;
        private Product product;

        public MotorCycle(string brand)
        {
            product = new Product();
            this.brandName = brand;
        }

        public void StartUpOperations()
        {
        }

        public  void BuildBody()
        {
            product.Add("This is a body of a Motorcycle");
        }

        public void InsertWheels()
        {
            product.Add("2 wheels are added");
        }

        public void AddHeadlights()
        {
            product.Add("1 Headlights are added");
        }

        public void EndOperations()
        {
            product.Add(string.Format("Motorcycle Model name :{0}", this.brandName));
        }

        public Product GetVehicle()
        {
            return product;
        }
    }

    class Product
    {
        private LinkedList<string> parts;

        public Product()
        {
            parts = new LinkedList<string>();
        }

        public void Add(string part)
        {
            parts.AddLast(part);
        }

        public void Show()
        {
            Console.WriteLine("\nProduct completed as below :");

            foreach (string part in parts)
                Console.WriteLine(part);
        }
    }

    class Director
    {
        IBuilder builder;

        public void Construct(IBuilder builder)
        {
            this.builder = builder;

            builder.StartUpOperations();
            builder.BuildBody();
            builder.InsertWheels();
            builder.AddHeadlights();
            builder.EndOperations();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Builder Pattern Demo***");
            Director director = new Director();

            IBuilder b1 = new Car("Ford");
            IBuilder b2 = new MotorCycle("Honda");

            // Making Car
            director.Construct(b1);
            Product p1 = b1.GetVehicle();
            p1.Show();

            // Making MotorCycle
            director.Construct(b2);
            Product p2 = b2.GetVehicle();
            p2.Show();

            Console.ReadLine();
        }
    }
}
```

### Output

```
***Builder Pattern Demo***

Product completed as below :
Car Model name :Ford
This is a body of a Car
4 wheels are added
2 Headlights are added

Product completed as below :
This is a body of a Motorcycle
2 wheels are added
1 Headlights are added
Motorcycle Model name :Honda
```

### Q&A Session

#### What is the advantage of using a Builder pattern?

- You direct the builder to build the objects step-by-step, and you promote encapsulation by hiding the details of the complex construction process. The director can retrieve the final product from the builder when the whole construction is over. In general, at a high level, you seem to have only one method that makes the complete product, but other internal methods are involved in the creation process. So, you have finer control over the construction process.

- Using this pattern, the same construction process can produce different products.

- You can also vary the internal representation of products.

#### How can you decide whether to use an abstract class or an interface in an application?

If you want to have some centralized or default behaviors, an abstract class is a better choice. In those cases, you can provide some default implementation. On the other hand, the interface implementation starts from scratch and indicates some kind of rules/contracts such as what is to be done, but it does not enforce the “how” part upon you. Also, interfaces are preferred when you are trying to implement the concept of multiple inheritance.

Remember that if you need to add a new method in an interface, then you need to track down all the implementations of that interface, and you need to put the concrete implementation for that method in all those places. In such a case, an abstract class is a better choice because you can add a new method in an abstract class with a default implementation, and the existing code can run smoothly.

MSDN provides following recommendations:

- If you anticipate creating multiple versions of your component, create an abstract class. Abstract classes provide a simple and easy way to version your components. By updating the base class, all inheriting classes are automatically updated with the change. Interfaces, on the other hand, cannot be changed once created. If a new version of an interface is required, you must create a whole new interface.

- If the functionality you are creating will be useful across a wide range of disparate objects, use an interface. Abstract classes should be used primarily for objects that are closely related, whereas interfaces are best suited for providing common functionality to unrelated classes.

- If you are designing small, concise bits of functionality, use interfaces. If you are designing large functional units, use an abstract class.

- If you want to provide common, implemented functionality among all implementations of your component, use an abstract class. Abstract classes allow you to partially implement your class, whereas interfaces contain no implementation for any members.

#### In the example, for cars, the model names are added in the beginning, but for motorcycles, the model names are added at the end. Is it intentional?

Yes. I did this to demonstrate the fact that each of the concrete builders can decide how it wants to produce the final products. They have this freedom.

## Factory Method Pattern

### GoF Definition

Define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory Method pattern lets a class defer instantiation to subclasses.

### Computer World Example

In an application, you may have different database users. For example, one user may use Oracle, and the other may use SQL Server. Whenever you need to insert data into your database, you need to create either a `SqlConnection` or an `OracleConnection` and only then can you proceed. If you put the code into `if-else` (or `switch`) statements, you need to repeat a lot of code, which isn’t easily maintainable. This is because whenever you need to support a new type of connection, you need to reopen your code and make those modifications. This type of problem can be resolved using the Factory Method pattern. Here I’ll provide an abstract creator class (`IAnimalFactory`) to define the basic structure. As per the definition, the instantiation process will be carried out through the subclasses that derive from this abstract class.

### Class Diagram

![Class Diagram Factory Method](./Images/Class%20Diagram%20Factory%20Method.png "Class Diagram Factory Method")

### Implementation

```csharp
using System;

namespace FactoryMethodPattern
{
    public interface IAnimal
    {
        void Speak();
        void Action();
    }

    public class Dog : IAnimal
    {
        public void Speak()
        {
            Console.WriteLine("Dog says: Bow-Wow.");
        }

        public void Action()
        {
            Console.WriteLine("Dogs prefer barking...\n");
        }
    }

    public class Tiger : IAnimal
    {
        public void Speak()
        {
            Console.WriteLine("Tiger says: Halum.");
        }

        public void Action()
        {
            Console.WriteLine("Tigers prefer hunting...\n");
        }
    }

    public abstract class IAnimalFactory
    {
        // Remember the GoF definition which says "....Factory method lets a class defer instantiation to subclasses."
        // Following method will create a Tiger or Dog.But at this point it does not know whether it will get a Dog or a Tiger.It will be decided by the subclasses i.e.DogFactory or TigerFactory.
        // So, the following method is acting like a factory (of creation).
        public abstract IAnimal CreateAnimal();
    }

    public class DogFactory : IAnimalFactory
    {
        public override IAnimal CreateAnimal()
        {
            return new Dog();
        }
    }

    public class TigerFactory : IAnimalFactory
    {
        public override IAnimal CreateAnimal()
        {
            return new Tiger();
        }
    }

    class Client
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Factory Pattern Demo***\n");

            // Creating a Tiger Factory
            IAnimalFactory tigerFactory = new TigerFactory();

            // Creating a tiger using the Factory Method
            IAnimal aTiger = tigerFactory.CreateAnimal();
            aTiger.Speak();
            aTiger.Action();

            // Creating a DogFactory
            IAnimalFactory dogFactory = new DogFactory();

            // Creating a dog using the Factory Method
            IAnimal aDog = dogFactory.CreateAnimal();
            aDog.Speak();
            aDog.Action();

            Console.ReadKey();
        }
    }
}
```

### Output

```
***Factory Pattern Demo***

Tiger says: Halum.
Tigers prefer hunting...

Dog says: Bow-Wow.
Dogs prefer barking...
```

### Modified Implementation

```csharp
using System;

// Modifying the IAnimalFactory class and calling methods as per the design in Client.
namespace FactoryMethodPatternBeautification
{
    public interface IAnimal
    {
        void Speak();
        void Action();
    }

    public class Dog : IAnimal
    {
        public void Speak()
        {
            Console.WriteLine("Dog says: Bow-Wow.");
        }

        public void Action()
        {
            Console.WriteLine("Dogs prefer barking...\n");
        }
    }

    public class Tiger : IAnimal
    {
        public void Speak()
        {
            Console.WriteLine("Tiger says: Halum.");
        }

        public void Action()
        {
            Console.WriteLine("Tigers prefer hunting...\n");
        }
    }

    // Modifying the IAnimalFactory class.
    public abstract class IAnimalFactory
    {
        public IAnimal MakeAnimal()
        {
            Console.WriteLine("AnimalFactory.MakeAnimal()-You cannot ignore parent rules.");
            /*
            At this point,it doesn't know whether it will get a Dog or a Tiger.
            It will be decided by the subclasses i.e.DogFactory or TigerFactory.
            But it knows that it will Speak and it will have a preferred way of Action.
            */
            IAnimal animal = CreateAnimal();
            animal.Speak();
            animal.Action();
            return animal;
        }

        // So, the following method is acting like a factory (of creation).
        public abstract IAnimal CreateAnimal();
    }

    public class DogFactory : IAnimalFactory
    {
        public override IAnimal CreateAnimal()
        {
            return new Dog();
        }
    }
    public class TigerFactory : IAnimalFactory
    {
        public override IAnimal CreateAnimal()
        {
            return new Tiger();
        }
    }

    class Client
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Beautification to Factory Pattern Demo***\n");

            // Creating a tiger  using the Factory Method
            IAnimalFactory tigerFactory = new TigerFactory();
            IAnimal aTiger = tigerFactory.MakeAnimal();
            //IAnimal aTiger = tigerFactory.CreateAnimal();
            //aTiger.Speak();
            //aTiger.Action();

            // Creating a dog  using the Factory Method
            IAnimalFactory dogFactory = new DogFactory();
            IAnimal aDog = dogFactory.MakeAnimal();
            //IAnimal aDog = dogFactory.CreateAnimal();
            //aDog.Speak();
            //aDog.Action();

            Console.ReadKey();
        }
    }
}
```

**Output**
```
AnimalFactory.MakeAnimal()-You cannot ignore parent rules.
Tiger says: Halum.
Tigers prefer hunting...

AnimalFactory.MakeAnimal()-You cannot ignore parent rules.
Dog says: Bow-Wow.
Dogs prefer barking...
```

### Q&A Session

#### What are the advantages of using a factory like this?

- You are separating the code that varies from the code that does not vary (in other words, the advantages of using the Simple Factory pattern are still present). This helps you to maintain the code easily.

- The code is not tightly coupled, so you can add new classes such as Lion, Bear, and so on, at any time in the system without modifying the existing architecture. In other words, I have followed the “closed for modification but open for extension” principle.

#### You should always mark the factory method with an abstract keyword so that subclasses can complete them. Is that understanding correct?

No. Sometimes you may be interested in a default factory method if the creator has no subclasses. In that case, you cannot mark the factory method with an abstract keyword.

However, to see the real power of the Factory Method pattern, you may need to follow the design that is implemented here.

## Abstract Factory Pattern

### GoF Definition

Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

### Concept

An abstract factory is called a factory of factories. In this pattern, you provide a way to encapsulate a group of individual factories that have a common theme. In this process, you do not mention or specify their concrete classes.

This pattern helps you to interchange specific implementations without changing the code that uses them, even at runtime. However, it may result in unnecessary complexity and extra work. Even debugging becomes tough in some cases.

### Computer World Example

ADO.NET has already implemented similar concepts to establish a connection to a database.

To understand this pattern, you may want to extend your understanding of the Factory Method pattern. In the Factory Method pattern, you have two factories; one is for creating dogs and another is for creating tigers. But now suppose, you want to categorize dogs and tigers further; you choose to get a pet animal (dog or tiger) or a wild animal (dog or tiger) through the factories. To fulfill that demand, you introduce two concrete factories: `WildAnimalFactory` and `PetAnimalFactory`. `WildAnimalFactory` is responsible for creating wild animals, and `PetAnimalFactory` is responsible for creating pet animals.

### Illustration

![Abstract Factory Pattern](./Images/Abstract%20Factory%20Pattern.png "Abstract Factory Pattern")

I will follow a similar structure in the implementation in this chapter. `Program.cs` is the client looking for some animals (which are dogs and tigers in this case). You are exploring the construction process of both pets and wild animals in this implementation.

In this example, you have two concrete factories: `WildAnimalFactory` and `PetAnimalFactory`. You can guess that they are responsible for creating the concrete products of dogs and tigers. `WildAnimalFactory` will create wild animals (wild dogs and wild tigers), and `PetAnimalFactory` will create pet animals (pet dogs and pet tigers). For your reference, the participants and their roles are summarized here:

- `IAnimalFactory`: Abstract factory.

- `WildAnimalFactory`: Concrete factory. This will create wild dogs and wild tigers.

- `PetAnimalFactory`: Another concrete factory. This will create pet dogs and pet tigers.

- `ITiger` and `IDog`: Abstract products in this case.

- `PetTiger`, `PetDog`, `WildTiger`, and `WildDog`: These are the concrete products in this example.

### Class Diagram

![Class Diagram Abstract Factory](./Images/Class%20Diagram%20Abstract%20Factory.png "Class Diagram Abstract Factory")

### Implementation

```csharp
using System;

namespace AbstractFactoryPattern
{
    public interface IDog
    {
        void Speak();
        void Action();
    }

    public interface ITiger
    {
        void Speak();
        void Action();
    }

    #region Wild Animal collections
    class WildDog : IDog
    {
        public void Speak()
        {
            Console.WriteLine("Wild Dog says: Bow-Wow.");
        }

        public void Action()
        {
            Console.WriteLine("Wild Dogs prefer to roam freely in jungles.\n");
        }
    }

    class WildTiger : ITiger
    {
        public void Speak()
        {
            Console.WriteLine("Wild Tiger says: Halum.");
        }

        public void Action()
        {
            Console.WriteLine("Wild Tigers prefer hunting in jungles.\n");
        }
    }
    #endregion

    #region Pet Animal collections
    class PetDog : IDog
    {
        public void Speak()
        {
            Console.WriteLine("Pet Dog says: Bow-Wow.");
        }

        public void Action()
        {
            Console.WriteLine("Pet Dogs prefer to stay at home.\n");
        }
    }

    class PetTiger : ITiger
    {
        public void Speak()
        {
            Console.WriteLine("Pet Tiger says: Halum.");
        }

        public void Action()
        {
            Console.WriteLine("Pet Tigers play in an animal circus.\n");
        }
    }
    #endregion

    // Abstract Factory
    public interface IAnimalFactory
    {
        IDog GetDog();
        ITiger GetTiger();
    }

    // Concrete Factory-Wild Animal Factory
    public class WildAnimalFactory : IAnimalFactory
    {
        public IDog GetDog()
        {
            return new WildDog();
        }

        public ITiger GetTiger()
        {
            return new WildTiger();
        }
    }

    // Concrete Factory-Pet Animal Factory
    public class PetAnimalFactory : IAnimalFactory
    {
        public IDog GetDog()
        {
            return new PetDog();
        }

        public ITiger GetTiger()
        {
            return new PetTiger();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Abstract Factory Pattern Demo***\n");

            // Making a wild dog through WildAnimalFactory
            IAnimalFactory wildAnimalFactory = new WildAnimalFactory();
            IDog wildDog = wildAnimalFactory.GetDog();
            wildDog.Speak();
            wildDog.Action();

            // Making a wild tiger through WildAnimalFactory
            ITiger wildTiger = wildAnimalFactory.GetTiger();
            wildTiger.Speak();
            wildTiger.Action();

            Console.WriteLine("******************");

            // Making a pet dog through PetAnimalFactory
            IAnimalFactory petAnimalFactory = new PetAnimalFactory();
            IDog petDog = petAnimalFactory.GetDog();
            petDog.Speak();
            petDog.Action();

            // Making a pet tiger through PetAnimalFactory
            ITiger petTiger = petAnimalFactory.GetTiger();
            petTiger.Speak();
            petTiger.Action();

            Console.ReadLine();
        }
    }
}
```

### Output

```
***Abstract Factory Pattern Demo***

Wild Dog says: Bow-Wow.
Wild Dogs prefer to roam freely in jungles.

Wild Tiger says: Halum.
Wild Tigers prefer hunting in jungles.

******************
Pet Dog says: Bow-Wow.
Pet Dogs prefer to stay at home.

Pet Tiger says: Halum.
Pet Tigers play in an animal circus.
```

### Q&A Session

#### What are the challenges of using an abstract factory like this?

Any change in the abstract factory will force you to propagate the modification to the concrete factories. If you follow the design philosophy that says “Program to an interface, not to an implementation,” you need to prepare for this. This is one of the key principles that developers should always keep in mind. In most scenarios, developers do not want to change their abstract factories.

In addition, the overall architecture may look complex. Also, debugging becomes tricky in some scenarios.

## Proxy Pattern

### GoF Definition

Provide a surrogate or placeholder for another object to control access to it.

### Concept

A proxy is basically a substitute for an intended object. When a client deals with a proxy object, it thinks that it is dealing with the actual object. You need to support this kind of design because dealing with an original object is not always possible. This is because of many factors such as security issues, for example. So, in this pattern, you may want to use a class that can perform as an interface to something else.

### Computer World Example

An ATM implementation will hold proxy objects for bank information that exists on a remote server. In the real programming world, creating multiple instances of a complex object (a heavy object) is costly in general. So, whenever you can, you should create multiple proxy objects that can point to the original object. This mechanism can also help you to save the computer/system memory.

### Illustration

In this program, you are calling the `DoSomeWork()` method of the proxy object that, in turn, calls the `DoSomeWork()` method of an object of `ConcreteSubject`. When customers see the output, they think they have invoked the method from their intended object directly.

### Class Diagram

![Class Diagram Proxy Pattern](./Images/Class%20Diagram%20Proxy%20Pattern.png "Class Diagram Proxy Pattern")

### Implementation

```csharp
using System;

namespace ProxyPattern
{
    public abstract class Subject
    {
        public abstract void DoSomeWork();
    }

    public class ConcreteSubject : Subject
    {
        public override void DoSomeWork()
        {
            Console.WriteLine("ConcreteSubject.DoSomeWork()");
        }
    }

    public class Proxy : Subject
    {
        Subject cs;

        public override void DoSomeWork()
        {
            Console.WriteLine("Proxy call happening now...");

            // Lazy initialization:We'll not instantiate until the method is called
            if (cs == null) {
                cs = new ConcreteSubject();
            }

            cs.DoSomeWork();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Proxy Pattern Demo***\n");

            Proxy px = new Proxy();

            px.DoSomeWork();
            Console.ReadKey();
        }
    }
}
```

### Output

```
***Proxy Pattern Demo***

Proxy call happening now...
ConcreteSubject.DoSomeWork()
```

## Decorator Pattern

### GoF Definition

Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

### Concept

This pattern promotes the concept that your class should be closed for modification but open for extension. In other words, you can add a functionality without disturbing the existing functionalities. The concept is useful when you want to add some special functionality to a specific object instead of the whole class. This pattern prefers object composition over inheritance. Once you master this technique, you can add new responsibilities to an object without affecting the underlying classes.

### Computer World Example

Suppose in a GUI-based toolkit you want to add some border properties. You could do this with inheritance, but that cannot be treated as an ultimate solution because you do not have absolute control over everything from the beginning. So, this technique is static in nature.

Decorators offer a flexible approach. They promote the concept of dynamic choices. For example, you can surround the component in another object. The enclosing object is termed a decorator, and it must conform to the interface of the component that it decorates. It will forward the requests to the component, and it can perform additional operations before or after those requests. In fact, you can add an unlimited number of responsibilities with this concept.

### Illustration

In this example, I have not modified the core `MakeHouse()` method. I have created two additional decorators, `ConcreteDecoratorEx1` and `ConcreteDecoratorEx2`, to serve my needs, but I have kept the original structure intact.

### Class Diagram

![Class Diagram Decorator Pattern](./Images/Class%20Diagram%20Decorator%20Pattern.png "Class Diagram Decorator Pattern")

### Implementation

```csharp
using System;

namespace DecoratorPattern
{
    abstract class Component
    {
        public abstract void MakeHouse();
    }

    class ConcreteComponent : Component
    {
        public override void MakeHouse()
        {
            Console.WriteLine("Original House is complete. It is closed for modification.");
        }
    }

    abstract class AbstractDecorator : Component
    {
        protected Component com;

        public void SetTheComponent(Component c)
        {
            com = c;
        }

        public override void MakeHouse()
        {
            if (com != null) {
                com.MakeHouse();
            }
        }
    }

    class ConcreteDecoratorEx1 : AbstractDecorator
    {
        public  override void MakeHouse()
        {
            base.MakeHouse();

            Console.WriteLine("***Using a decorator***");

            AddFloor();
        }

        private void AddFloor()
        {
            Console.WriteLine("I am making an additional floor on top of it.");
        }
    }

    class ConcreteDecoratorEx2 : AbstractDecorator
    {
        public  override void MakeHouse()
        {
            Console.WriteLine("");

            base.MakeHouse();

            Console.WriteLine("***Using another decorator***");

            PaintTheHouse();
        }

        private void PaintTheHouse()
        {
            Console.WriteLine("Now I am painting the house.");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Decorator pattern Demo***\n");
            ConcreteComponent cc = new ConcreteComponent();

            // ConcreteDecoratorEx1 decorator1 = new ConcreteDecoratorEx1();
            AbstractDecorator decorator1 = new ConcreteDecoratorEx1();
            decorator1.SetTheComponent(cc);
            decorator1.MakeHouse();

            // ConcreteDecoratorEx2 decorator2 = new ConcreteDecoratorEx2();
            AbstractDecorator decorator2 = new ConcreteDecoratorEx2();
            // Adding results from decorator1
            decorator2.SetTheComponent(decorator1);
            decorator2.MakeHouse();

            Console.ReadKey();
        }
    }
}
```

### Output

```
***Decorator pattern Demo***

Original House is complete. It is closed for modification.
***Using a decorator***
I am making an additional floor on top of it.

Original House is complete. It is closed for modification.
***Using a decorator***
I am making an additional floor on top of it.
***Using another decorator***
Now I am painting the house.
```

### Q&A Session

#### Can you explain how composition is promoting dynamic behavior that inheritance cannot?

When a derived class inherits from a base class, it actually inherits the behavior of the base class at that time only. Though different subclasses can extend the base/parent class in different ways, this type of binding is known at compile time. So, the method is static. But by using the concept of composition, as in the previous example, you get dynamic behavior.

When you design a base/parent class, you may not have enough visibility about what kind of additional responsibilities your clients may want in some later phase. Since the constraint is that you cannot modify the existing code, in this case object composition not only outclasses inheritance, but it also ensures that you are not introducing bugs in the old architecture.

Lastly, in this context, you must try to remember one key design principle that says classes should be open for extension but closed for modification.

#### What are the key advantages of using a decorator?

- The existing structure is untouched, so you cannot introduce bugs there.

- New functionalities can be added to an existing object easily.

- You do not need to predict/implement all the supported functionalities at once (in other words, in the initial design phase). You can develop incrementally. For example, you can add decorator objects one by one to support your needs. You must acknowledge that if you make a complex class first and then want to extend the functionalities, that will be a tedious process.

## Adapter Pattern

### GoF Definition

Convert the interface of a class into another interface that clients expect. The Adapter pattern lets classes work together that could not otherwise because of incompatible interfaces.

### Computer World Example

Suppose you have an application that can be broadly classified into two parts: the user interface (UI or front end) and the database (back end). Through the user interface, clients can pass some specific type of data or objects. Your database is compatible with those objects and can store them smoothly. Over a period of time, you may feel that you need to upgrade your software to make your clients happy. So, you may want to allow some other type of object also to pass through the UI. But in this case, the first resistance will come from your database because it cannot store these new types of objects. In such a situation, you can use an adapter that will take care of the conversion of these new objects to a compatible form that your old database can accept.

A simple use of this pattern is described in the following example.

### Illustration

In this example, you can calculate the area of a rectangle easily. In the `Calculator` class, you need to supply a rectangle in the `GetArea()` method to get the area of the rectangle. Now suppose you want to calculate the area of a triangle. But your constraint is that you want to get the area of it through the `GetArea()` method of the `Calculator` class.

How can you achieve that?

To deal with this problem, you make a `CalculatorAdapter` for a triangle and pass a triangle in its `GetArea()` method. In turn, the method will treat these triangles like rectangles to get the areas of them from the `GetArea()` method of the `Calculator` class.

### Class Diagram

![Class diagram Adapter Pattern](./Images/Class%20Diagram%20Adapter%20Pattern.png "Class diagram Adapter Pattern")

### Implementation

```csharp
using System;
namespace AdapterPattern_Modified
{
    interface RectInterface
    {
        void AboutRectangle();
        double CalculateAreaOfRectangle();
    }

    class Rect : RectInterface
    {
        public double Length;
        public double Width;

        public Rect(double l, double w)
        {
            this.Length = l;
            this.Width = w;
        }

        public double CalculateAreaOfRectangle()
        {
            return Length * Width;
        }

        public void AboutRectangle()
        {
            Console.WriteLine("Actually, I am a Rectangle");
        }
    }

    interface TriInterface
    {
        void AboutTriangle();
        double CalculateAreaOfTriangle();
    }

    class Triangle : TriInterface
    {
        public double BaseLength;
        public double Height;

        public Triangle(double b, double h)
        {
            this.BaseLength = b;
            this.Height = h;
        }

        public double CalculateAreaOfTriangle()
        {
            return 0.5 * BaseLength * Height;
        }

        public void AboutTriangle()
        {
            Console.WriteLine("Actually, I am a Triangle");
        }
    }

    /*
        TriangleAdapter is implementing RectInterface.
        So, it needs to implement all the methods defined in the target interface.
    */
    class TriangleAdapter : RectInterface
    {
        Triangle triangle; // Adaptee

        public TriangleAdapter(Triangle t)
        {
            this.triangle = t;
        }

        public void AboutRectangle()
        {
            triangle.AboutTriangle();
        }

        public double CalculateAreaOfRectangle()
        {
            return triangle.CalculateAreaOfTriangle();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Adapter Pattern Modified Demo***\n");

            // CalculatorAdapter cal = new CalculatorAdapter();
            Rect r = new Rect(20, 10);

            Console.WriteLine("Area of Rectangle is :{0} Square unit", r.CalculateAreaOfRectangle());

            Triangle t = new Triangle(20, 10);

            Console.WriteLine("Area of Triangle is :{0} Square unit", t.CalculateAreaOfTriangle());

            RectInterface adapter = new TriangleAdapter(t);

            // Passing a Triangle instead of a Rectangle
            Console.WriteLine("Area of Triangle using the triangle adapter is :{0} Square unit", GetArea(adapter));
            Console.ReadKey();
        }

        /*
            GetArea(RectInterface r) method  does not know that through TriangleAdapter, it is getting a Triangle instead of a Rectangle.
        */
        static double GetArea(RectInterface r)
        {
            r.AboutRectangle();
            return r.CalculateAreaOfRectangle();
        }
    }
}
```

### Output

```
***Adapter Pattern Modified Demo***

Area of Rectangle is :200 Square unit
Area of Triangle is :100 Square unit
Actually, I am a Triangle
Area of Triangle using the triangle adapter is :100 Square unit
```

## Composite Pattern

### GoF Definition

Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

### Concept

This pattern is useful to represent part-whole hierarchies of objects. In object-oriented programming, a composite is an object with a composition of one or more similar objects, where each of these objects has similar functionality. (This is also known as a “has-a” relationship among objects.) So, the usage of this pattern is common in treestructured data. If you can apply the concept properly, you do not need to discriminate between a branch and the leaf nodes. In simple words, you can achieve these two key goals with this pattern:

- You can compose objects into a tree structure to represent a partwhole hierarchy.

- You can access both the composite objects (branches) and the individual objects (leaf nodes) uniformly. As a result, you can reduce the complexity of the code and make the application less prone to errors.

### Computer World Example

I already mentioned that any tree data structure can follow this concept. In that case, clients can treat the leaves of the tree and the nonleaves (or, branches of the tree) in the same way.

### Class Diagram

![Class Diagram Composite Pattern](./Images/Class%20Diagram%20Composite%20Pattern.png "Class Diagram Composite Pattern")

### Implementation

```csharp
using System;
using System.Collections.Generic;

namespace CompositePattern
{
    interface IEmployee
    {
        void PrintStructures();
    }

    class CompositeEmployee : IEmployee
    {
        private string name;
        private string dept;
        private List<IEmployee> controls;

        public CompositeEmployee(string name, string dept)
        {
            this.name = name;
            this.dept = dept;
            controls = new List<IEmployee>();
        }

        public void Add(IEmployee e)
        {
            controls.Add(e);
        }

        public void Remove(IEmployee e)
        {
            controls.Remove(e);
        }

        public void PrintStructures()
        {
            Console.WriteLine("\t" + this.name + " works in  " + this.dept);

            foreach (IEmployee e in controls)
            {
                e.PrintStructures();
            }
        }
    }

    class Employee : IEmployee
    {
        private string name;
        private string dept;

        public Employee(string name, string dept)
        {
            this.name = name;
            this.dept = dept;
        }

        public void PrintStructures()
        {
            Console.WriteLine("\t\t"+this.name + " works in  " + this.dept);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Composite Pattern Demo ***");
            // Prinipal of the college
            CompositeEmployee Principal = new CompositeEmployee("Dr.S.Som(Principal)","Planning-Supervising-Managing");

            // The college has 2 Head of Departments-One from MAths, One from Computer Sc.
            CompositeEmployee hodMaths = new CompositeEmployee("Mrs.S.Das(HOD-Maths)","Maths");
            CompositeEmployee hodCompSc = new CompositeEmployee("Mr. V.Sarcar(HOD-CSE)", "Computer Sc.");

            // 2 other teachers works in Mathematics department
            Employee mathTeacher1 = new Employee("Math Teacher-1","Maths");
            Employee mathTeacher2 = new Employee("Math Teacher-2","Maths");

            // 3 other teachers works in Computer Sc. department
            Employee cseTeacher1 = new Employee("CSE Teacher-1","Computer Sc.");
            Employee cseTeacher2 = new Employee("CSE Teacher-2", "Computer Sc.");
            Employee cseTeacher3 = new Employee("CSE Teacher-3", "Computer Sc.");

            // Teachers of Mathematics directly reports to HOD-Maths
            hodMaths.Add(mathTeacher1);
            hodMaths.Add(mathTeacher2);

             // Teachers of Computer Sc directly reports to HOD-Comp.Sc
            hodCompSc.Add(cseTeacher1);
            hodCompSc.Add(cseTeacher2);
            hodCompSc.Add(cseTeacher3);

            // Principal is on top of college
            // HOD -Maths and Comp. Sc directly reports to him
            Principal.Add(hodMaths);
            Principal.Add(hodCompSc);

            // Printing the leaf-nodes and branches in the same way.
            // i.e. in each case, we are calling PrintStructures() method
            Console.WriteLine("\n Testing the structure of a Principal object");
            // Prints the complete structure
            Principal.PrintStructures();

            Console.WriteLine("\n Testing the structure of a HOD object:");
            Console.WriteLine("Teachers working at Computer Science department:");
            // Prints the details of Computer Sc, department
            hodCompSc.PrintStructures();

            // Leaf node
            Console.WriteLine("\n Testing the structure of a leaf node:");
            mathTeacher1.PrintStructures();

            // Suppose, one computer teacher is leaving now from the organization.
            hodCompSc.Remove(cseTeacher2);
            Console.WriteLine("\n After CSE Teacher-2 resigned, the organization has following members:");
            Principal.PrintStructures();

            Console.ReadKey();
        }
    }
}
```

### Output

```
***Composite Pattern Demo ***

Testing the structure of a Principal object
    Dr.S.Som(Principal) works in  Planning-Supervising-Managing
    Mrs.S.Das(HOD-Maths) works in  Maths
        Math Teacher-1 works in  Maths
        Math Teacher-2 works in  Maths
    Mr. V.Sarcar(HOD-CSE) works in  Computer Sc.
        CSE Teacher-1 works in  Computer Sc.
        CSE Teacher-2 works in  Computer Sc.
        CSE Teacher-3 works in  Computer Sc.

Testing the structure of a HOD object:
Teachers working at Computer Science department:
    Mr. V.Sarcar(HOD-CSE) works in  Computer Sc.
        CSE Teacher-1 works in  Computer Sc.
        CSE Teacher-2 works in  Computer Sc.
        CSE Teacher-3 works in  Computer Sc.

Testing the structure of a leaf node:
        Math Teacher-1 works in  Maths

After CSE Teacher-2 resigned, the organization has following members:
    Dr.S.Som(Principal) works in  Planning-Supervising-Managing
    Mrs.S.Das(HOD-Maths) works in  Maths
        Math Teacher-1 works in  Maths
        Math Teacher-2 works in  Maths
    Mr. V.Sarcar(HOD-CSE) works in  Computer Sc.
        CSE Teacher-1 works in  Computer Sc.
        CSE Teacher-3 works in  Computer Sc.
```

### Q&A Session

#### What are the advantages of using the Composite design pattern?

- In a tree-like structure, you can treat both the composite objects (branches) and the individual objects (leaf nodes) uniformly. Notice that in this example, I have used a common method called PrintStructures to print both the composite object structure (the principal or department heads) and the single objects (the leaf nodes like Math Teacher-1.)

- It is common to implement a part-whole hierarchy using this design pattern.

- You can easily add a new component to the architecture or delete an existing component from the architecture.

#### What are the challenges associated with using the Composite design pattern?

- If you want to maintain the ordering of child nodes (for example, if the parse trees are represented as components), you may need to take special care.

- If you are dealing with immutable objects, you cannot simply delete them.

- You can easily add a new component, but maintenance can become difficult over a period of time. Sometimes you may want to deal with a composite that has special components. This kind of constraint may cause additional costs to the development because you may need to implement a dynamic checking mechanism to support the concept.

#### I want to use an abstract class instead of an interface. Is this allowed?

In most cases, the simple answer is yes. But you need to understand the difference between an abstract class and an interface. In a typical scenario, you will find one of them may be more useful than the other one. Since throughout the book I am presenting only simple and easy-to-understand examples, you may not see much difference between these two.

## Bridge Pattern

### GoF Definition

Decouple an abstraction from its implementation so that the two can vary independently.

### Concept

This pattern is also known as the Handle/Body pattern. With it, you decouple an implementation class from an abstract class by providing a bridge between them.

This bridge interface makes the functionality of concrete classes independent from the interface implementer classes. You can alter different kinds of classes structurally without affecting each other.

### Computer World Example

GUI frameworks can use the Bridge pattern to separate abstractions from the platformspecific implementation. For example, using this pattern, you can separate a window abstraction from a window implementation for Linux or macOS.

### Class Diagram

![Class Diagram Bridge Pattern](./Images/Class%20Diagram%20Bridge%20Pattern.png "Class Diagram Bridge Pattern")

### Implementation

```csharp
using System;

namespace BridgePattern
{
    // Implementor
    public interface IState
    {
        void MoveState();
    }

    // ConcreteImplementor-1
    public class OnState : IState
    {
        public void MoveState()
        {
            Console.Write("On State");
        }
    }

    // ConcreteImplementor-2
    public class OffState : IState
    {
        public void MoveState()
        {
            Console.Write("Off State");
        }
    }

    // Abstraction
    public abstract class ElectronicGoods
    {
        // Composition - implementor
        protected IState state;

        // Alternative approach to properties :
        // we can also pass an implementor (as input argument) inside a constructor.
        // public ElectronicGoods(IState state)
        // {
        //    this.state = state;
        // }

        public IState State
        {
            get { return state; }
            set { state = value; }
        }

        abstract public void MoveToCurrentState();
    }

    // Refined Abstraction
    public class Television : ElectronicGoods
    {
        // public Television(IState state) : base(state)
        // {
        // }

        /*
            Implementation specific:
            We are delegating the implementation to the Implementor object
        */
        public override void MoveToCurrentState()
        {
            Console.Write("\n Television is functioning at : ");
            state.MoveState();
        }
    }

    public class VCD : ElectronicGoods
    {
        // public VCD(IState state) : base(state)
        // {
        // }

        /*
            Implementation specific:
            We are delegating the implementation to the Implementor object
        */
        public override void MoveToCurrentState()
        {
            Console.Write("\n VCD is functioning at : ");
            state.MoveState();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Bridge Pattern Demo***");

            Console.WriteLine("\n Dealing with a Television:");

            // ElectronicGoods eItem = new Television(presentState);
            ElectronicGoods eItem = new Television();
            IState presentState = new OnState();
            eItem.State = presentState;
            eItem.MoveToCurrentState();

            // Verifying Off state of the Television now
            presentState = new OffState();

            // eItem = new Television(presentState);
            eItem.State = presentState;
            eItem.MoveToCurrentState();

            Console.WriteLine("\n \n Dealing with a VCD:");
            presentState = new OnState();

            // eItem = new VCD(presentState);
            eItem = new VCD();
            eItem.State = presentState;
            eItem.MoveToCurrentState();

            presentState = new OffState();
            // eItem = new VCD(presentState);
            eItem.State = presentState;
            eItem.MoveToCurrentState();
            Console.ReadLine();
        }
    }
}
```

### Output

```
***Bridge Pattern Demo***

Dealing with a Television:

Television is functioning at : On State
Television is functioning at : Off State

Dealing with a VCD:

VCD is functioning at : On State
VCD is functioning at : Off State
```

### Q&A Session

#### It looks like this pattern is similar to the State pattern. Is this understanding correct?

No. The State pattern is a behavioral pattern, and its intent is different. In this chapter, I showed an example where the electronics items can be in different states, but the key intent was to show the following:

- How you avoid the tight coupling between the items and their states

- How you maintain two different hierarchies where both of them can be extended without impacting each other

- How you deal with multiple objects where implementations are shared among themselves

#### You could use simple subclassing instead of using this kind of design. Is this understanding correct?

No. With simple subclassing, your implementations cannot vary dynamically. Your implementations may seem to behave differently, but actually they are bound to the abstraction at compile time.

#### In this example, there is a lot of dead code. Why are you keeping it?

Some developers prefer constructors over properties (or, getter-setter methods). You can see both variations in different implementations. I am keeping them for your ready reference.

#### What are the key advantages of using a Bridge design pattern?

- Implementations are not bound to the abstractions.

- Both the abstractions and implementations can grow independently.

- Concrete classes are independent from the interface implementer classes. In other words, changes in one of them do not affect the other. So, you can also vary the interface and the concrete implementations in different ways.

#### What are the challenges associated with this pattern?

- The overall structure may become complex.

- Sometimes the Bridge pattern is confused with the Adapter pattern. (Remember that the key purpose of an Adapter pattern is to deal with incompatible interfaces only.)

## Observer Pattern

### GoF Definition

Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

### Computer World Example

In the world of computer science, consider a simple UI-based example. This UI is connected to some database. A user can execute some query through that UI, and after searching the database, the result is returned in the UI. With this pattern, you segregate the UI from the database. If a change occurs in the database, the UI should be notified so that it can update its display according to the change.

To simplify this scenario, assume that you are the person responsible for maintaining a particular database in your organization. Whenever there is a change made to the database, you want to get a notification so that you can take action if necessary.

### Illustration

For this example, I have created three observers and one subject. The subject maintains a list of all of its registered users. The observers want to receive a notification when the flag value changes in the subject. In the output, you will discover that these observers are getting notifications when the flag values are changed to 5, 50, and 100, respectively. But one of them does not receive any notification when the flag value changes to 50 because at that moment he was not a registered user in the subject. But at the end, he starts getting notifications again because he registers again.

### Class Diagram

![Class Diagram Observer Pattern](./Images/Class%20Diagram%20Observer%20Pattern.png "Class Diagram Observer Pattern")

### Implementation

```csharp
using System;
using System.Collections.Generic;

namespace ObserverPattern
{
    interface IObserver
    {
        void Update(int i);
    }

    class ObserverType1 : IObserver
    {
        string nameOfObserver;

        public ObserverType1(String name)
        {
            this.nameOfObserver = name;
        }

        public void Update(int i)
        {
            Console.WriteLine(" {0} has received an alert: Someone has updated myValue in Subject to: {1}", nameOfObserver, i);
        }
    }

    class ObserverType2 : IObserver
    {
        string nameOfObserver;

        public ObserverType2(String name)
        {
            this.nameOfObserver = name;
        }

        public void Update(int i)
        {
            Console.WriteLine(" {0} notified: myValue in Subject at present: {1}", nameOfObserver, i);
        }
    }

    interface ISubject
    {
        void Register(IObserver o);
        void Unregister(IObserver o);
        void NotifyRegisteredUsers(int i);
    }

    class Subject : ISubject
    {
        List<IObserver> observerList = new List<IObserver>();
        private int flag;

        public int Flag
        {
            get
            {
                return flag;
            }
            set
            {
                flag = value;
                // Flag value changed. So notify observer/s.
                NotifyRegisteredUsers(flag);
            }
        }

        public void Register(IObserver anObserver)
        {
            observerList.Add(anObserver);
        }

        public void Unregister(IObserver anObserver)
        {
            observerList.Remove(anObserver);
        }

        public void NotifyRegisteredUsers(int i)
        {
            foreach (IObserver observer in observerList)
            {
                observer.Update(i);
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(" ***Observer Pattern Demo***\n");

            // We have 3 observers- 2 of them are ObserverType1, 1 of them is of ObserverType2
            IObserver myObserver1 = new ObserverType1("Roy");
            IObserver myObserver2 = new ObserverType1("Kevin");
            IObserver myObserver3 = new ObserverType2("Bose");
            Subject subject = new Subject();

            // Registering the observers-Roy,Kevin,Bose
            subject.Register(myObserver1);
            subject.Register(myObserver2);
            subject.Register(myObserver3);
            Console.WriteLine(" Setting Flag = 5 ");
            subject.Flag = 5;

            // Unregistering an observer(Roy))
            subject.Unregister(myObserver1);

            // No notification this time Roy.Since it is unregistered.
            Console.WriteLine("\n Setting Flag = 50 ");
            subject.Flag = 50;

            // Roy is registering himself again
            subject.Register(myObserver1);
            Console.WriteLine("\n Setting Flag = 100 ");
            subject.Flag = 100;
            Console.ReadKey();
        }
    }
}
```

### Output

```
 ***Observer Pattern Demo***

Setting Flag = 5
Roy has received an alert: Someone has updated myValue in Subject to: 5
Kevin has received an alert: Someone has updated myValue in Subject to: 5
Bose notified: myValue in Subject at present: 5

Setting Flag = 50
Kevin has received an alert: Someone has updated myValue in Subject to: 50
Bose notified: myValue in Subject at present: 50

Setting Flag = 100
Kevin has received an alert: Someone has updated myValue in Subject to: 100
Bose notified: myValue in Subject at present: 100
Roy has received an alert: Someone has updated myValue in Subject to: 100
```

### Q&A Session

#### If I have only one observer, then I may not need to set up the interface. Is this understanding correct?

Yes. But if you want to follow the pure object-oriented programming guidelines, you may always prefer interfaces (or abstract classes) instead of using a concrete class. Aside from this point, usually you will have multiple observers, and you will want to keep the methods they implement consistent. That’s where you will get benefit from this kind of design.

#### Can you have different observers that may vary?

Yes. You should not think that for each observer you need to create a different class. Also, think about this in a real-world scenario. When anyone is making a crucial change in the organization’s database, multiple groups of people from different departments may want to know about the change (such as your boss and the owner of the database, who work at different levels) and act accordingly. So, if you create separate classes for each of them, it will be hard to maintain and at the same time, it will be difficult to identify the source of the changes at a given point of time.
