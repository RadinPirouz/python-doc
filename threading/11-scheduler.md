# 11 – Scheduler (`sched`)

The **`sched`** module runs functions at absolute or relative times. Often combined with threading so the scheduler does not block the main thread forever.

```python
import sched
import time
from threading import Thread

scheduler = sched.scheduler(time.time, time.sleep)

def job(name):
    print(time.strftime("%H:%M:%S"), name)

# run after delays (seconds from now)
scheduler.enter(2, 1, job, argument=("first",))
scheduler.enter(4, 1, job, argument=("second",))

# priority: lower number = higher priority if times collide
Thread(target=scheduler.run, daemon=True).start()

time.sleep(5)
print("done")
```

---

## Absolute time

```python
import sched
import time

s = sched.scheduler(time.time, time.sleep)
when = time.time() + 3
s.enterabs(when, 1, print, argument=("at absolute time",))
s.run()
```

| Method | Meaning |
|--------|---------|
| `enter(delay, priority, action, argument=())` | Schedule relative delay |
| `enterabs(time, priority, action, argument=())` | Schedule at absolute timestamp |
| `cancel(event)` | Cancel a pending event |
| `run()` | Process the queue (blocks until empty unless `blocking=False`) |

For production apps, also consider `threading.Timer` or external cron; `sched` is fine for simple in-process schedules.

Next: [12 – Barrier](12-barrier.md).
