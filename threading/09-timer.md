# 09 – Timer

A **`Timer`** is a thread that runs a function **once** after a delay.

```python
from threading import Timer

def hello():
    print("hello after 2 seconds")

t = Timer(2.0, hello)
t.start()
# t.cancel()  # stop it if it has not run yet
```

With arguments:

```python
def greet(name):
    print("hi", name)

Timer(1.5, greet, args=("Ali",)).start()
```

---

## Notes

* Subclass of `Thread` — one-shot delayed call
* Call `cancel()` before it fires to abort
* For repeating schedules, use [`sched`](11-scheduler.md) or a loop/`sleep`, not stacked Timers casually

```python
from threading import Timer

def tick():
    print("tick")
    Timer(1.0, tick).start()  # reschedule (simple heartbeat)

Timer(1.0, tick).start()
```

Next: [10 – Event](10-event.md).
