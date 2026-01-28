---
description: >-
  Composite is a structural design pattern that lets you compose objects into
  tree structures and then work with these structures as if they were individual
  objects.
icon: memo-circle-info
---

# 🌳 Composite Pattern

<figure><img src="../.gitbook/assets/Composite pattern.png" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Composite_Pattern_Uniformly_Handles_Hierarchies.m4a" %}

### <i class="fa-frown">:frown:</i> Problem <a href="#problem" id="problem"></a>

Using the Composite pattern makes sense only when the core model of your app can be represented as a tree.

For example, imagine that you have two types of objects: `Products` and `Boxes`. A `Box` can contain several `Products` as well as a number of smaller `Boxes`. These little `Boxes` can also hold some `Products` or even smaller `Boxes`, and so on.

Say you decide to create an ordering system that uses these classes. Orders could contain simple products without any wrapping, as well as boxes stuffed with products...and other boxes. How would you determine the total price of such an order?

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>An order might comprise various products, packaged in boxes, which are packaged in bigger boxes and so on. The whole structure looks like an upside down tree.</p></figcaption></figure>

You could try the direct approach: unwrap all the boxes, go over all the products and then calculate the total. That would be doable in the real world;&#x20;

but in a program, it’s not as simple as running a loop. You have to know the classes of `Products` and `Boxes` you’re going through, the nesting level of the boxes and other nasty details beforehand. All of this makes the direct approach either too awkward or even impossible.

### <i class="fa-smile-beam">:smile-beam:</i> Solution <a href="#solution" id="solution"></a>

The Composite pattern suggests that you work with `Products` and `Boxes` through a common interface which declares a method for calculating the total price.

How would this method work? For a product, it’d simply return the product’s price. For a box, it’d go over each item the box contains, ask its price and then return a total for this box. If one of these items were a smaller box, that box would also start going over its contents and so on, until the prices of all inner components were calculated. A box could even add some extra cost to the final price, such as packaging cost.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption><p>The Composite pattern lets you run a behavior recursively over all components of an object tree.</p></figcaption></figure>

### 🔹 1️⃣ What problem does it solve?

Sometimes we need to work with **individual objects** and **groups of objects** in the SAME WAY.

Examples:

* File system (file vs folder)
* Organization (employee vs manager)
* UI elements (button vs container)

Without Composite, you end up writing duplicate logic:

```csharp
if (item is File)
   open file
else if (item is Folder)
   loop children
```

Too many condition checks ❌\
Hard to maintain ❌

> **Composite Pattern** lets you treat **single objects** and **collections of objects** uniformly.

***

### 🧠 2️⃣ Real-life analogy

#### 🗂️ Folders & Files

* File → single item
* Folder → contains files AND folders

But you can:

* delete
* move
* copy

both using the SAME actions.

System doesn’t care:

> “file or folder?”

It treats them the same.

That’s **Composite**.

***

### ❌ 3️⃣ Bad approach — lots of if/else

```csharp
public void Show(Node node)
{
    if (node is File)
        Console.WriteLine("Opening file");

    if (node is Folder)
        foreach (var child in ((Folder)node).Children)
            Show(child);
}
```

Problems:

❌ special case logic everywhere\
❌ tight coupling\
❌ violates OCP

***

## ✅ 4️⃣ Correct Approach — Composite

We create:

✔ Common interface\
✔ Leaf (single object)\
✔ Composite (group object)

Both implement the SAME interface.

***

## 👨‍💻 5️⃣ Step-by-step C# Example — File System

#### Step 1 — Component interface

```csharp
public interface IFileSystemItem
{
    void Display();
}
```

***

#### Step 2 — Leaf (File)

```csharp
public class FileItem : IFileSystemItem
{
    private readonly string _name;

    public FileItem(string name)
    {
        _name = name;
    }

    public void Display()
        => Console.WriteLine($"File: {_name}");
}
```

***

#### Step 3 — Composite (Folder)

```csharp
public class Folder : IFileSystemItem
{
    private readonly string _name;
    private readonly List<IFileSystemItem> _children = new();

    public Folder(string name)
    {
        _name = name;
    }

    public void Add(IFileSystemItem item)
        => _children.Add(item);

    public void Display()
    {
        Console.WriteLine($"Folder: {_name}");

        foreach (var item in _children)
            item.Display();
    }
}
```

***

#### Step 4 — Client uses everything the SAME way

```csharp
var root = new Folder("Root");

root.Add(new FileItem("readme.txt"));

var images = new Folder("Images");
images.Add(new FileItem("logo.png"));
images.Add(new FileItem("banner.jpg"));

root.Add(images);

root.Display();
```

✔ uniform operations\
✔ no if-else\
✔ clean structure

***

## 🏢 6️⃣ Real enterprise examples

Composite is used anywhere hierarchies exist:

✔ Organization structures\
✔ Menus in applications\
✔ DOM tree in UI frameworks\
✔ Product categories\
✔ Workflow trees\
✔ Game object hierarchies

Example: _**Organization**_

* CEO
  * Manager
    * Developer
    * QA

Same operations across all.

***

## 📌 Applicability — When Should You Use Composite?

Use Composite when:

#### ✅ 1. You have hierarchical tree structures

Examples:

* Files & folders
* Categories & subcategories
* UI containers & controls

***

#### ✅ 2. You want to treat single objects and groups uniformly

So client doesn’t care whether it deals with:

✔ one object\
✔ a whole structure

***

#### ✅ 3. You want to avoid condition checks everywhere

Instead of:

```csharp
if (item is Leaf) ...
if (item is Composite) ...
```

everything implements the same interface.

***

#### ❗ Bonus remark

This is why people often confuse Composite with:

* **Decorator** (also wraps objects)
* **Iterator** (also works with collections)

But intent is different:

> Composite = **tree structure** where _**leaf + group behave the same.**_

***

## 🧠 Principles Used

#### ✔ SRP

Each class has clear responsibility.

#### ✔ OCP

Add new entity types (new Leafs/Composites) without modifying client code.

#### ✔ Polymorphism

Client works with interface — not concrete types.

***

## 📝 Quick Summary

<table><thead><tr><th width="145.1015625">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Need to handle single + group objects the same way</td></tr><tr><td>Solution</td><td>Build tree structure with common interface</td></tr><tr><td>Key Idea</td><td>Leaf and Composite share same operations</td></tr><tr><td>Benefits</td><td>Cleaner code, less if/else, easier to extend</td></tr><tr><td>Used In</td><td>Menus, org charts, files, UI, categories</td></tr><tr><td>Principles</td><td>SRP, OCP, Polymorphism</td></tr></tbody></table>

{% file src="../.gitbook/assets/The_Composite_Pattern_Taming_Hierarchies.pdf" %}

{% file src="../.gitbook/assets/Composite__One_and_Many.mp4" %}
