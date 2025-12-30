# Design Patterns: C# vs Java vs C++

This document highlights key differences in implementing standard design patterns across these three languages. While the abstract concepts remain the same, language-specific features (Memory Management, Threading, Syntax) dictate the implementation details.

## 1. Singleton Pattern

### Thread Safety implementation
*   **C#**: Uses the `lock` statement (syntactic sugar for `Monitor.Enter/Exit`) or `Lazy<T>`.
*   **Java**: Uses the `synchronized` keyword on the method or block. Old versions used double-checked locking with `volatile`.
*   **C++**: Uses `std::mutex` from `<mutex>` for thread safety. Since C++11, a static local variable is guaranteed to be thread-safe ("Magic Statics"), making the pattern trivial.

| Feature | C# | Java | C++ |
| :--- | :--- | :--- | :--- |
| **Locking** | `lock(_object)` | `synchronized(class)` | `std::lock_guard<std::mutex>` |
| **Lazy Loading** | `Lazy<T>` | Manual null check | Static local variable (C++11+) |

## 2. Observer Pattern

### Event Handling Mechanisms
*   **C#**: Has built-in support via `event` and `delegate` keywords. You don't always need a custom `IObserver` interface; you can just expose an `OnChange` event.
*   **Java**: Historically relied on `java.util.Observer` (now deprecated) or custom interfaces (`PropertyChangeListener`).
*   **C++**: Typically requires manual implementation using `std::vector<std::function<...>>` or abstract base classes.

### Memory Leaks (Lapsed Listener)
*   **C#**: If you forget `-=`, the subscriber stays alive. `WeakReference` or `WeakEventManager` can solve this.
*   **Java**: Similar to C#, requires explicit unsubscription. `WeakReference` is also available.
*   **C++**: No Garbage Collector. Use `std::weak_ptr` to hold observers. If the subject is destroyed, the weak pointer tells you it's gone.

## 3. Prototype Pattern (Cloning)

### Deep vs Shallow Copy
*   **C#**: `MemberwiseClone()` provides a shallow copy. `ICloneable` is standard but considered ambiguous (is it deep or shallow?).
*   **Java**: `Object.clone()` is a shallow copy. `Cloneable` interface is a marker interface.
*   **C++**: Copy Constructor and Copy Assignment Operator (`operator=`) are the standard way to define copy behavior. You have full control over deep copying via allocation (`new`).

## 4. Resource Management (Disposable Patterns)

### Cleaning up Resources
*   **C#**: `IDisposable` interface and `using` blocks. Critical for Proxies or Decorators holding file handles.
*   **Java**: `AutoCloseable` interface and `try-with-resources`.
*   **C++**: RAII (Resource Acquisition Is Initialization). Destructors (`~Class()`) automatically clean up resources when objects go out of scope. No "finally" block needed.

## 5. Generic/Template Implementation

### Factory/Builder Reuse
*   **C#**: Generics (`class Factory<T>`). Runtime type reification. Constraints `where T : new()`.
*   **Java**: Generics (`class Factory<T>`). Type erasure (generics don't exist at runtime). Cannot do `new T()`.
*   **C++**: Templates (`template <typename T>`). Compile-time code generation. More powerful ("duck typing" at compile time) but slower build times.

---
## Summary

*   **C#** offers the most "syntactic sugar" for patterns (Properties, Events, Lazy, LINQ).
*   **Java** offers a very standardized, interface-heavy approach.
*   **C++** offers the most control (memory, destruction) but requires manual handling of pointers and ownership (smart pointers).
