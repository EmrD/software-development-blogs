# What is a Race Condition in Concurrent Systems?

In modern software development, writing multi-threaded applications or microservices allows systems to process tasks in parallel, drastically improving performance. However, concurrency introduces a subtle and notorious class of software bugs known as **Race Conditions**.

In this article, you can find what a race condition is, how it occurs, real-world examples, and the strategies developers use to prevent it.

---

## What is a Race Condition?

A **Race Condition** occurs when two or more threads, processes, or services attempt to access and manipulate shared data simultaneously, and the final outcome depends on the non-deterministic timing or execution sequence of those operations.

In simpler terms: the system's behavior becomes a "race" between competing threads, where whoever finishes first dictates the final state—often leading to unpredictable bugs or data corruption.


```

Initial State: Account Balance = $100

```
       Thread A (Withdraw $80)                 Thread B (Withdraw $50)
                 |                                       |
                 +---- 1. Read Balance ($100) ----+      |
                 |                                |      |
                 |      +---- 2. Read Balance ($100) ----+
                 |      |                         |      |
                 |      v                         |      v
      3. Check: $100 >= $80? (PASS)     4. Check: $100 >= $50? (PASS)
                 |                                       |
      5. New Balance = $100 - $80         6. New Balance = $100 - $50
                 |                                       |
      7. Write Balance ($20)                     |
                 |                                       |
                 |                      8. Write Balance ($50)
                 v                                       v
                           Final State: $50
               (System allowed $130 total withdrawal!)

```

```

---

## Critical Sections and Data Races

To understand why race conditions happen, we need to recognize two core concepts in concurrent programming:

1. **Shared Resource:** Any piece of data, file, memory location, or database record accessible by multiple execution contexts.
2. **Critical Section:** A block of code that accesses a shared resource and must not be executed concurrently by more than one thread.

A race condition manifests when access to a **critical section** is not properly synchronized or controlled.

---

## Real-World Example: The E-Commerce Double-Spend Problem

Consider an inventory system where two users try to purchase the last available item in stock (`stock_count = 1`) at the exact same millisecond:

```python
# Unsafe Code Example
def purchase_item(item_id, user_id):
    stock = get_stock(item_id)          # Step 1: Read stock
    if stock > 0:                       # Step 2: Check availability
        decrement_stock(item_id)        # Step 3: Mutate stock
        create_order(user_id, item_id)  # Step 4: Complete transaction
        return True
    return False

```

If Request 1 and Request 2 reach `Step 1` simultaneously, both will read `stock = 1`. Both checks at `Step 2` pass. Both requests proceed to decrement the stock and create orders—resulting in **negative inventory (`stock = -1`)** and a double purchase of a single item.

---

## How to Prevent Race Conditions

Preventing race conditions involves ensuring **Mutual Exclusion (Mutex)**—guaranteeing that only one thread can access a critical section at a given time.

### 1. In-Memory Synchronization (Single Application Process)

* **Mutexes / Locks:** Threads must acquire a lock before entering a critical section and release it afterwards.
* **Atomic Operations:** Low-level CPU instructions (like Compare-And-Swap) that execute completely or not at all, avoiding partial state mutations.

```python
# Safe Code Example using a Mutex Lock
from threading import Lock

lock = Lock()

def purchase_item(item_id, user_id):
    with lock:  # Only one thread can enter this block at a time
        stock = get_stock(item_id)
        if stock > 0:
            decrement_stock(item_id)
            create_order(user_id, item_id)
            return True
        return False

```

### 2. Database-Level Control (Distributed Systems)

In distributed architectures, multiple instances of an application run on separate servers, making in-memory locks ineffective across nodes.

* **Pessimistic Locking:** Locks the target database row upon reading using `SELECT ... FOR UPDATE`, blocking other transactions until the first transaction commits or rolls back.
* **Optimistic Locking:** Adds a `version` or `timestamp` column to the table. When updating, the query verifies that the version hasn't changed since it was read:
```sql
UPDATE inventory 
SET stock = stock - 1, version = version + 1 
WHERE item_id = 42 AND version = 3;

```


If no rows were updated (because another thread modified `version` first), the application retries the transaction.

### 3. Distributed Locking (Microservices)

For systems coordinating across independent microservices and caching layers, a centralized lock manager like **Redis** (using `SETNX` or the **Redlock** algorithm) or **ZooKeeper** ensures that only one microservice instance executes the guarded workflow.

---

## Summary

Race conditions occur when system correctness relies on thread timing or execution order:

* **Root Cause:** Unsynchronized, simultaneous access to shared resources within a critical section.
* **Risk:** Silent data corruption, negative balances, duplicate transactions, and non-deterministic application crashes.
* **Solutions:** Use **Mutexes/Atomic Variables** for single processes, **Optimistic/Pessimistic Locking** for databases, and **Distributed Locks (Redis/Redlock)** for microservice architectures.
