# 30 – Virtual Environments and pip

Third-party libraries (FastAPI, Pydantic, requests) are installed with **pip** into a **virtual environment** so each project has its own isolated dependencies.

## Create a Virtual Environment

From your project root:

```bash
python3 -m venv .venv
```

This creates a `.venv/` directory with a private Python interpreter and `pip`.

## Activate the Environment

**Linux / macOS:**

```bash
source .venv/bin/activate
```

**Windows (PowerShell):**

```powershell
.venv\Scripts\Activate.ps1
```

Your shell prompt usually changes to show the environment is active. Always activate before installing packages or running the project.

## Install Packages

```bash
pip install fastapi uvicorn
```

## Freeze Dependencies

Export exact versions for reproducible builds:

```bash
pip freeze > requirements.txt
```

Commit `requirements.txt` to version control. Do **not** commit `.venv/`.

## Install from Requirements

```bash
pip install -r requirements.txt
```

## Summary

* **Virtual environment**: Isolated Python environment per project
* **pip**: Installs and manages third-party packages
* **requirements.txt**: Pins dependency versions for reproducibility

For organizing code into modules, see [14 – Modules](14-module.md).
