# async / await (Task-based Asynchronous Pattern)

## What is async / await?
`async` / `await` is C# syntax built on top of the **Task-based Asynchronous Pattern (TAP)** that allows writing **non-blocking asynchronous code** in a synchronous-looking style.

> async/await does **not create threads** — it orchestrates asynchronous work.

---

## Task-based Asynchronous Pattern (TAP)

- Introduced in .NET 4.0
- Uses `Task` and `Task<T>` to represent async operations
- Preferred over older patterns:
  - APM (Begin/End)
  - EAP (event-based async)

---

## How async / await Works Internally

1. Method marked `async`
2. Compiler rewrites method into a **state machine**
3. `await` checks:
   - If task is completed → continue synchronously
   - If not → register continuation and return
4. Continuation resumes when task completes

> This is **compiler magic**, not CLR magic.

---

## async Method Signatures

```csharp
async Task DoWorkAsync()
async Task<int> GetValueAsync()
async ValueTask<int> GetValueFastAsync()
```

Avoid:
```csharp
async void DoWork() // ❌ except event handlers
```

---

## Task vs ValueTask

| Aspect | Task | ValueTask |
|-----|------|-----------|
| Allocation | Heap | Stack (sometimes) |
| Reusability | Yes | No (careful!) |
| Complexity | Simple | Advanced |

> Use `ValueTask` only in **high-performance, hot paths**.

---

## IO-bound vs CPU-bound Work

### IO-bound (use async)
```csharp
await httpClient.GetAsync(url);
```

### CPU-bound (use Task.Run)
```csharp
await Task.Run(() => Compute());
```

> async does not make CPU work faster.

---

## SynchronizationContext (Interview Favorite)

- Captures context (UI thread, ASP.NET request)
- By default, `await` resumes on captured context

### Avoiding Context Capture
```csharp
await task.ConfigureAwait(false);
```

Use in:
- Libraries
- ASP.NET Core (mostly unnecessary but still valid)

---

## Deadlocks (Very Common Interview Question)

```csharp
var result = GetDataAsync().Result; // ❌
```

Cause:
- Blocking a thread waiting for async continuation
- Continuation tries to resume on same context

### Fix
```csharp
await GetDataAsync();
```

---

## Parallelism vs Asynchrony

- **Async** → waiting efficiently
- **Parallel** → doing multiple things at once

```csharp
await Task.WhenAll(task1, task2);
```

---

## Exception Handling in async

```csharp
try
{
    await DoWorkAsync();
}
catch (Exception ex)
{
}
```

- Exceptions are captured in the Task
- `Task.WhenAll` aggregates exceptions

---

## Cancellation Support

```csharp
var cts = new CancellationTokenSource();
await DoWorkAsync(cts.Token);
```

- Cooperative cancellation
- Use `ThrowIfCancellationRequested()`

---

## Common async Mistakes

- Mixing async and blocking calls
- Overusing `Task.Run`
- Fire-and-forget `async void`
- Forgetting cancellation

---

## 30-Second Interview Summary

> async/await is a C# language feature built on the Task-based Asynchronous Pattern. It enables non-blocking asynchronous programming using compiler-generated state machines. Understanding SynchronizationContext, deadlocks, IO vs CPU-bound work, and proper exception and cancellation handling is essential for writing scalable and correct async code.

