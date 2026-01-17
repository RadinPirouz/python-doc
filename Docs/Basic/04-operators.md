### **1. `for` Loop with `range()`**

```python
for i in range(2, 20, 1):
    print(f"i : {i}", end=" End \n")
```

- **`range(start, stop, step)`**: Generates a sequence of numbers starting from `start` (inclusive), up to `stop` (exclusive), increasing by `step`.
  - `range(2, 20, 1)` → numbers from 2 to 19 (inclusive), step 1.
- **`for` loop**: Iterates over each number in the range.
- **`print(f"i : {i}", end=" End \n")`**:
  - `f-string`: Formats the output with the current value of `i`.
  - `end=" End \n"`: Replaces the default newline (`\n`) with `" End "` followed by a newline.
- **Output**: Prints each number with `" End "` at the end of each line.

---

### **2. Looping Through a List Using Index and `enumerate()`**

```python
list = ['abbas', 'mmd', 2006]
for key in range(len(list)):
    value = list[key]
    print(key, value)
```

- **`len(list)`**: Returns the number of elements in the list (3).
- **`range(len(list))`**: Creates numbers `0, 1, 2` (indices of the list).
- **`list[key]`**: Accesses the element at index `key`.
- **Output**: Prints index and value pair for each element.

---

```python
for key, value in enumerate(list):
    print(key, value)
```

- **`enumerate(list)`**: Returns pairs of `(index, value)` for each element.
- **`key, value`**: Unpacks each pair into two variables.
- **Output**: Same as above, but more concise and Pythonic.

> ✅ **Best Practice**: Use `enumerate()` instead of `range(len())` for cleaner code.

---

### **3. `zip()` Function – Pairing Two Lists**

```python
name = ['egg', 'oil']
price = [370000, 500000]
for final in zip(name, price):
    print(final)
```

- **`zip(list1, list2)`**: Combines two lists element-wise into tuples.
  - `zip(['egg', 'oil'], [370000, 500000])` → `[('egg', 370000), ('oil', 500000)]`
- **`for final in zip(...)`**: Iterates over each tuple.
- **Output**: Prints each pair as a tuple.

> ✅ Use `zip()` when you need to process multiple lists in parallel.

---

### Summary of Key Concepts:

| Concept        | Purpose |
|----------------|--------|
| `range(start, stop, step)` | Generate a sequence of numbers |
| `for` loop | Iterate over a sequence |
| `len(list)` | Get number of elements |
| `enumerate()` | Get index and value in a loop |
| `zip()` | Combine two or more lists element-wise |
