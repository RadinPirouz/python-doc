# 12 – Sets

Sets are **unordered** collections with **no duplicate** members.

## Creating Sets

```python
numbers = {1, 2, 3, 3, 4}
print(numbers)  # {1, 2, 3, 4} — duplicates removed

fruits = set(['apple', 'orange', 'apple'])
print(fruits)   # {'apple', 'orange'}
```

## Adding and Removing

```python
s = {1, 2, 3}
s.add(4)
s.remove(2)
s.discard(99)   # no error if missing
```

## Set Operations

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)   # union: {1, 2, 3, 4, 5, 6}
print(a & b)   # intersection: {3, 4}
print(a - b)   # difference: {1, 2}
print(a ^ b)   # symmetric difference: {1, 2, 5, 6}
```

## Membership Test

```python
s = {'abbas', 'mmd', 'asghar'}
print('abbas' in s)   # True
print('ali' in s)     # False
```

Sets are useful for removing duplicates and fast membership checks.
