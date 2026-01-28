---
description: Proxy controls access to another object through a stand-in.
icon: memo-circle-info
---

# 🛡️ Proxy Pattern

<figure><img src="../.gitbook/assets/Proxy Pattern.png" alt=""><figcaption></figcaption></figure>

### 🔹 1️⃣ What problem does it solve?

Sometimes we need to control **access** to an object.

Because the real object may be:

* expensive (network / DB / API)
* remote (microservice / cloud)
* sensitive (security / permissions)
* resource-heavy (file loading, images)

But clients shouldn’t know the complexity.

> _**Proxy Pattern**_ acts as a **stand-in object** that controls access to the real object.

***

### 🧠 2️⃣ Real-life analogy

#### 💳 Credit Card vs Cash

You don’t always carry stacks of cash.

Instead you use a **credit card**:

* card checks balance
* authorizes transaction
* logs payment

The bank holds the real money.

Card = **Proxy**\
Bank = **Real Object**

You still _pay_, but indirectly.

***

### ❌ 3️⃣ Bad Approach — client calls everything directly

Example: calling a remote video stream:

```csharp
var video = new RealVideo("movie.mp4");
video.Play();
```

But:

* loading video is slow
* might not be needed
* heavy memory usage

Client has no control.

❌ no lazy load\
❌ no logging\
❌ no restrictions\
❌ tight coupling

***

## ✅ 4️⃣ Correct Approach — Proxy

> Create an object that looks like the real one…\
> but adds extra control before delegating calls.

Proxy can:

✔ lazy-load\
✔ cache\
✔ log\
✔ secure\
✔ validate\
✔ control access

Client still uses the **same interface**.

***

## 👨‍💻 5️⃣ Step-by-Step C# Example (Virtual Proxy — Lazy Loading)

We’ll simulate loading a heavy video file.

#### Step 1 — Common interface

```csharp
public interface IVideo
{
    void Play();
}
```

***

#### Step 2 — Real object (heavy)

```csharp
public class RealVideo : IVideo
{
    private readonly string _fileName;

    public RealVideo(string fileName)
    {
        _fileName = fileName;
        Console.WriteLine("Loading video from disk...");
    }

    public void Play()
        => Console.WriteLine($"Playing {_fileName}");
}
```

Loading happens immediately — heavy!

***

#### Step 3 — Proxy (delays creation until needed)

```csharp
public class VideoProxy : IVideo
{
    private readonly string _fileName;
    private RealVideo _video;

    public VideoProxy(string fileName)
    {
        _fileName = fileName;
    }

    public void Play()
    {
        if (_video == null)
            _video = new RealVideo(_fileName);   // load only when needed

        _video.Play();
    }
}
```

***

#### Step 4 — Client uses proxy normally

```csharp
IVideo video = new VideoProxy("movie.mp4");

Console.WriteLine("Proxy created.");
video.Play();   // video loads only now!
```

✔ same interface\
✔ lazy load\
✔ better performance

***

## 🏢 6️⃣ Real Enterprise Use Cases

#### ✔ Security Proxy

Check permissions before access:

* admin-only dashboards
* secure APIs
* sensitive operations

#### ✔ Remote Proxy

Communicate with remote services:

* microservices
* gRPC
* REST APIs

#### ✔ Caching Proxy

Store results to avoid repeated expensive calls:

* product API cache
* weather service cache

#### ✔ Logging/Monitoring Proxy

Track requests transparently.

#### ✔ Virtual/Lazy Proxy

Load expensive object only when needed.

***

## 📌 Applicability — When should you use Proxy?

Use Proxy when you need:

#### ✅ 1. Access control

Only certain users/functions allowed.

#### ✅ 2. Lazy loading

Object is expensive — create when needed.

#### ✅ 3. Logging / Monitoring

Track all calls transparently.

#### ✅ 4. Caching

Reuse results to avoid repeated expensive work.

#### ✅ 5. Remote communication

Object lives on another server.

> If you want to **control how or when a real object is accessed**, Proxy fits.

***

### ⚠️ When NOT to use

Avoid Proxy if:

✘ direct access is simple and cheap\
✘ adds unnecessary complexity\
✘ transparency isn’t needed\
✘ performance overhead becomes worse

Don’t add layers “just because”.

***

## 🧠 Principles Followed

#### ✔ SRP

Proxy handles access — real object handles core logic.

#### ✔ OCP

Add new proxy behavior without modifying real class.

#### ✔ DIP

Client depends on interfaces, not concrete objects.

***

## 📝 Quick Summary

| Item       | Meaning                                  |
| ---------- | ---------------------------------------- |
| Problem    | Need control over access to an object    |
| Solution   | Proxy stands in front of real object     |
| Key Idea   | Same interface — extra control logic     |
| Benefits   | Lazy loading, caching, security, logging |
| Used In    | APIs, DBs, remote calls, secure apps     |
| Principles | SRP, OCP, DIP                            |

{% file src="../.gitbook/assets/The_Proxy_Pattern_Controlled_Access.pdf" %}

{% file src="../.gitbook/assets/Proxy__Control_Access.mp4" %}

