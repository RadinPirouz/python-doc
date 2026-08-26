# 05 – Lock

Threads share memory. Without protection, two threads can corrupt the same variable (**race condition**).

A **`Lock`** allows only one thread into a critical section at a time.

```python
from threading import Thread, Lock
import time

counter = 0
lock = Lock()

def bump():
    global counter
    for _ in range(100_000):
        with lock:          # acquire → work → release
            counter += 1

threads = [Thread(target=bump) for _ in range(4)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(counter)  # 400000
```

---

## Without a lock (broken)

```python
counter = 0

def bump():
    global counter
    for _ in range(100_000):
        counter += 1  # read-modify-write is not atomic
```

Result is often less than `400000`.

---

## Manual acquire / release

```python
lock.acquire()
try:
    # critical section
    pass
finally:
    lock.release()
```

Prefer `with lock:` — it always releases, even on exceptions.

| Method | Meaning |
|--------|---------|
| `acquire()` | Wait until the lock is free, then take it |
| `release()` | Give the lock back |
| `locked()` | `True` if currently held |

Next: [06 – RLock](06-rlock.md).
