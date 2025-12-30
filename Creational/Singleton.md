# Singleton Pattern

## Definition
The **Singleton Pattern** ensures that a class has only one instance and provides a global point of access to that instance. It is one of the simplest design patterns but also one of the most controversial due to its similarity to global state.

## Real-World Analogy
**The Government**: A country can have only one official government. Regardless of the personal identities of the individuals who form the government, the title, "The Government of X," is a global point of access that identifies the group of people in charge.

## Structure
-   **Private Constructor**: Prevents direct instantiation of the class from outside.
-   **Private Static Instance**: Holds the single instance of the class in the class itself.
-   **Public Static Method**: The access point (often named `GetInstance`) that returns the instance, creating it if it doesn't exist yet.

## Code Example (C#)

```csharp
public sealed class Singleton
{
    // detailed: The 'sealed' keyword prevents derivation, which could add instances.
    private static Singleton _instance = null;
    private static readonly object _lock = new object();

    // Private constructor
    private Singleton()
    {
    }

    public static Singleton Instance
    {
        get
        {
            // Thread-safety using double-checked locking
            if (_instance == null)
            {
                lock (_lock)
                {
                    if (_instance == null)
                    {
                        _instance = new Singleton();
                    }
                }
            }
            return _instance;
        }
    }

    public void DoSomething()
    {
        Console.WriteLine("Singleton is working.");
    }
}
```

## Pros & Cons
### Pros
-   **Controlled Access**: You strictly control how and when clients access the instance.
-   **Namespace Management**: Avoids polluting the global namespace with global variables.

### Cons
-   **Global State**: Can hide dependencies and make code harder to understand.
-   **Testing Difficulty**: Difficult to mock or stub in unit tests because the constructor is private and the method is static.

---

### Java Example

```java
public class Singleton {
    // volatile ensures changes are visible to other threads immediately
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### C++ Example

```cpp
#include <mutex>

class Singleton {
private:
    static Singleton* instance;
    static std::mutex mutex_;
    
    // Private constructor
    Singleton() {}

public:
    // Delete copy constructor and assignment operator
    Singleton(const Singleton&) = delete;
    void operator=(const Singleton&) = delete;

    static Singleton* getInstance() {
        std::lock_guard<std::mutex> lock(mutex_);
        if (instance == nullptr) {
            instance = new Singleton();
        }
        return instance;
    }
};

// Initialize static members
Singleton* Singleton::instance = nullptr;
std::mutex Singleton::mutex_;
```
