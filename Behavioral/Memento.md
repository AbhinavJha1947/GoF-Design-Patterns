# Memento Pattern

## Definition
The **Memento Pattern** is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation.

## Real-World Analogy
**Save Game**: In a video game, you reach a boss fight. You save the game. If you lose, you reload the save. The save file (Memento) contains the snapshot of your character's stats and location. The game system (Caretaker) manages the save files. The character logic (Originator) knows how to create and read the save.

## Structure
-   **Originator**: The object that holds the state and can create a memento.
-   **Memento**: The value object that acts as a snapshot of the originator's state. It should be immutable.
-   **Caretaker**: The object that keeps track of multiple mementos (history).

## Code Example (C#)

```csharp
// Memento
public class Memento
{
    private string _state;

    public Memento(string state)
    {
        _state = state;
    }

    public string GetState()
    {
        return _state;
    }
}

// Originator
public class Originator
{
    private string _state;

    public void SetState(string state)
    {
        Console.WriteLine("Originator: Setting state to " + state);
        _state = state;
    }

    public Memento Save()
    {
        Console.WriteLine("Originator: Saving to Memento.");
        return new Memento(_state);
    }

    public void Restore(Memento memento)
    {
        _state = memento.GetState();
        Console.WriteLine("Originator: State after restoring from Memento: " + _state);
    }
}

// Caretaker
public class Caretaker
{
    private List<Memento> _mementos = new List<Memento>();
    private Originator _originator;

    public Caretaker(Originator originator)
    {
        _originator = originator;
    }

    public void Backup()
    {
        Console.WriteLine("\nCaretaker: Saving Originator's state...");
        _mementos.Add(_originator.Save());
    }

    public void Undo()
    {
        if (_mementos.Count == 0) return;

        var memento = _mementos.Last();
        _mementos.Remove(memento);

        Console.WriteLine("Caretaker: Restoring state...");
        try 
        {
            _originator.Restore(memento);
        }
        catch (Exception)
        {
            Undo();
        }
    }
}
```

## Pros & Cons
### Pros
-   **Encapsulation**: You can produce snapshots of the object's state without violating its encapsulation.
-   **Simplified Originator**: The Originator code is simplified because the Caretaker maintains the history of the state.

### Cons
-   **Memory Usage**: Saving state history can consume a lot of RAM if the states are heavy.

---

### Java Example

```java
// Memento
class Memento {
    private String state;
    public Memento(String state) { this.state = state; }
    public String getState() { return state; }
}

// Originator
class Originator {
    private String state;
    public void setState(String state) { this.state = state; }
    public String getState() { return state; }
    public Memento saveStateToMemento() { return new Memento(state); }
    public void getStateFromMemento(Memento memento) { state = memento.getState(); }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

// Memento
class Memento {
private:
    std::string state;
public:
    Memento(std::string s) : state(s) {}
    std::string getState() { return state; }
};

// Originator
class Originator {
private:
    std::string state;
public:
    void setState(std::string s) { state = s; }
    Memento save() { return Memento(state); }
    void restore(Memento m) { state = m.getState(); }
};
```
