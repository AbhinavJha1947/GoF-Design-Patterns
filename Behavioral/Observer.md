# Observer Pattern

## Definition
The **Observer Pattern** is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they're observing.

## Real-World Analogy
**Newspapers/News Feed**: You subscribe to a newspaper or a YouTube channel. When a new issue or video is released (Event), you (Observer) get notified automatically. The publisher (Subject) doesn't know who you are personally, just that you are on the list.

## Structure
-   **Subject**: The publisher. Handles subscriptions and notifies observers.
-   **Observer**: The subscriber interface. Declares the update method.
-   **Concrete Observer**: Reacts to the update notification.

## Code Example (C#)

```csharp
// Observer Interface
public interface IObserver
{
    void Update(ISubject subject);
}

// Subject Interface
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

// Concrete Subject
public class Subject : ISubject
{
    public int State { get; set; } = -0;
    private List<IObserver> _observers = new List<IObserver>();

    public void Attach(IObserver observer)
    {
        Console.WriteLine("Subject: Attached an observer.");
        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        _observers.Remove(observer);
        Console.WriteLine("Subject: Detached an observer.");
    }

    public void Notify()
    {
        Console.WriteLine("Subject: Notifying observers...");
        foreach (var observer in _observers)
        {
            observer.Update(this);
        }
    }

    public void SomeBusinessLogic()
    {
        Console.WriteLine("\nSubject: I'm doing something important.");
        State = new Random().Next(0, 10);
        Console.WriteLine("Subject: My state has just changed to: " + State);
        Notify();
    }
}

// Concrete Observer
public class ConcreteObserverA : IObserver
{
    public void Update(ISubject subject)
    {
        if ((subject as Subject).State < 3)
        {
            Console.WriteLine("ConcreteObserverA: Reacted to the event.");
        }
    }
}

public class ConcreteObserverB : IObserver
{
    public void Update(ISubject subject)
    {
        if ((subject as Subject).State == 0 || (subject as Subject).State >= 2)
        {
            Console.WriteLine("ConcreteObserverB: Reacted to the event.");
        }
    }
}
```

## Pros & Cons
### Pros
-   **Open/Closed Principle**: You can introduce new subscriber classes without having to change the publisher's code (and vice versa if there's a publisher interface).
-   **Relationships**: You can establish relations between objects at runtime.

### Cons
-   **Memory Leaks**: The "Lapsed Listener" problem occurs if observers are not explicitly deregistered.
-   **Order**: Subscribers are notified in random order.

---

### Java Example

```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(String message);
}

class Channel {
    private List<Observer> observers = new ArrayList<>();
    
    public void subscribe(Observer o) { observers.add(o); }
    public void notify(String msg) {
        for (Observer o : observers) o.update(msg);
    }
}

class Subscriber implements Observer {
    public void update(String msg) { System.out.println("Received: " + msg); }
}
```

### C++ Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Observer {
public:
    virtual void update(int value) = 0;
};

class Subject {
    std::vector<Observer*> observers;
public:
    void attach(Observer* o) { observers.push_back(o); }
    void notify(int val) {
        for (auto o : observers) o->update(val);
    }
};

class ConcreteObserver : public Observer {
public:
    void update(int val) override {
        std::cout << "Observed: " << val << std::endl;
    }
};
```
```
