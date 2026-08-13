# 20 – Methods

A **method** is a function defined inside a class.

## Defining and Calling Methods

```python
class test_class():
    def __init__(self, input):
        self.parm = input
        print("Class Created")

    def result(self):
        print(f"param is : {self.parm}")

var = test_class('abbas')
var.result()  # param is : abbas
```

### Explanation

* `result()` is a **method** — a function that belongs to the class.
* The first parameter is always `self`, which refers to the current object.
* `var.result()` calls the method on the object.

## self

`self` gives access to the object's attributes and other methods.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello, {self.name}")

p = Person("abbas")
p.greet()  # Hello, abbas
```

## Method vs Function

| | Function | Method |
|---|----------|--------|
| Defined | Outside a class | Inside a class |
| First arg | No `self` | Always `self` |
| Called | `func()` | `obj.method()` |

See [21 – Class Variables](21-class-variable.md) and [22 – Static and Class Methods](22-class-static-method.md) for other method types.
