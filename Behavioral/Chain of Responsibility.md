# Chain of Responsibility Pattern

## Definition
The **Chain of Responsibility Pattern** is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

## Real-World Analogy
**Tech Support**: YOU -> Level 1 Support (Basic scripts) -> Level 2 Support (Technical troubleshooting) -> Level 3 Support (Engineers). If Level 1 can't fix it, they pass it to Level 2, and so on.

## Structure
-   **Handler**: Declares the interface, common for all concrete handlers. It usually contains a method for setting the next handler.
-   **Concrete Handler**: Contains the actual code for processing requests. If it can't handle the request, it passes it to the next handler.

## Code Example (C#)

```csharp
// Handler Interface
public interface IHandler
{
    IHandler SetNext(IHandler handler);
    object Handle(object request);
}

// Base Handler
public abstract class AbstractHandler : IHandler
{
    private IHandler _nextHandler;

    public IHandler SetNext(IHandler handler)
    {
        _nextHandler = handler;
        return handler;
    }

    public virtual object Handle(object request)
    {
        if (_nextHandler != null)
        {
            return _nextHandler.Handle(request);
        }
        else
        {
            return null;
        }
    }
}

// Concrete Handlers
public class MonkeyHandler : AbstractHandler
{
    public override object Handle(object request)
    {
        if ((request as string) == "Banana")
        {
            return $"Monkey: I'll eat the {request}.\n";
        }
        else
        {
            return base.Handle(request);
        }
    }
}

public class SquirrelHandler : AbstractHandler
{
    public override object Handle(object request)
    {
        if ((request as string) == "Nut")
        {
            return $"Squirrel: I'll eat the {request}.\n";
        }
        else
        {
            return base.Handle(request);
        }
    }
}
```

## Pros & Cons
### Pros
-   **Decoupling**: You can control the order of request handling dynamically.
-   **Single Responsibility Principle**: You can decouple classes that invoke operations from classes that perform operations.

### Cons
-   **Uncertainty**: The request might end up unhandled if issues occur at the end of the chain.

---

### Java Example

```java
abstract class Logger {
    protected Logger nextLogger;
    public void setNext(Logger next) { this.nextLogger = next; }
    public void log(String message, int level) {
        if (this.level <= level) write(message);
        if (nextLogger != null) nextLogger.log(message, level);
    }
    protected int level;
    abstract protected void write(String message);
}

class ConsoleLogger extends Logger {
    public ConsoleLogger(int level) { this.level = level; }
    protected void write(String message) { System.out.println("Console: " + message); }
}

class FileLogger extends Logger {
    public FileLogger(int level) { this.level = level; }
    protected void write(String message) { System.out.println("File: " + message); }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

class Handler {
protected:
    Handler* next = nullptr;
public:
    virtual ~Handler() { delete next; }
    void setNext(Handler* n) { next = n; }
    virtual void handleRequest(std::string request) {
        if (next) next->handleRequest(request);
    }
};

class ConcreteHandlerA : public Handler {
public:
    void handleRequest(std::string request) override {
        if (request == "A") std::cout << "Handled by A" << std::endl;
        else Handler::handleRequest(request);
    }
};

class ConcreteHandlerB : public Handler {
public:
    void handleRequest(std::string request) override {
        if (request == "B") std::cout << "Handled by B" << std::endl;
        else Handler::handleRequest(request);
    }
};
```
