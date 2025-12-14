# SOLID Principles (With Violations & Refactoring)

This section is **interview‑oriented**, focusing on **intent, common violations, refactoring strategies, and real‑world relevance** in C# and .NET systems.

---

## What is SOLID?

SOLID is a set of **five object‑oriented design principles** aimed at building:
- Maintainable systems
- Extensible code
- Testable components

> SOLID is about **managing change**, not just writing clean code.

---

## S — Single Responsibility Principle (SRP)

### Definition
A class should have **only one reason to change**.

### Real‑life example
A **Restaurant Chef** cooks food. Billing is handled by **Cashier**.

---

### ❌ Violation
```csharp
public class InvoiceService
{
    public void CreateInvoice() {}
    public void SaveToDatabase() {}
    public void SendEmail() {}
}
```

**Problems**:
- Multiple responsibilities
- Hard to test
- Changes cascade

---

### ✅ Refactoring
```csharp
public class InvoiceService
{
    public void CreateInvoice() {}
}

public class InvoiceRepository
{
    public void Save() {}
}

public class EmailService
{
    public void Send() {}
}
```

---

### Interview Question
**Q:** Is SRP about "one method per class"?

**A:** No. It’s about **one reason to change**, not size.

---

## O — Open/Closed Principle (OCP)

### Definition
Software entities should be **open for extension but closed for modification**.

### Real‑life example
Plug‑ins in a browser.

---

### ❌ Violation
```csharp
public class DiscountCalculator
{
    public decimal Calculate(string type)
    {
        if (type == "Seasonal") return 10;
        if (type == "Festival") return 20;
        return 0;
    }
}
```

---

### ✅ Refactoring
```csharp
public interface IDiscount
{
    decimal Calculate();
}

public class SeasonalDiscount : IDiscount
{
    public decimal Calculate() => 10;
}

public class DiscountCalculator
{
    public decimal Calculate(IDiscount discount)
        => discount.Calculate();
}
```

---

### Interview Question
**Q:** Does OCP mean no changes ever?

**A:** No. Core logic should remain stable while behavior is extended.

---

## L — Liskov Substitution Principle (LSP)

### Definition
Derived classes must be substitutable for their base classes.

### Real‑life example
A **Square** should not break expectations of a **Rectangle**.

---

### ❌ Violation
```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }
}
```

---

### ✅ Refactoring
```csharp
public interface IShape
{
    int Area();
}
```

---

### Interview Question
**Q:** Is LSP about inheritance or behavior?

**A:** Behavior. The contract must not be violated.

---

## I — Interface Segregation Principle (ISP)

### Definition
Clients should not be forced to depend on interfaces they do not use.

### Real‑life example
A **Printer** should not require fax capabilities.

---

### ❌ Violation
```csharp
public interface IMachine
{
    void Print();
    void Scan();
    void Fax();
}
```

---

### ✅ Refactoring
```csharp
public interface IPrinter
{
    void Print();
}

public interface IScanner
{
    void Scan();
}
```

---

### Interview Question
**Q:** Is ISP about splitting interfaces always?

**A:** No. Interfaces should be split **only when clients differ**.

---

## D — Dependency Inversion Principle (DIP)

### Definition
High‑level modules should not depend on low‑level modules.

### Real‑life example
Switch vs bulb — depend on abstraction.

---

### ❌ Violation
```csharp
public class OrderService
{
    private SqlRepository _repo = new SqlRepository();
}
```

---

### ✅ Refactoring
```csharp
public interface IRepository {}

public class OrderService
{
    private readonly IRepository _repo;
    public OrderService(IRepository repo)
    {
        _repo = repo;
    }
}
```

---

### Interview Question
**Q:** Is DIP same as Dependency Injection?

**A:** No. DI is a technique to achieve DIP.

---

## SOLID — Common Interview Traps

- SOLID is **guidelines**, not rules
- Violations are sometimes acceptable for simplicity
- Over‑engineering is worse than violation

---

## 30‑Second Interview Summary

> SOLID principles guide object‑oriented design toward flexibility and maintainability. SRP limits reasons to change, OCP supports extension, LSP preserves behavioral contracts, ISP avoids bloated interfaces, and DIP promotes abstraction‑based design. Together, they enable scalable and testable systems.

