# Factory Method Pattern

## Definition
The **Factory Method Pattern** defines an interface for creating an object, but lets subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

## Real-World Analogy
**Logistics Management**: Imagine a logistics company. Initially, it only handles trucks. All your code lives inside the `Truck` class. Later, you add sea logistics. Now you need to add `Ship` logic. If you just add `if-else` statements, your code becomes messy. Instead, you create a `Logistics` interface with a `createTransport()` method. `RoadLogistics` returns a `Truck`, and `SeaLogistics` returns a `Ship`.

## Structure
-   **Product**: Declares the interface, which is common to all objects that can be created by the creator and its subclasses.
-   **Concrete Product**: Different implementations of the product interface.
-   **Creator**: Desclares the factory method, which returns new product objects.
-   **Concrete Creator**: Overrides the factory method to return a different type of product.

## Code Example (C#)

```csharp
// Product
public interface ITransport
{
    void Deliver();
}

// Concrete Products
public class Truck : ITransport
{
    public void Deliver() => Console.WriteLine("Delivering by land in a box.");
}

public class Ship : ITransport
{
    public void Deliver() => Console.WriteLine("Delivering by sea in a container.");
}

// Creator
public abstract class Logistics
{
    // The Factory Method
    public abstract ITransport CreateTransport();

    public void PlanDelivery()
    {
        var transport = CreateTransport();
        transport.Deliver();
    }
}

// Concrete Creators
public class RoadLogistics : Logistics
{
    public override ITransport CreateTransport() => new Truck();
}

public class SeaLogistics : Logistics
{
    public override ITransport CreateTransport() => new Ship();
}
```

## Pros & Cons
### Pros
-   **Decoupling**: Avoids tight coupling between the creator and the concrete products.
-   **Single Responsibility Principle**: You can move product creation code into one place in the program.
-   **Open/Closed Principle**: You can introduce new types of products into the program without breaking existing client code.

### Cons
-   **Complexity**: The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern (one creator for each product).

---

### Java Example

```java
// Product
interface Transport {
    void deliver();
}

class Truck implements Transport {
    public void deliver() { System.out.println("Deliver by land"); }
}

class Ship implements Transport {
    public void deliver() { System.out.println("Deliver by sea"); }
}

// Creator
abstract class Logistics {
    public abstract Transport createTransport();

    public void planDelivery() {
        Transport t = createTransport();
        t.deliver();
    }
}

class RoadLogistics extends Logistics {
    public Transport createTransport() { return new Truck(); }
}
```

### C++ Example

```cpp
#include <iostream>
#include <memory>

// Product
class Transport {
public:
    virtual void deliver() = 0;
    virtual ~Transport() {}
};

class Truck : public Transport {
public:
    void deliver() override { std::cout << "Deliver by land" << std::endl; }
};

class Ship : public Transport {
public:
    void deliver() override { std::cout << "Deliver by sea" << std::endl; }
};

// Creator
class Logistics {
public:
    virtual std::unique_ptr<Transport> createTransport() = 0;
    virtual ~Logistics() {}

    void planDelivery() {
        auto t = createTransport();
        t->deliver();
    }
};

class RoadLogistics : public Logistics {
public:
    std::unique_ptr<Transport> createTransport() override {
        return std::make_unique<Truck>();
    }
};
```
