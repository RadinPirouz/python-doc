# Python Data Types and Basic Operations

## Data Types Overview

| Type | Description | Examples |
| :--- | :--- | :--- |
| **int** | Normal whole numbers. | `1`, `-100`, `20326`, `0` |
| **float** | Numbers with decimal points. | (No example provided) |
| **string** | Text enclosed in quotes. | `"txt"` |
| **list** | Ordered, changeable collection of items. | `[1, 2, 3, 4, 5]`, `['abbas', 2, -10, 2.65]` |
| **dictionary** | Unordered, changeable collection of key-value pairs. | `{'name': 'abbas', 'l_name': 'gholi'}` |
| **set** | Unordered collection that does not allow duplicate members. | Like `set`, but no duplications. |

**Checking Type:**
```python
print(type(var)) # Return Variable Type
```

---

## Strings

Strings are **immutable** (cannot be changed after creation, e.g., `str[0] = 'i'` fails).

### String Indexing and Slicing

Indexing starts at 0. Negative indexing starts from the end (-1 is the last character).

```python
string="this is string about abbas gholi #1"
print(string[0])    # --> Return t
print(string[1:3])  # --> Return hi (End index is exclusive)
print(string[-1])   # --> Return 1
# Format: [start : end]
```

**Note on slicing examples provided (These might be incorrect based on standard Python slicing):**
```python
string='012345678'
print(string[5])    # --> return '5'
# print(string[1,5]) # Incorrect syntax for standard slicing
# print(string[1,4,2]) # Incorrect syntax for standard slicing
print(string[::-1]) # --> return 876543210 (Reverses the string)
```

### Multi-line Strings

Use triple quotes (`"""` or `'''`) for strings spanning multiple lines, including newlines and quotes.

```python
"""
Hi This Is String Without Any Limits Like
enter 
" "
and any more 
"""
```

### String Methods

```python
string="AbbAsGholi"
print(string.upper()) # Return ABBASGHOLI
print(string.lower()) # Return abbasgholi
print(string.islower()) # Return False
print(string.index('i')) # Return 8 (Index of first occurrence)
```

**Splitting Strings:**
```python
string = "abbas,mmd,asghar"
list_strings = string.split(",")
print(list_strings) # out put is : ['abbas','mmd','asghar'] 
```

### String Formatting

Multiple ways to embed variables into strings:

| Method | Example |
| :--- | :--- |
| `%` formatting | `final_data = " Information: first_name: %s , last_name: %s , age : %i" % (f_name,l_name,age)` |
| `.format()` (Positional) | `final_data = "Information: first_name: {} , last_name: {} , age : {}".format(f_name,l_name,age)` |
| `.format()` (Indexed) | `final_data = "Information: first_name: {0} , last_name: {1} , age : {2}".format(f_name,l_name,age)` |
| `.format()` (Named) | `final_data = "Information: first_name: {f} , last_name: {l} , age : {a}".format(f=f_name,l=l_name,a=age)` |
| **F-Strings (Recommended)** | `final_data = f"Information: first_name: {f_name} , last_name: {l_name} , age : {age}"` |

---

## Lists

Lists are ordered and mutable (changeable).

### Accessing and Length

```python
list = [1, 2, 3, 'pizze']
print(list[0])   # Return 1
print(list[1:3]) # Return [2, 3]
print(list[-1])  # Return 'pizze'
print(len(list)) # Return 4
```

### List Modification and Methods

```python
list = [1, 2, 3, 'pizze']
print(list.count(3)) # return 1 (Assuming you meant count of '3')

# Pop (removes and returns the last item)
list_last_value = list.pop()
print(list_last_value, list) # return 'pizze' , [1, 2, 3]

# Sorting
list_sort = [9, 5, 1, 10]
list_sort.sort()
print(list_sort) # [1, 5, 9, 10]

# Mutability (Changing an element)
list_mut = [9, 5, 1, 10]
list_mut[0] = 2
print(list_mut) # [2, 5, 1, 10]

# Reversing
list_mut.reverse()
print(list_mut) # [10, 1, 5, 2]
```

---

## Dictionaries

Dictionaries store data in key-value pairs and are mutable.

**Note:** The structure provided in the original input for nested fruits is missing commas between key-value pairs, which causes a syntax error in standard Python. The corrected structure is assumed below for accessing methods.

**Assumed Corrected Structure:**
```python
price = {
    "oil": 500000,
    "egg": 350000,
    "frute" : {
        "apple": 100000,
        "orange": 120000
    }
}
```

### Dictionary Access and Methods

```python
print(price['oil'])                 # Access by key: Return 500000
print(price['frute']['apple'])      # Nested access: Return 100000

print(price.keys())                 # show keys
print(price.values())               # show values
print(price.items())                # show all key , values

print(price.get('egg'))             # Return egg value (350000)
print(price.get('water'), -1)       # Return default value (-1) if key 'water' is not found
```