# Iterator Pattern

## Definition
The **Iterator Pattern** is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

## Real-World Analogy
**Sightseeing**: You are visiting Rome. You want to see the Colosseum, the Pantheon, and the Vatican. You can just walk around randomly (no iterator), or you can buy a bus ticket (Iterator) that takes you to each spot in a specific order. You don't need to know the city layout; you just get on the bus.

## Structure
-   **Iterator**: Declares an interface for accessing and traversing elements.
-   **Concrete Iterator**: Implements the iterator interface for a specific collection.
-   **Collection (Iterable)**: Declares the method to create an iterator.

## Code Example (C#)

```csharp
// Iterator Interface
public interface IIterator
{
    bool HasNext();
    object Next();
}

// Collection Interface
public interface IContainer
{
    IIterator GetIterator();
}

// Concrete Collection
public class NameRepository : IContainer
{
    public string[] names = { "Robert", "John", "Julie", "Lora" };

    public IIterator GetIterator()
    {
        return new NameIterator(this);
    }

    // Concrete Iterator (often an inner class)
    private class NameIterator : IIterator
    {
        int index;
        NameRepository _repo;

        public NameIterator(NameRepository repo)
        {
            _repo = repo;
        }

        public bool HasNext()
        {
            if (index < _repo.names.Length)
            {
                return true;
            }
            return false;
        }

        public object Next()
        {
            if (this.HasNext())
            {
                return _repo.names[index++];
            }
            return null;
        }
    }
}
```

## Pros & Cons
### Pros
-   **Single Responsibility Principle**: You can clean up the client code and the collections by moving bulky traversal algorithms into separate classes.
-   **Open/Closed Principle**: You can implement new types of collections and iterators and pass them to existing code without breaking anything.
-   **Parallel Iteration**: You can iterate over the same collection in parallel because each iterator object contains its own iteration state.

### Cons
-   **Overkill**: For simple collections, a simple loop is often sufficient and more efficient.

---

### Java Example

```java
import java.util.Iterator;

class NameRepository implements Iterable<String> {
    public String names[] = {"Robert", "John", "Julie", "Lora"};

    @Override
    public Iterator<String> iterator() {
        return new NameIterator();
    }

    private class NameIterator implements Iterator<String> {
        int index;
        public boolean hasNext() { return index < names.length; }
        public String next() { return hasNext() ? names[index++] : null; }
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <vector>

// Simple iterator wrapper style
class Container {
    std::vector<int> data;
public:
    void add(int v) { data.push_back(v); }

    // Standard C++ iterator exposure
    std::vector<int>::iterator begin() { return data.begin(); }
    std::vector<int>::iterator end() { return data.end(); }
};

int main() {
    Container c;
    c.add(1); c.add(2);
    
    for (auto it = c.begin(); it != c.end(); ++it) {
        std::cout << *it << std::endl;
    }
    return 0;
}
```
