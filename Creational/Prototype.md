# Prototype Pattern

## Definition
The **Prototype Pattern** is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

## Real-World Analogy
**Mitosis**: Mitotic cell division. A cell divides into two identical cells. The original cell acts as a prototype for the new one.
**Document Templates**: You have a standardized report format. Instead of formatting new documents from scratch every time, you copy a master "Template" document and fill in the blanks.

## Structure
-   **Prototype**: Declares the cloning method. In C#, this is often the `ICloneable` interface (though usually custom interfaces are preferred due to `ICloneable`'s shallow copy ambiguity).
-   **Concrete Prototype**: Implements the cloning method.

## Code Example (C#)

```csharp
public abstract class Shape
{
    public int X { get; set; }
    public int Y { get; set; }
    public string Color { get; set; }

    // Regular constructor
    public Shape() { }

    // Prototype constructor
    public Shape(Shape source)
    {
        this.X = source.X;
        this.Y = source.Y;
        this.Color = source.Color;
    }

    public abstract Shape Clone();
}

public class Rectangle : Shape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public Rectangle() { }

    public Rectangle(Rectangle source) : base(source)
    {
        this.Width = source.Width;
        this.Height = source.Height;
    }

    public override Shape Clone()
    {
        return new Rectangle(this);
    }
}

// Client Code
public class Client
{
    public void Run()
    {
        Rectangle rect = new Rectangle();
        rect.X = 10;
        rect.Y = 20;
        rect.Color = "Red";
        rect.Width = 100;
        rect.Height = 50;

        // Create an exact copy without strict coupling to the Rectangle class
        Rectangle copy = (Rectangle)rect.Clone();
    }
}
```

## Pros & Cons
### Pros
-   **Independence**: You can clone objects without coupling to their specific classes.
-   **Efficiency**: You can get rid of repeated initialization code in favor of cloning pre-built prototypes.
-   **Cost-Effective**: Cloning is often faster than creating new instances from scratch if initialization is expensive (e.g., DB calls).

### Cons
-   **Circular References**: Cloning complex objects that have circular references can be very tricky.
-   **Deep vs Shallow**: Deciding whether to implement a deep copy (copying all nested objects) or a shallow copy (copying references) can be difficult and error-prone.

---

### Java Example

```java
public abstract class Shape implements Cloneable {
    public int x, y;
    
    public Shape(Shape target) {
        if (target != null) {
            this.x = target.x;
            this.y = target.y;
        }
    }
    
    public abstract Shape clone();
}

public class Rectangle extends Shape {
    public int width;
    
    public Rectangle(Rectangle target) {
        super(target);
        if (target != null) {
            this.width = target.width;
        }
    }
    
    @Override
    public Shape clone() {
        return new Rectangle(this);
    }
}
```

### C++ Example

```cpp
// Abstract Prototype
class Shape {
public:
    int x, y;
    virtual Shape* clone() const = 0;
    virtual ~Shape() {}
};

class Rectangle : public Shape {
public:
    int width;
    
    Rectangle() {}
    Rectangle(const Rectangle& source) : Shape(source) {
        // Deep copy logic if needed, here just primitives
        width = source.width;
    }

    Shape* clone() const override {
        return new Rectangle(*this);
    }
};
```
