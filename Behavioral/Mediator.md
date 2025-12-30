# Mediator Pattern

## Definition
The **Mediator Pattern** is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

## Real-World Analogy
**Air Traffic Control**: Pilots don't talk to each other directly to decide who lands next. "Flight 101 talking to Flight 202, I'm landing now." No. They talk to the Tower (Mediator). The Tower controls the flow.

## Structure
-   **Mediator**: Declares an interface for communication with components.
-   **Concrete Mediator**: Encapsulates relations between various components.
-   **Components**: Various classes that contain business logic. Each component has a reference to a mediator declared.

## Code Example (C#)

```csharp
// Mediator Interface
public interface IMediator
{
    void Notify(object sender, string ev);
}

// Concrete Mediator
public class ConcreteMediator : IMediator
{
    private Component1 _component1;
    private Component2 _component2;

    public ConcreteMediator(Component1 component1, Component2 component2)
    {
        _component1 = component1;
        _component1.SetMediator(this);
        _component2 = component2;
        _component2.SetMediator(this);
    }

    public void Notify(object sender, string ev)
    {
        if (ev == "A")
        {
            Console.WriteLine("Mediator reacts on A and triggers following operations:");
            _component2.DoC();
        }
        if (ev == "D")
        {
            Console.WriteLine("Mediator reacts on D and triggers following operations:");
            _component1.DoB();
            _component2.DoC();
        }
    }
}

// Base Component
public class BaseComponent
{
    protected IMediator _mediator;

    public BaseComponent(IMediator mediator = null)
    {
        _mediator = mediator;
    }

    public void SetMediator(IMediator mediator)
    {
        _mediator = mediator;
    }
}

// Concrete Components
public class Component1 : BaseComponent
{
    public void DoA()
    {
        Console.WriteLine("Component 1 does A.");
        _mediator.Notify(this, "A");
    }

    public void DoB()
    {
        Console.WriteLine("Component 1 does B.");
        _mediator.Notify(this, "B");
    }
}

public class Component2 : BaseComponent
{
    public void DoC()
    {
        Console.WriteLine("Component 2 does C.");
        _mediator.Notify(this, "C");
    }

    public void DoD()
    {
        Console.WriteLine("Component 2 does D.");
        _mediator.Notify(this, "D");
    }
}
```

## Pros & Cons
### Pros
-   **Single Responsibility Principle**: You can extract the communications between various components into a single place, making it easier to comprehend and maintain.
-   **Open/Closed Principle**: You can introduce new mediators without having to change the actual components.
-   **Decoupling**: Reduces coupling between various components of a program.

### Cons
-   **God Object**: The mediator can evolve into a God Object that knows too much about the system.

---

### Java Example

```java
class ChatRoom {
    public static void showMessage(User user, String message) {
        System.out.println(user.getName() + ": " + message);
    }
}

class User {
    private String name;
    public User(String name) { this.name = name; }
    public String getName() { return name; }
    public void sendMessage(String message) {
        ChatRoom.showMessage(this, message);
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

class Colleague;

class Mediator {
public:
    virtual void sendMessage(std::string msg, Colleague* user) = 0;
    virtual ~Mediator() {}
};

class Colleague {
protected:
    Mediator* mediator;
public:
    Colleague(Mediator* m) : mediator(m) {}
    virtual void send(std::string msg) { mediator->sendMessage(msg, this); }
    virtual void receive(std::string msg) = 0;
};

class ConcreteMediator : public Mediator {
    Colleague* c1;
    Colleague* c2;
public:
    void setC1(Colleague* c) { c1 = c; }
    void setC2(Colleague* c) { c2 = c; }
    void sendMessage(std::string msg, Colleague* user) override {
        if (user == c1) c2->receive(msg);
        else c1->receive(msg);
    }
};
```
