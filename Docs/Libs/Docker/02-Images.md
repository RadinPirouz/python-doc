# Docker SDK for Python – Working with Images

This document explains how to **manage Docker images** using the Docker SDK for Python. Instead of treating the SDK as a set of function calls, we’ll approach images the same way Docker itself does: as immutable artifacts that are pulled, built, tagged, pushed, inspected, and eventually cleaned up.

All examples assume:

* Docker is installed and running
* The Python process has access to the Docker socket

---

## 1. Creating the Docker Client

```python
import docker

client = docker.DockerClient(base_url='unix://var/run/docker.sock')
ping_docker = client.ping()
```

### Explanation

* `DockerClient` establishes a connection to the Docker daemon.
* `ping()` verifies that Docker is reachable before doing any real work.

This is a common pattern in automation:

* Fail fast if Docker is unavailable
* Avoid partial execution later in the script

---

## 2. Pulling Images from a Registry

```python
def pull_image(name_image, tag_image):
    image = client.images.pull(name_image, tag=tag_image)
    print(image)
```

### What this does

* Downloads an image from a registry (Docker Hub or private registry).
* If the image already exists locally, Docker may reuse layers.

Parameters:

* `name_image`: repository name (e.g. `alpine`, `nginx`, `myrepo/app`)
* `tag_image`: specific version or variant (e.g. `3.20`, `latest`)

Returned value:

* An `Image` object representing the pulled image

Why this matters:

* Pulling explicitly avoids relying on implicit image downloads
* Makes automation predictable and repeatable

---

## 3. Building Images from a Dockerfile

### Basic build

```python
def build_image():
    image, logs = client.images.build(
        path=".",
        tag="myapp:1.0"
    )

    for log in logs:
        if "stream" in log:
            print(log["stream"].strip())
```

### Explanation

* `path="."` tells Docker to use the current directory as the build context.
* Docker automatically looks for a file named `Dockerfile`.
* `tag="myapp:1.0"` assigns a name and version to the resulting image.

The build process returns:

* `image`: the final built image object
* `logs`: a stream of build output messages

Printing build logs is important because:

* Docker build failures are only visible in logs
* CI pipelines rely on this output for debugging

---

## 4. Advanced Build with Custom Dockerfile and Build Arguments

```python
def build_image_2():
    image, logs = client.images.build(
        path=".",
        dockerfile="Dockerfile.prod",
        tag="myapp:prod",
        buildargs={
            "APP_ENV": "production",
            "VERSION": "1.0.0"
        }
    )
```

### Explanation

This version adds more control:

* `dockerfile="Dockerfile.prod"`

  * Allows multiple Dockerfiles per project
  * Common for dev vs prod builds

* `buildargs`

  * Passed to `ARG` instructions inside the Dockerfile
  * Enables parameterized builds without editing the Dockerfile

Typical DevOps use cases:

* Environment-specific builds
* Injecting version numbers
* Feature flags during build time

---

## 5. Tagging Images

```python
def tag_image():
    image = client.images.get("myapp:1.0")
    image.tag("myrepo/myapp", tag="latest")
```

### Explanation

* Docker images are immutable, but tags are not.
* This creates an additional reference to the same image ID.

Why tagging is important:

* One image can have multiple tags
* Tags represent lifecycle stages (`1.0`, `prod`, `latest`)

This is how promotion pipelines work:

* Build once
* Tag many times
* Push selectively

---

## 6. Removing Images

```python
def remove_image():
    client.images.remove("myapp:1.0", force=True)
```

### Explanation

* Removes the image reference from the local Docker host.
* `force=True` removes the image even if it is in use by stopped containers.

Use with care:

* Running containers still prevent deletion
* Forced removal is destructive

---

## 7. Pushing Images to a Registry

```python
def push_image():
    client.images.push(
        repository="myrepo/myapp",
        tag="latest"
    )
```

### Explanation

* Uploads the image layers to a registry.
* Requires prior authentication (`client.login`).

Important notes:

* Only new or changed layers are pushed
* Tags determine what remote users pull

This step is usually automated in:

* CI/CD pipelines
* Release workflows

---

## 8. Cleaning Up Unused Images

```python
def prune_images():
    result = client.images.prune()
    print(result)
```

### Explanation

* Removes dangling images (untagged and unused).
* Helps reclaim disk space on build servers.

The result includes:

* Number of images removed
* Amount of disk space freed

This is essential for:

* CI runners
* Long-lived build machines

---

## 9. Inspecting Image Metadata

```python
def inspect_image():
    alpine_image = client.images.get("alpine:3.20")

    print(alpine_image.attrs)
    print(alpine_image.attrs["Id"])
    print(alpine_image.attrs["Size"])
    print(alpine_image.attrs["Config"]["Env"])
    print(alpine_image.attrs["Config"]["Cmd"])
```

### Explanation

* `attrs` exposes the raw Docker image inspection data.
* This is equivalent to `docker image inspect`.

Useful fields:

* `Id`: content-addressable image hash
* `Size`: image size in bytes
* `Config.Env`: environment variables baked into the image
* `Config.Cmd`: default command

This information is often used for:

* Debugging unexpected behavior
* Auditing images
* Validating build output

---

## 10. Listing Local Images

```python
def image_list():
    images = client.images.list()
    for item in images:
        print(item.id, item.tags)
```

### Explanation

* Lists all images stored locally.
* Each image may have multiple tags or none.

This mirrors:

* `docker images`

---

## 11. Ensuring an Image Exists

```python
def ensure_image(name):
    try:
        client.images.get(name)
        print(f"{name} exists")
    except docker.errors.ImageNotFound:
        print(f"Pulling {name}")
        client.images.pull(name)
```

### Explanation

This is a very common DevOps pattern:

* Check if the image exists locally
* Pull it only if necessary

Benefits:

* Avoids unnecessary network calls
* Makes scripts idempotent

---

## 12. Important Exceptions to Know

When working with images, you must handle failures explicitly.

### `docker.errors.ImageNotFound`

* Raised when an image does not exist locally
* Common when calling `get()`

### `docker.errors.BuildError`

* Raised when an image build fails
* Usually due to Dockerfile errors or missing files

### `docker.errors.APIError`

* Raised for general Docker API failures
* Includes permission issues, daemon errors, and network problems

Catching these exceptions is critical for:

* Reliable automation
* Meaningful error reporting
* CI/CD stability

---

## 13. Summary

In this section, you learned how to:

* Pull images from registries
* Build images using Dockerfiles
* Use build arguments and multiple Dockerfiles
* Tag and push images
* Inspect and list images
* Clean up unused images
* Handle common Docker image errors

This image workflow is the backbone of:

* CI pipelines
* Release automation
* Platform engineering systems

---

## References

Official Docker SDK for Python documentation:

* [https://docker-py.readthedocs.io/](https://docker-py.readthedocs.io/)
