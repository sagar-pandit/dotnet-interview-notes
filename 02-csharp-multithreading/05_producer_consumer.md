# 05 – Producer–Consumer Pattern

## What is Producer–Consumer?

A concurrency pattern where:
- **Producer** creates data
- **Consumer** processes data
- Shared buffer sits in between

---

## Why this Pattern?
- Decouples data creation & processing
- Improves throughput
- Controls resource usage

---

## Traditional Approach (Queue + lock)

```csharp
Queue<int> queue = new();
object _lock = new();

void Producer()
{
    lock (_lock)
    {
        queue.Enqueue(1);
    }
}

void Consumer()
{
    lock (_lock)
    {
        if (queue.Count > 0)
            queue.Dequeue();
    }
}
```

### Problems
- Busy waiting
- Complex coordination
- Easy to get wrong

---

## BlockingCollection<T>

### What?
Thread-safe, blocking collection

```csharp
BlockingCollection<int> buffer = new(5);

Task.Run(() =>
{
    for (int i = 0; i < 10; i++)
    {
        buffer.Add(i);
        Console.WriteLine($"Produced {i}");
    }
    buffer.CompleteAdding();
});

Task.Run(() =>
{
    foreach (var item in buffer.GetConsumingEnumerable())
    {
        Console.WriteLine($"Consumed {item}");
    }
});
```

---

## Why BlockingCollection is Better

| Feature | Queue + lock | BlockingCollection |
|------|------|------|
Thread Safety | Manual | Built-in |
Blocking | No | Yes |
Complexity | High | Low |

---

## Mermaid Diagram – Producer Consumer

```mermaid
flowchart LR
    Producer -->|Add| Buffer
    Buffer -->|Take| Consumer
```

---

## Real-world Example
- Logging systems
- Message processing
- Data pipelines

---

## Hands-on Exercises
1. Create producer generating numbers
2. Add artificial delay
3. Consume using BlockingCollection
4. Observe blocking behavior

---

## Interview Quick Answers
- **Producer–Consumer**: Decoupled concurrency pattern
- **BlockingCollection**: Thread-safe blocking buffer

---

## Key Takeaways
- Prefer BlockingCollection
- Avoid manual locking
- Pattern scales well
