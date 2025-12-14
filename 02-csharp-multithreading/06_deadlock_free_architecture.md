# 06 – Deadlock‑Free Architecture

## Why Think About Architecture?

Deadlocks are **design problems**, not just coding mistakes. Fixing them late is expensive.

---

## Core Rules for Deadlock‑Free Design

### 1. Minimize Shared State
- Prefer immutable data
- Pass data, don’t share it

### 2. Avoid Nested Locks

```csharp
lock (a)
{
    lock (b) // danger
    {
    }
}
```

### 3. Lock Ordering

Always acquire locks in the **same global order**.

```csharp
lock (lockA)
{
    lock (lockB)
    {
    }
}
```

---

## Use Message Passing Instead of Locks

### Why?
- No shared state
- No lock contention

---

## Mermaid Diagram – Message Passing

```mermaid
flowchart LR
    ComponentA -->|Message| Queue
    Queue --> ComponentB
```

---

## Prefer High‑Level Concurrency

| Instead of | Use |
|---------|-----|
lock | Concurrent collections |
Thread | Task |
Shared memory | Message queues |

---

## Timeout & Cancellation

```csharp
if (!Monitor.TryEnter(_lock, 1000))
{
    throw new TimeoutException();
}
```

---

## Real‑World Architecture Examples
- Actor model systems
- Event‑driven microservices
- Data pipelines

---

## Hands‑on Exercise
1. Identify shared state in your app
2. Replace with message passing
3. Remove explicit locks

---

## Interview Quick Answers
- **Deadlock prevention**: Lock ordering
- **Best strategy**: Avoid shared state

---

## Key Takeaways
- Deadlock prevention starts at design time
- Architecture beats clever locking

