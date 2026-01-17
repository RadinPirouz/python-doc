### **1. Looping Through a List**

```python
list = [1, 2, 3, 5, 9, 'abbas']
for item in list:
    print(f'Item In List: {item}')
```

**Explanation:**
- A `list` is a collection of items in Python, enclosed in square brackets `[]`.
- The `for` loop iterates over each element in the list.
- `item` is a variable that takes the value of each element in the list one by one.
- `print(f'Item In List: {item}')` uses an f-string to display the current item.

**Output:**
```
Item In List: 1
Item In List: 2
Item In List: 3
Item In List: 5
Item In List: 9
Item In List: abbas
```

---

### **2. Looping Through a String**

```python
string = "abbas gholi"
for char_string in string:
    print(char_string)
```

**Explanation:**
- A `string` is a sequence of characters.
- The `for` loop iterates over each character in the string.
- `char_string` is a variable that holds each character in turn.
- Each character is printed on a new line.

**Output:**
```
a
b
b
a
s
 
g
h
o
l
i
```

---

### **3. Looping Through a Dictionary**

```python
price = {
    "oil": 500000,
    "egg": 350000,
    "frute": {
        "apple": 100000,
        "orange": 120000
    }
}
```

This is a dictionary with:
- Keys: `"oil"`, `"egg"`, `"frute"`
- Values: numbers and another dictionary

#### **Method 1: Looping Over Keys**

```python
for name in price:
    print(name, price[name])
```

**Explanation:**
- `for name in price` iterates over the keys of the dictionary.
- `price[name]` retrieves the value corresponding to the key.
- Prints each key and its value.

**Output:**
```
oil 500000
egg 350000
frute {'apple': 100000, 'orange': 120000}
```

#### **Method 2: Using `.items()`**

```python
for name, pr in price.items():
    print(name, pr)
```

**Explanation:**
- `.items()` returns key-value pairs as tuples.
- `name, pr` unpacks each tuple into two variables.
- More efficient and readable than accessing `price[name]`.

**Output:**
```
oil 500000
egg 350000
frute {'apple': 100000, 'orange': 120000}
```

---

### **Summary**

| Concept         | Description |
|----------------|-------------|
| **List**       | Ordered collection of items. Use `for item in list` to iterate. |
| **String**     | Sequence of characters. Use `for char in string` to access each character. |
| **Dictionary** | Key-value pairs. Use `for key in dict` or `for key, value in dict.items()` to loop. |

