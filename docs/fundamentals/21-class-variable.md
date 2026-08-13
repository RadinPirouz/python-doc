# 21 – Class Variables

Class attributes are shared across all instances; instance attributes are unique per object.

## Class Attribute vs Instance Attribute

```python
class test_class():
    test_value = 'abbas'

    def __init__(self, input):
        self.parm = input
        print("Class Created")

    def result(self):
        print(f"param is : {self.parm}")

var = test_class('abbas')
var2 = test_class('mmd')

var.result()
print(var.test_value)   # abbas
```

## Overriding Class Attributes

```python
var2.test_value = 'mmd'   # creates instance attribute on var2
print(var2.test_value)    # mmd
print(var.test_value)     # abbas (class attribute unchanged)
```

### Key Rule

* `test_value = 'abbas'` is a **class attribute** — shared by all objects.
* `self.parm = input` is an **instance attribute** — unique per object.
* Assigning `var2.test_value = 'mmd'` creates a **new instance attribute** on `var2` without changing the class attribute.

## Accessing Class Attributes

```python
class Counter:
    total = 0

    def __init__(self):
        Counter.total += 1

a = Counter()
b = Counter()
print(Counter.total)  # 2
```

Use the class name (`Counter.total`) when you want to read or modify the shared class attribute explicitly.
