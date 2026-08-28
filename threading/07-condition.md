# 07 – Condition

A **`Condition`** lets threads wait for a state change, then wake up when another thread signals.

Built on a lock (`Lock` or `RLock`). Pattern: **wait** until a condition is true; **notify** when it becomes true.

```python
from threading import Thread, Condition
import time

items = []
condition = Condition()

def consumer():
    with condition:
        while not items:          # always wait in a loop (spurious wakeups)
            print("waiting...")
            condition.wait()
        item = items.pop(0)
        print("got", item)

def producer():
    time.sleep(1)
    with condition:
        items.append("data")
        condition.notify()        # wake one waiter
        # condition.notify_all()  # wake all waiters

Thread(target=consumer).start()
Thread(target=producer).start()
```

---

## API

| Method | Meaning |
|--------|---------|
| `wait()` | Release lock and sleep until notified |
| `wait(timeout)` | Same, but wake after timeout |
| `notify()` | Wake one waiting thread |
| `notify_all()` | Wake all waiting threads |

Always call `wait` / `notify` **while holding** the condition’s lock (`with condition:`).

Classic uses: producer/consumer queues, “wait until ready”, thread handshakes.

Next: [08 – Semaphore](08-semaphore.md).
