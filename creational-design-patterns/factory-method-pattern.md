---
description: >-
  Factory Method is a creational design pattern that provides an interface for
  creating objects in a superclass, but allows subclasses to alter the type of
  objects that will be created.
icon: memo-circle-check
---

# 🏭 Factory Method Pattern

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

{% file src="../.gitbook/assets/Factory_Method_Pattern_for_Flexible_Code.m4a" %}

### 🔹 1️⃣ What problem does it solve?

> <mark style="color:blue;">You want to create objects —</mark>\ <mark style="color:blue;">without exposing creation logic —</mark>\ <mark style="color:blue;">and without hard-coding classes everywhere.</mark>

**Bad**:

```csharp
var payment = new RazorPayGateway();
```

Tomorrow business changes:

* Stripe
* PayPal
* Custom Gateway
* Bank Transfer
* Wallet

Now you must **edit code everywhere**.

That leads to:

❌ tight coupling\
❌ breaking changes\
❌ violates OCP\
❌ difficult testing\
❌ big `if / else` hell

Factory Method fixes this.

### 2️⃣ Real-life analogy

#### ☕ Starbucks

You don’t go behind the counter to make coffee.

You just say:

> “Give me a Latte.”

The barista decides:

* ingredients
* process
* tools

You only care about drinking.

👉 **You ask WHAT — barista decides HOW.**

Factory Method does the same.

### ❌ 3️⃣ Without Factory (tight coupling)

```csharp
public class PaymentService
{
    public void Pay(string method)
    {
        if (method == "Stripe")
            new Stripe().Process();
        else if (method == "Paypal")
            new Paypal().Process();
    }
}
```

#### ❗ Problems

1️⃣ Every new gateway = change code again\
2️⃣ Violates **Open/Closed Principle**\
3️⃣ Too many conditions\
4️⃣ Hard to test\
5️⃣ Not flexible

<figure><img src="../.gitbook/assets/Factory.png" alt=""><figcaption></figcaption></figure>

## ✅ 4️⃣ With Factory Method (Step-by-Step)

We follow **4 guided steps**.

***

### Step 1 — Create abstraction

```csharp
public interface IPayment
{
    void Pay();
}
```

<mark style="color:purple;">👉 Client depends on</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**interface**</mark><mark style="color:purple;">, not</mark> <mark style="color:purple;"></mark>_<mark style="color:purple;">concrete classes.</mark>_

***

### Step 2 — Implement different payment types

```csharp
public class StripePayment : IPayment
{
    public void Pay() => Console.WriteLine("Stripe payment done");
}
public class PaypalPayment : IPayment
{
    public void Pay() => Console.WriteLine("Paypal payment done");
}
```

_<mark style="color:purple;">Each class handles its own behavior.</mark>_

***

### Step 3 — Create Factory

Factory decides which object to create.

```csharp
public class PaymentFactory
{
    public static IPayment GetPayment(string method)
    {
        return method switch
        {
            "Stripe" => new StripePayment(),
            "Paypal" => new PaypalPayment(),
            _ => throw new ArgumentException("Invalid method")
        };
    }
}
```

👉 <mark style="color:purple;">Client no longer uses</mark> <mark style="color:purple;"></mark><mark style="color:purple;">**`new`**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">directly.</mark>

***

### Step 4 — Client uses factory

```csharp
var payment = PaymentFactory.GetPayment("Stripe");
payment.Pay();
```

✔️ Clean\
✔️ Flexible\
✔️ Easy to extend

***

## 🧩 5️⃣ What just happened?

#### Before:

Client → Payment classes directly

Tight coupling.

#### After Factory:

**Client** → **Factory** → **Concrete** classes

Client only knows:

* interface (`IPayment`)
* method name (`GetPayment`)

***

## 🔧 6️⃣ Improving Factory (More Scalable)

Switch works — but grows ugly with many gateways.

We can improve by **registration-based factory**.

#### Dynamic factory (no switch, easy extension)

```csharp
public static class PaymentFactory
{
    private static readonly Dictionary<string, Func<IPayment>> _map =
        new();
    public static void Register(string name, Func<IPayment> creator)
        => _map[name] = creator;
    public static IPayment Create(string name)
        => _map[name]();
}
```

Register anywhere:

```csharp
PaymentFactory.Register("Stripe", () => new StripePayment());
PaymentFactory.Register("Paypal", () => new PaypalPayment());
```

Use:

```csharp
var payment = PaymentFactory.Create("Paypal");
payment.Pay();
```

Now adding a new method:

```csharp
public class RazorPayPayment : IPayment
{
    public void Pay() => Console.WriteLine("RazorPay Done");
}
```

Just register:

```csharp
PaymentFactory.Register("RazorPay", () => new RazorPayPayment());
```

👉 No modification to factory code\
👉 Fully **OCP compliant**

***

## 🎯 7️⃣ When should you use Factory Method?

Use when:

✔ object creation may change\
✔ multiple variations exist\
✔ you want to remove `new` from everywhere\
✔ you want loose coupling\
✔ you want to follow OCP\
✔ easier unit testing

***

## 🚫 8️⃣ When you should NOT use it

Avoid when:

✘ only one concrete type exists\
✘ construction is simple\
✘ no flexibility required\
✘ unnecessary complexity

Sometimes:

```csharp
var user = new User();
```

is perfectly fine.

***

## 📝 9️⃣ Quick Summary

<table><thead><tr><th width="150.953125">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Hard-coded object creation causes tight coupling</td></tr><tr><td>Solution</td><td>Move creation logic into a factory method</td></tr><tr><td>What changes</td><td>Client doesn’t use <code>new</code> anymore</td></tr><tr><td>Benefits</td><td>Flexible, testable, follows OCP + DIP</td></tr><tr><td>Best for</td><td>Systems that grow and change over time</td></tr></tbody></table>

**Principles applied:**

✔ **OCP** — Open/Closed Principle\
✔ **DIP** — Dependency Inversion Principle

{% file src="../.gitbook/assets/Factory_Method_Escaping_Tight_Coupling.pdf" %}

{% file src="../.gitbook/assets/Factory_Pattern.mp4" %}
