# 10 – More on Lists

Advanced list iteration and pairing techniques.

## Looping with enumerate()

```python
list = ['abbas', 'mmd', 2006]
for key, value in enumerate(list):
    print(key, value)
```

**Output:**

```
0 abbas
1 mmd
2 2006
```

> Use `enumerate()` instead of `range(len())` for cleaner code.

## zip() — Pairing Two Lists

```python
name = ['egg', 'oil']
price = [370000, 500000]
for final in zip(name, price):
    print(final)
```

**Output:**

```
('egg', 370000)
('oil', 500000)
```

## List Comprehensions

```python
squares = [x ** 2 for x in range(5)]
print(squares)  # [0, 1, 4, 9, 16]

evens = [x for x in range(10) if x % 2 == 0]
print(evens)    # [0, 2, 4, 6, 8]
```

## Summary

| Concept | Purpose |
|---------|---------|
| `enumerate()` | Get index and value in a loop |
| `zip()` | Combine two or more lists element-wise |
| List comprehension | Build a new list from an expression |
