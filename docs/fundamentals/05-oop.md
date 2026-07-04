# 05 – Object-Oriented Programming (OOP) in Python

This document explains the basics of **Object-Oriented Programming (OOP)** in Python using simple examples.
We cover:

* Classes and objects
* Attributes and methods
* Class attributes vs instance attributes
* Inheritance
* Special (magic) methods

---

## 1. Basic Class, Attribute, and Method

### Code

```python
class test_class():
    def __init__(self, input):
        self.parm = input
        print("Class Created")

    def result(self):
        print(f"param is : {self.parm}") 

var = test_class('abbas')
var.result()
```

### Explanation

#### Class

* `test_class` is a **class**, which acts as a blueprint for creating objects.

#### `__init__` method (Constructor)

* `__init__` is a **special method** that runs automatically when a new object is created.
* `input` is a **parameter** passed when creating the object.
* `self.parm = input` creates an **instance attribute** called `parm`.

#### Attribute

* `parm` is an **attribute** (a variable that belongs to the object).
* It stores data specific to each object.

#### Method

* `result()` is a **method** (a function that belongs to the class).
* It uses `self.parm` to access the object’s data.

#### Object Creation

```python
var = test_class('abbas')
```

* Creates an object named `var`.
* Calls `__init__` automatically.

#### Method Call

```python
var.result()
```

* Calls the `result` method on the object.

---

## 2. Class Attributes vs Instance Attributes

### Code

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
var.test_value

var2.test_value = 'mmd'
var2.test_value
var.test_value
```

### Explanation

#### Class Attribute

```python
test_value = 'abbas'
```

* This is a **class attribute**.
* It belongs to the class itself.
* Shared by all objects unless overridden.

#### Instance Attribute

```python
self.parm = input
```

* This is an **instance attribute**.
* Each object has its own value.

#### Behavior Analysis

```python
var.test_value
```

* Accesses the class attribute → `'abbas'`

```python
var2.test_value = 'mmd'
```

* Creates a **new instance attribute** for `var2`.
* Does not change the class attribute.

```python
var2.test_value
```

* Returns `'mmd'` (instance attribute)

```python
var.test_value
```

* Still returns `'abbas'` (class attribute)

#### Key Rule

* Instance attributes override class attributes **only for that object**.

---

## 3. Inheritance

### Code

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

### Explanation

#### Parent Class

```python
class class_1():
```

* `class_1` is the **parent (base) class**.

#### Child Class

```python
class class_2(class_1):
```

* `class_2` **inherits** from `class_1`.
* It automatically has access to all public methods of `class_1`.

#### Method Usage

```python
self.hi()
```

* `hi()` is defined in `class_1`.
* Because of inheritance, `class_2` can call it.

#### Output Order

```text
Class 2 Created
Hi
```

#### Important Note

* `class_1.__init__()` is **not called automatically** here.
* To call it, you would need:

```python
super().__init__()
```

---

## 4. Special (Magic) Methods

### Code

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

### Explanation

Special methods start and end with **double underscores (`__`)** and control built-in behavior.

#### `__init__`

* Runs when an object is created.

#### `__len__`

```python
len(object)
```

* Defines the behavior of `len()` on the object.
* Returns `1` in this example.

#### `__str__`

```python
print(object)
```

* Defines the string representation of the object.
* Used by `print()` and `str()`.

#### `__del__`

* Runs when the object is deleted or garbage-collected.
* Used rarely in modern Python.
* Return value is ignored.

---

## Summary

* **Class**: Blueprint for objects
* **Object**: Instance of a class
* **Attribute**: Data stored in an object
* **Method**: Function inside a class
* **Class Attribute**: Shared across all objects
* **Instance Attribute**: Unique per object
* **Inheritance**: Child class reuses parent class logic
* **Magic Methods**: Customize built-in Python behavior

