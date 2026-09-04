# 14 – Done / Review

Quick map of what you covered:

| Topic | Tool | One-line idea |
|-------|------|----------------|
| Concepts | process / thread / GIL | Threads share memory; best for I/O |
| Create | `Thread(target=...)` | `start()` + `join()` |
| Subclass | `class T(Thread): run()` | Custom worker objects |
| Daemon | `daemon=True` | Dies when process exits |
| Inspect | `current_thread`, `enumerate` | Debug who is running |
| Pool | `ThreadPoolExecutor` | Reuse N workers |
| Lock | `Lock` | One thread in critical section |
| RLock | `RLock` | Same thread can re-enter |
| Condition | `Condition` | Wait / notify on state |
| Semaphore | `Semaphore(n)` | At most N inside |
| Timer | `Timer(delay, fn)` | Run once later |
| Event | `Event` | Simple on/off signal |
| Scheduler | `sched` | Timed job queue |
| Barrier | `Barrier(n)` | Wait until all arrive |
| Practice | socket + pool | Concurrent I/O checks |

---

## Remember

1. Prefer threads for **I/O bound** work; use **multiprocessing** for heavy CPU.
2. Shared mutable data → use **Lock / RLock / Condition / Semaphore**.
3. Prefer **`ThreadPoolExecutor`** over spawning unbounded threads.
4. Prefer **`with lock:`** so release always happens.
5. Daemon threads for helpers only — don’t put critical work only on daemons.

---

## Suggested order to re-read

1. [01 – Concepts](01-information.md)
2. [02 – Threads basics](02-threading.md)
3. [03 – Daemon](03-daemon.md)
4. [04 – Pool](04-pool.md)
5. Sync primitives: [Lock](05-lock.md) → [RLock](06-rlock.md) → [Condition](07-condition.md) → [Semaphore](08-semaphore.md)
6. Helpers: [Timer](09-timer.md) → [Event](10-event.md) → [Scheduler](11-scheduler.md) → [Barrier](12-barrier.md)
7. Practice: [Port check](13-port-scan.md)

Back to index: [README](README.md).
