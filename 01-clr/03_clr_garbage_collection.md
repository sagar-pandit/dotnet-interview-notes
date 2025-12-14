# Garbage Collection (GC) in .NET

## Interview Question
**Explain how Garbage Collection works in .NET.**

---

## What is Garbage Collection?

Garbage Collection is an automatic memory management mechanism provided by the **CLR (Common Language Runtime)**. It is responsible for allocating and reclaiming memory for **managed objects** on the heap.

The GC frees developers from manual memory deallocation and helps prevent common issues such as dangling pointers and double frees.

---

## Why Garbage Collection?

- Simplifies memory management
- Prevents memory corruption
- Improves application stability
- Optimizes performance using generational assumptions

**Key assumption:** Most objects die young.

---

## Managed Heap

- All **reference types** are allocated on the managed heap
- Heap is divided into **generations** to optimize collection

---

## Generational GC

### Generation 0 (Gen 0)
- Short-lived objects
- Collected very frequently
- Fastest collection

### Generation 1 (Gen 1)
- Acts as a buffer between Gen 0 and Gen 2
- Objects promoted from Gen 0

### Generation 2 (Gen 2)
- Long-lived objects
- Collected infrequently
- Most expensive collection

### Large Object Heap (LOH)
- Objects >= ~85,000 bytes
- Not compacted by default (prior to recent .NET improvements)
- Collected during Gen 2 collections

---

## GC Phases

1. **Mark** – Identifies live objects by walking object references
2. **Relocate (Compact)** – Moves live objects together
3. **Sweep** – Reclaims memory from dead objects

---

## Object Promotion

- Objects surviving a GC are **promoted** to the next generation
- Frequent survival increases cost of future collections

---

## Roots

GC determines object liveness using **GC Roots**:

- Stack variables
- Static fields
- CPU registers
- GC handles

Objects reachable from roots are considered alive.

---

## GC Modes

### Workstation GC
- Optimized for client applications
- Lower latency

### Server GC
- Optimized for server applications
- Uses multiple heaps
- Better throughput on multi-core machines

Configured via runtime settings.

---

## Background GC

- Introduced to reduce pause times
- Performs Gen 2 collection concurrently
- Improves responsiveness in server applications

---

## Finalization

- Objects with finalizers are placed on the **finalization queue**
- Finalizers run on a dedicated thread
- Delays memory reclamation

**Finalizers are expensive and unpredictable**

---

## IDisposable Pattern

- Provides deterministic cleanup of unmanaged resources
- Common examples:
  - File handles
  - Database connections
  - Network sockets

### Best Practice
- Prefer `IDisposable` over finalizers
- Use `using` / `await using`

---

## Common GC Problems (Interview Favorites)

### Managed Memory Leaks

Even with GC, leaks occur due to:
- Static references
- Event handlers not unsubscribed
- Long-lived caches
- Closures capturing objects

### LOH Fragmentation

- Frequent allocation/deallocation of large objects
- Can cause high memory usage

---

## Performance Considerations

- Avoid excessive allocations
- Reuse objects when possible
- Prefer structs for small, immutable data
- Use pooling (`ArrayPool<T>`, `ObjectPool<T>`)

---

## Common Interview Follow-ups

- Why is Gen 2 GC expensive?
- What is LOH and why is it special?
- Can value types be collected by GC?
- Difference between `Dispose` and finalizer?
- How can a GC cause application pauses?

---

## 30-Second Interview Summary

> .NET uses a generational garbage collector managed by the CLR. Objects are allocated on the managed heap and categorized into generations based on lifetime. The GC identifies live objects using roots, compacts memory, and reclaims unused space. Gen 0 collections are frequent and fast, while Gen 2 collections are expensive. For unmanaged resources, .NET relies on the IDisposable pattern rather than finalizers for deterministic cleanup.
