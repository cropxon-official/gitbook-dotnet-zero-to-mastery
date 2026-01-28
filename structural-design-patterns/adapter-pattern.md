---
description: >-
  Adapter is a structural design pattern that allows objects with incompatible
  interfaces to collaborate.
icon: memo-circle-info
---

# 🔌 Adapter Pattern

<figure><img src="../.gitbook/assets/Adapter Pattern.png" alt=""><figcaption></figcaption></figure>

### 🔹 1️⃣ Problem — incompatible systems must talk

Sometimes:

* You already have an existing system (old code, library, API)
* You want to integrate a new component
* But their interfaces don’t match

Example:

Your app expects:

```csharp
IPayment.Process(amount)
```

But the new third-party SDK exposes:

```csharp
ExecutePayment(value);
```

They can’t talk directly.

You **CANNOT** change:

❌ Legacy system\
❌ Third-party library\
❌ Vendor SDK

So you need something in between.

> Adapter makes incompatible interfaces work together.

### <i class="fa-frown">:frown:</i> Problem <a href="#problem" id="problem"></a>

Imagine that you’re creating a stock market monitoring app. The app downloads the stock data from multiple sources in XML format and then displays nice-looking charts and diagrams for the user.

At some point, you decide to improve the app by integrating a smart 3rd-party analytics library. But there’s a catch: the analytics library only works with data in JSON format.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

You could change the library to work with XML. However, this might break some existing code that relies on the library. And worse, you might not have access to the library’s source code in the first place, making this approach impossible.

### <i class="fa-smile-beam">:smile-beam:</i> Solution <a href="#solution" id="solution"></a>

You can create an _**adapter**_. This is a special object that converts the interface of one object so that another object can understand it.

An adapter wraps one of the objects to hide the complexity of conversion happening behind the scenes. The wrapped object isn’t even aware of the adapter. For example, you can wrap an object that operates in meters and kilometers with an adapter that converts all of the data to imperial units such as feet and miles.

Adapters can not only convert data into various formats but can also help objects with different interfaces collaborate. Here’s how it works:

1. The adapter gets an interface, compatible with one of the existing objects.
2. Using this interface, the existing object can safely call the adapter’s methods.
3. Upon receiving a call, the adapter passes the request to the second object, but in a format and order that the second object expects.

Sometimes it’s even possible to create a two-way adapter that can convert the calls in both directions.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

\
🧠 2️⃣ Real-life analogy

#### ⚡ Plug Adapter

Laptop plug (US)\
Socket (India)

They don’t fit.

You don’t:

* Replace laptop
* Break the wall

You add:

> A travel adapter that converts plug shape

Laptop remains same.\
Wall remains same.\
Adapter makes them compatible.

That is exactly the Adapter Pattern.

***

### ❌ 3️⃣ Bad Approach — modify everything

Developers often try to change existing classes.

Example:

Changing third-party payment SDK code.

```csharp
public class ThirdPartyGateway
{
    public void ExecutePayment(double amount)
    {
        Console.WriteLine("Third party payment executed");
    }
}
```

Then they say:

> “Let me just rename/remove/add methods…”

❌ Cannot modify vendor library\
❌ Will break future library updates\
❌ Violates Open/Closed Principle\
❌ High risk, bad practice

We need **a bridge**, not modification.

***

## ✅ 4️⃣ Correct Approach — Adapter

> Wrap the incompatible object inside a new class that exposes the interface we want.

We create:

* Target interface (what our app expects)
* Adapter class (converts calls)
* Existing system remains untouched

***

## 💻 5️⃣ Real Enterprise Example — Payment Integration

#### Your application expects:

```csharp
public interface IPaymentGateway
{
    void Pay(double amount);
}
```

But new vendor provides:

```csharp
public class RazorPaySdk
{
    public void MakePayment(double value)
    {
        Console.WriteLine($"RazorPay: {value} paid");
    }
}
```

Interfaces don’t match.

***

## 👨‍💻 6️⃣ Step-by-step C# Implementation

#### Step 1 — Define target interface (your system expects)

```csharp
public interface IPaymentGateway
{
    void Pay(double amount);
}
```

***

#### Step 2 — Existing incompatible third-party system (we cannot touch this)

```csharp
public class RazorPaySdk
{
    public void MakePayment(double value)
    {
        Console.WriteLine($"RazorPay: {value} paid");
    }
}
```

***

#### Step 3 — Create Adapter (bridge)

Adapter translates:

> **Pay() → MakePayment()**

```csharp
public class RazorPayAdapter : IPaymentGateway
{
    private readonly RazorPaySdk _sdk;

    public RazorPayAdapter(RazorPaySdk sdk)
    {
        _sdk = sdk;
    }

    public void Pay(double amount)
    {
        // convert request
        _sdk.MakePayment(amount);
    }
}
```

***

#### Step 4 — Client uses your interface (no changes!)

```csharp
var gateway = new RazorPayAdapter(new RazorPaySdk());
gateway.Pay(500);
```

✔ Client code stays clean\
✔ Easily replaceable later\
✔ No hard-coding to vendor

***

## 🧩 7️⃣ What just happened?

Without adapter:

❌ your app tightly couples to RazorPay

With adapter:

✔ your app talks only to IPaymentGateway\
✔ adapter translates calls internally\
✔ later switching gateway is easy

Tomorrow business says:

> “Use Stripe now.”

Just create:

```csharp
public class StripeAdapter : IPaymentGateway { ... }
```

No core changes. Plug & play.

***

## 🏢 8️⃣ Real Enterprise Scenarios Where Adapter is Common

#### ✔ Legacy → New System integration

Old system uses XML\
New uses JSON

Adapter converts formats.

#### ✔ Third-party API wrapper

Stripe, RazorPay, PayPal, AWS, Azure SDKs

Companies never call SDKs directly — always via adapters.

#### ✔ Database migrations

Old DB repo vs new ORM.

#### ✔ Microservice contracts

Internal service uses different DTO formats.

Adapters sit between services.

***

## 🎯 9️⃣ When to Use Adapter

Use Adapter when:

✔ two systems must work together\
✔ you cannot change existing code\
✔ new module’s interface doesn’t match\
✔ migrating from legacy system\
✔ integrating vendor APIs

Signs:

> “This method name / structure doesn’t match what we expect.”

***

## 🚫 When NOT to Use

Avoid when:

✘ you control both systems (just refactor)\
✘ only one simple mismatch exists\
✘ using adapter will overcomplicate logic

Sometimes a small change is simpler.

***

## 🧠 10 Principles Followed

#### ✔ SRP — Single Responsibility

Adapter only converts requests.

#### ✔ OCP — Open/Closed

We extend behavior using new adapters…

➡ without modifying existing systems.

#### ✔ DIP — Dependency Inversion

Client depends on **interface**, not implementation.

***

## 📝 11 Final Summary

<table><thead><tr><th width="143.6953125">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Two interfaces don’t match</td></tr><tr><td>Solution</td><td>Create adapter to bridge them</td></tr><tr><td>Goal</td><td>Compatibility without modification</td></tr><tr><td>Benefits</td><td>Reusable, flexible, safe integration</td></tr><tr><td>Best For</td><td>Legacy systems, APIs, SDKs, migrations</td></tr><tr><td>Principles</td><td>SRP, OCP, DIP</td></tr></tbody></table>

{% file src="../.gitbook/assets/Adapter_Pattern_Seamless_Integration.pdf" %}

{% file src="../.gitbook/assets/The_Adapter_Pattern.mp4" %}
