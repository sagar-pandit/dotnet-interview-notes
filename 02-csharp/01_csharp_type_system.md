# C# Type System

## Interview Question
**Explain the C# type system, including value types, reference types, and type behaviors.**

---

## Overview

C# has a **strongly-typed, statically-typed system** that enforces type safety at compile time. Types in C# are divided broadly into:

1. **Value Types**
2. **Reference Types**
3. **Pointer Types** (unsafe code)

Understanding the type system is fundamental for memory management, performance, and correct program behavior.

---

## Value Types

### Characteristics
- Stored **inline** (usually on the stack)
- Hold the actual data
- Copied by **value** when assigned or passed
- Cannot be null (nullable available via `Nullable<T>` / `T?`)

### Common Value Types
- Primitive types: `int`, `double`, `bool`, `char`
- Structs: `struct` (custom value types)
- Enumerations: `enum`

### Behavior
```csharp
int x = 10;
int y = x; // copy of value
x = 20;    // y remains 10
```

> Copy-by-value ensures independent copies.

---

## Reference Types

### Characteristics
- Stored on the **heap**
- Variables hold a **reference (pointer)** to the data
- Copied by reference when assigned or passed
- Can be null

### Common Reference Types
- Classes: `class`
- Interfaces: `interface`
- Arrays: `int[]`
- Delegates: `delegate`
- Strings: `string` (immutable)

### Behavior
```csharp
class Person { public string Name; }
Person p1 = new Person { Name = "Alice" };
Person p2 = p1;
p2.Name = "Bob";
// p1.Name is also "Bob" because both reference the same object
```

> Copy-by-reference shares the same underlying object.

---

## Nullable Types

- Value types can be made nullable using `T?` or `Nullable<T>`
- Commonly used for database mapping or optional values

```csharp
int? age = null;
if (age.HasValue) { Console.WriteLine(age.Value); }
```

---

## Pointer Types (Unsafe)

- Only allowed in `unsafe` context
- Provides low-level memory access
- Rarely used in high-level applications

```csharp
unsafe {
   int x = 10;
   int* ptr = &x;
   *ptr = 20;
}
```

---

## Boxing and Unboxing

- **Boxing**: value type → reference type (object)
- **Unboxing**: reference type → value type

**Performance impact:** Boxing creates objects on the heap and increases GC pressure

```csharp
int x = 42;
object o = x; // boxing
int y = (int)o; // unboxing
```

---

## Type Safety

- C# is strongly typed: implicit conversions only allowed when safe
- `as` and `is` operators help with safe casting
- `dynamic` bypasses compile-time type checking (runtime only)

---

## Common Interview Follow-ups

- Value vs reference types – memory and behavior differences
- Stack vs heap allocation
- Boxing/unboxing implications
- Nullable types usage
- Why string is a reference type but behaves immutably

---

## 30-Second Interview Summary

> C# has a strong, statically-typed system. Value types store data inline and are copied by value, whereas reference types store a reference to heap objects and are copied by reference. Nullable types allow value types to represent null. Boxing and unboxing convert between value and reference types, impacting performance. Understanding these fundamentals is crucial for memory management, performance, and safe coding practices.

