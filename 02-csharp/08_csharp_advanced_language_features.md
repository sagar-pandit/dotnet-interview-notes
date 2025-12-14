# Advanced C# Language Features

This section covers **modern C# features (C# 8 → C# 12)** that are frequently discussed in **senior and lead-level interviews**. The focus is on *why* these features exist and *when* to use them.

---

## Nullable Reference Types (C# 8)

```csharp
string? name = null;
string title = "DotNet";
```

- Introduced to eliminate **NullReferenceException**
- Compiler performs **static null analysis**
- Enabled via:
```csharp
#nullable enable
```

Best practices:
- Treat warnings as errors
- Use `!` (null-forgiving) sparingly

---

## Pattern Matching

### Type Pattern
```csharp
if (obj is Customer c)
```

### Switch Expressions
```csharp
var result = input switch
{
    > 0 => "Positive",
    < 0 => "Negative",
    _ => "Zero"
};
```

### Property Pattern
```csharp
if (order is { Amount: > 1000 })
```

> Pattern matching reduces casting and improves readability.

---

## with Expressions (Records)

```csharp
var updated = person with { Age = 40 };
```

- Non-destructive mutation
- Works best with immutable types

---

## init-only Setters

```csharp
public class User
{
    public string Name { get; init; }
}
```

- Allows setting properties only during initialization
- Encourages immutability

---

## required Members (C# 11)

```csharp
public class Order
{
    public required int Id { get; init; }
}
```

- Enforces object initialization correctness at compile time

---

## Top-level Statements

```csharp
Console.WriteLine("Hello");
```

- Simplifies Program.cs
- Common in minimal APIs

---

## Target-typed new

```csharp
List<int> numbers = new();
```

- Reduces verbosity
- Improves readability

---

## File-scoped Namespaces

```csharp
namespace MyApp.Services;
```

- Cleaner files
- Less indentation

---

## Default Interface Methods

```csharp
interface ILogger
{
    void Log(string msg);
    void LogInfo(string msg) => Log($"INFO: {msg}");
}
```

- Enables interface evolution
- Use cautiously

---

## Records vs Classes (Interview Favorite)

| Feature | Record | Class |
|------|-------|------|
| Equality | Value-based | Reference-based |
| Immutability | Default | Manual |
| Use case | DTO, Models | Behavior-rich objects |

---

## Common Interview Pitfalls

- Ignoring nullable warnings
- Overusing records
- Misusing default interface methods
- Pattern matching for trivial logic

---

## 30-Second Interview Summary

> Modern C# introduces features like nullable reference types, pattern matching, records, init-only setters, and required members to improve safety, readability, and correctness. Understanding when and why to use these features helps write cleaner, more maintainable, and less error-prone code.
