# 07 – Range

The `range()` function generates a sequence of numbers.

## Basic Syntax

```python
range(start, stop, step)
```

* `start` — first number (inclusive)
* `stop` — last number (exclusive)
* `step` — increment (default 1)

## Examples

```python
for i in range(2, 20, 1):
    print(f"i : {i}", end=" End \n")
```

`range(2, 20, 1)` produces numbers from 2 to 19.

```python
list(range(5))       # [0, 1, 2, 3, 4]
list(range(2, 10))   # [2, 3, 4, 5, 6, 7, 8, 9]
list(range(0, 10, 2)) # [0, 2, 4, 6, 8]
```

## Using range with len()

```python
list = ['abbas', 'mmd', 2006]
for key in range(len(list)):
    value = list[key]
    print(key, value)
```

Prefer `enumerate()` for this pattern — see [10 – More on Lists](10-more-on-list.md).
