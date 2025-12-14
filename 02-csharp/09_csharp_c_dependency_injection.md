# C# + Dependency Injection (Language-level Usage)

This section focuses on **how C# language features are used to implement Dependency Injection (DI)** — not framework-specific APIs. Interviewers often test whether candidates understand DI as a *design principle*, not just `AddScoped()`.

---

## What is Dependency Injection?

Dependency Injection is a technique where **dependencies are provided to a class instead of being created by it**.

Goals:
- Loose coupling
- Testability
- Replaceability

---

## Constructor Injection (Preferred)

```csharp
public class OrderService
{
    private readonly IOrderRepository _repo;

    public OrderService(IOrderRepository repo)
    {
        _repo = repo;
    }
}
```

Why preferred:
- Enforces required dependencies
- Supports immutability
- Easy to test

---

## Property Injection (Use Sparingly)

```csharp
public ILogger Logger { get; set; }
```

Problems:
- Dependencies may be unset
- Breaks invariants

---

## Method Injection

```csharp
public void Process(IEmailSender sender)
```

Use cases:
- Short-lived or optional dependencies

---

## Immutability + DI

Best practices:
- Use `readonly` fields
- Prefer records for configuration objects

```csharp
public class Service
{
    private readonly IDependency _dep;
}
```

---

## Interfaces and DI

```csharp
public interface IPaymentGateway
{
    void Pay();
}
```

- Code against abstractions
- Enables mocking and substitution

---

## Factory Injection Pattern

```csharp
Func<IService> serviceFactory;
```

Use when:
- Runtime decision is required
- Multiple implementations exist

---

## Common DI Anti-patterns (Interview Traps)

### Service Locator ❌
```csharp
var service = container.GetService<T>();
```

Why bad:
- Hides dependencies
- Harder to test

---

### Injecting Too Many Dependencies ❌

- Indicates SRP violation
- Refactor into smaller services

---

## Lifetime Awareness (Conceptual)

- Transient → new instance every time
- Scoped → per logical operation
- Singleton → application lifetime

> Lifetime mismatches cause subtle bugs.

---

## DI and async

- Avoid async work in constructors
- Use factories if async initialization is needed

---

## 30-Second Interview Summary

> Dependency Injection in C# is implemented using language constructs like constructors, interfaces, and immutability. Constructor injection is preferred as it enforces required dependencies and improves testability. Understanding DI as a design principle — not just a framework feature — is essential for writing maintainable and testable code.
