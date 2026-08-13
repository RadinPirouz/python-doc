# 14 – Modules and Packages

This document explains how **modules**, **packages**, and the `__name__` concept work in Python.

---

## 1. Importing External Modules

Python allows you to import **external libraries** installed in your environment.

### Example: Using the `emoji` Module

```python
import emoji
print(emoji.emojize("abbas is :red_heart:"))
```

* `import emoji` imports the entire `emoji` module.
* You must use the module name (`emoji`) to access its functions.

### Importing a Specific Function

```python
from emoji import emojize
print(emojize("abbas is :red_heart:"))
```

* Imports only the `emojize` function — call it directly without the module prefix.

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

* `__init__.py` tells Python that the directory is a package (can be empty).

### Importing from a Package

```python
from honor import hi
hi.hello()
```

```python
from honor.hi import hello
hello()
```

---

## 4. The `__name__` Concept

Every Python file has a built-in variable called `__name__`.

### When a File Is Run Directly

```bash
python3 abbas.py
```

Output: `__main__`

### When a File Is Imported

```python
import abbas
```

Output: `abbas` (the module name)

---

## 5. Why `__name__ == "__main__"` Is Important

```python
def main():
    print("Running directly")

if __name__ == "__main__":
    main()
```

* Code inside the `if` block runs only when the file is executed directly.
* Prevents unwanted execution when the file is imported as a module.

---

## Summary

* **Module**: A single `.py` file
* **Package**: A directory containing modules
* `__init__.py`: Marks a directory as a package
* `import module`: Imports the whole module
* `from module import item`: Imports specific items
* `__name__`: Identifies how a file is executed

For installing third-party packages, see [30 – Virtual Environments](30-venv.md).
