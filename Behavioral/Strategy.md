# Strategy Pattern

## Definition
The **Strategy Pattern** is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

## Real-World Analogy
**Navigator App**: When planning a route to the airport, the map app lets you choose the mode of transport:
-   **Car**: Fastest route avoiding traffic.
-   **Walking**: Shortest route using pedestrian paths.
-   **Public Transport**: Route using trains/buses.
These are different "strategies" for the same goal (getting to the airport). The app (Context) switches the routing algorithm (Strategy) based on user choice.

## Structure
-   **Context**: Maintains a reference to one of the concrete strategies and communicates with this object only via the strategy interface.
-   **Strategy**: Interace common to all supported algorithms.
-   **Concrete Strategies**: Implement different variations of an algorithm.

## Code Example (C#)

```csharp
// Strategy Interface
public interface IStrategy
{
    object DoAlgorithm(object data);
}

// Context
public class Context
{
    private IStrategy _strategy;

    public Context(IStrategy strategy)
    {
        _strategy = strategy;
    }

    public void SetStrategy(IStrategy strategy)
    {
        _strategy = strategy;
    }

    public void DoSomeBusinessLogic()
    {
        Console.WriteLine("Context: Sorting data using the strategy (not sure how it'll do it)");
        var result = _strategy.DoAlgorithm(new List<string> { "a", "e", "c", "b", "d" });
        string listStr = string.Join(",", result as List<string>);
        Console.WriteLine(listStr);
    }
}

// Concrete Strategy A (Alphabetical Sort)
public class ConcreteStrategyA : IStrategy
{
    public object DoAlgorithm(object data)
    {
        var list = data as List<string>;
        list.Sort();
        return list;
    }
}

// Concrete Strategy B (Reverse Sort)
public class ConcreteStrategyB : IStrategy
{
    public object DoAlgorithm(object data)
    {
        var list = data as List<string>;
        list.Sort();
        list.Reverse();
        return list;
    }
}
```

## Pros & Cons
### Pros
-   **Interchangeability**: You can swap algorithms used inside an object at runtime.
-   **Isolation**: You can isolate the implementation details of an algorithm from the code that uses it.
-   **Composition over Inheritance**: A clean alternative to subclassing.

### Cons
-   **Client Awareness**: Clients must be aware of the differences between strategies to select the right one.

---

### Java Example

```java
interface Strategy {
    int doOperation(int num1, int num2);
}

class OperationAdd implements Strategy {
    public int doOperation(int num1, int num2) { return num1 + num2; }
}

class Context {
    private Strategy strategy;
    public Context(Strategy strategy) { this.strategy = strategy; }
    public int executeStrategy(int num1, int num2) {
        return strategy.doOperation(num1, num2);
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <memory>

class Strategy {
public:
    virtual int execute(int a, int b) = 0;
    virtual ~Strategy() {}
};

class ConcreteStrategyAdd : public Strategy {
public:
    int execute(int a, int b) override { return a + b; }
};

class Context {
private:
    std::unique_ptr<Strategy> strategy;
public:
    Context(std::unique_ptr<Strategy> &&strategy) : strategy(std::move(strategy)) {}
    void set_strategy(std::unique_ptr<Strategy> &&strategy) {
        this->strategy = std::move(strategy);
    }
    int executeStrategy(int a, int b) {
        return strategy->execute(a, b);
    }
};
```
