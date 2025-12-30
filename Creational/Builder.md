# Builder Pattern

## Definition
The **Builder Pattern** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

## Real-World Analogy
**Building a House**: To build a simple house, you need four walls and a floor, inserting a door and a pair of windows, and building a roof. But what if you want a bigger, brighter house, with a backyard and other goodies (like a heating system, plumbing, and electrical wiring)? The simplest solution is to extend the base House class and create a set of subclasses to cover all combinations of the parameters. But eventually, you'll end up with a considerable number of subclasses. The Builder pattern organizes object construction into a set of steps (`buildWalls`, `buildDoor`, etc.). To create an object, you execute a series of these steps on a builder object.

## Structure
-   **Builder**: The interface that declares product construction steps.
-   **Concrete Builder**: Provides different implementations of the construction steps.
-   **Product**: The complex object that is being built.
-   **Director** (Optional): The class that constructs an object using the Builder interface. It knows the *order* of building steps.

## Code Example (C#)

```csharp
// Product
public class House
{
    public int Windows { get; set; }
    public int Doors { get; set; }
    public bool HasGarage { get; set; }
    public bool HasSwimmingPool { get; set; }

    public override string ToString() => $"House with {Windows} windows, {Doors} doors. Garage: {HasGarage}. Pool: {HasSwimmingPool}.";
}

// Builder Interface
public interface IHouseBuilder
{
    void Reset();
    void BuildWindows(int number);
    void BuildDoors(int number);
    void BuildGarage();
    void BuildSwimmingPool();
    House GetProduct();
}

// Concrete Builder
public class LuxuryHouseBuilder : IHouseBuilder
{
    private House _house = new House();

    public void Reset() => _house = new House();
    public void BuildWindows(int number) => _house.Windows = number;
    public void BuildDoors(int number) => _house.Doors = number;
    public void BuildGarage() => _house.HasGarage = true;
    public void BuildSwimmingPool() => _house.HasSwimmingPool = true;
    public House GetProduct() { var result = _house; Reset(); return result; }
}

// Director (Optional)
public class CivilEngineer
{
    private IHouseBuilder _builder;

    public CivilEngineer(IHouseBuilder builder)
    {
        _builder = builder;
    }

    public void ConstructLuxuryHouse()
    {
        _builder.Reset();
        _builder.BuildWindows(10);
        _builder.BuildDoors(4);
        _builder.BuildGarage();
        _builder.BuildSwimmingPool();
    }
}
```

## Pros & Cons
### Pros
-   **Control**: You can construct objects step-by-step, defer construction steps or run steps recursively.
-   **Reusability**: You can reuse the same construction code when building various representations of products.
-   **Isolation**: Isolate complex construction code from the business logic of the product.

### Cons
-   **Complexity**: Requires creating multiple new classes (Builder interface, Concrete Builders, Director), leading to increased code size.

---

### Java Example

```java
// Product
class House {
    int windows;
    int doors;
}

// Builder
interface HouseBuilder {
    void buildWindows(int n);
    void buildDoors(int n);
    House getResult();
}

class ConcreteBuilder implements HouseBuilder {
    private House house = new House();
    public void buildWindows(int n) { house.windows = n; }
    public void buildDoors(int n) { house.doors = n; }
    public House getResult() { return house; }
}

// Director
class Director {
    public void constructSimpleHouse(HouseBuilder builder) {
        builder.buildWindows(2);
        builder.buildDoors(1);
    }
}
```

### C++ Example

```cpp
#include <iostream>

// Product
class House {
public:
    int windows;
    int doors;
};

// Builder
class HouseBuilder {
public:
    virtual void buildWindows(int n) = 0;
    virtual void buildDoors(int n) = 0;
    virtual House* getResult() = 0;
    virtual ~HouseBuilder() {}
};

class ConcreteBuilder : public HouseBuilder {
private:
    House* house;
public:
    ConcreteBuilder() { this->reset(); }
    ~ConcreteBuilder() { delete house; }
    
    void reset() { house = new House(); }
    void buildWindows(int n) override { house->windows = n; }
    void buildDoors(int n) override { house->doors = n; }
    
    House* getResult() override {
        House* result = house;
        this->reset();
        return result;
    }
};
```
