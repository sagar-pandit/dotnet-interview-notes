# Exception Handling in C#

Exception handling in C# is a **core reliability and correctness topic** and is frequently used to assess **maturity and judgment** in senior-level interviews.

---

## What is an Exception?

An exception represents an **unexpected or erroneous condition** that disrupts the normal flow of program execution.

- All exceptions derive from `System.Exception`
- Exceptions are **expensive** (stack unwinding, allocation)

---

## try / catch / finally

```csharp
try
{
    DoWork();
}
catch (InvalidOperationException ex)
{
    // handle specific exception
}
finally
{
    // always executed
}
```

### finally block
- Executes regardless of exception
- Used for cleanup (when `using` is not applicable)

---

## Exception Hierarchy (Important)

```
System.Exception
 ├── System.SystemException
 │    ├── ArgumentException
 │    ├── InvalidOperationException
 │    └── NullReferenceException
 └── ApplicationException (rarely used)
```

> **Best practice:** Catch the most specific exception possible.

---

## Exception Filters (C# 6+)

```csharp
catch (SqlException ex) when (ex.Number == 1205)
{
}
```

Benefits:
- Avoids rethrowing
- Cleaner logic
- No stack trace loss

---

## Rethrowing Exceptions (Very Common Interview Trap)

```csharp
catch (Exception ex)
{
    throw;    // ✅ preserves stack trace
    // throw ex; ❌ resets stack trace
}
```

---

## Custom Exceptions

```csharp
public class BusinessRuleException : Exception
{
    public BusinessRuleException(string message) : base(message) {}
}
```

Guidelines:
- Suffix with `Exception`
- Make them meaningful
- Avoid deep inheritance

---

## When to Throw Exceptions

Throw exceptions for:
- Invalid state
- Broken invariants
- Programmer errors

Do NOT throw exceptions for:
- Normal control flow
- Validation in hot paths

---

## Checked vs Unchecked (Conceptual)

- C# has **only unchecked exceptions**
- Compiler does not force handling

---

## Performance Considerations

- Throwing exceptions is expensive
- Prefer guard clauses

```csharp
if (!isValid)
    return false; // better than exception
```

---

## async / await and Exceptions

```csharp
await DoWorkAsync();
```

- Exceptions are captured in `Task`
- Observed when awaited
- `Task.WhenAll` aggregates exceptions

---

## Global Exception Handling

- ASP.NET Core: Middleware
- Console apps: `AppDomain.UnhandledException`
- TaskScheduler.UnobservedTaskException

---

## Common Interview Mistakes

- Catching `Exception` everywhere
- Swallowing exceptions
- Logging and rethrowing incorrectly
- Using exceptions for flow control

---

## 30-Second Interview Summary

> Exception handling in C# is used to manage unexpected runtime failures. Best practices include catching specific exceptions, preserving stack traces when rethrowing, avoiding exceptions for control flow, and understanding how exceptions behave in async code. Proper exception handling improves reliability without sacrificing performance.

