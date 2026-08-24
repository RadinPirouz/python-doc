# 03 – Daemon Threads

A **daemon** thread is a background helper. When the main program finishes and only daemon threads are left, Python exits — daemon threads are stopped abruptly.

---

## Basic Example

```python
from threading import Thread
import time

def background():
    while True:
        print("daemon working...")
        time.sleep(1)

t = Thread(target=background, daemon=True)
t.start()

time.sleep(3)
print("main exiting")  # process ends; daemon is killed
```

Without `daemon=True`, the infinite loop would keep the process alive forever.

---

## Daemon vs Non-Daemon

| | Non-daemon (default) | Daemon |
|--|----------------------|--------|
| Keeps process alive? | Yes until it finishes | No |
| On process exit | Allowed to finish | Stopped immediately |
| Typical use | Real work you must complete | Heartbeats, watchers, side helpers |

```python
from threading import Thread
import time

def worker(name):
    time.sleep(2)
    print(name, "done")

# Non-daemon: main waits (if you join) / process stays until this finishes
t1 = Thread(target=worker, args=("work",))

# Daemon: dies when main ends (unless you join it)
t2 = Thread(target=worker, args=("bg",), daemon=True)

t1.start()
t2.start()
t1.join()
print("main done")
```

---

## How to Set Daemon

Must be set **before** `start()`:

```python
t = Thread(target=fn)
t.daemon = True
t.start()

# or in the constructor:
t = Thread(target=fn, daemon=True)
t.start()
```

Check with:

```python
print(t.daemon)      # True / False
print(t.isDaemon())  # older alias (still works)
```

---

## Compare Side by Side

**Non-daemon** — process waits for the thread:

```python
from threading import Thread
import time

def slow():
    time.sleep(3)
    print("slow finished")

Thread(target=slow).start()
print("main finished")
# program stays ~3s until slow finishes
```

**Daemon** — process exits with main:

```python
from threading import Thread
import time

def slow():
    time.sleep(3)
    print("slow finished")  # often never prints

Thread(target=slow, daemon=True).start()
print("main finished")
# exits right away; slow is killed
```

If you still need a daemon to finish a chunk of work, call `join()` (optionally with a timeout):

```python
t = Thread(target=slow, daemon=True)
t.start()
t.join(timeout=1)  # wait at most 1 second
```

---

## Common Uses

* Background logging / metrics flushers
* Periodic health checks
* Running `sched.scheduler` in the background
* UI / server helpers that should not block shutdown

---

## Warnings

1. Do **not** put critical work only on a daemon — it may be cut off mid-write (files, DB, network).
2. Daemons do not run cleanup/`finally` reliably on process exit.
3. Prefer non-daemon + `join()` for work that must complete.
4. `ThreadPoolExecutor` workers are non-daemon by default in recent Python — shut the pool down with `with` / `shutdown()`.

---

## Checklist

* Use `daemon=True` only for disposable background helpers
* Set daemon **before** `start()`
* `join()` if you need a daemon to finish (or wait a bit)
* Important work → non-daemon threads

Next: [04 – Thread Pool](04-pool.md).
