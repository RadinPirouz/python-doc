# 24 – Special (Magic) Methods

Special methods start and end with **double underscores (`__`)** and control built-in Python behavior.

## Common Magic Methods

```python
class class_1():
    def __init__(self):
        print("Class 1 Created")

    def __len__(self):
        return 1

    def __str__(self):
        return 'print command on class'

    def __del__(self):
        return 'on del value'
```

| Method | Triggered by | Purpose |
|--------|-------------|---------|
| `__init__` | Object creation | Constructor |
| `__len__` | `len(object)` | Define length |
| `__str__` | `print(object)`, `str(object)` | Human-readable string |
| `__repr__` | `repr(object)` | Developer representation |
| `__del__` | Object deletion | Cleanup (rarely used) |

## Examples

```python
class Box:
    def __init__(self, items):
        self.items = items

    def __len__(self):
        return len(self.items)

    def __str__(self):
        return f"Box with {len(self.items)} items"

b = Box([1, 2, 3])
print(len(b))   # 3
print(b)        # Box with 3 items
```

## Other Useful Magic Methods

* `__eq__` — equality (`==`)
* `__lt__` — less than (`<`)
* `__getitem__` — indexing (`obj[key]`)
* `__call__` — calling the object like a function
