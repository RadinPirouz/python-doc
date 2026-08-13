# 19 – Classes

A **class** is a blueprint for creating objects.

## Basic Class

```python
class test_class():
    def __init__(self, input):
        self.parm = input
        print("Class Created")

var = test_class('abbas')
```

### Explanation

* `test_class` is a **class** — a blueprint for creating objects.
* `__init__` is the **constructor** — runs automatically when a new object is created.
* `self.parm = input` creates an **instance attribute** called `parm`.
* `var = test_class('abbas')` creates an **object** (instance) named `var`.

## Attributes

An **attribute** is a variable that belongs to an object.

```python
print(var.parm)  # abbas
```

Each object has its own copy of instance attributes.

## Multiple Objects

```python
var1 = test_class('abbas')
var2 = test_class('mmd')
print(var1.parm)  # abbas
print(var2.parm)  # mmd
```

See [20 – Methods](20-method.md) for defining behavior on classes.
