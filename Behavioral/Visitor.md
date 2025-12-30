# Visitor Pattern

## Definition
The **Visitor Pattern** is a behavioral design pattern that lets you separate algorithms from the objects on which they operate. It allows you to add new operations to existing class hierarchies without modifying them.

## Real-World Analogy
**Insurance Agent**: An insurance agent visits different buildings (houses, banks, factories). The agent performs different actions depending on the building type (selling medical insurance to a house family, theft insurance to a bank, fire insurance to a factory). The buildings just "accept" the visitor, and the visitor does the rest.

## Structure
-   **Visitor**: Declares a visit operation for each class of Concrete Element.
-   **Concrete Visitor**: Implements each visit method.
-   **Element**: Declares a method for accepting a visitor (`Accept(IVisitor visitor)`).
-   **Concrete Element**: Implements the acceptance method, usually delegating to the visitor (`visitor.VisitConcreteElement(this)`).

## Code Example (C#)

```csharp
// Element Interface
public interface IShape
{
    void Accept(IVisitor visitor);
}

// Concrete Elements
public class Dot : IShape
{
    public void Accept(IVisitor visitor) => visitor.VisitDot(this);
}

public class Circle : IShape
{
    public void Accept(IVisitor visitor) => visitor.VisitCircle(this);
}

public class Rectangle : IShape
{
    public void Accept(IVisitor visitor) => visitor.VisitRectangle(this);
}

// Visitor Interface
public interface IVisitor
{
    void VisitDot(Dot dot);
    void VisitCircle(Circle circle);
    void VisitRectangle(Rectangle rectangle);
}

// Concrete Visitor (XMLExportVisitor)
public class XMLExportVisitor : IVisitor
{
    public void VisitDot(Dot dot) => Console.WriteLine("<dot>...</dot>");
    public void VisitCircle(Circle circle) => Console.WriteLine("<circle>...</circle>");
    public void VisitRectangle(Rectangle rectangle) => Console.WriteLine("<rectangle>...</rectangle>");
}

// Client
public class Client
{
    public static void Main()
    {
        List<IShape> shapes = new List<IShape> { new Dot(), new Circle(), new Rectangle() };
        
        var exportVisitor = new XMLExportVisitor();
        
        foreach (var shape in shapes)
        {
            shape.Accept(exportVisitor);
        }
    }
}
```

## Pros & Cons
### Pros
-   **Open/Closed Principle**: You can introduce a new behavior that can work with objects of different classes without changing these classes.
-   **Single Responsibility Principle**: You can move multiple versions of the same behavior into the same class.

### Cons
-   **Updates**: Adding a new element to the object structure is hard because you have to change the Visitor interface and all concrete visitors.
-   **Encapsulation**: Visitors might lack the necessary access to the private fields and methods of the elements that they're supposed to work with.

---

### Java Example

```java
interface ComputerPart {
    public void accept(ComputerPartVisitor visitor);
}

class Keyboard implements ComputerPart {
    public void accept(ComputerPartVisitor visitor) { visitor.visit(this); }
}

interface ComputerPartVisitor {
    public void visit(Keyboard keyboard);
}

class DisplayVisitor implements ComputerPartVisitor {
    public void visit(Keyboard keyboard) { System.out.println("Displaying Keyboard."); }
}
```

### C++ Example

```cpp
#include <iostream>

class Keyboard; // Forward declaration

class Visitor {
public:
    virtual void visit(Keyboard* element) = 0;
};

class Element {
public:
    virtual void accept(Visitor* v) = 0;
};

class Keyboard : public Element {
public:
    void accept(Visitor* v) override { v->visit(this); }
};

class DisplayVisitor : public Visitor {
public:
    void visit(Keyboard* element) override { std::cout << "Visisted Keyboard" << std::endl; }
};
```
