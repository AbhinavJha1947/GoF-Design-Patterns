# Abstract Factory Pattern

## Definition
The **Abstract Factory Pattern** provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Real-World Analogy
**Furniture Shop**: Imagine a furniture shop simulator. You have families of products: `Chair` + `Sofa` + `CoffeeTable`. You also have variants of these families: `Modern`, `Victorian`, `ArtDeco`. You need a way to create individual furniture objects so that they match other objects of the same family. A `ModernFactory` would create a `ModernChair` and a `ModernSofa`.

## Structure
-   **Abstract Factory**: Declares an interface for operations that create abstract product objects.
-   **Concrete Factory**: Implements the operations to create concrete product objects.
-   **Abstract Product**: Declares an interface for a type of product object.
-   **Concrete Product**: Defines a product object to be created by the corresponding concrete factory.

## Code Example (C#)

```csharp
// Abstract Products
public interface IChair { void SitOn(); }
public interface ISofa { void LieOn(); }

// Concrete Products (Modern Family)
public class ModernChair : IChair { public void SitOn() => Console.WriteLine("Sitting on a modern chair."); }
public class ModernSofa : ISofa { public void LieOn() => Console.WriteLine("Lying on a modern sofa."); }

// Concrete Products (Victorian Family)
public class VictorianChair : IChair { public void SitOn() => Console.WriteLine("Sitting on a victorian chair."); }
public class VictorianSofa : ISofa { public void LieOn() => Console.WriteLine("Lying on a victorian sofa."); }

// Abstract Factory
public interface IFurnitureFactory
{
    IChair CreateChair();
    ISofa CreateSofa();
}

// Concrete Factories
public class ModernFurnitureFactory : IFurnitureFactory
{
    public IChair CreateChair() => new ModernChair();
    public ISofa CreateSofa() => new ModernSofa();
}

public class VictorianFurnitureFactory : IFurnitureFactory
{
    public IChair CreateChair() => new VictorianChair();
    public ISofa CreateSofa() => new VictorianSofa();
}

// Client
public class Client
{
    private readonly IChair _chair;
    private readonly ISofa _sofa;

    public Client(IFurnitureFactory factory)
    {
        _chair = factory.CreateChair();
        _sofa = factory.CreateSofa();
    }

    public void Paint()
    {
        _chair.SitOn();
        _sofa.LieOn();
    }
}
```

## Pros & Cons
### Pros
-   **Compatibility**: You can be sure that the products you're getting from a factory are compatible with each other.
-   **Decoupling**: You avoid tight coupling between concrete products and client code.

### Cons
-   **Complexity**: The code becomes more complicated than it needs to be, introducing a lot of new interfaces and classes.
-   **Extensibility**: Adding a new kind of product (e.g., "Table") requires changing the Abstract Factory interface and all of its subclasses.

---

### Java Example

```java
// Abstract Products
interface Chair { void sitOn(); }
interface Sofa { void lieOn(); }

// Concrete Products
class ModernChair implements Chair { public void sitOn() { System.out.println("Modern Chair"); } }
class ModernSofa implements Sofa { public void lieOn() { System.out.println("Modern Sofa"); } }

// Abstract Factory
interface FurnitureFactory {
    Chair createChair();
    Sofa createSofa();
}

// Concrete Factory
class ModernFurnitureFactory implements FurnitureFactory {
    public Chair createChair() { return new ModernChair(); }
    public Sofa createSofa() { return new ModernSofa(); }
}
```

### C++ Example

```cpp
#include <iostream>
#include <memory>

// Abstract Products
class Chair {
public:
    virtual void sitOn() = 0;
    virtual ~Chair() {}
};

class Sofa {
public:
    virtual void lieOn() = 0;
    virtual ~Sofa() {}
};

// Concrete Products
class ModernChair : public Chair {
public:
    void sitOn() override { std::cout << "Modern Chair" << std::endl; }
};

class ModernSofa : public Sofa {
public:
    void lieOn() override { std::cout << "Modern Sofa" << std::endl; }
};

// Abstract Factory
class FurnitureFactory {
public:
    virtual std::unique_ptr<Chair> createChair() = 0;
    virtual std::unique_ptr<Sofa> createSofa() = 0;
    virtual ~FurnitureFactory() {}
};

// Concrete Factory
class ModernFurnitureFactory : public FurnitureFactory {
public:
    std::unique_ptr<Chair> createChair() override {
        return std::make_unique<ModernChair>();
    }
    std::unique_ptr<Sofa> createSofa() override {
        return std::make_unique<ModernSofa>();
    }
};
```
