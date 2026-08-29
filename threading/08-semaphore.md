# 08 – Semaphore

A **`Semaphore`** limits how many threads can enter a section at once (like N parking spots).

```python
from threading import Thread, Semaphore
import time

sem = Semaphore(2)  # at most 2 threads inside

def worker(n):
    with sem:
        print(n, "entered")
        time.sleep(1)
        print(n, "leaving")

for i in range(5):
    Thread(target=worker, args=(i,)).start()
```

Only two workers print `entered` at the same time.

---

## `Semaphore` vs `BoundedSemaphore`

```python
from threading import Semaphore, BoundedSemaphore

s = Semaphore(3)
s.release()  # counter can go above 3 (usually a bug)

b = BoundedSemaphore(3)
# b.release() without matching acquire → ValueError
```

| | Meaning |
|--|---------|
| `Semaphore(n)` | Up to `n` concurrent holders; extra `release` raises the count |
| `BoundedSemaphore(n)` | Same, but forbids releasing past the initial value |

Use for rate limits, connection pools, “max N downloads”.

Next: [09 – Timer](09-timer.md).
