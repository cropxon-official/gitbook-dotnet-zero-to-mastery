---
description: >-
  Abstract Factory is a creational design pattern that lets you produce families
  of related objects without specifying their concrete classes.
icon: memo-circle-check
---

# 🏭 Abstract Factory Pattern

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Mastering_the_Abstract_Factory_Pattern.m4a" %}

### 🔹 1️⃣ What problem does it solve?

_Factory Method_ created **one object at a time**.

But sometimes we must create **groups of related objects together**.

Example:

Theme system needs consistent UI elements:

* Button
* Checkbox
* Textbox

#### ❌ Problem

Without Abstract Factory — developers may accidentally mix:

> Dark Button + Light Checkbox

Bad design. Inconsistent UI.

#### Goal:

> Create **families of related objects**\
> WITHOUT exposing creation logic\
> and WITHOUT breaking compatibility.

***

### 🧠 2️⃣ Real-life analogy

#### 🛋️ Furniture Store

You want to buy furniture for one room.

You don’t mix:

* Modern table + Vintage chair

You choose:

> “Give me a _Modern Set_.”

The shop gives:

* Modern Sofa
* Modern Bed
* Modern Table

Everything matches.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

That’s **Abstract Factory**.

***

### ❌ 3️⃣ Without Abstract Factory (tight coupling)

Developers may hard-code UI creation:

```csharp
public class Screen
{
    public void Render()
    {
        var button = new DarkButton();
        var checkbox = new LightCheckbox(); // mixed theme!
    }
}
```

Problems:

❌ Inconsistent UI\
❌ Strong coupling\
❌ Hard to switch themes\
❌ Violates OCP

***

<figure><img src="../.gitbook/assets/Abstract Factory.png" alt=""><figcaption></figcaption></figure>

## ✅ 4️⃣ Abstract Factory — Step-by-Step

Think of it as:

> _<mark style="color:blue;">Factory of Factories</mark>_

We won’t create **just one object**…\
We create **families of related objects**.

***

#### Step 1 — Define product interfaces

```csharp
public interface IButton
{
    void Render();
}

public interface ICheckbox
{
    void Render();
}
```

***

#### Step 2 — Implement variations

**Dark theme versions**

```csharp
public class DarkButton : IButton
{
    public void Render() => Console.WriteLine("Dark Button");
}

public class DarkCheckbox : ICheckbox
{
    public void Render() => Console.WriteLine("Dark Checkbox");
}
```

**Light theme versions**

```csharp
public class LightButton : IButton
{
    public void Render() => Console.WriteLine("Light Button");
}

public class LightCheckbox : ICheckbox
{
    public void Render() => Console.WriteLine("Light Checkbox");
}
```

***

#### Step 3 — Create Abstract Factory interface

This factory knows how to create related products.

```csharp
public interface IUIFactory
{
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}
```

***

#### Step 4 — Implement concrete factories

**Dark Factory**

```csharp
public class DarkUIFactory : IUIFactory
{
    public IButton CreateButton() => new DarkButton();
    public ICheckbox CreateCheckbox() => new DarkCheckbox();
}
```

**Light Factory**

```csharp
public class LightUIFactory : IUIFactory
{
    public IButton CreateButton() => new LightButton();
    public ICheckbox CreateCheckbox() => new LightCheckbox();
}
```

***

#### Step 5 — Client only talks to factory

```csharp
public class Screen
{
    private readonly IUIFactory _factory;

    public Screen(IUIFactory factory)
    {
        _factory = factory;
    }

    public void Render()
    {
        var button = _factory.CreateButton();
        var checkbox = _factory.CreateCheckbox();

        button.Render();
        checkbox.Render();
    }
}
```

Usage:

```csharp
var factory = new DarkUIFactory();
// var factory = new LightUIFactory();

var screen = new Screen(factory);
screen.Render();
```

👉 Switch the factory → entire theme changes\
👉 No code modification inside **`Screen`**

***

## 🧩 5️⃣ What just happened?

_Before_:

Client created each object manually.

_**Now**_:

Client requests from factory — factory ensures **compatibility**.

Abstract Factory:

✔ hides creation logic\
✔ guarantees objects work together\
✔ makes swapping families easy

## 💻 6️⃣ Real-world software uses

Abstract Factory is common in:

✔ UI Libraries\
✔ Cross-platform apps\
✔ Game engines (weapons, skins, UI packs)\
✔ Dependency injection configurations\
✔ Multi-tenant systems\
✔ Cloud provider wrappers

***

## 🎯 7️⃣ When to use Abstract Factory

Use when:

✔ you need **families of related objects**\
✔ you must guarantee compatibility\
✔ you want to switch variants easily\
✔ object creation shouldn’t be exposed

Typical signs:

* “dark theme / light theme”
* “Windows / Mac / Linux implementation”
* “Free / Premium version”
* “Dev / QA / Production setups”

***

## 🚫 8️⃣ When NOT to use

Avoid when:

✘ only one product type exists\
✘ you don’t need related families\
✘ it makes code over-engineered\
✘ factory adds unnecessary layers

Sometimes a simple **Factory Method** is enough.

***

## 🧠 9️⃣ Principles followed

#### ✔ SRP — Single Responsibility

Factories create objects.\
Clients just use them.

#### ✔ OCP — Open/Closed

Add a new theme?\
Create new factory — no modification needed.

#### ✔ DIP — Dependency Inversion

Client depends on **interfaces**, not concrete classes.

***

{% file src="../.gitbook/assets/Abstract_Factory_Pattern_Mastery.pdf" %}

{% file src="../.gitbook/assets/The_Abstract_Factory_Pattern.mp4" %}

## 📝 10. Quick Summary

<table><thead><tr><th width="143.67578125">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Need groups of related objects without mixing</td></tr><tr><td>Solution</td><td>Factory that creates whole families</td></tr><tr><td>Key Idea</td><td>“Factory of factories”</td></tr><tr><td>Benefits</td><td>Consistency, flexibility, loose coupling</td></tr><tr><td>Patterns Used</td><td>Abstract Factory + (sometimes) Factory Method</td></tr><tr><td>Principles</td><td>SRP, OCP, DIP</td></tr></tbody></table>
