# 10 – Event

An **`Event`** is a simple flag: threads can **wait** until another thread **sets** it.

```python
from threading import Thread, Event
import time

ready = Event()

def worker():
    print("waiting for signal...")
    ready.wait()          # blocks until set()
    print("got signal, working")

t = Thread(target=worker)
t.start()

time.sleep(2)
print("signaling")
ready.set()
t.join()
```

---

## API

| Method | Meaning |
|--------|---------|
| `wait()` / `wait(timeout)` | Block until the flag is set (or timeout) |
| `set()` | Set flag to true; wake all waiters |
| `clear()` | Reset flag to false |
| `is_set()` | Check without blocking |

```python
ready.clear()   # reuse the same Event for another round
```

**Event vs Condition:** Event is a boolean signal. Condition is “wait until data/state is ready” with a shared lock.

Next: [11 – Scheduler](11-scheduler.md).
