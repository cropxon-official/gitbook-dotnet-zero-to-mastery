---
description: Patterns usually help us follow principles correctly.
icon: book-open
---

# Patterns vs Principles

<figure><img src=".gitbook/assets/Patterns vs Principles.png" alt=""><figcaption></figcaption></figure>

### 1️⃣ Principles → how we **think**

_<mark style="color:blue;">**Principles**</mark>_ are like **laws of good architecture**.

They warn you when:

* your class is doing too much
* your code is too tightly coupled
* changes are risky
* bugs spread everywhere when you modify one thing

They DO NOT tell you:

> “use this class + that interface + that inheritance”

They simply say:

> “Don’t do this — aim for that.”

***

### 2️⃣ Patterns → how we **implement**

_<mark style="color:purple;">**Patterns**</mark>_ are like ready-made templates:

> “Here’s a proven structure that solves this type of problem.”

You can implement a pattern **differently** in every project — but the idea stays same.

***

## 🔗 How Principles lead to Patterns

Think of it like:

> Principle = motivation\
> Pattern = solution

Let’s connect each one.

***

## 🔹 1. SRP → small collaborating classes

#### ❌ Without SRP

One **God class**:

```csharp
public class OrderService
{
    public void CreateOrder(){}
    public void ValidatePayment(){}
    public void SaveToDB(){}
    public void SendEmail(){}
}
```

Hard to test\
Hard to change\
Breaks easily.

#### ✅ Apply SRP

Split responsibilities:

```csharp
public class OrderService { }
public class PaymentValidator { }
public class OrderRepository { }
public class NotificationService { }
```

**Now patterns start appearing naturally:**

* _<mark style="color:purple;">Repository Pattern</mark>_ (persistence separation)
* <mark style="color:purple;">Service Layer Pattern</mark> (business logic)
* <mark style="color:purple;">Event/Observer Pattern</mark> (notifications)

👉 Notice:\
We didn’t start with patterns.\
We followed SRP → then patterns emerged.

***

## 🔹 2. OCP → extend instead of modify

> “Don’t touch tested code. Add new behavior beside it.”

#### ❌ Violating OCP

```csharp
public double CalculateShipping(string type, double weight)
{
    if (type == "Air") return weight * 2;
    if (type == "Road") return weight * 1.5;
    if (type == "Sea") return weight * 1.2;
    return 0;
}
```

To add train shipping → we must edit code again.

Risky.

#### ✅ Follow OCP → introduce abstraction

```csharp
public abstract class Shipping
{
    public abstract double Calculate(double weight);
}
public class AirShipping : Shipping
{
    public override double Calculate(double weight) => weight * 2;
}
public class RoadShipping : Shipping
{
    public override double Calculate(double weight) => weight * 1.5;
}
```

Now adding Train:

```csharp
public class TrainShipping : Shipping
{
    public override double Calculate(double weight) => weight * 1.1;
}
```

We didn’t modify existing logic — we **extended**.

And guess what pattern we just used?

👉 **Strategy Pattern**

We created multiple interchangeable strategies.

***

## 🔹 3. DIP → leads to Strategy, Factory, Adapter

### DIP says:

> Depend on interfaces, not concrete classes.

#### ❌ Without DIP

```csharp
public class NotificationService
{
    private EmailSender _email = new EmailSender();

    public void Notify()
    {
        _email.Send();
    }
}
```

Now we are stuck with Email.

Want SMS?\
WhatsApp?\
Push notification?

You must edit code everywhere.

***

#### ✅ DIP applied

```csharp
public interface INotifier
{
    void Send();
}
public class EmailNotifier : INotifier
{
    public void Send() {}
}
public class SmsNotifier : INotifier
{
    public void Send() {}
}
```

High level depends on abstraction:

```csharp
public class NotificationService
{
    private readonly INotifier _notifier;
    public NotificationService(INotifier notifier)
    {
        _notifier = notifier;
    }
    public void Notify()
    {
        _notifier.Send();
    }
}
```

Now **anything** can plug in.

➡ <mark style="color:purple;">Strategy Pattern</mark> (swap behaviors easily)\
➡ <mark style="color:purple;">Factory Pattern</mark> (decide notifier dynamically)\
➡ <mark style="color:purple;">Adapter Pattern</mark> (wrap third-party APIs behind interface)

Patterns are tools DIP enables.

***

## 🧩 Mapping Principles → Patterns

<table><thead><tr><th width="111.0078125">Principle</th><th>What it pushes you toward</th><th>Example Patterns</th></tr></thead><tbody><tr><td>SRP</td><td>break big classes into roles</td><td>Repository, Service, Observer</td></tr><tr><td>OCP</td><td>extend not modify</td><td>Strategy, Decorator, Template Method</td></tr><tr><td>LSP</td><td>safe inheritance</td><td>Interface-based design, Composition</td></tr><tr><td>ISP</td><td>small focused interfaces</td><td>Role interfaces, Ports/Adapters</td></tr><tr><td>DIP</td><td>depend on abstractions</td><td>Strategy, Factory, Adapter, Mediator</td></tr></tbody></table>

***

## ⚠️ Important: Don’t force patterns

Bad Dev:

> “Let me find where I can use Strategy today.”

Good Dev:

> “My code is getting hard to change — which principle am I breaking?”

Then:

> “Is there a pattern that can help?”

Patterns are **a consequence**, not the goal.

***

## 🎯 Simple rule to remember

👉 **Principles protect design quality**\
👉 **Patterns implement flexible designs**

Principles = compass\
Patterns = toolbox
