# Memory Management in .NET

## Interview Question
**Explain memory management in .NET, including stack vs heap and common memory issues.**

---

## Overview

Memory management in .NET is handled by the **CLR (Common Language Runtime)** using a combination of:
- Stack allocation
- Managed heap allocation
- Garbage Collection (GC)

Understanding *where* objects live and *how long* they live is critical for performance and avoiding memory leaks.

---

## Stack Memory

### Characteristics
- Stores **method call frames**
- Stores **local variables of value types**
- Allocation and deallocation are very fast (LIFO)
- Automatically cleaned up when method exits

### What lives on the stack?
- Value type locals (`int`, `struct`, `enum`)
- Method parameters
- References to heap objects (not the objects themselves)

---

## Heap Memory (Managed Heap)

### Characteristics
- Stores **reference types**
- Memory managed by Garbage Collector
- Slower than stack allocation

### What lives on the heap?
- Objects created using `new`
- Arrays
- Strings (immutable)
- Boxed value types

---

## Value Types vs Reference Types

### Value Types
- Stored on stack (usually)
- Copied by value
- Short-lived

### Reference Types
- Stored on heap
- Passed by reference
- Require GC tracking

> **Important:** Location depends on *usage*, not just type.

---

## Boxing and Unboxing

### Boxing
- Converts value type → reference type
- Allocates object on heap
- Performance cost

### Unboxing
- Converts reference type → value type
- Requires explicit cast

### Interview Trap
- Boxing increases GC pressure

---

## Strings and Immutability

- Strings are reference types
- Immutable by design
- Frequent string concatenation creates garbage

### Best Practices
- Use `StringBuilder`
- Use string interpolation carefully

---

## Object Lifetime & GC Impact

- Short-lived objects → Gen 0
- Long-lived objects → Gen 2
- Long lifetimes increase GC cost

---

## Common Causes of Memory Leaks in Managed Code

### Static References
- Objects referenced by static fields live for app lifetime

### Event Handlers
- Publisher holds reference to subscriber
- Forgetting to unsubscribe causes leaks

### Closures
- Lambdas capture variables
- Captured objects may live longer than expected

### Caching
- Unbounded caches
- Improper eviction policies

---

## IDisposable & Unmanaged Resources

- GC does NOT manage unmanaged resources
- Examples:
  - File handles
  - DB connections
  - OS resources

### Best Practices
- Implement `IDisposable`
- Use `using` / `await using`
- Avoid relying on finalizers

---

## Structs vs Classes (Memory Perspective)

### Structs
- Allocated inline
- No GC tracking
- Copy cost

### Classes
- Heap allocated
- GC managed
- Reference semantics

### Interview Tip
- Prefer structs for small, immutable data

---

## Memory Optimization Techniques

- Reduce allocations
- Object pooling
- Use `ArrayPool<T>`
- Use `Span<T>` / `Memory<T>` for slicing
- Avoid unnecessary boxing

---

## Diagnosing Memory Issues

- `dotnet-counters`
- `dotnet-dump`
- `dotnet-trace`
- PerfView

---

## Common Interview Follow-ups

- Why can managed applications leak memory?
- Difference between stack and heap?
- What happens during boxing?
- How do closures affect memory?
- When should structs be avoided?

---

## 30-Second Interview Summary

> .NET manages memory using stack and heap allocations. Value types typically live on the stack, while reference types are allocated on the managed heap and tracked by the garbage collector. Excessive allocations, boxing, static references, event handlers, and closures can lead to memory pressure or leaks. Proper use of IDisposable, pooling, and modern features like Span helps optimize memory usage.
