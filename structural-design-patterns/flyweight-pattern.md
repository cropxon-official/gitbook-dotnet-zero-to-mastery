---
description: Flyweight saves memory by sharing reusable data between objects.
icon: memo-circle-info
---

# 🪶 Flyweight Pattern

<figure><img src="../.gitbook/assets/FlyWeight.png" alt=""><figcaption></figcaption></figure>

### 🔹 1️⃣ What problem does it solve?

Sometimes your app creates **millions of very similar objects**.

_**Example**_:

* game trees
* map tiles
* text characters
* UI icons
* product attributes

Most of those objects share the **same data**, like:

* color
* texture
* type
* style

But each object also has **small unique data**, like:

* position
* current state
* selection

If you duplicate all data per object → memory explodes 🚨

> Flyweight lets you **share common (reusable) data**\
> and store only unique data per object.

***

### 🧠 2️⃣ Real-life analogy

#### ✈️ Airlines seat map

On a plane:

* Seat model (shape, material, description) → SAME for many seats
* Seat number (12A, 12B, 13A…) → DIFFERENT

They **don’t build unique seats for every passenger**.

They reuse the model — and only attach a number.

That’s **Flyweight**.

***

### ❌ 3️⃣ Bad approach — duplicate everything

Example: representing trees in a game.

```csharp
public class Tree
{
    public string Type;
    public string Color;
    public string Texture;
    public int X;
    public int Y;
}
```

If we create **100,000 trees**, each contains:

* same Type
* same Color
* same Texture

Huge memory waste.

***

## ✅ 4️⃣ Correct Approach — Flyweight

Split the data:

#### ✔ Intrinsic (shared, reusable)

Doesn’t change across objects:

* Type
* Color
* Texture

#### ✔ Extrinsic (unique, external)

Changes per object:

* X, Y coordinates
* state

We store shared data ONCE,\
and pass unique data separately.

***

## 👨‍💻 5️⃣ Step-by-Step C# Example — Tree Rendering

#### Step 1 — Flyweight (shared object)

```csharp
public class TreeType
{
    public string Name;
    public string Color;
    public string Texture;

    public TreeType(string name, string color, string texture)
    {
        Name = name;
        Color = color;
        Texture = texture;
    }

    public void Draw(int x, int y)
    {
        Console.WriteLine($"Drawing {Name} at ({x},{y})");
    }
}
```

***

#### Step 2 — Flyweight Factory (ensures reuse)

```csharp
public static class TreeFactory
{
    private static readonly Dictionary<string, TreeType> _types = new();

    public static TreeType GetTreeType(string name, string color, string texture)
    {
        var key = $"{name}-{color}-{texture}";

        if (!_types.ContainsKey(key))
            _types[key] = new TreeType(name, color, texture);

        return _types[key];
    }
}
```

***

#### Step 3 — Lightweight object (stores only unique state)

```csharp
public class Tree
{
    private readonly int _x;
    private readonly int _y;
    private readonly TreeType _type;

    public Tree(int x, int y, TreeType type)
    {
        _x = x;
        _y = y;
        _type = type;
    }

    public void Draw()
        => _type.Draw(_x, _y);
}
```

***

#### Step 4 — Usage

```csharp
var oakType = TreeFactory.GetTreeType("Oak", "Green", "Rough");

var t1 = new Tree(10, 20, oakType);
var t2 = new Tree(15, 25, oakType);
var t3 = new Tree(20, 30, oakType);

t1.Draw();
t2.Draw();
t3.Draw();
```

✔ TreeType stored once\
✔ Each Tree stores only coordinates

Memory saved dramatically.

***

## 🏢 6️⃣ Real enterprise examples

#### ✔ Text editors

Every character shares:

* font style
* size
* formatting rules

Only position and content differ.

#### ✔ Maps & games

Millions of objects reuse:

* icons
* textures
* models

#### ✔ Product catalogs

Same product attributes shared across variants.

#### ✔ UI frameworks

Shared style sheets (CSS-like concepts)

***

## 📌 Applicability — When Should You Use Flyweight?

Use Flyweight when:

#### ✅ You have LOTS of similar objects

Millions of:

* tiles
* characters
* icons
* nodes

#### ✅ Most fields are identical

Shared data should be reused.

#### ✅ Memory usage becomes a problem

RAM grows fast → optimization needed.

<figure><img src="../.gitbook/assets/image (22).png" alt="" width="320"><figcaption></figcaption></figure>

***

#### ⚠️ When NOT to use

Avoid Flyweight when:

✘ small number of objects\
✘ objects differ too much\
✘ memory is not a real problem\
✘ added complexity outweighs benefits

Flyweight is an **optimization pattern** — use when needed.

***

## 🧠 Principles Used

#### ✔ SRP

Shared + unique data responsibilities separated.

#### ✔ OCP

We can add new shared types without rewriting code.

#### ✔ Memory optimization through sharing

(“Don’t duplicate what can be shared”)

***

## 📝 Quick Summary

<table><thead><tr><th width="124.8984375">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Too many similar objects waste memory</td></tr><tr><td>Solution</td><td>Share common data, store only unique state</td></tr><tr><td>Key Idea</td><td>Split intrinsic (shared) vs extrinsic (unique)</td></tr><tr><td>Benefits</td><td>Huge memory savings, better performance</td></tr><tr><td>Used In</td><td>Games, maps, text editors, UI frameworks</td></tr><tr><td>Principles</td><td>SRP, OCP, optimization via sharing</td></tr></tbody></table>

{% file src="../.gitbook/assets/Flyweight_Pattern_Outsmarting_Memory_Waste.pdf" %}

{% file src="../.gitbook/assets/Flyweight_Pattern.mp4" %}

