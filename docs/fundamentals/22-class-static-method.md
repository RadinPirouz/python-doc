# 22 – Class and Static Methods

Besides regular instance methods, Python supports **class methods** and **static methods**.

## @classmethod

Receives the class (`cls`) as the first argument instead of an instance (`self`).

```python
class Person:
    count = 0

    def __init__(self, name):
        self.name = name
        Person.count += 1

    @classmethod
    def get_count(cls):
        return cls.count

    @classmethod
    def from_string(cls, data):
        name = data.split('-')[0]
        return cls(name)

p = Person.from_string("abbas-25")
print(p.name)              # abbas
print(Person.get_count())  # 1
```

* `@classmethod` is often used for **alternative constructors** like `from_string`.

## @staticmethod

Does not receive `self` or `cls` — behaves like a regular function inside the class namespace.

```python
class MathHelper:
    @staticmethod
    def add(a, b):
        return a + b

print(MathHelper.add(3, 5))  # 8
```

## Comparison

| Type | First argument | Typical use |
|------|----------------|-------------|
| Instance method | `self` | Access/modify instance data |
| `@classmethod` | `cls` | Factory methods, class-level logic |
| `@staticmethod` | None | Utility functions related to the class |
