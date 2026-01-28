---
description: Creational patterns deal with HOW objects are created.
icon: layer-plus
---

# Creational Design Patterns

> **Creational patterns deal with HOW objects are created.**

Instead of blindly doing:

```csharp
var obj = new MyClass();
```

they help us:

✔️ control creation\
✔️ hide complexity\
✔️ avoid duplication\
✔️ avoid tight coupling\
✔️ make testing easier\
✔️ support future changes

They answer one big question:

#### 🧠 “Who should be responsible for creating objects — and how?”

***

## ❌ Problem if we DON'T use them

Imagine:

* **`new`** everywhere
* switching DB → everything breaks
* object construction becomes messy
* constructors have 10 parameters
* duplicated creation logic everywhere

Hard to extend\
Hard to test\
Risky to change

_Creational patterns solve exactly this._

***

## 🟡 Types of Creational Patterns (5)

According to **Gang of Four (GoF)**:

1️⃣ Factory Method\
2️⃣ Abstract Factory\
3️⃣ Builder\
4️⃣ Prototype\
5️⃣ Singleton

We’ll now understand each at a **high conceptual level**.

<figure><img src="../.gitbook/assets/Creational Patterns.png" alt=""><figcaption></figcaption></figure>

***

### 1️⃣ Factory Method — “Let subclasses decide what to create”

#### ✅ What

A method decides which object to return — NOT the caller.

#### ❓ Why

Avoid:

```csharp
if(type == "A") new A();
if(type == "B") new B();
```

Spread everywhere.

#### ⏰ When to use

When:

✔️ object type may change\
✔️ new types may be added later\
✔️ you want to hide creation logic

#### 🛠️ How (idea)

Create object inside a **factory method**, not using <mark style="color:blue;">**`new`**</mark> directly.

***

### 2️⃣ Abstract Factory — “Create families of related objects”

#### ✅ What

Produces **sets of related objects** that should work together.

Example:\
Theme Factory produces:

* Button
* Checkbox
* Textbox

#### ❓ Why

So you don’t accidentally mix incompatible objects.

#### ⏰ When to use

When:

✔️ UI themes\
✔️ cross-platform systems\
✔️ multiple product variations

#### 🛠️ How

Client talks to an interface → factory produces groups of related objects.

***

### 3️⃣ Builder — “Build object step by step”

#### ✅ What

Constructs complex objects using small steps.

#### ❓ Why

Avoid:

```csharp
var user = new User("A", null, null, "B", null, true, null);
```

Hard to read, hard to maintain.

#### ⏰ When to use

When:

✔️ object has many optional fields\
✔️ creation requires multiple steps\
✔️ different configurations required

#### 🛠️ How

Separate:

* **construction process**
* **final object**

Let builder assemble gradually.

***

### 4️⃣ Prototype — “Clone instead of re-creating”

#### ✅ What

Create new objects by **copying an existing object**.

#### ❓ Why

Creating from scratch may be:

* expensive
* slow
* complicated

#### ⏰ When to use

When:

✔️ object construction is costly\
✔️ many similar copies needed\
✔️ structure already exists

#### 🛠️ How

Provide a `Clone()` method that returns a copy.

***

### 5️⃣ Singleton — “Only ONE instance globally”

#### ✅ What

Guarantees:

✔️ single instance\
✔️ global access

#### ❓ Why

Some components should exist only once:

* configuration
* logger
* caching engine

#### ⏰ When to use

When:

✔️ one central object controls something\
✔️ multiple instances cause problems

#### 🛠️ How

Private constructor + controlled static instance.

***

## 🧠 Quick Analogy View

| Pattern          | Analogy                     |
| ---------------- | --------------------------- |
| Factory Method   | Ordering specific coffee    |
| Abstract Factory | Buying full furniture set   |
| Builder          | Building house step by step |
| Prototype        | Photocopying a document     |
| Singleton        | Only one Prime Minister     |

***

## 🎯 Big Picture — When to Think About Creational Patterns?

Ask yourself:

#### ❓ “Is object creation becoming messy or scattered?”

Think creational when:

✔️ too many constructors\
✔️ many variations of same object\
✔️ need conditional creation\
✔️ testing is difficult\
✔️ want to hide creation logic\
✔️ many objects share common structure

***

## 🧩 Summary — One Look Table

<table><thead><tr><th width="147.49609375">Pattern</th><th>What</th><th>Why</th><th>When</th><th>Core Idea</th></tr></thead><tbody><tr><td>Factory Method</td><td>Method creates objects</td><td>Avoid if/else + new everywhere</td><td>Multiple variations</td><td>Move creation into factory</td></tr><tr><td>Abstract Factory</td><td>Create related families</td><td>Keep objects compatible</td><td>Themes, product families</td><td>Factory of factories</td></tr><tr><td>Builder</td><td>Step-by-step creation</td><td>Constructors too complex</td><td>Many optional fields</td><td>Separate build process</td></tr><tr><td>Prototype</td><td>Clone objects</td><td>Save time</td><td>Many similar objects</td><td>Copy instead of recreate</td></tr><tr><td>Singleton</td><td>One instance only</td><td>Centralized control</td><td>Shared services</td><td>Private constructor + static instance</td></tr></tbody></table>
