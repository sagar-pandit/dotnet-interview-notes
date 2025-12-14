# Collections and Generics in C#

## Interview Question
**Explain C# collections and generics. When would you use each, and why are generics important?**

---

## Overview

C# provides **collections** to store and manage groups of objects. **Generics** provide type safety, reduce boxing/unboxing, and improve performance. Together, they form the backbone of efficient data structures in .NET.

---

## Collection Types in C#

### 1. Arrays
- Fixed size
- Strongly typed
- Fast indexing (`O(1)`)

```csharp
int[] numbers = new int[5];
numbers[0] = 10;
```

### 2. List<T>
- Dynamic array
- Generic version avoids boxing/unboxing
- Common methods: `Add`, `Remove`, `Contains`, `Sort`

```csharp
List<string> names = new List<string>();
names.Add("Alice");
```

### 3. Dictionary<TKey, TValue>
- Key-value pairs
- Fast lookup (`O(1)` average)
- Throws if key already exists unless using `TryAdd`

```csharp
Dictionary<int, string> dict = new Dictionary<int, string>();
dict[1] = "One";
```

### 4. HashSet<T>
- Unordered, unique elements
- Fast membership test

```csharp
HashSet<int> set = new HashSet<int>();
set.Add(10);
```

### 5. Queue<T> and Stack<T>
- FIFO: Queue
- LIFO: Stack
- Useful for algorithms and processing sequences

---

## Generics in C#

### Why Generics?
- **Type safety** at compile time
- Avoids **boxing/unboxing** for value types
- Improves **code reuse** and maintainability
- Better **runtime performance**

### Generic Classes & Methods
```csharp
public class Repository<T> {
    private List<T> items = new List<T>();
    public void Add(T item) => items.Add(item);
}
```

### Generic Interfaces
```csharp
IEnumerable<T>, ICollection<T>, IList<T>, IDictionary<TKey,TValue>
```

### Covariance & Contravariance
- **Covariance (`out`)**: Allows assignment of more derived types
- **Contravariance (`in`)**: Allows assignment of less derived types

```csharp
IEnumerable<string> strings = new List<string>(); // covariant
Action<object> act = (Action<string>)SomeMethod; // contravariant
```

---

## Performance Considerations

- Prefer generic collections over non-generic (`ArrayList`, `Hashtable`)
- `List<T>` vs array: dynamic resizing has cost
- Use `Span<T>` / `Memory<T>` for high-performance slicing
- Avoid frequent reallocations for large collections

---

## Common Interview Follow-ups

- Difference between Array and List<T>
- Why HashSet<T> is faster than List<T> for membership checks
- When to use Dictionary vs SortedDictionary
- Boxing/unboxing impact
- Covariance vs contravariance practical examples

---

## 30-Second Interview Summary

> C# collections provide structures to store and manage multiple objects efficiently. Generics allow type-safe, reusable, and performant collections. Arrays are fixed-size, Lists are dynamic, Dictionaries provide key-value mapping, and HashSets ensure uniqueness. Covariance and contravariance enable safe assignment between compatible generic types. Using generics avoids boxing/unboxing and improves runtime performance.