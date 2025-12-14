# Memory Leaks in .NET (Managed Code)

## Interview Question
**If .NET has Garbage Collection, how can memory leaks still occur?**

---

## Short Answer (Interview Hook)

Garbage Collection only collects objects that are **no longer reachable**. If an object is still referenced—intentionally or unintentionally—the GC cannot reclaim it, leading to **managed memory leaks**.

---

## What is a Memory Leak in .NET?

A memory leak occurs when:
- Memory is no longer needed
- But objects remain reachable via references
- Therefore GC cannot collect them

This is different from unmanaged memory leaks, but just as dangerous in long-running applications.

---

## Common Causes of Memory Leaks

### 1. Static References

- Static fields live for the entire application lifetime
- Any object referenced by a static field is effectively permanent

**Example:**
- Static caches without eviction
- Static event handlers

---

### 2. Event Handler Leaks

- Publisher holds a strong reference to subscribers
- Forgetting to unsubscribe prevents GC

**Typical Scenario:**
- UI events
- Domain events
- Long-lived services

**Solution:**
- Unsubscribe explicitly
- Use weak events when applicable

---

### 3. Closures & Captured Variables

- Lambdas capture variables, not values
- Captured variables are promoted to heap objects

**Impact:**
- Objects live longer than expected
- Hidden references cause leaks

---

### 4. Long-Lived Caches

- Unbounded dictionaries
- No expiration policies

**Best Practices:**
- Use size limits
- Use time-based eviction
- Prefer `IMemoryCache`

---

### 5. IDisposable Misuse

- GC does NOT release unmanaged resources
- Not calling `Dispose` leads to resource leaks

**Examples:**
- Database connections
- File handles
- Network sockets

---

### 6. Finalizers

- Objects with finalizers survive at least one GC cycle
- Finalization delays memory reclamation

**Rule:** Avoid finalizers unless absolutely necessary

---

## Large Object Heap (LOH) Issues

- Large objects are rarely compacted
- Frequent allocations cause fragmentation
- Appears as memory leak

---

## Symptoms of Memory Leaks

- Gradual memory growth
- Increased Gen 2 collections
- Long GC pause times
- OutOfMemoryException

---

## Diagnosing Memory Leaks

### Tools

- `dotnet-counters`
- `dotnet-dump`
- `dotnet-trace`
- PerfView
- Visual Studio Diagnostic Tools

---

## What to Look For in Dumps

- High object counts
- Unexpected root references
- Static fields holding objects
- Event handler chains

---

## Prevention Best Practices

- Avoid unnecessary static state
- Always unsubscribe events
- Dispose deterministically
- Avoid long-lived object graphs
- Monitor memory in production

---

## Common Interview Follow-ups

- Why are event handlers a common leak source?
- How do closures cause leaks?
- What is the LOH and why is it problematic?
- How do you diagnose a production memory leak?

---

## 30-Second Interview Summary

> Although .NET uses garbage collection, memory leaks can still occur when objects remain reachable through references such as static fields, event handlers, closures, or unbounded caches. The GC can only collect unreachable objects. Diagnosing leaks involves analyzing GC roots and object lifetimes using tools like dotnet-dump and PerfView, and prevention requires careful resource management and deterministic disposal.

