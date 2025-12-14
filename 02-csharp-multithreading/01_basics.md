# 01 – Multithreading Basics

## What is Multithreading?
Multithreading allows **multiple threads (units of execution)** to run concurrently within a **single process**.

---

## Why Multithreading?
- Better CPU utilization
- Improved responsiveness
- Parallel execution on multi-core CPUs

---

## Process vs Thread

| Process | Thread |
|------|------|
Own memory space | Shares process memory |
Heavyweight | Lightweight |

### Analogy
Process = House, Thread = People inside the house

---

## Single Thread vs Multithreaded

### Single Thread
```text
Task A → Task B → Task C
```

### Multithreaded
```text
Task A ─────────
Task B ─────────
Task C ─────────
```

---

## Hands-on Exercise
1. Write a console app
2. Print numbers 1–10 sequentially
3. Observe execution order

---

## Key Takeaways
- Every C# app starts with one Main thread
- Multithreading improves throughput, not always performance

