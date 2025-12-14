# JIT, ReadyToRun, and AOT in .NET

## Interview Question
**Explain JIT compilation in .NET and compare it with ReadyToRun and AOT.**

---

## What is JIT?

**JIT (Just-In-Time compiler)** is a component of the **CLR (Common Language Runtime)** that converts **IL (Intermediate Language)** into **native machine code** at runtime.

Compilation happens **method-by-method** when a method is called for the first time.

---

## Why JIT?

- Platform independence (IL → native at runtime)
- Runtime optimizations based on actual environment
- Smaller deployment size

---

## JIT Compilation Flow

1. Application starts
2. CLR loads assemblies
3. Method is invoked
4. JIT compiles method IL to native code
5. Native code is cached in memory
6. Subsequent calls execute native code directly

---

## JIT Optimizations

- Inlining
- Dead code elimination
- Bounds check elimination
- Register allocation
- CPU-specific optimizations

> JIT can optimize based on actual CPU architecture.

---

## Downsides of JIT

- Startup overhead
- First-call latency
- Cold start impact

---

## ReadyToRun (R2R)

**ReadyToRun** is a precompilation technique where assemblies contain **pre-generated native code** alongside IL.

### Characteristics
- Faster startup
- Still contains IL
- JIT may recompile methods if needed

### When to use
- Large applications
- Faster startup scenarios

---

## AOT (Ahead-Of-Time Compilation)

**AOT (Ahead-Of-Time)** compiles IL into native code **before runtime**, eliminating JIT.

### Native AOT
- Fully native executable
- No JIT at runtime
- Smaller memory footprint
- Faster startup

### Limitations
- Limited reflection support
- Longer build time
- Less dynamic behavior

---

## JIT vs ReadyToRun vs AOT

| Feature | JIT | ReadyToRun | AOT |
|------|----|------------|-----|
| Startup time | Slower | Faster | Fastest |
| Runtime optimization | Best | Medium | Limited |
| Reflection | Full | Full | Limited |
| Deployment size | Small | Larger | Small |
| Runtime flexibility | High | Medium | Low |

---

## Tiered Compilation

- Starts with quick JIT
- Re-JITs hot methods with higher optimizations
- Improves startup and throughput

---

## Common Interview Pitfalls

- Assuming AOT replaces JIT everywhere
- Thinking ReadyToRun removes JIT completely
- Ignoring tiered compilation

---

## Common Interview Follow-ups

- When would you choose Native AOT?
- Why is JIT still relevant?
- How does tiered compilation work?
- How does JIT affect startup performance?

---

## 30-Second Interview Summary

> In .NET, the JIT compiler converts IL to native machine code at runtime on first execution of a method. This allows platform independence and runtime optimizations. ReadyToRun improves startup by precompiling some native code but still relies on JIT when needed. Native AOT fully compiles the application ahead of time, offering faster startup and lower memory usage at the cost of reduced runtime flexibility.

