# Facade Pattern

## Definition
The **Facade Pattern** is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.

## Real-World Analogy
**Phone Support**: When you call a company's support line, an operator ("Facade") answers. You tell them your problem. They might look up your account, transfer you to billing, or check with logistics. You don't have to call each of those departments directly; the operator handles the complexity of the internal system for you.

## Structure
-   **Facade**: Provides convenient access to a particular part of the subsystem's functionality. It knows where to direct the client's request and how to operate all the moving parts.
-   **Subsystem classes**: The complex objects that do the actual work. They are not aware of the facade.

## Code Example (C#)

```csharp
// Subsystem 1
public class VideoFile { public VideoFile(string filename) { } }

// Subsystem 2
public class OggCompressionCodec { }

// Subsystem 3
public class MPEG4CompressionCodec { }

// Subsystem 4
public class BitrateReader
{
    public static string Read(string filename, object codec) => "buffer";
    public static string Convert(string buffer, object codec) => "result";
}

// Facade
public class VideoConverter
{
    public string Convert(string filename, string format)
    {
        var file = new VideoFile(filename);
        var sourceCodec = new OggCompressionCodec();
        // ... complex logic to determine destination codec ...
        var destCodec = new MPEG4CompressionCodec(); 
        var buffer = BitrateReader.Read(filename, sourceCodec);
        var result = BitrateReader.Convert(buffer, destCodec);
        return result;
    }
}

// Client
public class Application
{
    public void Main()
    {
        var converter = new VideoConverter();
        // Simple interface hiding all the complexity above
        string mp4 = converter.Convert("funny-cats-video.ogg", "mp4");
    }
}
```

## Pros & Cons
### Pros
-   **Simplicity**: Simplifies the usage of a complex system for clients.
-   **Decoupling**: Decouples the subsystem from its clients and other subsystems.

### Cons
-   **God Object**: An app is prone to creating a "God Object" coupled to all classes of an app.

---

### Java Example

```java
class VideoFile { public VideoFile(String name) {} }
class MPEG4CompressionCodec {}

// Facade
class VideoConverter {
    public String convert(String filename, String format) {
        VideoFile file = new VideoFile(filename);
        System.out.println("Facade: Orchestrating conversion complex logic...");
        return "mp4_content";
    }
}

// Client
public class Application {
    public static void main(String[] args) {
        VideoConverter converter = new VideoConverter();
        converter.convert("vid.ogg", "mp4");
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

class VideoFile { public: VideoFile(std::string name) {} };
class MPEG4Codec {};

// Facade
class VideoConverter {
public:
    std::string convert(std::string filename, std::string format) {
        VideoFile file(filename);
        std::cout << "Facade: Orchestrating conversion logic..." << std::endl;
        return "mp4_content";
    }
};

// Client
int main() {
    VideoConverter converter;
    converter.convert("vid.ogg", "mp4");
    return 0;
}
```
