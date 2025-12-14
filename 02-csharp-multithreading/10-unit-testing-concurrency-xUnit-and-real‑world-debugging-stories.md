# 10 – Unit Testing Concurrency (xUnit) & Real‑World Debugging Stories

---

## Part A – Unit Testing Concurrency in C# (xUnit + async)

Concurrency bugs are **timing‑dependent**, so testing must focus on:
- Correctness under parallel execution
- Absence of deadlocks
- Deterministic assertions

---

## 1. Testing Async Methods (Correct Way)

### Rule
**Test methods must return `Task`, never `void`**.

```csharp
public async Task<int> AddAsync(int a, int b)
{
    await Task.Delay(50);
    return a + b;
}
```

```csharp
[Fact]
public async Task AddAsync_ReturnsSum()
{
    var result = await AddAsync(2, 3);
    Assert.Equal(5, result);
}
```

✔ Exceptions are captured  
✔ Test runner waits correctly

---

## 2. Testing Parallel Execution

```csharp
[Fact]
public async Task Counter_IsThreadSafe()
{
    int counter = 0;
    var tasks = Enumerable.Range(0, 1000)
        .Select(_ => Task.Run(() => Interlocked.Increment(ref counter)));

    await Task.WhenAll(tasks);

    Assert.Equal(1000, counter);
}
```

### Why `Interlocked`?
- Atomic operations
- Lock‑free
- Deterministic results

---

## 3. Testing Race Conditions (Demonstration Test)

```csharp
[Fact]
public async Task Counter_WithLock_IsCorrect()
{
    int counter = 0;
    object _lock = new();

    var tasks = Enumerable.Range(0, 1000)
        .Select(_ => Task.Run(() =>
        {
            lock (_lock)
            {
                counter++;
            }
        }));

    await Task.WhenAll(tasks);

    Assert.Equal(1000, counter);
}
```

---

## 4. Testing for Deadlocks (Timeout Pattern)

```csharp
[Fact]
public async Task Method_DoesNotDeadlock()
{
    var task = SomeAsyncMethod();

    var completed = await Task.WhenAny(task, Task.Delay(2000));

    Assert.Equal(task, completed);
}
```

✔ Prevents hanging test suite  
✔ Detects deadlock risk early

---

## 5. Common Testing Mistakes

❌ `.Wait()` or `.Result` in tests  
❌ `async void` tests  
❌ Using `Thread.Sleep` for synchronization

✔ Prefer `TaskCompletionSource` for coordination

---

## Part B – Real‑World Debugging Stories (Production Inspired)

---

## Story 1 – The Midnight Deadlock Incident

### Symptoms
- App frozen under load
- CPU low, threads high
- No exceptions

### Root Cause
```csharp
lock (cache)
{
    logger.Log(); // logger also used cache internally
}
```

✔ Hidden nested lock  
✔ Circular dependency

### Fix
- Removed logging from critical section
- Introduced async queue for logs

---

## Story 2 – Async Deadlock in Web API

### Code
```csharp
var data = service.GetDataAsync().Result;
```

### Why It Failed
- ASP.NET SynchronizationContext
- Thread blocked, continuation waiting

### Fix
```csharp
var data = await service.GetDataAsync();
```

---

## Story 3 – "It Works on My Machine"

### Issue
- Tests passing locally
- Failing intermittently on CI

### Root Cause
- Race condition masked by fast CPU

### Fix
- Removed shared mutable state
- Introduced `BlockingCollection`

---

## Story 4 – Thread Pool Starvation

### Symptoms
- Requests timing out
- No CPU spike

### Cause
```csharp
Task.Run(() => Thread.Sleep(5000));
```

✔ Blocking thread‑pool threads

### Fix
- Replaced with async I/O
- Removed long blocking calls

---

## Debugging Checklist (Production)

✔ Capture thread dumps  
✔ Inspect call stacks  
✔ Look for lock ordering issues  
✔ Search for `.Wait()` / `.Result`  
✔ Reduce critical section scope

---

## Key Takeaways

- Concurrency tests must be deterministic
- Timeouts protect test suites
- Most production deadlocks are **design mistakes**
- Async bugs hide until load increases

---

✅ You now know **how to test, debug, and reason about concurrency like a senior engineer**.

