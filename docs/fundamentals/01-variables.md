# 01 – Variables and Data Types

## Data Types Overview

| Type | Description | Examples |
| :--- | :--- | :--- |
| **int** | Normal whole numbers. | `1`, `-100`, `20326`, `0` |
| **float** | Numbers with decimal points. | `3.14`, `-0.5`, `2.0` |
| **string** | Text enclosed in quotes. | `"txt"`, `'hello'` |
| **bool** | True or False. | `True`, `False` |

## Assigning Variables

```python
name = "abbas"
age = 25
price = 3.14
is_active = True
```

## Checking Type

```python
print(type(name))   # <class 'str'>
print(type(age))    # <class 'int'>
print(type(price))  # <class 'float'>
```

## Type Conversion

```python
x = int("42")       # string → int
y = float("3.14")   # string → float
z = str(100)        # int → string
```

See [02 – Strings](02-string.md) for text operations and [03 – Lists](03-list.md) for collections.
