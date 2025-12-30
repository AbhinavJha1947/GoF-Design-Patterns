# Command Pattern

## Definition
The **Command Pattern** is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you parameterize methods with different requests, delay or queue a request's execution, and support undoable operations.

## Real-World Analogy
**Waiter and Chef**: You (Client) give an Order (Command) to a Waiter (Invoker). The Waiter takes the order to the kitchen and sticks it on the wall. The Chef (Receiver) picks it up and cooks the meal. You don't talk to the chef directly. The order slip contains all the instructions needed.

## Structure
-   **Command**: Declares an interface for executing an operation.
-   **Concrete Command**: Implements various kinds of requests.
-   **Receiver**: The object that performs the actual work.
-   **Invoker**: The object that triggers the command.

## Code Example (C#)

```csharp
// Command Interface
public interface ICommand
{
    void Execute();
}

// Concrete Command
public class SimpleCommand : ICommand
{
    private string _payload;

    public SimpleCommand(string payload)
    {
        _payload = payload;
    }

    public void Execute()
    {
        Console.WriteLine($"SimpleCommand: Printing ({_payload})");
    }
}

// Complex Command
public class ComplexCommand : ICommand
{
    private Receiver _receiver;
    private string _a;
    private string _b;

    public ComplexCommand(Receiver receiver, string a, string b)
    {
        _receiver = receiver;
        _a = a;
        _b = b;
    }

    public void Execute()
    {
        Console.WriteLine("ComplexCommand: Complex stuff should be done by a receiver object.");
        _receiver.DoSomething(_a);
        _receiver.DoSomethingElse(_b);
    }
}

// Receiver
public class Receiver
{
    public void DoSomething(string a) => Console.WriteLine($"Receiver: Working on ({a}.)");
    public void DoSomethingElse(string b) => Console.WriteLine($"Receiver: Also working on ({b}.)");
}

// Invoker
public class Invoker
{
    private ICommand _onStart;
    private ICommand _onFinish;

    public void SetOnStart(ICommand command) => _onStart = command;
    public void SetOnFinish(ICommand command) => _onFinish = command;

    public void DoSomethingImportant()
    {
        Console.WriteLine("Invoker: Does anybody want something done before I begin?");
        _onStart?.Execute();
        
        Console.WriteLine("Invoker: ...doing something really important...");
        
        Console.WriteLine("Invoker: Does anybody want something done after I finish?");
        _onFinish?.Execute();
    }
}
```

## Pros & Cons
### Pros
-   **Single Responsibility Principle**: You can decouple classes that invoke operations from classes that perform these operations.
-   **Open/Closed Principle**: You can introduce new commands into the app without breaking existing client code.
-   **Undo/Redo**: You can implement undo/redo functionality easily.

### Cons
-   **Complexity**: Adds a layer of indirection and many small classes.

---

### Java Example

```java
// Command
interface Command {
    void execute();
}

// Receiver
class Light {
    public void turnOn() { System.out.println("Light On"); }
}

// Concrete Command
class TurnOnCommand implements Command {
    private Light light;
    public TurnOnCommand(Light light) { this.light = light; }
    public void execute() { light.turnOn(); }
}

// Invoker
class Button {
    private Command command;
    public Button(Command command) { this.command = command; }
    public void press() { command.execute(); }
}
```

### C++ Example

```cpp
#include <iostream>

// Receiver
class Light {
public:
    void turnOn() { std::cout << "Light On" << std::endl; }
};

// Command
class Command {
public:
    virtual void execute() = 0;
    virtual ~Command() {}
};

// Concrete Command
class LightOnCommand : public Command {
private:
    Light* light;
public:
    LightOnCommand(Light* l) : light(l) {}
    void execute() override { light->turnOn(); }
};

// Invoker
class Remote {
private:
    Command* cmd;
public:
    void setCommand(Command* c) { cmd = c; }
    void buttonPressed() { if (cmd) cmd->execute(); }
};
```
