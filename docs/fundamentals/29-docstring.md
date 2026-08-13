# 29 – Docstrings

A **docstring** is a string literal that documents a module, class, function, or method. It is stored in `__doc__` and shown by `help()`.

## Function Docstring

```python
def add(a, b):
    """Return the sum of a and b."""
    return a + b

print(add.__doc__)   # Return the sum of a and b.
help(add)
```

## Class Docstring

```python
class Person:
    """Represent a person with a name and age."""

    def __init__(self, name, age):
        """Initialize a Person with name and age."""
        self.name = name
        self.age = age
```

## Module Docstring

Place at the very top of a `.py` file:

```python
"""
User management module.

Provides functions for creating and validating users.
"""
```

## Conventions

* Use triple double-quotes (`"""..."""`)
* One-line docstring for simple functions
* Multi-line for complex functions — summary line, blank line, then details

```python
def connect(host, port):
    """
    Connect to a server.

    Args:
        host: Server hostname or IP.
        port: Port number.

    Returns:
        A connection object.
    """
```

See [PEP 257](https://peps.python.org/pep-0257/) for full docstring conventions.
