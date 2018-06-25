# Writing a Class Versus an Interface

**As a guideline**:

-   Use classes and subclasses for types that naturally share an implementation.
-   Use interfaces for types that have independent implementations.

Consider the following classes:

```csharp
abstract class Animal {}
abstract class Bird : Animal {}
abstract class Insect : Animal {}
abstract class FlyingCreature : Animal {}
abstract class Carnivore : Animal {}

// Concrete classes:

class Ostrich : Bird {}
class Eagle : Bird, FlyingCreature, Carnivore {}    // Illegal
class Bee : Insect, FlyingCreature {}               // Illegal
class Flea : Insect, Carnivore {}                   // Illegal
```

The `Eagle`, `Bee`, and `Flea` classes do not compile because inheriting from multiple classes is prohibited. To resolve this, we must convert some of the types to interfaces. The question then arises, which types? Following our general rule, we could say that insects share an implementation, and birds share an implementation, so they remain classes. In contrast, flying creatures have independent mechanisms for flying, and carnivores have independent strategies for eating animals, so we would convert `FlyingCreature` and `Carnivore` to interfaces:

```csharp
interface IFlyingCreature {}
interface ICarnivore {}
```

In a typical scenario, Bird and Insect might correspond to a Windows control and a web control; `FlyingCreature` and `Carnivore` might correspond to `IPrintable` and `IUndoable`.
