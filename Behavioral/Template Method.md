# Template Method Pattern

## Definition
The **Template Method Pattern** is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

## Real-World Analogy
**Home Builders**: Building a house follows a standard plan (Template): Foundation -> Frame -> Walls -> Roof -> Plumbing/Wiring.
However, a **Wooden House** uses wood for walls, while a **Glass House** uses glass. The steps are the same, but the implementation of specific steps differs.

## Structure
-   **Abstract Class**: Declares methods that act as steps of an algorithm, and the template method which calls these steps in a specific order.
-   **Concrete Class**: Overrides some of the steps.

## Code Example (C#)

```csharp
// Abstract Class
public abstract class DataMiner
{
    // The Template Method - usually 'final' so subclasses can't change the flow
    public void Mine(string path)
    {
        var file = OpenFile(path);
        var rawData = ExtractData(file);
        var data = ParseData(rawData);
        var analysis = AnalyzeData(data);
        SendReport(analysis);
        CloseFile(file);
    }

    // Abstract steps - must be implemented
    protected abstract string ExtractData(object file);
    protected abstract string ParseData(string rawData);

    // Concrete steps - shared by all subclasses
    protected object OpenFile(string path) => "FileStream";
    protected void CloseFile(object file) => Console.WriteLine("Closing file");
    protected string AnalyzeData(string data) => "Analysis Result";
    protected void SendReport(string analysis) => Console.WriteLine("Sending Report: " + analysis);

    // Hooks - optional steps
    protected virtual void Hook() { } 
}

// Concrete Class
public class PDFDataMiner : DataMiner
{
    protected override string ExtractData(object file) => "PDF Data extraction";
    protected override string ParseData(string rawData) => "Parsing PDF logic";
}

public class CSVDataMiner : DataMiner
{
    protected override string ExtractData(object file) => "CSV Data extraction";
    protected override string ParseData(string rawData) => "Parsing CSV logic";
}
```

## Pros & Cons
### Pros
-   **Code Reuse**: You can let clients override only certain parts of a large algorithm, making them less affected by changes that happen to other parts of the algorithm.
-   **Duplicate Code Removal**: You can pull the duplicate code into a superclass.

### Cons
-   **Rigidity**: Some clients may be limited by the customized skeleton of an algorithm.
-   **Liskov Substitution**: Violation can occur if subclasses suppress default step implementations specified in the parent class.

---

### Java Example

```java
abstract class Game {
    abstract void initialize();
    abstract void startPlay();
    abstract void endPlay();

    // Template method
    public final void play() {
        initialize();
        startPlay();
        endPlay();
    }
}

class Cricket extends Game {
    void initialize() { System.out.println("Cricket Initialized"); }
    void startPlay() { System.out.println("Cricket Started"); }
    void endPlay() { System.out.println("Cricket Finished"); }
}
```

### C++ Example

```cpp
#include <iostream>

class Game {
public:
    virtual void initialize() = 0;
    virtual void startPlay() = 0;
    virtual void endPlay() = 0;

    // Template method
    void play() {
        initialize();
        startPlay();
        endPlay();
    }
    virtual ~Game() {}
};

class Mario : public Game {
public:
    void initialize() override { std::cout << "Mario Init" << std::endl; }
    void startPlay() override { std::cout << "Mario Start" << std::endl; }
    void endPlay() override { std::cout << "Mario End" << std::endl; }
};
```
