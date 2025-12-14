# LINQ (Language Integrated Query)

## What is LINQ?
LINQ is a set of features that introduces **query capabilities directly into C#** using strongly typed expressions. It allows querying **in-memory collections, XML, databases, and remote providers** in a unified way.

> LINQ is not a database feature — it is a **C# language + library feature**.

---

## Core Components

### 1. IEnumerable<T> vs IQueryable<T>

| Aspect | IEnumerable<T> | IQueryable<T> |
|------|---------------|---------------|
| Execution | In-memory | Translated to provider query |
| Provider | LINQ to Objects | EF Core, LINQ to SQL |
| Evaluation | Deferred | Deferred (remote) |
| Performance | Pulls data first | Optimized query execution |

> **Rule of thumb:** Use `IQueryable` only when query translation matters.

---

## Deferred vs Immediate Execution

### Deferred Execution
Query executes **only when enumerated**:
```csharp
var query = numbers.Where(n => n > 10); // Not executed
foreach (var n in query) { }           // Executed here
```

### Immediate Execution
Executes immediately:
```csharp
var list = numbers.Where(n => n > 10).ToList();
```

Common immediate operators:
- `ToList()`, `ToArray()`
- `Count()`, `Any()`, `First()`

---

## Query Syntax vs Method Syntax

### Query Syntax
```csharp
var result = from n in numbers
             where n > 10
             select n;
```

### Method Syntax (Preferred in Interviews)
```csharp
var result = numbers.Where(n => n > 10);
```

> Query syntax compiles into method syntax.

---

## Commonly Used Operators

### Filtering & Projection
- `Where`
- `Select`
- `SelectMany`

### Aggregation
- `Count`, `Sum`, `Average`, `Min`, `Max`

### Ordering
- `OrderBy`, `ThenBy`, `OrderByDescending`

### Set Operations
- `Distinct`
- `Union`, `Intersect`, `Except`

### Partitioning
- `Take`, `Skip`, `TakeWhile`, `SkipWhile`

---

## Multiple Enumeration Pitfall

```csharp
var query = users.Where(u => u.IsActive);

var count = query.Count();
var list = query.ToList(); // Executes again
```

### Fix
```csharp
var list = users.Where(u => u.IsActive).ToList();
var count = list.Count;
```

---

## Side Effects in LINQ (Anti-pattern)

```csharp
numbers.Select(n => { total += n; return n; }); // ❌
```

> LINQ should be **pure and side-effect free**.

---

## LINQ and Performance

- Prefer `Any()` over `Count() > 0`
- Use `Select` before `Where` only when reducing payload
- Avoid `ToList()` too early
- Be careful with `Contains()` on large collections

---

## LINQ with EF Core (Interview Favorite)

```csharp
context.Users
    .Where(u => u.IsActive)
    .Select(u => new { u.Id, u.Name })
```

- Expression trees are translated to SQL
- Not all .NET methods are translatable
- Client-side evaluation can kill performance

---

## Advanced Topics

- Expression Trees (`Expression<Func<T>>`)
- Custom LINQ providers
- Compiled queries (EF Core)

---

## 30-Second Interview Summary

> LINQ is a C# feature that enables strongly typed querying over various data sources. It supports deferred execution, expressive filtering, projection, and aggregation. Understanding `IEnumerable` vs `IQueryable`, execution timing, and performance pitfalls is critical for writing efficient LINQ code, especially with EF Core.

