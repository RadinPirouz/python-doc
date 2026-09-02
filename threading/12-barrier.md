# 12 – Barrier

A **`Barrier`** makes N threads wait until **all** of them reach the same point, then they continue together.

```python
from threading import Thread, Barrier
import time
import random

barrier = Barrier(3)

def worker(n):
    time.sleep(random.uniform(0.1, 1.0))
    print(n, "at barrier")
    barrier.wait()
    print(n, "passed")

for i in range(3):
    Thread(target=worker, args=(i,)).start()
```

All three print `at barrier` (in some order), then all print `passed`.

---

## Options

```python
barrier = Barrier(
    parties=3,
    action=lambda: print("all arrived"),  # runs in one of the threads
    timeout=5,
)
```

| Method / attr | Meaning |
|---------------|---------|
| `wait()` | Block until all parties arrive |
| `abort()` | Break the barrier; waiters get `BrokenBarrierError` |
| `reset()` | Reset a broken/aborted barrier |
| `n_waiting` | How many are currently waiting |
| `parties` | Required party count |

Use when phases must start together (e.g. “everyone finished setup → start race”).

Next: [13 – Concurrent Port Check](13-port-scan.md).
