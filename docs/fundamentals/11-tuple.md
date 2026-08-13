# 11 – Tuples

Tuples are **ordered** and **immutable** collections — like lists, but cannot be changed after creation.

## Creating Tuples

```python
point = (10, 20)
single = (42,)       # comma required for one-element tuple
mixed = ('abbas', 25, True)
empty = ()
```

## Accessing Elements

```python
point = (10, 20, 30)
print(point[0])    # 10
print(point[-1])   # 30
print(point[1:3])  # (20, 30)
```

## Tuple vs List

| Feature | Tuple | List |
|---------|-------|------|
| Mutable | No | Yes |
| Syntax | `(1, 2, 3)` | `[1, 2, 3]` |
| Use case | Fixed data, dict keys | Changeable collections |

## Unpacking

```python
name, age, city = ('abbas', 25, 'Tehran')
print(name)  # abbas

a, *rest = (1, 2, 3, 4)
print(a)     # 1
print(rest)  # [2, 3, 4]
```

## When to Use Tuples

* Return multiple values from a function
* Dictionary keys (lists cannot be keys)
* Data that should not change
