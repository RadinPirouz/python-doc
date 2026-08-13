# 03 – Lists

Lists are **ordered** and **mutable** (changeable) collections.

## Creating and Accessing

```python
list = [1, 2, 3, 'pizze']
print(list[0])    # 1
print(list[1:3])  # [2, 3]
print(list[-1])   # 'pizze'
print(len(list))  # 4
```

## Adding and Removing

```python
list = [1, 2, 3]
list.append(4)       # [1, 2, 3, 4]
list.insert(0, 0)      # [0, 1, 2, 3, 4]
list.remove(2)         # [0, 1, 3, 4]
last = list.pop()      # removes and returns last item
```

## List Methods

```python
list = [1, 2, 3, 'pizze']
print(list.count(3))  # 1

list_sort = [9, 5, 1, 10]
list_sort.sort()
print(list_sort)      # [1, 5, 9, 10]

list_mut = [9, 5, 1, 10]
list_mut[0] = 2
list_mut.reverse()
print(list_mut)       # [10, 1, 5, 2]
```

See [10 – More on Lists](10-more-on-list.md) for `enumerate()`, `zip()`, and advanced iteration.
