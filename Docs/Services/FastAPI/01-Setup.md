# FastAPI – Environment Setup

This document describes the initial setup required to prepare a Python environment for developing a FastAPI application.
The steps below ensure isolation, dependency management, and reproducibility across environments.

---

## Prerequisites

Before starting, ensure the following are installed on your system:

* Python 3.8 or higher
* `pip` (Python package manager)
* `venv` module (included by default with most Python installations)

You can verify your Python version with:

```bash
python3 --version
```

---

## Step 1: Create a Virtual Environment

A virtual environment isolates project dependencies and prevents conflicts with system-wide Python packages.

From the project root directory, run:

```bash
python3 -m venv .venv
```

This command creates a `.venv` directory containing the isolated Python environment.

---

## Step 2: Activate the Virtual Environment

Activate the virtual environment before installing any dependencies.

### Linux / macOS

```bash
source .venv/bin/activate
```

### Windows (PowerShell)

```powershell
.venv\Scripts\Activate.ps1
```

After activation, your shell prompt should indicate that the virtual environment is active.

---

## Step 3: Install FastAPI

With the virtual environment activated, install FastAPI using `pip`:

```bash
pip install fastapi
```

This installs the FastAPI framework and its required dependencies.

> Note: In a real application, FastAPI is commonly used with an ASGI server such as `uvicorn`, which can be installed later if needed.

---

## Step 4: Freeze Dependencies

To ensure reproducibility across environments (local, CI/CD, staging, production), export the installed dependencies to a requirements file:

```bash
pip freeze > requirement.txt
```

This file should be committed to version control and used by deployment pipelines to install exact dependency versions.

---

## Resulting Files

After completing the steps above, your project directory should include:

* `.venv/` – Python virtual environment (should not be committed to VCS)
* `requirement.txt` – Dependency lock file

---

## Best Practices

* Always activate the virtual environment before working on the project
* Do not commit `.venv/` to source control
* Keep `requirement.txt` updated when adding or upgrading dependencies
* Use the same `requirement.txt` in CI/CD pipelines for consistent builds

---
