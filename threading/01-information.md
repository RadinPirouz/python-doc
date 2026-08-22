# 01 – Multithreading Concepts

Before writing threaded code, understand **process**, **thread**, and when to use multithreading vs other approaches.

---

## What Is a Process?

A **process** is a running program. The OS gives each process its own memory space.

* Starting `python3 app.py` creates one process
* Processes do **not** share memory by default (safer, but heavier)
* Creating many processes costs more RAM and startup time

```python
import os

print(os.getpid())  # this process ID
```

---

## What Is a Thread?

A **thread** is a unit of execution **inside** a process.

* One process can have many threads
* Threads in the same process **share memory** (same variables, same heap)
* Lighter than processes, but shared data needs synchronization (locks, etc.)

```text
Process (python3 app.py)
├── Main Thread
├── Thread-1
└── Thread-2
```

---

## Synchronous Programming

Code runs **one step after another**. The next line waits until the current one finishes.

```python
import time

def download(name):
    print(f"start {name}")
    time.sleep(2)
    print(f"done {name}")

download("A")
download("B")  # starts only after A finishes
```

Total time ≈ 4 seconds.

---

## Asynchronous Programming

Code can **pause** while waiting (I/O) and let other work run in the meantime — without using OS threads for every task.

In Python this usually means `async` / `await` with `asyncio` (one thread, many tasks).

See also: [35-async-await.md](../docs/fundamentals/35-async-await.md).

---

## I/O Bound vs CPU Bound

| Type | What it waits for | Examples | Best tools |
|------|-------------------|----------|------------|
| **I/O bound** | Network, disk, sleep, DB | HTTP requests, file read, sockets | Threads or `asyncio` |
| **CPU bound** | Heavy computation | Image processing, crypto, math | Multiprocessing (bypass GIL) |

**GIL (Global Interpreter Lock):** In CPython, only one thread runs Python bytecode at a time. Threads still help for I/O because they release the GIL while waiting.

---

## Multithreading

Run multiple threads in **one process** so I/O-bound work overlaps in time.

```python
from threading import Thread
import time

def work(name):
    time.sleep(2)
    print(name, "done")

t1 = Thread(target=work, args=("A",))
t2 = Thread(target=work, args=("B",))
t1.start(); t2.start()
t1.join(); t2.join()
```

Total time ≈ 2 seconds (both sleep at the same time).

---

## Async I/O (`asyncio`)

Cooperative concurrency on **one thread**: tasks yield at `await`.

| | Multithreading | Async I/O |
|--|----------------|-----------|
| Model | OS threads | Coroutines on one thread |
| Sharing | Shared memory (need locks) | Same memory (careful with blocking) |
| Best for | Blocking I/O libs, simple parallelism | Many network connections, async libs |
| Cost | Thread overhead | Very light tasks |

---

## Multiprocessing

Run multiple **processes** (separate memory). Useful for CPU-bound work.

```python
from multiprocessing import Process

def heavy(n):
    print(sum(range(n)))

p = Process(target=heavy, args=(10_000_000,))
p.start()
p.join()
```

| | Multithreading | Multiprocessing |
|--|----------------|-----------------|
| Memory | Shared | Separate |
| Startup | Cheap | Heavier |
| GIL | Limited for CPU | Each process has its own GIL |
| Best for | I/O bound | CPU bound |

---

## Quick Decision Guide

```text
Waiting on network / disk / sleep?  → threading or asyncio
Heavy CPU math / parsing?           → multiprocessing
Need both?                          → mix (processes + threads/async)
```

Next: [02 – Create Threads](02-threading.md).
