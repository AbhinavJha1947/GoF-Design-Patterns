# Composite Pattern

## Definition
The **Composite Pattern** is a structural design pattern that lets you compose objects into tree structures and then work with these structures as if they were individual objects.

## Real-World Analogy
**Army Hierarchy**: An army consists of soldiers. Soldiers are grouped into squads, squads into platoons, platoons into companies. A general can give an order "Attack" to a company. The company commander delegates it to platoons, and so on, until the individual soldier executes it. The general doesn't care if they are talking to a single soldier or a whole division; the command is the same.

## Structure
-   **Component**: The interface for all objects in the composition (both leaves and composites).
-   **Leaf**: Basic element of a tree that doesn't have sub-elements.
-   **Composite**: An element that has sub-elements.

## Code Example (C#)

```csharp
// Component
public abstract class Graphic
{
    public abstract void Draw();
}

// Leaf
public class Dot : Graphic
{
    public override void Draw() => Console.WriteLine("Draw Dot");
}

// Leaf
public class Circle : Graphic
{
    public override void Draw() => Console.WriteLine("Draw Circle");
}

// Composite
public class CompoundGraphic : Graphic
{
    private List<Graphic> _children = new List<Graphic>();

    public void Add(Graphic child) => _children.Add(child);
    public void Remove(Graphic child) => _children.Remove(child);

    public override void Draw()
    {
        foreach (var child in _children)
        {
            child.Draw();
        }
    }
}
```

## Pros & Cons
### Pros
-   **Simplicity**: You can work with complex tree structures more conveniently: use polymorphism and recursion to your advantage.
-   **Open/Closed Principle**: You can introduce new element types into the app without breaking the existing code, which now works with the object tree.

### Cons
-   **Generalization**: It might be difficult to provide a common interface for classes whose functionality differs too much. You might need to over-complexify the component interface.

---

### Java Example

```java
import java.util.ArrayList;
import java.util.List;

// Component
abstract class Graphic {
    public abstract void draw();
}

// Leaf
class Dot extends Graphic {
    public void draw() { System.out.println("Draw Dot"); }
}

class Circle extends Graphic {
    public void draw() { System.out.println("Draw Circle"); }
}

// Composite
class CompoundGraphic extends Graphic {
    private List<Graphic> children = new ArrayList<>();

    public void add(Graphic child) { children.add(child); }
    public void remove(Graphic child) { children.remove(child); }

    public void draw() {
        for (Graphic child : children) {
            child.draw();
        }
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Component
class Graphic {
public:
    virtual void draw() = 0;
    virtual ~Graphic() {}
};

// Leaf
class Dot : public Graphic {
public:
    void draw() override { std::cout << "Draw Dot" << std::endl; }
};

// Composite
class CompoundGraphic : public Graphic {
private:
    std::vector<Graphic*> children;

public:
    void add(Graphic* child) { children.push_back(child); }
    
    // In real C++, consider smart pointers for ownership
    void draw() override {
        for (auto child : children) {
            child->draw();
        }
    }
};
```
