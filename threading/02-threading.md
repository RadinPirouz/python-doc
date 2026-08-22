# 02 – Create Threads, Subclass, Current Thread

Read [01-information.md](01-information.md) first.

---

## 1. Sequential Execution

Without threads, work runs one after another:

```python
import time

start = time.perf_counter()

def test(name):
    print(f"starting {name}")
    time.sleep(3)
    print(f"{name} finished")

test("mmd")
test("ali")

print(time.perf_counter() - start)  # ~6 seconds
```

---

## 2. Create a Thread (`Thread`)

```python
from threading import Thread
import time

start = time.perf_counter()

def test(name):
    print(f"starting {name}")
    time.sleep(3)
    print(f"{name} finished")

t1 = Thread(target=test, args=("mmd",))
t2 = Thread(target=test, args=("ali",))

t1.start()
t2.start()

t1.join()  # wait until t1 finishes
t2.join()

print(time.perf_counter() - start)  # ~3 seconds
```

### Important methods

| Method / attr | Meaning |
|---------------|---------|
| `start()` | Begin running the thread (calls `run()` in the background) |
| `join()` | Block until that thread finishes |
| `args` / `kwargs` | Arguments passed to `target` |
| `name` | Optional thread name |

`start()` must be called once. Do not call `run()` yourself if you use `target=`.

---

## 3. Subclass `Thread`

Override `run()` when the worker needs its own state or logic:

```python
from threading import Thread
import time

def show_task(name, delay):
    print(f"{name} started")
    time.sleep(delay)
    print(f"{name} completed")

class Task(Thread):
    def __init__(self, name, delay):
        super().__init__()
        self.task_name = name
        self.delay = delay

    def run(self):
        show_task(self.task_name, self.delay)

start = time.perf_counter()

task1 = Task("Task 1", 1)
task2 = Task("Task 2", 3)
task3 = Task("Task 3", 2)

task1.start()
task2.start()
task3.start()

task1.join()
task2.join()
task3.join()

print(f"Time taken: {round(time.perf_counter() - start, 2)} seconds")
print("Main thread completed")
```

`start()` still schedules the thread; the OS eventually runs `run()`.

---

## 4. Current Thread & Thread Info

```python
from threading import Thread, active_count, current_thread, enumerate
import time

def show_task(name, delay):
    print(f"Active threads: {active_count()}")
    print(f"Name: {current_thread().name}")
    print(f"Ident: {current_thread().ident}")
    print(f"All threads: {enumerate()}")
    print(f"{name} started")
    time.sleep(delay)
    print(f"{name} completed")

task1 = Thread(target=show_task, args=("Task 1", 1), name="Task 1")
task2 = Thread(target=show_task, args=("Task 2", 2), name="Task 2")
task3 = Thread(target=show_task, args=("Task 3", 3))

for t in (task1, task2, task3):
    t.start()

for t in (task1, task2, task3):
    t.join()

print("Main thread completed")
```

| Function | Meaning |
|----------|---------|
| `current_thread()` | The thread object running this code |
| `active_count()` | How many threads are alive |
| `enumerate()` | List of all alive `Thread` objects |
| `thread.name` | Human-readable name |
| `thread.ident` | Thread id (or `None` before start) |

Main thread name is usually `"MainThread"`.

---

## Checklist

* Use `Thread(target=..., args=...)` for simple jobs
* Subclass and override `run()` for reusable workers
* Always `join()` threads you care about finishing
* Inspect with `current_thread()`, `active_count()`, `enumerate()`
* Daemon threads: see [03 – Daemon](03-daemon.md)

Next: [03 – Daemon Threads](03-daemon.md).
