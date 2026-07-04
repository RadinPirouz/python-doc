# Docker SDK for Python – Working with Containers

This document explains how to **manage Docker containers** using the Docker SDK for Python. Containers are the runtime unit of Docker, so this section focuses on lifecycle management: listing, creating, running, starting, stopping, restarting, pausing, and inspecting logs.

The goal is to understand **how container state changes**, how the Python SDK maps to Docker CLI behavior, and how these operations are typically used in real DevOps automation.

---

## 1. Creating the Docker Client

```python
import docker
import time

client = docker.DockerClient(base_url='unix://var/run/docker.sock')
ping_docker = client.ping()
```

### Explanation

* Establishes a connection to the Docker daemon via the Unix socket.
* `ping()` is used as a sanity check before executing container operations.

In production-grade scripts, this check prevents:

* Silent failures
* Partial execution when Docker is down

---

## 2. Listing Running Containers

```python
def get_containers_list():
    containers = client.containers.list()

    for c in containers:
        print(c.id, c.name, c.status)
```

### Explanation

* `containers.list()` returns **only running containers** by default.
* Each returned object represents a live container instance.

Common attributes:

* `id`: unique container identifier
* `name`: human-readable container name
* `status`: runtime state (running)

This is equivalent to:

* `docker ps`

---

## 3. Listing All Containers (Including Stopped)

```python
def get_all_containers_list():
    containers = client.containers.list(all=True)

    for c in containers:
        print(c.id, c.name, c.status)
```

### Explanation

* `all=True` includes stopped, exited, and created containers.
* Useful for cleanup, auditing, and lifecycle reconciliation.

Equivalent CLI command:

* `docker ps -a`

---

## 4. Creating a Container (Without Starting It)

```python
def create_container():
    container = client.containers.create(
        image="nginx:latest",
        name="my_nginx",
        ports={"80/tcp": 8080},
        detach=True
    )
```

### Explanation

This creates a container in the **created** state.

Key parameters:

* `image`: base image for the container
* `name`: explicit container name
* `ports`: port mapping (container → host)
* `detach=True`: container runs in background when started

Important distinction:

* `create()` does **not** start the container
* This mirrors `docker create`

Use cases:

* Deferred startup
* Multi-step initialization

---

## 5. Running a Container (Create + Start)

```python
def run_container():
    container = client.containers.run(
        "nginx:latest",
        name="nginx_run",
        ports={"80/tcp": 8081},
        detach=True
    )
```

### Explanation

* `run()` is a convenience method.
* Internally performs `create()` followed by `start()`.

Equivalent CLI command:

* `docker run -d -p 8081:80 nginx:latest`

This is the most common approach for:

* Short-lived workloads
* Simple services

---

## 6. Managing Container Lifecycle

```python
def start_container():
    container = client.containers.get("my_nginx")

    container.start()
    time.sleep(1000)

    container.restart()
    time.sleep(1000)

    container.pause()
    container.unpause()
    time.sleep(1000)

    container.stop(timeout=10)
```

### Explanation

This function demonstrates multiple lifecycle transitions.

#### `containers.get(name)`

* Retrieves an existing container by name or ID.
* Raises an exception if the container does not exist.

#### `start()`

* Transitions container from `created` or `stopped` → `running`.

#### `restart()`

* Stops and immediately starts the container.
* Often used after configuration changes.

#### `pause()` / `unpause()`

* Freezes container processes using cgroups.
* Network and filesystem state remain intact.

#### `stop(timeout=10)`

* Sends SIGTERM, then SIGKILL after timeout.
* Allows graceful shutdown.

The `sleep()` calls simulate:

* Long-running services
* Observing container behavior between states

---

## 7. Reading Container Logs

```python
def log_container():
    container = client.containers.get("my_nginx")
    logs = container.logs(tail=20)
    print(logs.decode())
```

### Explanation

* Retrieves logs from the container’s stdout/stderr.
* `tail=20` limits output to the last 20 lines.

Important details:

* Logs are returned as bytes
* Decoding is required for readable output

Equivalent CLI command:

* `docker logs --tail 20 my_nginx`

This is critical for:

* Debugging runtime issues
* Health checks
* Observability tooling

---

## 8. Conditional Execution Based on Docker Availability

```python
if ping_docker:
    get_containers_list()
```

### Explanation

* Ensures container operations only run if Docker is reachable.
* Prevents unhandled API errors.

This pattern is common in:

* Entry-point scripts
* Health probes
* Automation jobs

---

## 9. Container States (Conceptual Model)

Understanding container states is essential:

* `created`: container exists but is not running
* `running`: container processes are active
* `paused`: execution is suspended
* `exited`: container stopped normally
* `dead`: container failed unexpectedly

Every method in this document transitions the container between these states.

---

## 10. Summary

In this section, you learned how to:

* List running and stopped containers
* Create containers separately from starting them
* Run containers in a single step
* Control container lifecycle states
* Read container logs
* Safely execute operations based on Docker availability

This container-level control is the foundation for:

* Service orchestration
* CI/CD job execution
* Debugging and recovery automation

---

## References

Official Docker SDK for Python documentation:

* [https://docker-py.readthedocs.io/](https://docker-py.readthedocs.io/)
