# Design Patterns Interview Questions & Answers

This guide covers common interview questions related to the Gang of Four (GoF) Design Patterns. It is divided into Creational, Structural, and Behavioral patterns.

## Table of Contents

### Creational Patterns
1. [What is the Singleton Pattern and when should it be used?](#1-what-is-the-singleton-pattern-and-when-should-it-be-used)
2. [Explain the Factory Method Pattern.](#2-explain-the-factory-method-pattern)
3. [What is the difference between Abstract Factory and Factory Method?](#3-what-is-the-difference-between-abstract-factory-and-factory-method)
4. [Describe the Builder Pattern.](#4-describe-the-builder-pattern)
5. [What is the Prototype Pattern?](#5-what-is-the-prototype-pattern)

### Structural Patterns
6. [What is the Adapter Pattern?](#6-what-is-the-adapter-pattern)
7. [Explain the Bridge Pattern.](#7-explain-the-bridge-pattern)
8. [What is the Composite Pattern?](#8-what-is-the-composite-pattern)
9. [Describe the Decorator Pattern and how it differs from inheritance.](#9-describe-the-decorator-pattern-and-how-it-differs-from-inheritance)
10. [What is the Facade Pattern?](#10-what-is-the-facade-pattern)
11. [Explain the Flyweight Pattern.](#11-explain-the-flyweight-pattern)
12. [What is the Proxy Pattern?](#12-what-is-the-proxy-pattern)

### Behavioral Patterns
13. [What is the Chain of Responsibility Pattern?](#13-what-is-the-chain-of-responsibility-pattern)
14. [Explain the Command Pattern.](#14-explain-the-command-pattern)
15. [What is the Interpreter Pattern?](#15-what-is-the-interpreter-pattern)
16. [Describe the Iterator Pattern.](#16-describe-the-iterator-pattern)
17. [What is the Mediator Pattern?](#17-what-is-the-mediator-pattern)
18. [Explain the Memento Pattern.](#18-explain-the-memento-pattern)
19. [What is the Observer Pattern?](#19-what-is-the-observer-pattern)
20. [Describe the State Pattern.](#20-describe-the-state-pattern)
21. [What is the Strategy Pattern?](#21-what-is-the-strategy-pattern)
22. [Explain the Template Method Pattern.](#22-explain-the-template-method-pattern)
23. [What is the Visitor Pattern?](#23-what-is-the-visitor-pattern)

### Advanced Questions (SDE-2/3 Level)
24. [What is the difference between Proxy, Decorator, and Adapter?](#24-what-is-the-difference-between-proxy-decorator-and-adapter)
25. [How do you implement a Thread-Safe Singleton?](#25-how-do-you-implement-a-thread-safe-singleton)
26. [What is the "Lapsed Listener" problem in the Observer Pattern?](#26-what-is-the-lapsed-listener-problem-in-the-observer-pattern)
27. [Deep Copy vs. Shallow Copy in Prototype Pattern?](#27-deep-copy-vs-shallow-copy-in-prototype-pattern)
28. [Scenario: Design a Cross-Platform UI Framework.](#28-scenario-design-a-cross-platform-ui-framework)
29. [Scenario: Implement Undo/Redo in a Text Editor.](#29-scenario-implement-undoredo-in-a-text-editor)
30. [Why is Singleton often considered an Anti-Pattern?](#30-why-is-singleton-often-considered-an-anti-pattern)

---

## Creational Patterns

### 1. What is the Singleton Pattern and when should it be used?

**Answer:**
The **Singleton Pattern** ensures that a class has only one instance and provides a global point of access to it.

*   **Key usage:**
    *   Logging services.
    *   Database connection pools.
    *   Configuration settings managers.
*   **Implementation:** Usually involves a private static instance variable, a private constructor, and a public static method (`getInstance`) that returns the instance. Thread safety is a critical consideration (e.g., using double-checked locking).

[Back to Top](#table-of-contents)

### 2. Explain the Factory Method Pattern.

**Answer:**
The **Factory Method Pattern** defines an interface for creating an object, but lets subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

*   **Key usage:**
    *   When a class doesn't know in advance what class of objects it must create.
    *   When a class wants its subclasses to specify the objects it creates.

[Back to Top](#table-of-contents)

### 3. What is the difference between Abstract Factory and Factory Method?

**Answer:**
*   **Factory Method** relies on inheritance: object creation is delegated to subclasses which implement the factory method to create objects.
*   **Abstract Factory** relies on object composition: object creation is implemented in methods exposed in the factory interface.
*   **Scope:** Factory Method creates a single type of object, whereas Abstract Factory creates families of related or dependent objects (e.g., a UI toolkit creating Buttons, Windows, and Scrollbars for Windows vs. MacOS).

[Back to Top](#table-of-contents)

### 4. Describe the Builder Pattern.

**Answer:**
The **Builder Pattern** separates the construction of a complex object from its representation so that the same construction process can create different representations.

*   **Key usage:**
    *   Constructing complex objects with many optional parameters (avoids "telescoping constructor" anti-pattern).
    *   Creating immutable objects that require complex initialization steps.

[Back to Top](#table-of-contents)

### 5. What is the Prototype Pattern?

**Answer:**
The **Prototype Pattern** specifies the kinds of objects to create using a prototypical instance, and creates new objects by copying this prototype.

*   **Key usage:**
    *   When the cost of creating a new object from scratch is expensive (e.g., database operations).
    *   Deep copy vs. Shallow copy is a key concept here.

[Back to Top](#table-of-contents)

---

## Structural Patterns

### 6. What is the Adapter Pattern?

**Answer:**
The **Adapter Pattern** converts the interface of a class into another interface the clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

*   **Example:** A card reader acting as an adapter between a memory card and a computer's USB port.

[Back to Top](#table-of-contents)

### 7. Explain the Bridge Pattern.

**Answer:**
The **Bridge Pattern** decouples an abstraction from its implementation so that the two can vary independently.

*   **Structure:** It involves an `Abstraction` interface and an `Implementor` interface. The Abstraction maintains a reference to the Implementor.
*   **Benefits:** Changes in the implementation don't affect the abstraction and vice versa.

[Back to Top](#table-of-contents)

### 8. What is the Composite Pattern?

**Answer:**
The **Composite Pattern** composes objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

*   **Example:** A file system where directories can contain files or other directories. `File` and `Directory` both implement a common `FileSystemComponent` interface.

[Back to Top](#table-of-contents)

### 9. Describe the Decorator Pattern and how it differs from inheritance.

**Answer:**
The **Decorator Pattern** attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

*   **Difference from Inheritance:** Inheritance extends behavior at compile-time and applies to the entire class. Decorators extend behavior at runtime and can be applied to individual objects.
*   **Example:** Buffered streams in Java I/O (`BufferedReader` wraps `FileReader`).

[Back to Top](#table-of-contents)

### 10. What is the Facade Pattern?

**Answer:**
The **Facade Pattern** provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

*   **Key usage:** hiding the complexity of a library or API behind a simpler interface.

[Back to Top](#table-of-contents)

### 11. Explain the Flyweight Pattern.

**Answer:**
The **Flyweight Pattern** uses sharing to support large numbers of fine-grained objects efficiently.

*   **Mechanism:** Separates "intrinsic" state (shared, context-independent) from "extrinsic" state (unique, context-dependent). Only intrinsic state is stored in the object; extrinsic state is passed in by the client.
*   **Example:** Representing characters in a text document where the font/glyph is shared, but position is extrinsic.

[Back to Top](#table-of-contents)

### 12. What is the Proxy Pattern?

**Answer:**
The **Proxy Pattern** provides a surrogate or placeholder for another object to control access to it.

*   **Types:**
    *   **Remote Proxy:** Represents an object in a different address space.
    *   **Virtual Proxy:** Creates expensive objects on demand (lazy loading).
    *   **Protection Proxy:** Controls access rights to the original object.

[Back to Top](#table-of-contents)

---

## Behavioral Patterns

### 13. What is the Chain of Responsibility Pattern?

**Answer:**
The **Chain of Responsibility Pattern** avoids coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. It chains the receiving objects and passes the request along the chain until an object handles it.

*   **Example:** Exception handling (try-catch blocks), Event bubbling in UI frameworks.

[Back to Top](#table-of-contents)

### 14. Explain the Command Pattern.

**Answer:**
The **Command Pattern** encapsulates a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

*   **Components:** Command Interface, Concrete Command, Receiver, Invoker.

[Back to Top](#table-of-contents)

### 15. What is the Interpreter Pattern?

**Answer:**
The **Interpreter Pattern** given a language, defines a represention for its grammar along with an interpreter that uses the representation to interpret sentences in the language.

*   **Usage:** SQL parsers, compiler expression evaluation.

[Back to Top](#table-of-contents)

### 16. Describe the Iterator Pattern.

**Answer:**
The **Iterator Pattern** provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

*   **Benefit:** Supports uniform traversal across different collection types (List, Set, Tree) via a common interface (`hasNext()`, `next()`).

[Back to Top](#table-of-contents)

### 17. What is the Mediator Pattern?

**Answer:**
The **Mediator Pattern** defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.

*   **Analogy:** An Air Traffic Control tower acts as a mediator for airplanes (colleagues) which do not communicate directly with each other.

[Back to Top](#table-of-contents)

### 18. Explain the Memento Pattern.

**Answer:**
The **Memento Pattern** captures and externalizes an object's internal state so that the object can be restored to this state later, without violating encapsulation.

*   **Key usage:** Undo/Redo functionality in editors.

[Back to Top](#table-of-contents)

### 19. What is the Observer Pattern?

**Answer:**
The **Observer Pattern** defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

*   **Example:** Model-View-Controller (MVC) architecture, Event Listeners.

[Back to Top](#table-of-contents)

### 20. Describe the State Pattern.

**Answer:**
The **State Pattern** allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

*   **Implementation:** State-specific behavior is extracted into separate State classes. The context object delegates behavior updates to the current State object.

[Back to Top](#table-of-contents)

### 21. What is the Strategy Pattern?

**Answer:**
The **Strategy Pattern** defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

*   **Difference from State:** Strategy is usually about *how* something is done (algorithm), whereas State is about *what* an object is (state of being). Strategy is often passed in once; State transitions over time.

[Back to Top](#table-of-contents)

### 22. Explain the Template Method Pattern.

**Answer:**
The **Template Method Pattern** defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

*   **Keywords:** Hooks, invariant steps (in base class), variant steps (abstract methods).

[Back to Top](#table-of-contents)

### 23. What is the Visitor Pattern?

**Answer:**
The **Visitor Pattern** represents an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

*   **Mechanism:** Double dispatch. `Element.accept(Visitor)` calls `Visitor.visit(Element)`.
*   **Usage:** Performing operations (exporting, validating) on complex object structures like abstract syntax trees.

[Back to Top](#table-of-contents)

---

## Advanced Questions (SDE-2/3 Level)

### 24. What is the difference between Proxy, Decorator, and Adapter?

**Answer:**
While all three are structural patterns that "wrap" an object, their **intent** differs significantly:
*   **Adapter**: Changes the interface of an existing object to match another interface. It makes things work together that couldn't otherwise. *Intent: Compatibility.*
*   **Decorator**: Adds new responsibilities/behavior to an object dynamically without changing its interface. It supports recursive composition. *Intent: Extensibility.*
*   **Proxy**: Controls access to an object. It maintains the same interface as the real subject. It's used for lazy loading, access control, logging, etc. *Intent: Control.*

[Back to Top](#table-of-contents)

### 25. How do you implement a Thread-Safe Singleton?

**Answer:**
In C#, the standard implementations are:
1.  **Double-Checked Locking**: Using `lock` inside a null check to avoid locking overhead once the instance is created.
2.  **Static Initialization (Lazy<T>)**: Using .NET's `Lazy<T>` type is the preferred, modern, thread-safe way.

**Double-Checked Locking Example:**
```csharp
private static Singleton _instance;
private static readonly object _lock = new object();

public static Singleton Instance {
    get {
        if (_instance == null) {
            lock (_lock) {
                if (_instance == null) {
                    _instance = new Singleton();
                }
            }
        }
        return _instance;
    }
}
```
**Lazy<T> Example (Preferred):**
```csharp
private static readonly Lazy<Singleton> _lazy = new Lazy<Singleton>(() => new Singleton());
public static Singleton Instance => _lazy.Value;
```

[Back to Top](#table-of-contents)

### 26. What is the "Lapsed Listener" problem in the Observer Pattern?

**Answer:**
The **Lapsed Listener Problem** is a memory leak that occurs when an observer subscribes to a subject but never unsubscribes.
*   **Cause**: The subject holds a strong reference to the observer in its list of subscribers. Even if the observer is no longer needed elsewhere in the app, the Garbage Collector (GC) cannot reclaim it because the subject still "points" to it.
*   **Solution**:
    *   Explicitly call `Detach()` or `Unsubscribe()` when the observer is disposed.
    *   Use **Weak References** used in the subject implementation (e.g., `WeakEventManager` in WPF) so the GC can collect subscribers even if they haven't explicitly unsubscribed.

[Back to Top](#table-of-contents)

### 27. Deep Copy vs. Shallow Copy in Prototype Pattern?

**Answer:**
*   **Shallow Copy**: Copies the object's top-level structure. Value types (int, bool) are copied, but reference types (objects, lists) are shared. If you modify a shared list in the copy, the original is also affected. C#'s `MemberwiseClone()` performs a shallow copy.
*   **Deep Copy**: Recursively copies the object and all objects it references. The new object is completely independent.
*   **Implementation**: Deep copy usually requires custom logic (serializing/deserializing to JSON, or recursively calling `Clone()` on child objects).

[Back to Top](#table-of-contents)

### 28. Scenario: Design a Cross-Platform UI Framework.

**Answer:**
**Pattern: Abstract Factory + Bridge.**
*   **Challenge**: You need to render buttons, windows, and text boxes on Windows, macOS, and Linux.
*   **Solution**:
    *   Use **Abstract Factory** (`IGUIFactory`) to create families of components (`CreateButton()`, `CreateWindow()`).
    *   Concrete factories (`WindowsFactory`, `MacFactory`) return platform-specific implementations.
    *   Use **Bridge** if the components themselves have complex logic that needs to vary independently of the platform implementation.

[Back to Top](#table-of-contents)

### 29. Scenario: Implement Undo/Redo in a Text Editor.

**Answer:**
**Pattern: Command + Memento.**
*   **Command**: Encapsulate every user action (Type 'A', Delete line, Bold text) as a Command object with `Execute()` and `Undo()` methods. Store these commands in a `History` stack.
*   **Memento**: For complex state changes where calculating the reverse operation is hard, use Memento to save a snapshot of the state before the command executes. The `Undo()` method effectively restores that snapshot.

[Back to Top](#table-of-contents)

### 30. Why is Singleton often considered an Anti-Pattern?

**Answer:**
Singleton is controversial and often avoided in modern development because:
1.  **Global State**: It introduces global state, which hides dependencies. A class using a Singleton hides the fact that it needs that resource, making the API definition dishonest.
2.  **Testing**: Singletons are hard to mock in unit tests. You can't easily swap out a real DatabaseSingleton with a MockDatabase for testing.
3.  **Tight Coupling**: It tightly couples code to a specific implementation.
*   **Alternative**: Use **Dependency Injection (DI)**. Register the class as a "Singleton" in the DI container (state lifetime management) and inject it via constructor. This keeps the class itself testable and loosely coupled.

[Back to Top](#table-of-contents)
