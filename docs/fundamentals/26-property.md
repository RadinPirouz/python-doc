# 26 – Property

The `@property` decorator lets you define **getter**, **setter**, and **deleter** methods that look like attribute access.

## Basic Property

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value

    @property
    def area(self):
        return 3.14159 * self._radius ** 2

c = Circle(5)
print(c.radius)  # 5
print(c.area)    # 78.53975

c.radius = 10
print(c.area)    # 314.159
```

## Why Use @property?

* Validate values before assignment
* Compute derived values on access (like `area`)
* Keep a clean attribute-style API without exposing internal storage

## Without @property

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius

    def get_radius(self):
        return self.radius

c = Circle(5)
c.get_radius()  # works, but less Pythonic
```

`@property` makes `c.radius` feel like a simple attribute while running validation logic behind the scenes.
