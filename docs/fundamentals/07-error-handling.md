# 06 – Error Handling, Linting, Formatting, and Testing in Python

This document explains how Python handles runtime errors, how to write safer code using `try / except`, and how to improve code quality using **linting**, **formatting**, and **unit testing** tools.

---

## 1. Error Handling with `try / except`

Python uses `try / except` blocks to handle runtime errors gracefully without crashing the program.

### Example

```python
def abbas(a, b):
    try:
        res = a / b
        print(res)
    except ZeroDivisionError:
        print("Zero Number Detected")
    except Exception as e:
        print(f"Error Detected {e}")

abbas(1, 0)
```

### Explanation

* The code inside `try` is executed first.
* If no error occurs, the result is printed.
* If `b` is `0`, a `ZeroDivisionError` is raised.
* The specific `ZeroDivisionError` block runs first.
* Any other error is caught by the generic `Exception` block.

### Key Rule

* Always catch **specific exceptions first**.
* Use `Exception` only as a fallback.

---

## 2. Full `try / except` Structure

Python supports additional blocks for more control.

### Syntax

```python
try:
    # code that may raise an error
except:
    # runs if an error occurs
else:
    # runs if no error occurs
finally:
    # always runs
```

### Explanation

* `try`: code that may fail
* `except`: handles errors
* `else`: runs only if no exception occurred
* `finally`: runs no matter what (used for cleanup)

---

## 3. Linting with `pylint`

Linting analyzes code for:

* Syntax errors
* Style problems
* Bad practices

### Basic Command

```bash
pylint main.py
```

### Detailed Report

```bash
pylint --report y main.py
```

### Explanation

* `pylint` gives a score and suggestions.
* Helps maintain readable and maintainable code.
* Commonly used in professional Python projects.

---

## 4. Code Formatting with `black`

`black` is an automatic code formatter that enforces a consistent style.

### Command

```bash
black main.py
```

### Explanation

* Reformats code automatically.
* Removes style debates.
* Safe and widely used.

---

## 5. Unit Testing with `unittest`

Unit tests verify that individual parts of code behave as expected.

---

### Application Code

#### `abbas.py`

```python
def bemola(a, b):
    try:
        res = a / b
        print(res)
    except ZeroDivisionError:
        print("Zero Number Detected")
    except Exception as e:
        print(f"Error Detected {e}")
```

---

### Test Code

#### `abbas_test.py`

```python
import unittest
from abbas import bemola

class TestAbbas(unittest.TestCase):

    def test_bemola(self):
        a = 10
        b = 2
        self.assertEqual(bemola(a, b), 5)

if __name__ == "__main__":
    unittest.main()
```

---

### Explanation

#### `unittest.TestCase`

* Base class for writing test cases.

#### Test Method

```python
def test_bemola(self):
```

* Test methods must start with `test_`.

#### Assertion

```python
self.assertEqual(bemola(a, b), 5)
```

* Checks if the function returns the expected result.

---

### Important Note (Design Issue)

The function `bemola` **prints** the result but does not return it.

```python
print(res)
```

This causes the test to fail because the function returns `None`.

#### Correct Implementation

```python
def bemola(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "Zero Number Detected"
    except Exception as e:
        return f"Error Detected {e}"
```

This version:

* Returns values instead of printing
* Is testable
* Follows best practices

---

## Summary

* `try / except` prevents program crashes
* `else` runs only when no error occurs
* `finally` always runs
* `pylint` improves code quality
* `black` enforces formatting
* `unittest` verifies correctness
* Functions should **return values**, not print them, when tested
