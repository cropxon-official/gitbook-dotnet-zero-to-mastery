---
description: >-
  Bridge is a structural design pattern that lets you split a large class or a
  set of closely related classes into two separate hierarchies—abstraction and
  implementatio ...
icon: memo-circle-info
---

# 🌉 Bridge Pattern

![](<../.gitbook/assets/image (19).png>)

**Bridge** is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

<figure><img src="../.gitbook/assets/Bridge Pattern.png" alt=""><figcaption></figcaption></figure>

### 🔹 1️⃣ What problem does it solve?

Sometimes we have **two dimensions of change**.

Example:

Shape types:

* Circle
* Square

Rendering types:

* Raster (pixels)
* Vector (lines)

Many devs do this:

* RasterCircle
* VectorCircle
* RasterSquare
* VectorSquare

Then if you add **Triangle**:

Now 6 classes.\
Add more? Boom 💥

> Bridge separates **Abstraction** from **Implementation**\
> so they can evolve independently.

***

### 🧠 2️⃣ Real-life analogy

#### 🎮 Game Controller & Console

Controller (abstraction):

* Play
* Pause
* Move

Consoles (implementation):

* PlayStation
* Xbox
* Nintendo

You don’t build:

* PlayStationController
* XboxController
* NintendoController

Instead:

Controller works with any console.

Same idea in Bridge.

***

### ❌ 3️⃣ Bad approach — inheritance explosion

```csharp
public class PdfDarkReport {}
public class PdfLightReport {}
public class ExcelDarkReport {}
public class ExcelLightReport {}
```

Add one new report type?\
Everything explodes.

***

## ✅ 4️⃣ Correct Approach — Bridge

We split system into two layers:

1️⃣ Abstraction layer (WHAT we do)\
2️⃣ Implementation layer (HOW it is done)

They connect with a bridge.

***

## 🏢 5️⃣ Scenario 1 — Reports (Enterprise Example)

#### Dimension 1 — Report types

* SalesReport
* InventoryReport

#### Dimension 2 — Export format

* PDF
* Excel

Instead of mixing them…

We create:

✔ Report abstraction\
✔ Exporter implementation

***

### 👨‍💻 C# Implementation

#### Step 1 — Implementation interface (HOW)

```csharp
public interface IReportExporter
{
    void Export(string content);
}
```

***

#### Step 2 — Concrete implementations

```csharp
public class PdfExporter : IReportExporter
{
    public void Export(string content)
    {
        Console.WriteLine("Exporting PDF: " + content);
    }
}

public class ExcelExporter : IReportExporter
{
    public void Export(string content)
    {
        Console.WriteLine("Exporting Excel: " + content);
    }
}
```

***

#### Step 3 — Abstraction (WHAT)

```csharp
public abstract class Report
{
    protected readonly IReportExporter _exporter;

    protected Report(IReportExporter exporter)
    {
        _exporter = exporter;
    }

    public abstract void Generate();
}
```

***

#### Step 4 — Concrete Abstractions

```csharp
public class SalesReport : Report
{
    public SalesReport(IReportExporter exporter) : base(exporter) {}

    public override void Generate()
    {
        _exporter.Export("Sales Report Data");
    }
}

public class InventoryReport : Report
{
    public InventoryReport(IReportExporter exporter) : base(exporter) {}

    public override void Generate()
    {
        _exporter.Export("Inventory Report Data");
    }
}
```

***

#### Step 5 — Client

```csharp
var salesPdf = new SalesReport(new PdfExporter());
salesPdf.Generate();

var inventoryExcel = new InventoryReport(new ExcelExporter());
inventoryExcel.Generate();
```

✔ Report logic separate\
✔ Export logic separate\
✔ Combine freely

***

## 🌍 6️⃣ Scenario 2 — Device + Control (Smart Home)

#### Dimension 1 — Devices

* TV
* Light

#### Dimension 2 — Controls

* Remote
* Voice Assistant
* Mobile App

Bridge allows combinations without class explosion.

***

### Example Structure (concept):

```
Control (Abstraction)
   -> RemoteControl
   -> VoiceControl

Device (Implementation)
   -> TV
   -> Light
```

Control talks to device via bridge.

***

## 🚗 7️⃣ Scenario 3 — Vehicles + Workshop (Service Centers)

Vehicles:

* Car
* Bike

Workshops:

* Repair Workshop
* Paint Workshop

Bridge:

Vehicle abstraction — Workshop implementation.

No explosion like:

* CarRepair
* CarPaint
* BikeRepair
* BikePaint

Instead combine dynamically.

***

## 🎯 8️⃣ When to use Bridge

Use when:

✔ two (or more) dimensions vary independently\
✔ you see inheritance explosion\
✔ new features require adding too many classes\
✔ you want loose coupling between layers

Typical indicators:

> “If I add one more type, I need 4 more classes.”

***

## 🚫 When NOT to use

Avoid if:

✘ System is simple\
✘ Only one dimension varies\
✘ Patterns add unnecessary complexity

Don’t force it.

***

## 🧠 9️⃣ Principles Followed

#### ✔ SRP — Single Responsibility

Abstraction and implementation have separate jobs.

#### ✔ OCP

Add new abstraction or implementation — without breaking others.

#### ✔ DIP

Abstraction depends on interfaces.

***

### <i class="fa-lightbulb-on">:lightbulb-on:</i> Applicability <a href="#applicability" id="applicability"></a>

{% hint style="info" %}
&#x20;Use the **Bridge pattern** when you want to divide and organize a monolithic class that has several variants of some functionality (for example, if the class can work with various database servers).
{% endhint %}

#### ✅ 1. When a class is becoming too big and has many variations

If one class handles many different behaviors (like supporting multiple databases), it becomes:

* hard to understand
* risky to modify
* easy to break accidentally

Bridge helps by **splitting it into separate hierarchies**, so each part can change independently.

***

#### ✅ 2. When you need to extend a class in multiple independent directions

Example:

* Reports (Sales, Inventory)
* Formats (PDF, Excel)

Instead of exploding into many subclasses, Bridge lets each dimension grow separately.

***

#### ✅ 3. When you want to switch implementations at runtime

You can change the implementation object dynamically:

```csharp
report.Exporter = new PdfExporter();
report.Exporter = new ExcelExporter();
```

Just replace the implementation — no rewrites.

> This is why people confuse Bridge with Strategy —\
> BOTH let you swap implementations — but Bridge focuses on **separating abstraction from implementation**, not just behavior.

***

### ⭐ One-line takeaway

> Use Bridge when your class has multiple variants and risks becoming a giant mess —\
> split the logic into independent layers that can change without breaking each other.

## 📝 Final Summary

<table><thead><tr><th width="126.875">Item</th><th>Meaning</th></tr></thead><tbody><tr><td>Problem</td><td>Two dimensions of change cause class explosion</td></tr><tr><td>Solution</td><td>Separate abstraction &#x26; implementation via bridge</td></tr><tr><td>Key Idea</td><td>Compose instead of multiply inheritance combos</td></tr><tr><td>Benefits</td><td>Flexible, scalable, avoids duplication</td></tr><tr><td>Used In</td><td>Reporting, devices, rendering, smart systems</td></tr><tr><td>Principles</td><td>SRP, OCP, DIP</td></tr></tbody></table>

{% file src="../.gitbook/assets/Bridge_Pattern_For_System_Flexibility.pdf" %}

{% file src="../.gitbook/assets/The_Bridge_Design_Pattern.mp4" %}
