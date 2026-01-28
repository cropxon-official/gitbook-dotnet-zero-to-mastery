---
description: >-
  Singleton is a creational design pattern that lets you ensure that a class has
  only one instance, while providing a global access point to this instance.
icon: memo-circle-check
---

# 👑 Singleton Pattern

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Singleton_Anti-Pattern_or_Must-Have.m4a" %}

### 🔹 1️⃣ What problem does it solve?

Sometimes we need:

✔ exactly **one instance** of a class\
✔ shared across the entire application

Example objects:

* configuration manager
* logger
* caching engine
* database connection manager
* application settings
* service registry

If multiple instances exist:

❌ inconsistent state\
❌ more memory usage\
❌ conflicting data\
❌ connection overload

Singleton ensures:

> _“Create only ONE instance —_\
> _and provide global access to it.”_

***

### 🧠 2️⃣ Real-life analogy

#### 🏛️ Prime Minister (or President)

A country doesn’t elect:

* 2 Prime Ministers
* 3 Prime Ministers

There is **one leader at a time**.

Everyone refers to the same person.

That’s Singleton.

_**Another analogy:**_

#### 🛎️ Hotel Reception Desk

One reception desk serves the entire hotel.

Guests don’t create new desks.

### ❌ 3️⃣ Without Singleton

Imagine logging:

```csharp
public class Logger
{
    public void Log(string msg)
    {
        Console.WriteLine(msg);
    }
}
```

Usage:

```csharp
var log1 = new Logger();
var log2 = new Logger();
var log3 = new Logger();
```

Problem:

❌ multiple logger instances\
❌ harder to manage\
❌ duplicate memory usage\
❌ inconsistent log formatting

We want **ONE shared logger**.

***

## ✅ 4️⃣ Singleton — Step-by-Step Implementation

We want:

✔ private constructor\
✔ static instance\
✔ global access method

***

#### Step 1 — private constructor

```csharp
public sealed class Logger
{
    private Logger() {}   // no one can create instances
}
```

***

#### Step 2 — static instance field

```csharp
public sealed class Logger
{
    private static Logger _instance;

    private Logger() {}
}
```

***

#### Step 3 — global access method

```csharp
public sealed class Logger
{
    private static Logger _instance;
    private Logger() {}

    public static Logger Instance
    {
        get
        {
            if (_instance == null)
                _instance = new Logger();

            return _instance;
        }
    }

    public void Log(string message)
        => Console.WriteLine(message);
}
```

Usage:

```csharp
Logger.Instance.Log("App started");
```

Whenever we call `Instance` → same object is returned.

***

## ⚠️ Thread-Safety Issue

Above version has issue in **multi-threaded systems**:

Two threads may create two instances at the same time.

We fix using **Lazy initialization**.

## 🔐 Thread-Safe Singleton (Best C# way)

#### ✔ Recommended approach using Lazy\<T>

```csharp
public sealed class Logger
{
    private static readonly Lazy<Logger> _instance =
        new(() => new Logger());

    public static Logger Instance => _instance.Value;

    private Logger() {}

    public void Log(string message)
        => Console.WriteLine(message);
}
```

Why this is best:

✔ thread-safe\
✔ lazy loaded\
✔ simple\
✔ no manual locking\
✔ fast

Usage:

```csharp
Logger.Instance.Log("Singleton works!");
```

***

## 💻 Real-World Where Singleton is Used

✔ Logging

✔ App Configuration

```csharp
ConfigurationManager.AppSettings["DbConnection"]
```

✔ Caching

✔ Service locator (DI containers internally)

✔ Shared connection pool

✔ Game engines (single game manager)

***

## 🎯 When to use Singleton

Use Singleton when:

✔ Only one instance should exist\
✔ Instance is expensive to create\
✔ Instance must be shared globally\
✔ Same state must be reused

Examples:

* App Settings
* Logging
* Caching
* Feature flags
* Device manager
* Thread pools

***

## 🚫 When NOT to use Singleton

Many developers OVERUSE Singleton.

**Avoid when:**

✘ multiple instances are valid\
✘ object has different configurations\
✘ you are hiding global state\
✘ replacing dependency injection improperly\
✘ unit testing becomes harder

**Singleton can create:**

⚠ tight coupling\
⚠ hidden dependencies\
⚠ global shared state bugs

Prefer **Dependency Injection** when possible.

***

## 🧠 Principles followed

#### ✔ SRP (somewhat)

Singleton controls only creation.

#### ✔ OCP

Singleton behavior can evolve without breaking other code.

#### ❌ BUT — DIP is often violated

Because global access can act like a hidden dependency.

So use carefully.

***

<figure><img src="../.gitbook/assets/Singleton Patterns.png" alt=""><figcaption></figcaption></figure>

## 📝 Final Summary

<table><thead><tr><th width="129.953125">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>You need exactly one instance globally</td></tr><tr><td>Solution</td><td>Private constructor + controlled static instance</td></tr><tr><td>Goal</td><td>Shared state, controlled access</td></tr><tr><td>Benefits</td><td>Memory control, consistency, centralized state</td></tr><tr><td>Risks</td><td>Tight coupling, hidden dependencies, testing difficulty</td></tr><tr><td>Best Practice</td><td>Use Lazy + thread-safe Singleton</td></tr></tbody></table>

{% file src="../.gitbook/assets/The_Singleton_Pattern_A_Beacon_in_Your_Architecture.pdf" %}

{% file src="../.gitbook/assets/The_Singleton_Pattern.mp4" %}
