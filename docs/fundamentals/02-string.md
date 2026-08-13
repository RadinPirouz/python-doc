# 02 – Strings

Strings are **immutable** (cannot be changed after creation, e.g., `str[0] = 'i'` fails).

## String Indexing and Slicing

Indexing starts at 0. Negative indexing starts from the end (-1 is the last character).

```python
string = "this is string about abbas gholi #1"
print(string[0])    # --> t
print(string[1:3])  # --> hi (end index is exclusive)
print(string[-1])   # --> 1
# Format: [start : end]
```

```python
string = '012345678'
print(string[5])    # --> '5'
print(string[::-1]) # --> 876543210 (reverses the string)
```

## Multi-line Strings

Use triple quotes (`"""` or `'''`) for strings spanning multiple lines.

```python
"""
Hi This Is String Without Any Limits Like
enter 
" "
and any more 
"""
```

## String Methods

```python
string = "AbbAsGholi"
print(string.upper())    # ABBASGHOLI
print(string.lower())    # abbasgholi
print(string.islower())  # False
print(string.index('i')) # 8 (index of first occurrence)
```

**Splitting strings:**

```python
string = "abbas,mmd,asghar"
list_strings = string.split(",")
print(list_strings)  # ['abbas', 'mmd', 'asghar']
```

For embedding variables in strings, see [15 – F-Strings](15-fstring.md) and [16 – Format](16-format.md).
