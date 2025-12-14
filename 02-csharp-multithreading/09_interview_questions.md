# 09 – Multithreading Interview Questions (Crisp Answers)

## Beginner

**Q1. What is a thread?**  
A lightweight execution unit within a process.

**Q2. Process vs Thread?**  
Process has its own memory; threads share memory.

---

## Intermediate

**Q3. What is race condition?**  
Concurrent access to shared data causing incorrect results.

**Q4. Mutex vs Semaphore?**  
Mutex allows one thread; Semaphore allows N threads.

---

## Advanced

**Q5. Can async cause deadlock?**  
Yes, when async code is blocked synchronously.

**Q6. How to debug deadlock?**  
Analyze thread dumps and call stacks.

**Q7. Why BlockingCollection over Queue+lock?**  
It removes manual synchronization errors.

---

## Architect-Level

**Q8. How to design deadlock-free systems?**  
Avoid shared state, use message passing.

**Q9. Lock vs Actor Model?**  
Actor eliminates locks using message queues.

---


