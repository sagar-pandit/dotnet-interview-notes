# Delegates and Events in C#

## Interview Question
**Explain delegates and events in C#. How are they used and what are common interview traps?**

---

## Delegates

### What is a Delegate?
- A **delegate** is a type-safe object that **holds a reference to a method**
- Allows methods to be passed as parameters
- Supports single-cast and multi-cast invocation

### Syntax
```csharp
public delegate void Notify(string message);

Notify notify = Display;
notify("Hello World");

void Display(string msg) => Console.WriteLine(msg);
```

### Types of Delegates
- **Single-cast**: Holds reference to one method
- **Multi-cast**: Can hold multiple method references (`+=` / `-=`)
- **Func<T>, Action<T>, Predicate<T>**: Built-in generic delegates

---

## Events

### What is an Event?
- **Events** are a special delegate used for **publisher-subscriber** patterns
- Only the declaring class can invoke the event, ensuring encapsulation

### Syntax
```csharp
public class Publisher {
    public event EventHandler OnChange;
    public void RaiseEvent() => OnChange?.Invoke(this, EventArgs.Empty);
}

Publisher pub = new Publisher();
pub.OnChange += (s, e) => Console.WriteLine("Event fired!");
pub.RaiseEvent();
```

### Key Points
- Events wrap delegates to restrict access
- Supports multi-cast subscriptions
- EventHandlers often use `EventArgs` or custom derived types

---

## Lambdas and Anonymous Methods

- **Anonymous methods**: inline delegate implementation
```csharp
Notify notify = delegate(string msg) { Console.WriteLine(msg); };
```
- **Lambda expressions**: concise syntax for inline delegates
```csharp
Notify notify = msg => Console.WriteLine(msg);
```
- Often used with LINQ and events

---

## Func, Action, Predicate

### Action
- No return value
- Can take 0+ parameters
```csharp
Action<string> print = s => Console.WriteLine(s);
```

### Func
- Returns a value
- Last type parameter is the return type
```csharp
Func<int,int,int> add = (a,b) => a+b;
```

### Predicate
- Returns a boolean
- One input parameter
```csharp
Predicate<int> isEven = x => x%2==0;
```

---

## Common Interview Pitfalls

- Confusing delegate and event (anyone can call a delegate but only class can fire an event)
- Forgetting to unsubscribe events → memory leaks
- Not knowing built-in delegates (`Func`, `Action`, `Predicate`)
- Multi-cast delegate return value: only last method’s return is preserved

---

## Performance Considerations

- Delegate invocation slightly slower than direct method call
- Avoid large numbers of multi-cast subscriptions in tight loops
- Always unsubscribe events in long-lived objects to prevent memory leaks

---

## Common Interview Follow-ups

- Difference between delegate and event
- How multicast delegates behave
- When to use Action vs Func vs Predicate
- Event memory leak scenarios
- Lambda capturing and closures (memory implications)

---

## 30-Second Interview Summary

> Delegates are type-safe references to methods, allowing methods to be passed as parameters. Events are encapsulated delegates for the publisher-subscriber pattern. Lambdas and anonymous methods simplify delegate syntax. Built-in delegates (Func, Action, Predicate) provide reusable generic signatures. Always manage event subscriptions carefully to avoid memory leaks and understand the behavior of multicast delegates for predictable outcomes.

