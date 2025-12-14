# How .NET Works – CLR (Common Language Runtime)

## Interview Question

**Explain how a .NET application executes from source code to runtime.**

---

## High-Level Flow

1. C# source code is compiled by the C# compiler into **IL (Intermediate Language)**.
2. IL, metadata, and a manifest are packaged into an **Assembly (DLL or EXE)**.
3. At runtime, the **CLR (Common Language Runtime)** loads the assembly.
4. CLR performs verification, type safety checks, and prepares the code for execution.
5. Methods are compiled into native machine code by the **JIT (Just-In-Time compiler)** when first invoked.
6. Native code is executed by the CPU while the CLR manages memory, garbage collection, exceptions, and threading.

---

## CLR Responsibilities

* Loading and executing assemblies
* Memory management via **GC (Garbage Collector)**
* Type safety and verification
* Exception handling
* Thread management
* Security enforcement

---

## Assemblies

* Physical unit of deployment (`.dll` or `.exe`)
* Contains:

  * IL (Intermediate Language)
  * Metadata (types, references)
  * Manifest (version, dependencies)

### Types of Assemblies

* **Private Assembly** – used by a single application
* **Shared Assembly** – stored in **GAC (Global Assembly Cache)**

---

## JIT (Just-In-Time Compiler)

* Converts IL to native machine code
* Compilation happens **method-by-method** on first execution

### JIT Variants

* **Normal JIT** – default
* **ReadyToRun (R2R)** – partially precompiled native code
* **AOT (Ahead-Of-Time)** – fully precompiled (Native AOT)

---

## Memory Model

### Stack

* Stores value types and method call frames
* Fast allocation and deallocation

### Heap

* Stores reference types
* Managed by the Garbage Collector

---

## Garbage Collection (GC)

### Generations

* **Gen 0** – short-lived objects
* **Gen 1** – medium-lived objects
* **Gen 2** – long-lived objects

### GC Modes

* **Workstation GC** – optimized for desktop apps
* **Server GC** – optimized for multi-core servers

---

## Finalizers & IDisposable

* Finalizers are called before object memory is reclaimed
* `IDisposable` allows deterministic resource cleanup
* `using` / `await using` is preferred

---

## Common Interview Follow-ups

* Why generational GC?
* Difference between stack and heap?
* When does JIT compilation occur?
* How can managed code still leak memory?

---

## 30-Second Interview Summary

> The C# compiler converts source code into IL, which is packaged into an assembly. At runtime, the CLR loads the assembly, verifies it, and manages execution. Methods are compiled into native code by the JIT compiler on first use. The CLR handles memory management, garbage collection, exception handling, and threading while the native code executes on the CPU.
