# 06 – RLock (Reentrant Lock)

A normal `Lock` **deadlocks** if the same thread tries to acquire it twice. An **`RLock`** (reentrant lock) can be acquired multiple times by the **same** thread.

```python
from threading import Thread, RLock

lock = RLock()

def outer():
    with lock:
        print("outer")
        inner()  # needs the same lock again

def inner():
    with lock:
        print("inner")

Thread(target=outer).start()
```

Each `acquire` must have a matching `release` (or nested `with` blocks).

---

## Lock vs RLock

| | `Lock` | `RLock` |
|--|--------|---------|
| Same thread acquire twice | Deadlock | Allowed |
| Owner tracking | No | Yes (count + owner) |
| Typical use | Simple critical sections | Nested helpers that both need the lock |

```python
from threading import Lock, RLock

# Lock — would hang if same thread acquires twice
# RLock — safe for nested calls
```

Use `RLock` when one locked function calls another that also needs the same lock.

Next: [07 – Condition](07-condition.md).
