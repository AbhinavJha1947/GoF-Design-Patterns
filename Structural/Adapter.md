# Adapter Pattern

## Definition
The **Adapter Pattern** is a structural design pattern that allows objects with incompatible interfaces to collaborate. It acts as a wrapper between two objects.

## Real-World Analogy
**Travel Adapter**: You have a US laptop plug, but you are in Europe. The wall socket is different. You use a power adapter that accepts the US plug and fits into the European socket. The adapter translates the interface.

## Structure
-   **Target**: The interface that the client is expecting to use.
-   **Adaptee**: The class that acts as the source of data/behavior but has an incompatible interface.
-   **Adapter**: The wrapper class that implements the Target interface and wraps the Adaptee.

## Code Example (C#)

```csharp
// Target Interface
public interface ILightningPhone
{
    void Recharge();
    void UseLightning();
}

// Adaptee (Incompatible)
public class AndroidPhone
{
    public void RechargeUSB() => Console.WriteLine("Recharging Android via MicroUSB");
    public void UseMicroUSB() => Console.WriteLine("Using MicroUSB");
}

// Adapter
public class LightningToMicroUSBAdapter : ILightningPhone
{
    private readonly AndroidPhone _androidPhone;

    public LightningToMicroUSBAdapter(AndroidPhone androidPhone)
    {
        _androidPhone = androidPhone;
    }

    public void Recharge()
    {
        _androidPhone.RechargeUSB();
    }

    public void UseLightning()
    {
        Console.WriteLine("Adapter converts Lightning signal to MicroUSB...");
        _androidPhone.UseMicroUSB();
    }
}
```

## Pros & Cons
### Pros
-   **Single Responsibility Principle**: Separates the interface conversion code from the primary business logic.
-   **Open/Closed Principle**: You can introduce new types of adapters without breaking the existing code.

### Cons
-   **Complexity**: Increases the overall complexity of the code by creating additional classes. Sometimes it's simpler just to change the service class if possible.

---

### Java Example

```java
// Target
interface LightningPhone {
    void recharge();
    void useLightning();
}

// Adaptee
class AndroidPhone {
    public void rechargeUSB() { System.out.println("Recharging Android via MicroUSB"); }
    public void useMicroUSB() { System.out.println("Using MicroUSB"); }
}

// Adapter
class LightningToMicroUSBAdapter implements LightningPhone {
    private AndroidPhone androidPhone;

    public LightningToMicroUSBAdapter(AndroidPhone androidPhone) {
        this.androidPhone = androidPhone;
    }

    public void recharge() {
        androidPhone.rechargeUSB();
    }

    public void useLightning() {
        System.out.println("Adapter converts Lightning signal to MicroUSB...");
        androidPhone.useMicroUSB();
    }
}
```

### C++ Example

```cpp
#include <iostream>

// Target
class LightningPhone {
public:
    virtual void recharge() = 0;
    virtual void useLightning() = 0;
    virtual ~LightningPhone() {}
};

// Adaptee
class AndroidPhone {
public:
    void rechargeUSB() { std::cout << "Recharging Android via MicroUSB" << std::endl; }
    void useMicroUSB() { std::cout << "Using MicroUSB" << std::endl; }
};

// Adapter
class LightningToMicroUSBAdapter : public LightningPhone {
private:
    AndroidPhone* androidPhone;

public:
    LightningToMicroUSBAdapter(AndroidPhone* phone) : androidPhone(phone) {}

    void recharge() override {
        androidPhone->rechargeUSB();
    }

    void useLightning() override {
        std::cout << "Adapter converts Lightning signal to MicroUSB..." << std::endl;
        androidPhone->useMicroUSB();
    }
};
```
