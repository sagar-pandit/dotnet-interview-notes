# Abstract Class vs Interface
---

## What is an Abstract Class?

An **abstract class** is a partially implemented class that **can contain state and behavior** and **cannot be instantiated**.

### Real-life example
A **Vehicle** is abstract. You never have a generic vehicle; you have a **Car**, **Bike**, or **Truck**.

### C# Example
```csharp
public abstract class Vehicle
{
    public int Speed { get; protected set; }

    public abstract void Start();

    public virtual void Stop()
    {
        Console.WriteLine("Vehicle stopped");
    }
}

public class Car : Vehicle
{
    public override void Start()
    {
        Speed = 10;
        Console.WriteLine("Car started");
    }
}
```

### Key Characteristics
- Can have fields and constructors
- Can have implemented + abstract methods
- Supports inheritance only (single inheritance)

---

## What is an Interface?

An **interface** defines a **contract (capability)** without implementation (mostly).

### Real-life example
A **USB port**: different devices implement the same interface.

### C# Example
```csharp
public interface IPaymentProcessor
{
    void Process(decimal amount);
}

public class RazorpayProcessor : IPaymentProcessor
{
    public void Process(decimal amount)
    {
        Console.WriteLine($"Processing {amount}");
    }
}
```

### Modern C# Interface (default methods)
```csharp
public interface ILogger
{
    void Log(string message);

    void LogInfo(string message)
    {
        Console.WriteLine($"INFO: {message}");
    }
}
```

---

## Abstract Class vs Interface (Side-by-Side)

| Feature | Abstract Class | Interface |
|------|---------------|-----------|
| Instantiation | ❌ No | ❌ No |
| Multiple inheritance | ❌ No | ✅ Yes |
| Fields | ✅ Yes | ❌ No |
| Constructors | ✅ Yes | ❌ No |
| Access modifiers | ✅ Yes | ❌ No (public only) |
| State | ✅ Yes | ❌ No |
| Default implementation | ✅ Yes | ✅ (C# 8+) |

---

## When to Use Abstract Class?

Use abstract class when:
- You want to **share base behavior**
- You need **state (fields)**
- You control all derived classes

### Example
```csharp
public abstract class FileProcessor
{
    protected string FilePath;

    public abstract void Process();

    protected void Validate()
    {
        Console.WriteLine("Validating file");
    }
}
```

---

## When to Use Interface?

Use interface when:
- You want **loose coupling**
- You need **multiple inheritance**
- You design for **DI and testing**

### Example (DI-friendly)
```csharp
public class OrderService
{
    private readonly IPaymentProcessor _processor;

    public OrderService(IPaymentProcessor processor)
    {
        _processor = processor;
    }
}
```

---

## Tricky Interview Questions

### Q1: Can an abstract class implement an interface?

**Answer:** Yes

```csharp
public abstract class LoggerBase : ILogger
{
    public abstract void Log(string message);
}
```

---

### Q2: Can an interface have fields?

**Answer:** No. Interfaces can have constants only.

---

### Q3: Can we use abstract class with DI?

**Answer:** Yes, but interface is preferred for flexibility.

---

### Q4: Interface vs Abstract class – which is faster?

**Answer:** No meaningful performance difference. Design choice matters more.

---

## Interview Decision Rule (Golden Rule)

> If you need **capability**, use **interface**.
> If you need **shared behavior or state**, use **abstract class**.

---

## 30-Second Interview Summary

> Abstract classes model "is-a" relationships with shared behavior and state, while interfaces define capabilities for loose coupling. Modern .NET favors interfaces, especially for dependency injection and testability, while abstract classes are used sparingly for base behavior reuse.


> OOP in C# is about managing complexity using encapsulation, abstraction, polymorphism, and composition. Modern C# favors composition and interfaces over inheritance to reduce coupling, improve testability, and support clean architecture.

