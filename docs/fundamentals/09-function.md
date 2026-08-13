# 09 – Functions

Functions are reusable blocks of code defined with `def`.

## Defining and Calling

```python
def greet(name):
    print(f"Hello, {name}")

greet("abbas")  # Hello, abbas
```

## Parameters and Return Values

```python
def add(a, b):
    return a + b

result = add(3, 5)
print(result)  # 8
```

## Default Parameters

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}")

greet("abbas")           # Hello, abbas
greet("mmd", "Hi")       # Hi, mmd
```

## Keyword Arguments

```python
def create_user(name, age, city):
    print(f"{name}, {age}, {city}")

create_user(name="abbas", city="Tehran", age=25)
```

## *args and **kwargs

```python
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3, 4))  # 10

def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="abbas", age=25)
```

## Scope

Variables defined inside a function are **local**. See [18 – Scope](18-scope.md) for details.
