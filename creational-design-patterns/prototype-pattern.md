---
description: >-
  Prototype is a creational design pattern that lets you copy existing objects
  without making your code dependent on their classes.
icon: memo-circle-check
---

# 🧬 Prototype Pattern

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Prototype_Pattern_Deep_Copy_Risks.m4a" %}

### 🔹 1️⃣ What problem does it solve?

Sometimes creating objects is:

* expensive
* slow
* complex
* requires lots of setup logic

Example:

* loading images
* parsing configuration files
* constructing complex graphs of objects
* expensive DB lookups

Instead of **re-creating** the object every time…

> We make one “_template_”\
> …and clone copies from it.

***

### 🧠 2️⃣ Real-life analogy

#### 📄 Photocopy / Xerox

You write an application form once.

Instead of writing it again and again,\
you:

> put it in the copier → get as many copies as you want.

Same layout, structure — you just change some fields.

That is **Prototype**.

### ❌ 3️⃣ Without Prototype (rebuilding again and again)

Example: creating employees with default benefits.

```csharp
public class Employee
{
    public string Role;
    public string Department;
    public List<string> Permissions;

    public Employee(string role, string department)
    {
        Role = role;
        Department = department;
        // heavy logic
        Permissions = LoadPermissions(role);
    }
}
```

Usage:

```csharp
var dev1 = new Employee("Developer", "IT");
var dev2 = new Employee("Developer", "IT");
var dev3 = new Employee("Developer", "IT");
```

Each time:

❌ loads permissions again\
❌ repeats heavy logic\
❌ wastes CPU

***

<figure><img src="../.gitbook/assets/Prototype Pattern.png" alt=""><figcaption></figcaption></figure>

## ✅ 4️⃣ With Prototype — Step-by-Step

**Goal**:

> _Create a base object, then clone it._

***

#### Step 1 — Create Prototype interface

```csharp
public interface IPrototype<T>
{
    T Clone();
}
```

***

#### Step 2 — Implement cloning

```csharp
public class Employee : IPrototype<Employee>
{
    public string Role;
    public string Department;
    public List<string> Permissions = new();

    public Employee(string role, string department)
    {
        Role = role;
        Department = department;
    }

    public Employee Clone()
    {
        return (Employee)this.MemberwiseClone();
    }
}
```

***

#### Step 3 — Use prototype

```csharp
var baseDev = new Employee("Developer", "IT");

var dev1 = baseDev.Clone();
var dev2 = baseDev.Clone();
var dev3 = baseDev.Clone();
```

Fast. Simple. Efficient.

## 🔍 5️⃣ Shallow copy vs Deep copy

Very important.

#### ✔ Shallow Copy (default MemberwiseClone)

Copies values only — BUT references still point to same object.

Example:

```csharp
var dev1 = baseDev.Clone();
dev1.Permissions.Add("NewAccess");
```

Both objects now share same list — bad.

#### ✔ Deep Copy

We manually duplicate nested objects.

```csharp
public Employee Clone()
{
    var copy = (Employee)this.MemberwiseClone();
    copy.Permissions = new List<string>(Permissions);
    return copy;
}
```

Now each clone has its own list.

***

## 💻 6️⃣ Real-world uses of Prototype

Used heavily in:

✔ Game engines\
✔ GUI editors (duplicate shapes)\
✔ Document editors (copy/paste objects)\
✔ ORM & caching systems\
✔ Prototyping test data quickly

Example: drawing tools

* copy rectangle
* change color
* resize
* move

Prototype is perfect there.

***

## 🎯 7️⃣ When to use Prototype

Use it when:

✔ object creation is expensive\
✔ you repeatedly create similar objects\
✔ many variations share same base state\
✔ copying is cheaper than rebuilding

***

## 🚫 8️⃣ When NOT to use

Avoid when:

✘ object is small/simple\
✘ cloning logic becomes too complex\
✘ object has many deep references that are hard to duplicate\
✘ immutability is preferred

Sometimes **Builder** or **Factory** is better.

***

## 🧠 9️⃣ Principles followed

#### ✔ SRP — Single Responsibility

Object handles its own cloning logic.

#### ✔ OCP — Open/Closed

Add new types\
→ override clone method\
→ no change elsewhere.

***

{% file src="../.gitbook/assets/Prototype_Pattern_An_Efficiency_Blueprint.pdf" %}

{% file src="../.gitbook/assets/The_Prototype_Pattern.mp4" %}

## 📝 10 Quick Summary

| Item       | Meaning                                |
| ---------- | -------------------------------------- |
| Problem    | Re-creating heavy objects repeatedly   |
| Solution   | Clone an existing base object          |
| Key Idea   | Copy instead of rebuild                |
| Benefits   | Faster, reusable, avoids duplication   |
| Risk       | Be careful with deep vs shallow copies |
| Principles | SRP, OCP                               |
