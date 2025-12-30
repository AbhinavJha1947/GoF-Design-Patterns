# State Pattern

## Definition
The **State Pattern** is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

## Real-World Analogy
**Smartphone Lock Screen**: Your phone behaves differently depending on whether it's locked or unlocked.
-   **Locked**: Pressing buttons might turn on the screen or ask for a password.
-   **Unlocked**: Pressing buttons launches apps, types messages, etc.
 The phone object delegates the behavior to the current "State" object (LockedState vs UnlockedState).

## Structure
-   **Context**: Stores a reference to one of the concrete state objects and delegates to it all state-specific work.
-   **State**: The interface for encapsulating the behavior associated with a particular state of the Context.
-   **Concrete States**: Implement various behaviors associated with a state of the Context.

## Code Example (C#)

```csharp
// State Interface
public abstract class State
{
    protected Context _context;

    public void SetContext(Context context)
    {
        _context = context;
    }

    public abstract void Handle1();
    public abstract void Handle2();
}

// Context
public class Context
{
    private State _state;

    public Context(State state)
    {
        this.TransitionTo(state);
    }

    public void TransitionTo(State state)
    {
        Console.WriteLine($"Context: Transition to {state.GetType().Name}.");
        _state = state;
        _state.SetContext(this);
    }

    public void Request1() => _state.Handle1();
    public void Request2() => _state.Handle2();
}

// Concrete State A
public class ConcreteStateA : State
{
    public override void Handle1()
    {
        Console.WriteLine("ConcreteStateA handles request1.");
        Console.WriteLine("ConcreteStateA wants to change the state of the context.");
        this._context.TransitionTo(new ConcreteStateB());
    }

    public override void Handle2()
    {
        Console.WriteLine("ConcreteStateA handles request2.");
    }
}

// Concrete State B
public class ConcreteStateB : State
{
    public override void Handle1()
    {
        Console.WriteLine("ConcreteStateB handles request1.");
    }

    public override void Handle2()
    {
        Console.WriteLine("ConcreteStateB handles request2.");
        Console.WriteLine("ConcreteStateB wants to change the state of the context.");
        this._context.TransitionTo(new ConcreteStateA());
    }
}
```

## Pros & Cons
### Pros
-   **Single Responsibility Principle**: Organizes the code related to particular states into separate classes.
-   **Simplification**: Simplifies the code of the context by eliminating bulky state machine conditionals (`if` or `switch` statements).

### Cons
-   **Overkill**: Applying the pattern can be overkill if a state machine has only a few states or rarely changes.
-   **Files**: Increases the number of classes (one for each state).

---

### Java Example

```java
interface State {
    void doAction(Context context);
}

class Context {
    private State state;
    public void setState(State state) { this.state = state; }
    public State getState() { return state; }
}

class StartState implements State {
    public void doAction(Context context) {
        System.out.println("Player is in start state");
        context.setState(this);
    }
}
```

### C++ Example

```cpp
#include <iostream>

class Context;

class State {
public:
    virtual void handle(Context* ctx) = 0;
    virtual ~State() {}
};

class Context {
    State* state;
public:
    Context(State* s) : state(s) {}
    void transitionTo(State* s) {
        std::cout << "Transitioning state..." << std::endl;
        delete state; // Warning: memory management strategy needed here
        state = s;
    }
    void request() { state->handle(this); }
};

class ConcreteStateA : public State {
public:
    void handle(Context* ctx) override {
        std::cout << "State A handles request." << std::endl;
    }
};
```
