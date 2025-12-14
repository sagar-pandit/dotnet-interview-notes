# Latest C# Language Features (As of December 2025)

This document summarizes **recent and modern C# features (C# 13 & C# 14)** with **concise code snippets** and **interview-oriented explanations**.

---

## C# 14 (with .NET 10)

### 1. Extension Members (Methods, Properties, Operators)

```csharp
public static class StringExtensions
{
    public static int WordCount(this string s)
        => s.Split(' ', StringSplitOptions.RemoveEmptyEntries).Length;
}

int count = "Hello modern C#".WordCount();
```

Why it matters:
- Groups extension logic cleanly
- Reduces utility-class clutter

---

### 2. Null-Conditional Assignment

```csharp
person?.Name ??= "Unknown";
```

Why it matters:
- Eliminates verbose null checks
- Cleaner defensive coding

---

### 3. nameof with Unbound Generics

```csharp
string typeName = nameof(List<>); // "List"
```

Why it matters:
- Useful in logging, diagnostics, and source generators

---

### 4. Improved Span<T> / ReadOnlySpan<T> Conversions

```csharp
void Process(ReadOnlySpan<char> span) { }

string text = "Hello";
Process(text); // implicit conversion
```

Why it matters:
- Less boilerplate
- Fewer allocations in hot paths

---

### 5. Lambda Parameter Modifiers (ref / in / scoped)

```csharp
Func<ref int, int> increment = ref int x => ++x;
```

Why it matters:
- High-performance, allocation-aware lambdas

---

### 6. Field-backed Properties (`field` keyword)

```csharp
public int Age
{
    get => field;
    set => field = value < 0 ? 0 : value;
}
```

Why it matters:
- Avoids manual backing fields
- Cleaner encapsulation

---

### 7. Partial Events & Constructors

```csharp
public partial class Logger
{
    public partial Logger();
}

public partial class Logger
{
    public partial Logger()
    {
        Console.WriteLine("Initialized");
    }
}
```

Why it matters:
- Source generators & large codebases

---

### 8. User-defined Compound Assignment Operators

```csharp
public struct Counter
{
    public int Value;
    public static Counter operator +(Counter a, int b)
        => new() { Value = a.Value + b };
}

var c = new Counter();
c += 5;
```

Why it matters:
- Better operator semantics for domain types

---

## C# 13 (with .NET 9)

### 9. Params Collections (Beyond Arrays)

```csharp
void Log(params ReadOnlySpan<string> messages)
{
    foreach (var m in messages)
        Console.WriteLine(m);
}

Log("A", "B", "C");
```

Why it matters:
- Allocation-free variadic APIs

---

### 10. New Lock Type (`System.Threading.Lock`)

```csharp
private readonly Lock _lock = new();

lock (_lock)
{
    // thread-safe code
}
```

Why it matters:
- Better diagnostics and safety than `object`

---

### 11. ESC (`\e`) Escape Sequence

```csharp
string esc = "\e[31mRed Text\e[0m";
```

Why it matters:
- Terminal / ANSI control sequences

---

### 12. Partial Properties & Indexers

```csharp
public partial class Config
{
    public partial int Timeout { get; set; }
}

public partial class Config
{
    public partial int Timeout { get; set; } = 30;
}
```

Why it matters:
- Code generation scenarios

---

### 13. ref / unsafe in async methods

```csharp
async Task ProcessAsync()
{
    Span<int> buffer = stackalloc int[10];
    await Task.Delay(10);
}
```

Why it matters:
- Performance + async together (with constraints)

---

### 14. ref struct Improvements

```csharp
ref struct Buffer<T>
{
    public Span<T> Data;
}
```

Why it matters:
- Enables high-performance abstractions

---

### 15. Overload Resolution Priority

```csharp
[OverloadResolutionPriority(1)]
void Log(object o) { }

[OverloadResolutionPriority(2)]
void Log(string s) { }
```

Why it matters:
- API evolution without breaking changes

---

## 30-Second Interview Summary

> As of December 2025, C# continues to evolve with features focused on safety, expressiveness, and performance. C# 13 and 14 introduce extension members, improved Span usage, lambda parameter modifiers, field-backed properties, partial constructs, advanced locking, and allocation-free APIs. These enhancements help write cleaner, faster, and more maintainable modern .NET applications.
