# 13 – Concurrent Port Check (Practice)

A classic multithreading practice: try many TCP connections in parallel. Threads help because each `connect` is **I/O bound** (waiting on the network).

**Use only on hosts/ports you own or have permission to test** (e.g. `127.0.0.1`).

---

## Thread-per-port

```python
from threading import Thread, Lock
import socket

print_lock = Lock()
open_ports = []

def check_port(host, port, timeout=0.5):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.settimeout(timeout)
    try:
        result = sock.connect_ex((host, port))
        if result == 0:
            with print_lock:
                open_ports.append(port)
                print("open:", port)
    finally:
        sock.close()

host = "127.0.0.1"
ports = range(1, 1025)

threads = []
for port in ports:
    t = Thread(target=check_port, args=(host, port))
    t.start()
    threads.append(t)

for t in threads:
    t.join()

print("open ports:", sorted(open_ports))
```

Creating 1024 threads is heavy. Prefer a **pool**.

---

## Better: `ThreadPoolExecutor`

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import socket

def is_open(host, port, timeout=0.5):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        sock.settimeout(timeout)
        return port if sock.connect_ex((host, port)) == 0 else None

host = "127.0.0.1"
ports = range(1, 1025)

open_ports = []
with ThreadPoolExecutor(max_workers=100) as pool:
    futures = {pool.submit(is_open, host, p): p for p in ports}
    for f in as_completed(futures):
        port = f.result()
        if port is not None:
            open_ports.append(port)
            print("open:", port)

print("done:", sorted(open_ports))
```

---

## Why threading fits

| Approach | Behavior |
|----------|----------|
| Sequential | One connect after another → slow |
| Many threads / pool | Many connects wait in parallel → much faster for I/O |

This is the same idea as parallel downloads or many HTTP requests.

Next: [14 – Done / Review](14-done.md).
