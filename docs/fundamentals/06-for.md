# 06 – For Loops

## Looping Through a List

```python
list = [1, 2, 3, 5, 9, 'abbas']
for item in list:
    print(f'Item In List: {item}')
```

**Output:**

```
Item In List: 1
Item In List: 2
Item In List: 3
Item In List: 5
Item In List: 9
Item In List: abbas
```

## Looping Through a String

```python
string = "abbas gholi"
for char_string in string:
    print(char_string)
```

Each character is printed on a new line.

## Looping with range()

See [07 – Range](07-range.md) for generating number sequences.

```python
for i in range(3):
    print(i)  # 0, 1, 2
```

## Summary

| Concept | Usage |
|---------|-------|
| **List** | `for item in list` |
| **String** | `for char in string` |
| **Dictionary** | See [13 – Dictionaries](13-dict.md) |
