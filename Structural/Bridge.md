# Bridge Pattern

## Definition
The **Bridge Pattern** is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently.

## Real-World Analogy
**Remote Control**: The Remote Control (Abstraction) communicates with a TV (Implementation). You can have different remotes (Basic, Advanced) and different TVs (Sony, Samsung). The remote doesn't need to know the internal details of the TV, just the standard interface (turn on, turn off, set channel).

## Structure
-   **Abstraction**: Provides high-level control logic and relies on the implementation object to do the actual low-level work.
-   **Implementation**: Declares the interface for all implementation classes.
-   **Refined Abstraction**: Variants of the control logic.
-   **Concrete Implementation**: Specific implementations of the interface.

## Code Example (C#)

```csharp
// Implementor
public interface IDevice
{
    void Enable();
    void Disable();
    void SetVolume(int percent);
}

// Concrete Implementations
public class Radio : IDevice
{
    public void Enable() => Console.WriteLine("Radio: Enabled");
    public void Disable() => Console.WriteLine("Radio: Disabled");
    public void SetVolume(int percent) => Console.WriteLine($"Radio: Volume at {percent}");
}

public class TV : IDevice
{
    public void Enable() => Console.WriteLine("TV: Enabled");
    public void Disable() => Console.WriteLine("TV: Disabled");
    public void SetVolume(int percent) => Console.WriteLine($"TV: Volume at {percent}");
}

// Abstraction
public class RemoteControl
{
    protected IDevice _device;

    public RemoteControl(IDevice device)
    {
        _device = device;
    }

    public virtual void TogglePower()
    {
        Console.WriteLine("Remote: Power toggle");
        _device.Enable(); // Simplified toggle logic
    }
}

// Refined Abstraction
public class AdvancedRemoteControl : RemoteControl
{
    public AdvancedRemoteControl(IDevice device) : base(device) { }

    public void Mute()
    {
        Console.WriteLine("Remote: Mute");
        _device.SetVolume(0);
    }
}
```

## Pros & Cons
### Pros
-   **Decoupling**: Decouples interface from implementation.
-   **Extensibility**: You can extend the Abstraction and Implementation hierarchies independently.

### Cons
-   **Complexity**: The code might become overly complicated when applying this pattern to a highly cohesive class.

---

### Java Example

```java
// Implementor
interface Device {
    void enable();
    void disable();
    void setVolume(int percent);
}

class Radio implements Device {
    public void enable() { System.out.println("Radio: Enabled"); }
    public void disable() { System.out.println("Radio: Disabled"); }
    public void setVolume(int percent) { System.out.println("Radio: Volume at " + percent); }
}

// Abstraction
class RemoteControl {
    protected Device device;

    public RemoteControl(Device device) {
        this.device = device;
    }

    public void togglePower() {
        System.out.println("Remote: Power toggle");
        device.enable(); // Simplified
    }
}

// Refined Abstraction
class AdvancedRemoteControl extends RemoteControl {
    public AdvancedRemoteControl(Device device) {
        super(device);
    }
    
    public void mute() {
        System.out.println("Remote: Mute");
        device.setVolume(0);
    }
}
```

### C++ Example

```cpp
#include <iostream>

// Implementor
class Device {
public:
    virtual void enable() = 0;
    virtual void disable() = 0;
    virtual void setVolume(int percent) = 0;
    virtual ~Device() {}
};

class Radio : public Device {
public:
    void enable() override { std::cout << "Radio: Enabled" << std::endl; }
    void disable() override { std::cout << "Radio: Disabled" << std::endl; }
    void setVolume(int percent) override { std::cout << "Radio: Volume " << percent << std::endl; }
};

// Abstraction
class RemoteControl {
protected:
    Device* device;

public:
    RemoteControl(Device* dev) : device(dev) {}
    
    virtual void togglePower() {
        std::cout << "Remote: Power toggle" << std::endl;
        device->enable();
    }
    virtual ~RemoteControl() {}
};

// Refined Abstraction
class AdvancedRemoteControl : public RemoteControl {
public:
    AdvancedRemoteControl(Device* dev) : RemoteControl(dev) {}

    void mute() {
        std::cout << "Remote: Mute" << std::endl;
        device->setVolume(0);
    }
};
```
