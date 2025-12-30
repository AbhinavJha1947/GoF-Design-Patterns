# Decorator Pattern

## Definition
The **Decorator Pattern** is a structural design pattern that lets you attach new behaviors to objects by placing them inside special wrapper objects that contain the behaviors.

## Real-World Analogy
**Clothing**: When you are cold, you wrap yourself in a sweater. If you are still cold, you can wear a jacket on top. If it's raining, you might put on a raincoat. All of these "garments" extend your basic behavior (keeping warm/dry) but you are still you underneath.

## Structure
-   **Component**: The common interface for wrappers and the wrapped object.
-   **Concrete Component**: The class of objects being wrapped.
-   **Base Decorator**: A class that has a field for referencing a wrapped object and delegates operations to it.
-   **Concrete Decorators**: Define extra behaviors that can be added to components dynamically.

## Code Example (C#)

```csharp
// Component
public interface INotifier
{
    void Send(string message);
}

// Concrete Component
public class EmailNotifier : INotifier
{
    public void Send(string message) => Console.WriteLine($"Sending Email: {message}");
}

// Base Decorator
public abstract class NotifierDecorator : INotifier
{
    protected INotifier _wrappe;

    public NotifierDecorator(INotifier notifier)
    {
        _wrappe = notifier;
    }

    public virtual void Send(string message)
    {
        _wrappe.Send(message);
    }
}

// Concrete Decorator
public class SMSDecorator : NotifierDecorator
{
    public SMSDecorator(INotifier notifier) : base(notifier) { }

    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"Sending SMS: {message}");
    }
}

// Concrete Decorator
public class SlackDecorator : NotifierDecorator
{
    public SlackDecorator(INotifier notifier) : base(notifier) { }

    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"Sending Slack: {message}");
    }
}
```

## Pros & Cons
### Pros
-   **Flexibility**: You can extend an object's behavior without making a new subclass.
-   **Runtime Modification**: You can add or remove responsibilities from an object at runtime.
-   **Composition**: Enables "Recursive Composition" (wrapping a wrapper).

### Cons
-   **Complexity**: Can result in many small classes in the design, and configuring the layers of decorators can be ugly.

---

### Java Example

```java
// Component
interface Notifier {
    void send(String message);
}

class EmailNotifier implements Notifier {
    public void send(String message) { System.out.println("Email: " + message); }
}

// Base Decorator
abstract class NotifierDecorator implements Notifier {
    protected Notifier wrapper;

    public NotifierDecorator(Notifier notifier) {
        this.wrapper = notifier;
    }

    public void send(String message) {
        wrapper.send(message);
    }
}

// Concrete Decorator
class SMSDecorator extends NotifierDecorator {
    public SMSDecorator(Notifier notifier) { super(notifier); }

    public void send(String message) {
        super.send(message);
        System.out.println("SMS: " + message);
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

// Component
class Notifier {
public:
    virtual void send(std::string message) = 0;
    virtual ~Notifier() {}
};

class EmailNotifier : public Notifier {
public:
    void send(std::string message) override { std::cout << "Email: " << message << std::endl; }
};

// Base Decorator
class NotifierDecorator : public Notifier {
protected:
    Notifier* wrapper;
public:
    NotifierDecorator(Notifier* n) : wrapper(n) {}
    void send(std::string message) override {
        wrapper->send(message);
    }
};

// Concrete Decorator
class SMSDecorator : public NotifierDecorator {
public:
    SMSDecorator(Notifier* n) : NotifierDecorator(n) {}
    void send(std::string message) override {
        NotifierDecorator::send(message);
        std::cout << "SMS: " << message << std::endl;
    }
};
```
