---
description: >-
  Structural design patterns explain how to assemble objects and classes into
  larger structures, while keeping these structures flexible and efficient.
icon: layer-plus
---

# Structural Design Patterns

{% file src="../.gitbook/assets/Structural_Design_Patterns_and_Composition.m4a" %}

<figure><img src="../.gitbook/assets/Structural Pattern.png" alt=""><figcaption></figcaption></figure>

### 🔹 What are Structural Patterns?

> Structural patterns define **how classes and objects are organized and combined**\
> so that the system becomes flexible, reusable, and easy to maintain.

They answer questions like:

* How should objects connect?
* How do I extend functionality without modifying classes?
* How can I simplify complex subsystems?
* How do I make two incompatible systems work together?

Structural patterns focus on:

✔️ composition\
✔️ object relationships\
✔️ wrappers / adapters\
✔️ simplifying complexity

Not on **creation** (that was creational)\
Not on **behavior logic** (that will be behavioral)

***

### ❓ Why do we need Structural Patterns?

Without them, systems quickly become:

❌ tightly coupled\
❌ difficult to modify\
❌ full of duplicate code\
❌ hard to integrate with other systems\
❌ overloaded with complex objects calling each other

You change one class → 10 others break.

Structural patterns make systems:

✔ modular\
✔ extensible\
✔ easier to integrate\
✔ easier to replace components\
✔ aligned with OCP (extend instead of modify)

***

### 🎯 When should we think about Structural Patterns?

Ask yourself:

> “Is my code structure getting complicated?”

Think about them when:

✔ you need to connect systems\
✔ you are wrapping 3rd party APIs\
✔ you need to add extra functionality to existing classes\
✔ objects should behave like a hierarchy (tree)\
✔ you want a simpler public interface over a complex system\
✔ you need to control object access (lazy load, proxy, etc.)

***

### 🛠️ How Structural Patterns Work (Conceptually)

General strategy:

> Instead of modifying objects —\
> we **compose**, **wrap**, or **combine** them smartly.

Key ideas used repeatedly:

✔ composition over inheritance\
✔ adapters around incompatible systems\
✔ wrappers to extend behavior\
✔ proxies to control access\
✔ facades to simplify complexity\
✔ bridges to separate abstraction from implementation

## 🧩 Types of Structural Patterns (7)

We will cover them _later in depth_ one by one.

For now — quick conceptual intro + examples + analogies.

***

### 1️⃣ Adapter Pattern — “Make incompatible things work together”

#### 🧠 Analogy

Charging a US laptop in India → plug adapter.

#### 💼 Real Enterprise Example

Integrating legacy payment system with new API.

> Your system = JSON\
> Old system = XML

Adapter converts formats.

***

### 2️⃣ Facade Pattern — “Give a simple interface to a complex system”

#### 🧠 Analogy

TV remote control.

Inside = wires + chips\
You = simple buttons.

#### 💼 Real Enterprise Example

One service hides complex microservices:

```
OrderService -> internally calls
Inventory + Payment + Shipping + Notification
```

Client calls only OrderService.

***

### 3️⃣ Decorator Pattern — “Add features without modifying class”

#### 🧠 Analogy

Coffee shop:

* Coffee
* Coffee + Milk
* Coffee + Milk + Caramel

Layer decorations.

#### 💼 Real Enterprise Example

Logging, caching, validation wrappers around services.

***

### 4️⃣ Composite Pattern — “Tree-like structures”

#### 🧠 Analogy

Folder inside folders.

#### 💼 Real Enterprise Example

Organization hierarchy:

* CEO
  * Managers
    * Employees

Treat both “individual” and “group” the same.

***

### 5️⃣ Bridge Pattern — “Separate abstraction from implementation”

#### 🧠 Analogy

Remote (buttons) vs TV (device).

Remote stays same\
TV brand can change.

#### 💼 Real Enterprise Example

Reporting system:

* PDF / Excel / HTML exporters
* but same Report abstraction.

***

### 6️⃣ Proxy Pattern — “Object representing another object”

#### 🧠 Analogy

Credit card = proxy for your bank account.

You don’t talk to bank directly.

#### 💼 Real Enterprise Example

Used in:

* lazy loading
* access control
* caching proxies
* API gateway

***

### 7️⃣ Flyweight Pattern — “Reuse objects instead of duplicating memory”

#### 🧠 Analogy

Hotel sharing common items (TV, AC type specs) instead of copying per room.

#### 💼 Real Enterprise Example

Game engines reuse:

* trees
* rocks
* bullets

Millions of objects, shared internal state.

***

<figure><img src="../.gitbook/assets/structural patterns types.png" alt=""><figcaption></figcaption></figure>

## 🧠 Structural Patterns Summary in One Table

<table><thead><tr><th width="160.68359375">Pattern</th><th>Key Idea</th><th>Real-Life Analogy</th><th>Enterprise Example</th></tr></thead><tbody><tr><td>Adapter</td><td>Make two systems compatible</td><td>Plug adapter</td><td>Legacy API integration</td></tr><tr><td>Facade</td><td>Simplify complex subsystems</td><td>TV remote</td><td>Orchestration service</td></tr><tr><td>Decorator</td><td>Add features dynamically</td><td>Add toppings to pizza</td><td>Logging / caching</td></tr><tr><td>Composite</td><td>Tree structure</td><td>Folders</td><td>Org hierarchy</td></tr><tr><td>Bridge</td><td>Separate abstraction &#x26; implementation</td><td>Remote + TV</td><td>UI themes / exporters</td></tr><tr><td>Proxy</td><td>Control access / lazy loading</td><td>Credit card</td><td>API gateway / security</td></tr><tr><td>Flyweight</td><td>Memory optimization via sharing</td><td>Shared AC manual</td><td>Game objects</td></tr></tbody></table>

***

## 🎯 Big Takeaway

Structural patterns focus on:

✔ organizing objects\
✔ better relationships\
✔ cleaner architecture\
✔ extend without breaking existing code

They heavily support:

✔ OCP — Open/Closed\
✔ SRP — Single Responsibility\
✔ Composition over inheritance

{% file src="../.gitbook/assets/Structural_Architecting_Better_Systems.pdf" %}

{% file src="../.gitbook/assets/Structural_Design_Patterns.mp4" %}
