# C# Memory & Performance Features

This section focuses on **modern C# features that directly impact memory allocation, performance, and low-level behavior**. These topics are increasingly common in **senior and staff-level interviews**.

---

## struct vs class (Value vs Reference Revisited)

### struct
- Allocated inline (stack or inside objects)
- Copied by value
- Cannot have parameterless constructor (before C# 10)
- Ideal for **small, immutable data**

### class
- Allocated on heap
- Passed by reference
- Supports inheritance

> **Interview tip:** Prefer `struct` only when size is small and lifetime is short.

---

## readonly struct

```csharp
public readonly struct Point
{
    public int X { get; }
    public int Y { get; }
}
```

- Prevents defensive copies
- Improves performance
- Encourages immutability

---

## ref, in, out Keywords

### ref
- Pass by reference
- Caller and callee share same variable

### in
- Pass by reference (read-only)
- Avoids copying large structs

### out
- Used for multiple return values

```csharp
void Process(in LargeStruct data)
```

---

## Records (C# 9+)

```csharp
public record Person(string Name, int Age);
```

- Reference type with **value-based equality**
- Immutability by default
- Ideal for DTOs and domain models

> Records reduce boilerplate and bugs.

---

## Span<T>

`Span<T>` represents a **contiguous region of memory**.

- Stack-only (`ref struct`)
- No heap allocation
- Cannot escape method boundary

```csharp
Span<int> buffer = stackalloc int[10];
```

Use cases:
- Parsing
- High-performance pipelines
- Avoiding allocations

---

## Memory<T>

- Heap-safe alternative to `Span<T>`
- Can cross async boundaries

```csharp
Memory<byte> memory;
```

---

## stackalloc

```csharp
Span<int> numbers = stackalloc int[100];
```

- Allocates memory on stack
- Extremely fast
- Use carefully to avoid stack overflow

---

## Immutability & Performance

Benefits:
- Thread safety
- Predictable behavior
- Reduced defensive copying

Techniques:
- readonly fields
- records
- immutable collections

---

## Boxing & Unboxing (Performance Cost)

```csharp
int x = 10;
object o = x; // Boxing
int y = (int)o; // Unboxing
```

- Causes heap allocation
- Avoid via generics

---

## Allocation-Free Patterns

- Reuse buffers
- Avoid closures in hot paths
- Use pooling (`ArrayPool<T>`)

---

## 30-Second Interview Summary

> Modern C# provides several features to write high-performance, allocation-efficient code. Understanding `struct` vs `class`, `Span<T>` vs `Memory<T>`, records, ref semantics, and immutability helps reduce GC pressure and improve throughput in performance-critical applications.
