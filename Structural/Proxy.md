# Proxy Pattern

## Definition
The **Proxy Pattern** is a structural design pattern that lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

## Real-World Analogy
**Credit Card**: A credit card is a proxy for a bank account (and essentially, for a bundle of cash). You can use it to pay, and it ensures the money is transferred. It can also add security (PIN) and limits (credit limit) that carrying a sack of gold coins doesn't have.

## Structure
-   **Service Interface**: The interface of the Service. The Proxy must follow this interface to be able to masquerade as a service object.
-   **Service**: The class providing useful business logic.
-   **Proxy**: The class that wraps the service object. It intercepts calls to the service.

## Code Example (C#)

```csharp
// Service Interface
public interface IThirdPartyYouTubeLib
{
    string ListVideos();
    string GetVideoInfo(int id);
}

// Service
public class ThirdPartyYouTubeClass : IThirdPartyYouTubeLib
{
    public string ListVideos()
    {
        Thread.Sleep(1000); // Simulate network latency
        return "Video List Data";
    }
    public string GetVideoInfo(int id) => "Video Info";
}

// Proxy (Caching Proxy)
public class CachedYouTubeClass : IThirdPartyYouTubeLib
{
    private ThirdPartyYouTubeClass _service;
    private string _listCache;
    private bool _needReset;

    public CachedYouTubeClass(ThirdPartyYouTubeClass service)
    {
        _service = service;
    }

    public string ListVideos()
    {
        if (_listCache == null || _needReset)
        {
            Console.WriteLine("Proxy: Fetching new list from YouTube...");
            _listCache = _service.ListVideos();
        }
        else
        {
            Console.WriteLine("Proxy: Retrieving list from cache...");
        }
        return _listCache;
    }

    public string GetVideoInfo(int id) => _service.GetVideoInfo(id);
}
```

## Pros & Cons
### Pros
-   **Control**: You can control the service object without clients knowing about it.
-   **Lifecycle Management**: You can manage the lifecycle of the service object when clients don't care about it.
-   **Security**: Can enforce access permissions.

### Cons
-   **Latency**: The response from the service might be delayed.
-   **Complexity**: Introduces another layer of abstraction.

---

### Java Example

```java
interface YouTubeLib {
    String listVideos();
}

class YouTubeService implements YouTubeLib {
    public String listVideos() { return "List from Network"; }
}

class CachedYouTubeProxy implements YouTubeLib {
    private YouTubeService service;
    private String listCache;

    public CachedYouTubeProxy(YouTubeService service) {
        this.service = service;
    }

    public String listVideos() {
        if (listCache == null) {
            listCache = service.listVideos();
        }
        return listCache;
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

interface IYouTube {
    virtual std::string listVideos() = 0;
    virtual ~IYouTube() {}
};

class YouTubeService : public IYouTube {
public:
    std::string listVideos() override { return "Network List"; }
};

class CachedProxy : public IYouTube {
private:
    YouTubeService* service;
    std::string cache;
    bool hasCache = false;

public:
    CachedProxy(YouTubeService* s) : service(s) {}

    std::string listVideos() override {
        if (!hasCache) {
            cache = service->listVideos();
            hasCache = true;
        }
        return cache;
    }
};
```
