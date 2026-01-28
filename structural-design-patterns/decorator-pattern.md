---
description: >-
  Decorator is a structural design pattern that lets you attach new behaviors to
  objects by placing these objects inside special wrapper objects that contain
  the behaviors.
icon: memo-circle-info
---

# 🎭 Decorator Pattern

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### <i class="fa-frown">:frown:</i> Problem <a href="#problem" id="problem"></a>

Imagine that you’re working on a notification library which lets other programs notify their users about important events.

The initial version of the library was based on the `Notifier` class that had only a few fields, a constructor and a single `send` method. The method could accept a message argument from a client and send the message to a list of emails that were passed to the notifier via its constructor. A third-party app which acted as a client was supposed to create and configure the notifier object once, and then use it each time something important happened.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

At some point, you realize that users of the library expect more than just email notifications. Many of them would like to receive an SMS about critical issues. Others would like to be notified on Facebook and, of course, the corporate users would love to get Slack notifications.

![](<../.gitbook/assets/image (18).png>)<br>

### 🔹 1️⃣ What problem does it solve?

Sometimes we need to **add features to an object**…

…but we DON’T want to:

❌ modify the original class\
❌ create endless subclasses

Example:

**Base coffee:**

* Coffee

**New features:**

* Coffee + Milk
* Coffee + Milk + Sugar
* Coffee + Milk + Caramel + Ice

If we use inheritance:

* CoffeeWithMilk
* CoffeeWithMilkAndSugar
* CoffeeWithSugarAndCaramel

Classes explode. Hard to maintain.

> Decorator lets us add behavior **dynamically** by wrapping objects.

***

### 🧠 2️⃣ Real-life analogy

#### ☕ Coffee Shop

**Base coffee** → then add **toppings**:

* milk
* sugar
* chocolate
* caramel

Each topping is **wrapped around** the original drink.

Coffee doesn’t change — decorations wrap it.

That is **Decorator**.

***

### ❌ 3️⃣ Bad Approach — inheritance explosion

```csharp
public class Coffee
{
    public virtual double Cost() => 50;
}

public class CoffeeWithMilk : Coffee
{
    public override double Cost() => base.Cost() + 10;
}

public class CoffeeWithMilkAndSugar : CoffeeWithMilk
{
    public override double Cost() => base.Cost() + 5;
}
```

Problems:

❌ too many subclasses\
❌ not flexible\
❌ difficult when combinations grow\
❌ violates OCP (keep modifying classes)

***

## ✅ 4️⃣ Correct Approach — Decorator

> _**Wrap object**_ → add feature instead of modifying or subclassing.

***

## 🏢 5️⃣ Real Enterprise Example — Logging, Caching, Validation

A service:

* Fetch data from DB

Later business says:

✔ add logging\
✔ then caching\
✔ later retry\
✔ later metrics

Instead of rewriting service each time…

We wrap the service with decorators.

***

## 👨‍💻 6️⃣ Step-by-Step C# Implementation

#### Step 1 — Base interface

```csharp
public interface ICoffee
{
    double Cost();
    string Description();
}
```

***

#### Step 2 — Concrete object

```csharp
public class BasicCoffee : ICoffee
{
    public double Cost() => 50;
    public string Description() => "Basic Coffee";
}
```

***

#### Step 3 — Base decorator

Decorator must:

✔ implement same interface\
✔ hold wrapped object

```csharp
public abstract class CoffeeDecorator : ICoffee
{
    protected readonly ICoffee _coffee;

    protected CoffeeDecorator(ICoffee coffee)
    {
        _coffee = coffee;
    }

    public virtual double Cost() => _coffee.Cost();
    public virtual string Description() => _coffee.Description();
}
```

***

#### Step 4 — Concrete decorators

**Add Milk**

```csharp
public class MilkDecorator : CoffeeDecorator
{
    public MilkDecorator(ICoffee coffee) : base(coffee) {}

    public override double Cost() => base.Cost() + 10;
    public override string Description() => base.Description() + ", Milk";
}
```

**Add Sugar**

```csharp
public class SugarDecorator : CoffeeDecorator
{
    public SugarDecorator(ICoffee coffee) : base(coffee) {}

    public override double Cost() => base.Cost() + 5;
    public override string Description() => base.Description() + ", Sugar";
}
```

***

#### Step 5 — Client builds combinations dynamically

```csharp
ICoffee coffee = new BasicCoffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);

Console.WriteLine(coffee.Description()); // Basic Coffee, Milk, Sugar
Console.WriteLine(coffee.Cost());        // 65
```

✔ No change to original class\
✔ No subclass explosion\
✔ Fully flexible

***

## 🧩 7️⃣ What just happened?

Before:

👉 modify class every time\
👉 inheritance hell

After:

👉 add features by wrapping\
👉 plug-and-play decorations\
👉 dynamic behavior

Decorator = **composition instead of inheritance**

***

## 🌍 8️⃣ Where is Decorator used in real systems?

Very common:

✔ ASP.NET Middleware\
✔ Logging wrappers\
✔ Caching wrappers\
✔ Retry / Polly patterns\
✔ UI frameworks\
✔ Stream APIs

Example from .NET:

```csharp
var stream = new GZipStream(
                 new FileStream("file.txt"),
                 CompressionMode.Compress);
```

Stream wrapped inside another stream → decorator.

***

## 🎯 9️⃣ When to use Decorator

Use when:

✔ want to add behavior without modifying class\
✔ need different combinations of features\
✔ subclassing becomes uncontrollable\
✔ runtime toggling is required

Signs you need Decorator:

> “We keep adding features to the same class.”

***

## 🚫 When NOT to use

Avoid when:

✘ simple object — no dynamic features\
✘ too many decorators make debugging hard\
✘ better solved with configuration or strategy

Overusing decorator leads to:

⚠ “Wrap hell”

So use only when pattern fits.

***

## 🧠 10 Principles Followed

#### ✔ OCP — Open/Closed

Add new behavior without modifying existing code.

#### ✔ SRP

Each decorator has a single responsibility.

#### ✔ Composition Over Inheritance

Prefer wrappers instead of subclassing.

***

## 📝 Final Summary

<table><thead><tr><th width="145.29296875">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Need to add features without modifying class</td></tr><tr><td>Solution</td><td>Wrap object and extend behavior</td></tr><tr><td>Key Idea</td><td>Composition, not inheritance</td></tr><tr><td>Benefits</td><td>Flexible, reusable, avoids subclass explosion</td></tr><tr><td>Used In</td><td>Logging, caching, middleware, UI, streams</td></tr><tr><td>Principles</td><td>SRP, OCP, Composition over Inheritance</td></tr></tbody></table>

{% file src="../.gitbook/assets/Decorator_Pattern_Dynamic_Behavior.pdf" %}

{% file src="../.gitbook/assets/Decorator_Design_Pattern.mp4" %}
