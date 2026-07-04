# Docker SDK for Python – Setup and First Steps

This document introduces the **Docker SDK for Python (`docker` library)** and explains how to connect to the Docker Engine, inspect its status, and authenticate with a registry. The goal is not just to show code, but to clearly explain **what each part does, why it exists, and when you would use it**.

This guide assumes you already understand basic Docker concepts (images, containers, daemon) and are approaching this from a DevOps perspective.

---

## 1. Installing the Docker SDK for Python

Before Python can communicate with Docker, we need to install the official SDK.

```bash
pip install docker
```

### What this does

* Installs the **docker** Python package (often called *docker-py*).
* This package acts as a **client wrapper** around Docker’s REST API.
* All interactions ultimately talk to the Docker daemon (`dockerd`) over a socket or TCP connection.

Important note:

* Installing the library alone is not enough.
* Docker **must already be installed and running** on the system.

---

## 2. Connecting to Docker Using Environment Configuration

The simplest and most common way to create a Docker client is by using environment-based configuration.

```python
import docker

client = docker.from_env()
version = client.version()
ping_docker = client.ping()

print(version, ping_docker)
```

### Step-by-step explanation

#### `import docker`

* Imports the Docker SDK.
* This module exposes high-level objects for interacting with Docker resources.

#### `docker.from_env()`

* Automatically creates a `DockerClient` instance.
* Reads Docker connection details from environment variables such as:

  * `DOCKER_HOST`
  * `DOCKER_TLS_VERIFY`
  * `DOCKER_CERT_PATH`
* On Linux, this usually resolves to:

  * `unix:///var/run/docker.sock`

This is why `from_env()` is preferred:

* Works across Linux, macOS, Windows, and CI systems
* Requires no hardcoded connection strings

#### `client.version()`

* Calls the Docker Engine `/version` API endpoint.
* Returns detailed metadata such as:

  * Docker Engine version
  * API version
  * Go version
  * OS and architecture

This is commonly used for:

* Debugging
* Compatibility checks
* Logging runtime environment details

#### `client.ping()`

* Sends a lightweight request to the Docker daemon.
* Returns `True` if Docker is reachable and responsive.

This is a **health check**, often used in:

* Startup validation
* Monitoring scripts
* CI/CD pipelines

---

## 3. What Does “from_env” Actually Mean?

Using `from_env()` tells the SDK:

> “Figure out how to connect to Docker using the same configuration the Docker CLI uses.”

Behind the scenes, it:

* Detects whether Docker is local or remote
* Determines socket vs TCP
* Applies TLS settings if required

### When should you use it?

* Local development
* Kubernetes nodes
* CI runners
* Most production automation

### When not to use it?

* When you need explicit control over connection parameters
* When connecting to a **remote Docker daemon** with custom networking

---

## 4. Creating a Docker Client Explicitly

Instead of relying on environment detection, you can create a client manually.

```python
import docker

client = docker.DockerClient(base_url='unix://var/run/docker.sock')
version = client.version()
ping_docker = client.ping()

print(version, ping_docker)
```

### Explanation

#### `docker.DockerClient(...)`

* Directly instantiates a Docker client object.
* Requires you to specify how to reach the Docker daemon.

#### `base_url='unix://var/run/docker.sock'`

* Points to the Unix socket used by Docker on Linux systems.
* This socket is owned by root and the `docker` group.

Important implications:

* Your Python process must have permission to access the socket
* Usually means running as root or a user in the `docker` group

### When this approach is useful

* Controlled environments
* Educational examples
* Explicit infrastructure scripts

### Why it is less common

* Not portable across OSes
* Hardcodes infrastructure assumptions

---

## 5. Inspecting Docker and Authenticating

Once connected, the client can retrieve detailed system information and authenticate with registries.

```python
import docker

client = docker.DockerClient(base_url='unix://var/run/docker.sock')
version = client.version()
ping_docker = client.ping()
information = client.info()

client.login(
    username='user',
    password='password',
    registry='registry_link'
)

print(information, ping_docker)
```

### `client.info()`

* Calls the Docker `/info` endpoint.
* Returns a comprehensive snapshot of the Docker host, including:

  * Number of containers (running/stopped)
  * Number of images
  * Storage driver
  * Cgroup and kernel features
  * Security options (AppArmor, seccomp)

This is extremely valuable for:

* Capacity planning
* Debugging runtime issues
* Auditing host configuration

### `client.login(...)`

* Authenticates Docker with a container registry.
* Stores credentials in Docker’s credential store.

Parameters:

* `username`: registry account username
* `password`: registry password or access token
* `registry`: registry URL (e.g. Docker Hub or private registry)

Common use cases:

* Pulling private images
* Pushing images in CI/CD pipelines
* Automating registry interactions

Security note:

* Avoid hardcoding credentials in source code
* Prefer environment variables or secret managers

---

## 6. High-Level vs Low-Level API (Important Concept)

What you are using here is the **high-level Docker client**.

Characteristics:

* Pythonic object model
* Resource-oriented (containers, images, networks)
* Easier to read and maintain

The SDK also exposes a **low-level API**:

* Direct access to Docker REST endpoints
* More control, less abstraction

As a DevOps engineer, you will typically:

* Use high-level API for automation
* Drop to low-level API for advanced edge cases

---

## 7. Summary

In this setup phase, you learned how to:

* Install the Docker SDK for Python
* Connect to Docker using environment-based configuration
* Explicitly define Docker connection settings
* Verify Docker availability
* Retrieve system-level Docker information
* Authenticate with container registries

This foundation is critical before moving on to:

* Managing containers
* Building images
* Working with volumes and networks

---

## References

Official Docker SDK for Python documentation:

* [https://docker-py.readthedocs.io/](https://docker-py.readthedocs.io/)
