---
description: 🔹 Creational Patterns | 🔹 Structural Patterns | 🔹 Behavioral Patterns
icon: book-open
---

# Main Categories (Gang of Four)

<figure><img src=".gitbook/assets/GOF.png" alt=""><figcaption></figcaption></figure>

## 🟦 Big Picture: The 3 Categories

Design patterns are grouped into three major families:

1️⃣ **Creational** – how objects are created\
2️⃣ **Structural** – how objects are organized or combined\
3️⃣ **Behavioral** – how objects communicate and behave

***

## 1️⃣ Creational Patterns

> **Focus: how objects get created.**

#### 🟡 WHAT

They solve problems related to:

* object creation
* object initialization
* controlling how many objects exist
* avoiding tight coupling when we create objects

#### ❓ WHY

Without creational patterns:

* you write **`new`** everywhere
* code becomes tightly coupled
* difficult to swap implementations
* difficult to test

#### 🛠️ THE WAY (core idea)

> **“Hide object creation behind a flexible mechanism.”**

You don’t expose _how_ the object is created — only _what_ it does.

#### ⏰ WHEN TO THINK ABOUT THEM

Think of creational patterns when you notice:

✔️ too many **`new ClassName()`** everywhere\
✔️ object creation logic becoming complex\
✔️ you might need different types later\
✔️ testing is hard because creation is fixed

#### 🧩 Examples (names only for now)

* **Factory Method**
* **Abstract Factory**
* **Builder**
* **Prototype**
* **Singleton**

#### 🏠 REAL-LIFE ANALOGY

_Ordering pizza:_

You don’t enter kitchen and cook.

You just say:

> **“I want a Margherita.”**

The restaurant decides:

* where dough comes from
* how toppings added
* which oven used

That’s **Factory**.

Another analogy:

A hotel gives only **one reception desk** → Singleton.

***

## 2️⃣ Structural Patterns

> **Focus: how objects are arranged and connected.**

#### 🟡 WHAT

They solve:

* object relationships
* combining classes
* simplifying complex structures
* adapting incompatible systems

#### ❓ WHY

Without structural patterns, systems become:

* over-complicated
* deeply nested
* difficult to modify
* hard to understand

#### 🛠️ THE WAY (core idea)

> “Organize objects to make systems flexible and easier to maintain.”

Often they wrap or combine objects instead of modifying them.

#### ⏰ WHEN TO THINK ABOUT THEM

Consider structural patterns when:

✔️ classes feel tightly coupled\
✔️ you need to add features without rewriting code\
✔️ you want to expose a simpler interface\
✔️ you must integrate third-party libraries

#### 🧩 Examples

* **Adapter**
* **Facade**
* **Decorator**
* **Composite**
* **Bridge**
* **Proxy**

#### 🏗️ REAL-LIFE ANALOGY

Phone charger adapter:

Indian plug → U.S socket

You don’t change the wall or device.\
You add **Adapter** in between.

Another analogy:

Remote control vs TV system.

Remote gives simple buttons while inside there is complex circuitry.

That is **Facade**.

***

## 3️⃣ Behavioral Patterns

> **Focus: how objects communicate and behave together.**

#### 🟡 WHAT

They solve:

* communication rules
* responsibilities
* workflow
* object collaboration

#### ❓ WHY

Without behavioral patterns:

* logic spreads everywhere
* code becomes unpredictable
* too many if/else chains
* changing behavior breaks things

#### 🛠️ THE WAY (core idea)

> _**“Define clear communication and responsibility rules between objects.”**_

#### ⏰ WHEN TO THINK ABOUT THEM

Consider behavioral patterns when:

✔️ you have complex workflows\
✔️ different behaviors should be switchable\
✔️ too many conditions everywhere\
✔️ business rules keep changing

#### 🧩 Examples

* **Strategy**
* **Observer**
* **Command**
* **State**
* **Mediator**
* **Template Method**
* **Iterator**

#### 🧠 REAL-LIFE ANALOGY

Google Maps navigation:

* Drive
* Walk
* Train

You choose strategy — path calculation changes.

That’s **Strategy Pattern**.

Another analogy:

You subscribe to YouTube channel → notifications come automatically.

That’s **Observer Pattern**.

***

## 🎯 Summary (simple table)

<table><thead><tr><th width="145.2734375">Category</th><th width="219.34375">Focus</th><th>Think About It When</th></tr></thead><tbody><tr><td>Creational</td><td>Object creation</td><td>Too many <code>new</code>, complex creation</td></tr><tr><td>Structural</td><td>Organizing objects</td><td>Integration, simplify structure</td></tr><tr><td>Behavioral</td><td>Communication &#x26; logic</td><td>Changing rules, workflows, conditions</td></tr></tbody></table>

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
