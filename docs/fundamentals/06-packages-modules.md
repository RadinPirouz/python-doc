# 06 – Packages and Modules in Python

This document explains how **modules**, **packages**, and the `__name__` concept work in Python.
These features help organize code, reuse functionality, and build scalable projects.

---

## 1. Importing External Modules

Python allows you to import **external libraries** installed in your environment.

### Example: Using the `emoji` Module

### Code

```python
import emoji
print(emoji.emojize("abbas is :red_heart:"))
```

### Explanation

* `import emoji` imports the entire `emoji` module.
* `emoji.emojize()` converts emoji aliases into actual emoji characters.
* You must use the module name (`emoji`) to access its functions.

---

### Importing a Specific Function

### Code

```python
from emoji import emojize
print(emojize("abbas is :red_heart:"))
```

### Explanation

* `from emoji import emojize` imports only the `emojize` function.
* You can call the function directly without prefixing the module name.
* This approach is cleaner when you only need a specific function.

---

## 2. Creating a Module

A **module** is a single Python file containing functions, classes, or variables.

### File Structure

```
hi.py
main.py
```

### `hi.py`

```python
def hi():
    print("Hi :)")
```

### `main.py`

```python
import hi
hi.hi()
```

### Explanation

* `hi.py` is a module.
* `hi()` is a function defined inside the module.
* `import hi` loads the module.
* `hi.hi()` calls the function from the module.

---

## 3. Creating a Package

A **package** is a directory that contains multiple modules.

### Package Structure

```
honor/
│── __init__.py
│── hi.py
main.py
```

### `honor/hi.py`

```python
def hello():
    print("Hi :)")
```

### `honor/__init__.py`

```python
```

### Explanation

* The `honor` directory is a package.
* `__init__.py` tells Python that this directory is a package.
* The file can be empty, but it **must exist** (especially for older Python versions and clarity).

---

### Importing from a Package (Method 1)

### `main.py`

```python
from honor import hi
hi.hello()
```

#### Explanation

* Imports the `hi` module from the `honor` package.
* Accesses the function using `hi.hello()`.

---

### Importing from a Package (Method 2)

### `main.py`

```python
from honor.hi import hello
hello()
```

#### Explanation

* Imports the `hello` function directly.
* Allows calling the function without the module name.
* Cleaner when only one function is needed.

---

## 4. The `__name__` Concept

Every Python file has a built-in variable called `__name__`.

### Code

```python
print(__name__)
```

### Behavior

#### When a File Is Run Directly

```bash
python3 abbas.py
```

Output:

```text
__main__
```

* This means the file is the **entry point** of the program.

#### When a File Is Imported

```python
import abbas
```

Output:

```text
abbas
```

* `__name__` is set to the **module name**, not `__main__`.

---

## 5. Why `__name__ == "__main__"` Is Important

This pattern allows code to run **only when the file is executed directly**, not when imported.

### Example

```python
def main():
    print("Running directly")

if __name__ == "__main__":
    main()
```

### Explanation

* The code inside the `if` block runs only when the file is executed directly.
* Prevents unwanted execution when the file is imported as a module.
* This is a standard Python best practice.

---

## 6. Virtual Environments and `pip`

Third-party libraries (FastAPI, Pydantic, requests) are installed with **pip** into a **virtual environment** so each project has its own isolated dependencies.

### Create a virtual environment

From your project root:

```bash
python3 -m venv .venv
```

This creates a `.venv/` directory with a private Python interpreter and `pip`.

### Activate the environment

**Linux / macOS:**

```bash
source .venv/bin/activate
```

**Windows (PowerShell):**

```powershell
.venv\Scripts\Activate.ps1
```

Your shell prompt usually changes to show the environment is active. Always activate before installing packages or running the project.

### Install packages

```bash
pip install fastapi uvicorn
```

### Freeze dependencies

Export exact versions for reproducible builds:

```bash
pip freeze > requirements.txt
```

Commit `requirements.txt` to version control. Do **not** commit `.venv/`.

### Install from requirements

```bash
pip install -r requirements.txt
```

---

## Summary

* **Module**: A single `.py` file
* **Package**: A directory containing modules
* `__init__.py`: Marks a directory as a package
* `import module`: Imports the whole module
* `from module import item`: Imports specific items
* `__name__`: Identifies how a file is executed
* **Virtual environment**: Isolated Python environment per project
* **pip**: Installs and manages third-party packages

