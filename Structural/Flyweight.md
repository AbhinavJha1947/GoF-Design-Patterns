# Flyweight Pattern

## Definition
The **Flyweight Pattern** is a structural design pattern that lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

## Real-World Analogy
**Library**: A library has thousands of books. It doesn't buy a physical new copy of a book every time a patron wants to read "Harry Potter". It keeps a few copies (Flyweights) and loans them out. The intrinsic state (Text, Author) is shared. The extrinsic state (Current holder, Due date) is tracked separately in the loan record.

## Structure
-   **Flyweight**: Stores the common state (intrinsic).
-   **Context**: Specific object that stores the unique state (extrinsic) and references the flyweight.
-   **Flyweight Factory**: Manages the pool of flyweights.

## Code Example (C#)

```csharp
// Flyweight
public class TreeType
{
    private string _name;
    private string _color; // Shared state
    private string _texture;

    public TreeType(string name, string color, string texture)
    {
        _name = name;
        _color = color;
        _texture = texture;
    }

    public void Draw(int x, int y) // Extrinsic state passed in
    {
        Console.WriteLine($"Drawing {_name} tree at ({x}, {y}) with color {_color}");
    }
}

// Flyweight Factory
public class TreeFactory
{
    private static Dictionary<string, TreeType> _treeTypes = new Dictionary<string, TreeType>();

    public static TreeType GetTreeType(string name, string color, string texture)
    {
        string key = $"{name}_{color}_{texture}";
        if (!_treeTypes.ContainsKey(key))
        {
            _treeTypes[key] = new TreeType(name, color, texture);
        }
        return _treeTypes[key];
    }
}

// Context
public class Tree
{
    private int _x;
    private int _y;
    private TreeType _type; // Reference to shared object

    public Tree(int x, int y, TreeType type)
    {
        _x = x;
        _y = y;
        _type = type;
    }

    public void Draw()
    {
        _type.Draw(_x, _y);
    }
}
```

## Pros & Cons
### Pros
-   **Memory Efficiency**: Significant reduction in RAM usage when supporting huge numbers of objects.

### Cons
-   **Complexity**: Code becomes much more complicated.
-   **CPU vs RAM**: You might be trading RAM for CPU cycles (if the intrinsic data needs to be recalculated/marshaled).

---

### Java Example

```java
import java.util.HashMap;
import java.util.Map;

// Flyweight
class TreeType {
    private String name;
    private String color;

    public TreeType(String name, String color) {
        this.name = name;
        this.color = color;
    }

    public void draw(int x, int y) {
        System.out.println("Draw tree " + name + " at " + x + "," + y);
    }
}

// Factory
class TreeFactory {
    static Map<String, TreeType> treeTypes = new HashMap<>();

    public static TreeType getTreeType(String name, String color) {
        String key = name + "_" + color;
        if (!treeTypes.containsKey(key)) {
            treeTypes.put(key, new TreeType(name, color));
        }
        return treeTypes.get(key);
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>
#include <map>

// Flyweight
class TreeType {
private:
    std::string name;
    std::string color;
public:
    TreeType(std::string n, std::string c) : name(n), color(c) {}
    void draw(int x, int y) {
        std::cout << "Draw tree " << name << " at " << x << "," << y << std::endl;
    }
};

// Factory
class TreeFactory {
private:
    static std::map<std::string, TreeType*> treeTypes;
public:
    static TreeType* getTreeType(std::string name, std::string color) {
        std::string key = name + "_" + color;
        if (treeTypes.find(key) == treeTypes.end()) {
            treeTypes[key] = new TreeType(name, color);
        }
        return treeTypes[key];
    }
};
std::map<std::string, TreeType*> TreeFactory::treeTypes;
```
