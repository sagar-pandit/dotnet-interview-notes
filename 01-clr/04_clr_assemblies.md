# Assemblies in .NET

## Interview Question
**What is an Assembly in .NET? Explain DLL vs EXE and how assemblies are loaded.**

---

## What is an Assembly?

An **Assembly** is the smallest **versioning, deployment, and security unit** in .NET.

It is a logical container that includes:
- **IL (Intermediate Language)**
- **Metadata** (types, methods, references)
- **Manifest** (assembly identity)

Assemblies are produced by the compiler and executed by the **CLR (Common Language Runtime)**.

---

## Assembly Types

### 1. EXE Assembly
- Entry point (`Main` method)
- Can be executed directly
- Typically application-level

### 2. DLL Assembly
- No entry point
- Used as a reusable library
- Loaded by another assembly

> **Key point:** There is **no runtime difference** between DLL and EXE; only the presence of an entry point differs.

---

## Assembly Identity (Manifest)

The manifest contains:
- Assembly name
- Version (`Major.Minor.Build.Revision`)
- Culture
- Public key (for strong-named assemblies)
- Referenced assemblies

This identity is used by the CLR during loading and binding.

---

## Private vs Shared Assemblies

### Private Assembly
- Used by a single application
- Located in the application directory
- No registration required

### Shared Assembly
- Can be used by multiple applications
- Stored in **GAC (Global Assembly Cache)**
- Must be **strong-named**

---

## Strong-Named Assemblies

A strong-named assembly:
- Has a unique identity
- Is signed using a public/private key pair
- Prevents name collisions

### Why Strong Naming?
- Versioning support
- Security and integrity
- Required for GAC deployment

---

## GAC (Global Assembly Cache)

The **GAC (Global Assembly Cache)** is a machine-wide store for shared assemblies.

### Characteristics
- Stores strong-named assemblies only
- Supports side-by-side versioning
- Mostly relevant to .NET Framework (less used in modern .NET)

---

## Assembly Loading

### Load Contexts
- **Default load context**
- **Load-from context**
- **Custom load context** (`AssemblyLoadContext`)

### Loading Mechanism
1. CLR checks already loaded assemblies
2. Probes application base paths
3. Applies binding rules
4. Loads required assembly

---

## Assembly Binding & Versioning

- Binding redirects (config-based)
- Side-by-side version execution
- Assembly resolution failures

---

## Reflection & Assemblies

- Assemblies expose metadata at runtime
- Used for:
  - Dependency Injection
  - Serialization
  - ORMs
  - Plug-in systems

---

## Common Interview Pitfalls

- DLL vs EXE difference misunderstood
- Assuming GAC is common in modern .NET
- Confusing assembly with namespace

---

## Common Interview Follow-ups

- Can two versions of the same assembly coexist?
- What happens if an assembly is missing?
- How does strong naming help versioning?
- How does .NET Core handle assembly loading?

---

## 30-Second Interview Summary

> An assembly is the fundamental unit of deployment and versioning in .NET. It contains IL, metadata, and a manifest that defines its identity. Assemblies can be EXEs with an entry point or DLLs used as libraries. The CLR loads assemblies based on identity and context, supports versioning, and can share strong-named assemblies via the Global Assembly Cache.
