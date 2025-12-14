# C# Multithreading – Complete Step-by-Step Guide

> **Audience**: Beginner to Advanced .NET developers  
> **Goal**: Clear understanding of multithreading concepts, safe usage, real-world patterns, debugging, and interview readiness

---

## Table of Contents
1. Introduction to Multithreading
2. Process vs Thread
3. Single Thread vs Multithreading
4. Creating Threads in C#
5. Race Conditions
6. Synchronization and Locks
7. Deadlocks (with Debugging)
8. Mutex vs Semaphore
9. Task Parallel Library (TPL)
10. Async and Await
11. Producer–Consumer Pattern
12. Deadlock-Free Architecture
13. Refactoring Sync Code to Async
14. Advanced Concurrency Patterns
    - Pipeline Pattern
    - Actor Pattern
15. Summary Mental Models
16. Question Bank (Beginner → Advanced)

---

## 1. Introduction to Multithreading

### What?
Multithreading allows **multiple threads (units of execution)** to run concurrently within a **single process**.

### Why?
- Better CPU utilization
- Improved responsiveness
- Higher throughput

### How?
C# provides:
- `Thread`
- `Task` (Task Parallel Library)
- `async` / `await`
- Synchronization primitives

---

## 2. Process vs Thread

| Process | Thread |
|------|------|
Independent running application | Execution unit inside a process |
Own memory space | Shared memory |
Heavyweight | Lightweight |

**Analogy**: Process = House, Thread = People inside

---

## 3. Single Thread vs Multithreading

### Single Threaded
```
Task A → Task B → Task C
```

### Multithreaded
```
Task A ────────
Task B ────────
Task C ────────
```

---

## 4. Creating Threads in C#

```csharp
Thread worker = new Thread(DoWork);
worker.Start();

void DoWork()
{
    Thread.Sleep(1000);
    Console.WriteLine("Work done");
}
```

---

## 5. Race Conditions

### What?
Multiple threads accessing shared data simultaneously causing incorrect results.

```csharp
counter++;
```
Not atomic → causes race condition.

---

## 6. Synchronization and Locks

```csharp
lock(lockObject)
{
    counter++;
}
```

### Key Rules
- Use same lock object
- Keep lock scope minimal

---

## 7. Deadlocks (with Debugging)

### Deadlock Example
```csharp
lock(A)
{
    lock(B)
    {
    }
}
```

### Debugging in Visual Studio
1. Debug → Break All
2. Debug → Windows → Threads
3. Inspect Call Stack

### Fix
- Consistent lock ordering
- Timeout-based locking

---

## 8. Mutex vs Semaphore

### Full Forms
- Mutex → Mutual Exclusion
- Semaphore → Signaling Mechanism

| Feature | Mutex | Semaphore |
|------|------|------|
Access | One | N |
Ownership | Yes | No |
Use case | Single resource | Resource pool |

---

## 9. Task Parallel Library (TPL)

```csharp
await Task.Run(() => DoWork());
```

### Benefits
- Thread pool
- Better scalability
- Exception handling

---

## 10. Async and Await

```csharp
public async Task DownloadAsync()
{
    await Task.Delay(1000);
}
```

### Key Rule
> Never block async code using `.Result` or `.Wait()`

---

## 11. Producer–Consumer Pattern

```csharp
BlockingCollection<int> queue = new(5);
```

### Why BlockingCollection?
- Thread-safe
- Built-in blocking
- No manual locks

---

## 12. Deadlock-Free Architecture

### Core Rules
- Single lock per resource
- Lock ordering
- Message passing over shared state
- Immutable data

---

## 13. Refactoring Sync Code to Async

### Legacy (❌)
```csharp
var data = GetDataAsync().Result;
```

### Correct (✅)
```csharp
var data = await GetDataAsync();
```

### Golden Rule
> Async must propagate upwards

---

## 14. Advanced Concurrency Patterns

### 14.1 Pipeline Pattern

```csharp
read.LinkTo(process);
process.LinkTo(save);
```

### Use Case
High-throughput sequential stages

---

### 14.2 Actor Pattern

```csharp
actor.Tell(() => UpdateState());
```

### Benefits
- No locks
- No race conditions
- No deadlocks

---

## 15. Summary Mental Models

- Locks protect data
- Tasks manage concurrency
- Async improves scalability
- Message passing removes locks

---

## 16. Question Bank

### Beginner
1. What is a thread?
2. Difference between process and thread?
3. What is race condition?

### Intermediate
4. What causes deadlock?
5. Mutex vs Semaphore?
6. Why avoid Thread.Sleep?

### Advanced
7. Can async cause deadlock?
8. How to debug deadlock in production?
9. When to avoid locks?
10. Explain Actor pattern benefits

---

**End of Document**

