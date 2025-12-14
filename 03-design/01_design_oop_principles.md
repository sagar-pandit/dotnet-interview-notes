# OOP Principles (Object-Oriented Programming)

This section is **interview-focused**, with **real-life analogies, C# code examples, and tricky questions**.

---

## 1. Encapsulation

**Definition**: Bundling data and behavior together and controlling access.

### Real-life example
A **bank account**: you cannot directly modify balance; you use `Deposit` or `Withdraw`.

### C# Example
```csharp
public class BankAccount
{
    private decimal _balance;

    public decimal Balance => _balance;

    public void Deposit(decimal amount)
    {
        if (amount <= 0) throw new ArgumentException();
        _balance += amount;
    }
}
```

### Why it matters
- Protects invariants
- Improves maintainability
- Enables refactoring without breaking consumers

### Tricky Interview Question
**Q:** Is a public field encapsulation?

**A:** No. Encapsulation requires controlled access. Public fields expose internal state directly.

---

## 2. Abstraction

**Definition**: Exposing *what* an object does, not *how* it does it.

### Real-life example
A **remote control** abstracts TV internals.

### C# Example
```csharp
public interface IPaymentGateway
{
    void Pay(decimal amount);
}
```

### Why it matters
- Reduces coupling
- Enables substitution and testing

### Tricky Interview Question
**Q:** Can abstraction exist without inheritance?

**A:** Yes. Interfaces and composition enable abstraction without inheritance.

---

## 3. Inheritance

**Definition**: One class derives behavior from another.

### Real-life example
A **Car** is a **Vehicle**.

### Basic Example
```csharp
public class Vehicle
{
    public virtual void Start()
    {
        Console.WriteLine("Vehicle started");
    }
}

public class Car : Vehicle
{
    public override void Start()
    {
        Console.WriteLine("Car started");
    }
}
```

### Method Override
- Requires `virtual` in base
- Requires `override` in derived
- Supports runtime polymorphism

### Method Hiding
```csharp
public class Bike : Vehicle
{
    public new void Start()
    {
        Console.WriteLine("Bike started");
    }
}
```

### Key Difference
| Override | Method Hiding |
|--------|---------------|
| Runtime dispatch | Compile-time dispatch |
| Uses `virtual/override` | Uses `new` |
| Polymorphic | Not polymorphic |

### Tricky Interview Question
```csharp
Vehicle v = new Bike();
v.Start();
```
**Output?**

**Answer:** `Vehicle started` (method hiding, not override)

---

## 4. Polymorphism

**Definition**: Same interface, different behavior.

### 4.1 Static Polymorphism (Compile-time)
Achieved via **method overloading**.

```csharp
public class Calculator
{
    public int Add(int a, int b) => a + b;
    public double Add(double a, double b) => a + b;
}
```

- Resolved at compile time
- No inheritance required

### 4.2 Dynamic Polymorphism (Runtime)
Achieved via **method overriding**.

```csharp
Vehicle v = new Car();
v.Start(); // Car started
```

- Resolved at runtime
- Requires inheritance + virtual methods

### Tricky Interview Question
**Q:** Can static methods be polymorphic?

**A:** No. Static methods are resolved at compile time and cannot be overridden.

---

## 5. Composition over Inheritance

**Definition**: Prefer building behavior by combining objects rather than inheriting.

### Real-life example
A **Car HAS an Engine**, not IS an Engine.

### C# Example
```csharp
public class Engine
{
    public void Start() => Console.WriteLine("Engine started");
}

public class Car
{
    private readonly Engine _engine;

    public Car(Engine engine)
    {
        _engine = engine;
    }

    public void Start() => _engine.Start();
}
```

### Why preferred
- Avoids fragile base class problem
- Improves testability
- Encourages SRP

---

## Common OOP Anti-patterns (Interview Red Flags)

- Deep inheritance hierarchies
- God objects
- Exposing internal state
- Base classes with unused virtual methods

---

## Quick Interview Fire Questions

**Q:** Why inheritance is discouraged in modern C#?

**A:** It creates tight coupling; composition provides flexibility.

**Q:** Interface vs abstract class?

**A:** Interface defines capability; abstract class shares implementation.

**Q:** Can constructors be virtual?

**A:** No. Constructors are not polymorphic.

---

## 30-Second Interview Summary

> OOP in C# is about managing complexity using encapsulation, abstraction, polymorphism, and composition. Modern C# favors composition and interfaces over inheritance to reduce coupling, improve testability, and support clean architecture.

