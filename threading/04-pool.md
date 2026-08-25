# 04 – Thread Pool (`ThreadPoolExecutor`)

Creating many `Thread` objects by hand is noisy. A **pool** reuses a fixed number of worker threads.

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import time

def work(n):
    time.sleep(1)
    return n * n

with ThreadPoolExecutor(max_workers=3) as pool:
    futures = [pool.submit(work, i) for i in range(5)]
    for f in as_completed(futures):
        print(f.result())
```

---

## `submit` vs `map`

```python
from concurrent.futures import ThreadPoolExecutor

def fetch(url):
    return len(url)

urls = ["a.com", "bb.com", "ccc.com"]

with ThreadPoolExecutor(max_workers=4) as pool:
    # map: results in same order as inputs
    for length in pool.map(fetch, urls):
        print(length)

    # submit: get Future objects; finish order may differ
    futures = [pool.submit(fetch, u) for u in urls]
    for f in futures:
        print(f.result())
```

| API | Use when |
|-----|----------|
| `pool.map(fn, items)` | Same function over a list; want ordered results |
| `pool.submit(fn, *args)` | Different calls / need each `Future` |

---

## `Future` basics

```python
future = pool.submit(work, 10)
print(future.done())   # False until finished
print(future.result()) # blocks until value (or raises)
```

`with ThreadPoolExecutor(...)` waits for pending work and shuts the pool down cleanly.

Next: [05 – Lock](05-lock.md).
