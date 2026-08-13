# 23 – Inheritance

**Inheritance** lets a child class reuse attributes and methods from a parent class.

## Basic Inheritance

```python
class class_1():
    def __init__(self):
        print("Class 1 Created")

    def hi(self):
        print("Hi")

class class_2(class_1):
    def __init__(self):
        print("Class 2 Created")
        self.hi()

b = class_2()
```

**Output:**

```
Class 2 Created
Hi
```

### Explanation

* `class_1` is the **parent (base) class**.
* `class_2(class_1)` **inherits** from `class_1` and can use its methods.
* `hi()` is defined in `class_1` but callable from `class_2`.

## Calling the Parent Constructor

`class_1.__init__()` is **not called automatically** unless you use `super()`.

```python
class class_2(class_1):
    def __init__(self):
        super().__init__()
        print("Class 2 Created")
```

## Overriding Methods

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):
    def speak(self):
        print("Woof")

d = Dog()
d.speak()  # Woof
```

The child class replaces the parent's method with its own version.
