---
icon: memo-circle-info
---

# 🎛️ Facade Pattern

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### <i class="fa-frown">:frown:</i> Problem <a href="#problem" id="problem"></a>

Imagine that you must make your code work with a broad set of objects that belong to a sophisticated library or framework. Ordinarily, you’d need to initialize all of those objects, keep track of dependencies, execute methods in the correct order, and so on.

As a result, the business logic of your classes would become tightly coupled to the implementation details of 3rd-party classes, making it hard to comprehend and maintain.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### <i class="fa-smile-beam">:smile-beam:</i> Solution <a href="#solution" id="solution"></a>

A facade is a class that provides a simple interface to a complex subsystem which contains lots of moving parts. A facade might provide limited functionality in comparison to working with the subsystem directly. However, it includes only those features that clients really care about.

Having a facade is handy when you need to integrate your app with a sophisticated library that has dozens of features, but you just need a tiny bit of its functionality.

For instance, an app that uploads short funny videos with cats to social media could potentially use a professional video conversion library. However, all that it really needs is a class with the single method `encode(filename, format)`. After creating such a class and connecting it with the video conversion library, you’ll have your first facade.

### 🔹 1️⃣ What problem does it solve?

Large systems often have:

* many services
* many method calls
* complex workflows
* too many details exposed to the client

So client code looks like this:

```csharp
var user = authService.Login();
inventory.CheckStock();
payment.Process();
shipment.CreateOrder();
notification.SendEmail();
```

Client needs to know:

❌ order of calls\
❌ dependencies\
❌ business rules\
❌ error handling everywhere

This leads to:

❌ tight coupling\
❌ duplicated logic\
❌ fragile code\
❌ hard onboarding

We want:

> _**One simple entry point that hides complexity.**_

That is **Facade**.

***

### 🧠 2️⃣ Real-life analogy

#### 📺 TV Remote

Inside TV:

* circuits
* cables
* chips
* decoders
* power modules

But you don’t deal with all that.

You get:

> **On / Off / Volume / Channel**

Remote = **Facade**\
TV internals = complex subsystem

Same idea in software.

***

### ❌ 3️⃣ Bad Approach — client talks to everything

```csharp
public void PlaceOrder()
{
    var user = auth.Login();
    var inStock = inventory.Check();
    payment.Pay();
    shipment.Book();
    email.Send();
}
```

Problems:

❌ client must know entire system\
❌ repeated code across modules\
❌ complex, error-prone\
❌ difficult to change workflow later

If tomorrow process changes → modify everywhere.

Bad.

***

## ✅ 4️⃣ Correct Approach — Facade

> Provide <mark style="color:blue;">**ONE unified interface**</mark> to a complex system.

Client talks only to Facade.\
Facade talks to internal services.

***

### 🏢 5️⃣ Real enterprise example

#### Order Processing System

Services:

* AuthenticationService
* InventoryService
* PaymentGateway
* ShippingService
* EmailService

Instead of exposing all…

Create:

```
OrderFacade
```

Client calls just **one** method:

```csharp
orderFacade.PlaceOrder();
```

Behind the scenes — everything happens in order.

***

## 👨‍💻 6️⃣ Step-by-Step C# Implementation

#### Step 1 — Subsystems (complex modules)

```csharp
public class AuthService
{
    public void Login() => Console.WriteLine("User logged in");
}

public class InventoryService
{
    public void CheckStock() => Console.WriteLine("Stock checked");
}

public class PaymentService
{
    public void Pay() => Console.WriteLine("Payment completed");
}

public class ShippingService
{
    public void Ship() => Console.WriteLine("Order shipped");
}

public class EmailService
{
    public void SendConfirmation() => Console.WriteLine("Email sent");
}
```

***

#### Step 2 — Create Facade

```csharp
public class OrderFacade
{
    private readonly AuthService _auth;
    private readonly InventoryService _inventory;
    private readonly PaymentService _payment;
    private readonly ShippingService _shipping;
    private readonly EmailService _email;

    public OrderFacade()
    {
        _auth = new AuthService();
        _inventory = new InventoryService();
        _payment = new PaymentService();
        _shipping = new ShippingService();
        _email = new EmailService();
    }

    public void PlaceOrder()
    {
        _auth.Login();
        _inventory.CheckStock();
        _payment.Pay();
        _shipping.Ship();
        _email.SendConfirmation();
    }
}
```

***

#### Step 3 — Client uses only Facade

```csharp
var order = new OrderFacade();
order.PlaceOrder();
```

✔ Super simple\
✔ No need to know full workflow\
✔ No exposure of complexity

***

## 🧩 7️⃣ What just happened?

Without Facade:

Client talks to 5 subsystems.

With Facade:

Client talks to ONE object.

Facade:

✔ hides complexity\
✔ centralizes workflow\
✔ reduces coupling

Subsystems still exist — but isolated.

***

## 🌍 8️⃣ Where Facade is used in real systems

Very common in enterprise:

✔ Microservices gateways\
✔ Payment orchestration\
✔ Order processing\
✔ SDK wrappers\
✔ UI/Backend service calls\
✔ Cloud service orchestration

Even many frameworks internally use Facade.

Example:

```csharp
Console.WriteLine();
```

Internally tons of work happens —\
but you see just one method.

***

## 🎯 9️⃣ When to Use Facade

Use Facade when:

✔ system is complex\
✔ too many classes exposed\
✔ repeated workflow logic everywhere\
✔ onboarding devs becomes difficult\
✔ want a single entrypoint API

Great for:

* Legacy cleanup
* Simplifying SDK usage
* Providing clean API layer
* Microservice orchestration

***

## 🚫 When NOT to Use

Avoid Facade if:

✘ system is very small\
✘ complexity isn’t real\
✘ it hides too much business logic\
✘ developers need full control internally

Over-using Facade may create:

⚠ “God” service\
⚠ anti-pattern if everything ends up here

***

## 🧠 10 Principles Followed

#### ✔ SRP — Single Responsibility

Facade coordinates workflow — subsystems do tasks.

#### ✔ OCP

We can modify internals without breaking client.

#### ✔ Least Knowledge Principle (Law of Demeter)

Client knows as little as possible.

***

<figure><img src="../.gitbook/assets/Facade Pattern.png" alt=""><figcaption></figcaption></figure>

## 📝 11 Final Summary

<table><thead><tr><th width="143.4453125">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Too many complex subsystems exposed to client</td></tr><tr><td>Solution</td><td>Provide unified, simple interface</td></tr><tr><td>Goal</td><td>Hide complexity, reduce coupling</td></tr><tr><td>Benefits</td><td>Cleaner API, easier maintenance, safer changes</td></tr><tr><td>Used In</td><td>Microservices, SDKs, orchestration, legacy systems</td></tr><tr><td>Principles</td><td>SRP, OCP, Law of Demeter</td></tr></tbody></table>

{% file src="../.gitbook/assets/Facade_Simplicity_Out_of_Chaos.pdf" %}

{% file src="../.gitbook/assets/Mastering_the_Facade_Pattern.mp4" %}
