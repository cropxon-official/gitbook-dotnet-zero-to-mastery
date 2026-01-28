---
description: >-
  Builder is a creational design pattern that lets you construct complex objects
  step by step. The pattern allows you to produce different types and
  representations of an object using the same construct
icon: memo-circle-check
---

# 🏗️ Builder Pattern

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Fixing_Complex_Object_Creation_With_the_Builder_Pattern.m4a" %}

### 🔹 1️⃣ What problem does it solve?

Sometimes objects are:

* complex
* created in multiple steps
* have many optional fields

Example:

```csharp
var user = new User("abhi", null, null, true, null, "India", null, null);
```

😵 unreadable\
😵 easy to pass values in wrong order\
😵 impossible to maintain

Builder fixes this by letting you **construct an object step-by-step**.

### 2️⃣ Real-life analogy

#### 🏠 Building a house

You don’t say:

> “Give me finished house in one instruction.”

Construction happens in steps:

1️⃣ foundation\
2️⃣ structure\
3️⃣ walls\
4️⃣ windows\
5️⃣ paint

Different builders → same process, different style.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Builder = organized construction process.

***

### ❌ 3️⃣ Without Builder (complex constructors)

```csharp
public class Computer
{
    public Computer(string cpu,
                    string ram,
                    string storage,
                    bool hasGraphics,
                    bool hasWifi,
                    string os)
    {
        // set properties
    }
}
```

Usage:

```csharp
var pc = new Computer("i7", "16GB", "1TB SSD", true, true, "Windows");
```

Problems:

❌ too many parameters\
❌ easy to mix values\
❌ adding more options breaks everything\
❌ readability = zero

<figure><img src="../.gitbook/assets/BuilderPattern.png" alt=""><figcaption></figcaption></figure>

## ✅ 4️⃣ Builder — Step by Step

Goal:

> _<mark style="color:blue;">Build complex objects gradually and clearly.</mark>_

***

#### Step 1 — Create product class

```csharp
public class Computer
{
    public string CPU { get; set; }
    public string RAM { get; set; }
    public string Storage { get; set; }
    public bool HasGraphics { get; set; }
    public string OS { get; set; }

    public override string ToString()
        => $"{CPU}, {RAM}, {Storage}, Graphics: {HasGraphics}, OS: {OS}";
}
```

***

#### Step 2 — Create Builder

```csharp
public class ComputerBuilder
{
    private readonly Computer _computer = new();

    public ComputerBuilder SetCPU(string cpu)
    {
        _computer.CPU = cpu;
        return this; // enables chaining
    }

    public ComputerBuilder SetRAM(string ram)
    {
        _computer.RAM = ram;
        return this;
    }

    public ComputerBuilder SetStorage(string storage)
    {
        _computer.Storage = storage;
        return this;
    }

    public ComputerBuilder AddGraphicsCard()
    {
        _computer.HasGraphics = true;
        return this;
    }

    public ComputerBuilder InstallOS(string os)
    {
        _computer.OS = os;
        return this;
    }

    public Computer Build() => _computer;
}
```

***

#### Step 3 — Client builds step-by-step

```csharp
var gamingPc = new ComputerBuilder()
                    .SetCPU("i9")
                    .SetRAM("32GB")
                    .SetStorage("2TB SSD")
                    .AddGraphicsCard()
                    .InstallOS("Windows 11")
                    .Build();

Console.WriteLine(gamingPc);
```

✔️ readable\
✔️ flexible\
✔️ safe

## 🧩 5️⃣ What just happened?

Instead of creating the object **all at once**, we:

✔️ guided creation\
✔️ enforced clear steps\
✔️ avoided long constructors\
✔️ made object easier to customize

Builder separates:

> **HOW we build** vs **WHAT we build**

***

## 💻 6️⃣ Real-world example — Building HTTP requests

Think about creating API requests:

```csharp
var request = new HttpRequestBuilder()
                  .WithUrl("/users")
                  .WithMethod("POST")
                  .WithHeader("Authorization", token)
                  .WithBody(data)
                  .Build();
```

This is how libraries like:

* ASP.NET HttpClient
* FluentValidation
* Entity Framework

design their APIs — using **Builder style**.

***

## 🎯 7️⃣ When to use Builder

Use when:

✔ object has many optional fields\
✔ object construction requires multiple steps\
✔ different configurations needed\
✔ readability matters\
✔ constructors feel heavy

Signs you need Builder:

> “constructor parameter list keeps growing”

***

## 🚫 8️⃣ When NOT to use Builder

Avoid when:

✘ object is simple\
✘ only 2–3 properties\
✘ no optional configuration

Otherwise, you over-engineer.

***

## 🧠 9️⃣ Principles followed

#### ✔ SRP — Single Responsibility

Object = represents data\
Builder = constructs object\
Separate responsibilities.

#### ✔ OCP — Open/Closed (indirectly)

You add new build steps\
→ without breaking existing clients.

***

{% file src="../.gitbook/assets/Builder_From_Chaos_to_Clarity_Building_with_the_Builder_Pattern.pdf" %}

{% file src="../.gitbook/assets/The_Builder_Pattern.mp4" %}

## 📝 10 Quick Summary

<table><thead><tr><th width="169.86328125">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Complex constructors, many optional values</td></tr><tr><td>Solution</td><td>Build step-by-step using a builder</td></tr><tr><td>Key idea</td><td>Separate object from construction process</td></tr><tr><td>Benefits</td><td>Readable, safe, extendable</td></tr><tr><td>Where used</td><td>APIs, configuration, object creation</td></tr><tr><td>Principles</td><td>SRP, OCP (partly)</td></tr></tbody></table>
