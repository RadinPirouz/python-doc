# 18 – Scope

Scope determines where a variable can be accessed in your code.

## Local Scope

Variables defined inside a function are **local** — only visible inside that function.

```python
def greet():
    message = "Hello"
    print(message)

greet()
# print(message)  # NameError — message does not exist outside greet()
```

## Global Scope

Variables defined at the module level are **global** — accessible anywhere in the module.

```python
name = "abbas"

def show():
    print(name)

show()  # abbas
```

## Modifying Global Variables

Use the `global` keyword to change a global variable inside a function.

```python
count = 0

def increment():
    global count
    count += 1

increment()
print(count)  # 1
```

## Enclosing Scope (nonlocal)

Use `nonlocal` to modify a variable in an enclosing (non-global) scope.

```python
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 5
        print(x)
    inner()

outer()  # 15
```

## LEGB Rule

Python looks up names in this order:

1. **L**ocal — inside the current function
2. **E**nclosing — in enclosing functions
3. **G**lobal — at module level
4. **B**uilt-in — Python built-in names
