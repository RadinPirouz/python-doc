# 17 – Python File Handling

This document explains common operations performed on files in Python.

### Opening a File

The `open()` function is used to open a file. It takes two main arguments: the file name and the mode.

```python
f = open("file.txt")
```

### Reading File Content

1. **`f.read()`**: Reads the entire content of the file as a single string.
2. **`f.readlines()`**: Reads all lines of the file and returns them as a list of strings.
3. **`with open(...) as f:`**: This context manager automatically closes the file after the block of code is executed. This is the recommended way to work with files.

```python
with open("file.txt") as f:
    lines = f.readlines()
    print(lines[-1])
```

### Closing a File

It's important to close the file after you are done with it to release system resources.

```python
file = open("file.txt", 'r')  # read
file.read()
file.close()
```

### Writing to a File

1. **`'w'` (write)**: Opens the file for writing. Overwrites existing content or creates a new file.
2. **`'a'` (append)**: Opens the file for appending. New data is added to the end.

```python
file = open("file.txt", 'r')
file.read()
file.close()

file = open("file.txt", 'w')
file.write("This Is Write Message")
file.close()

file = open("file.txt", 'a')
file.write("This Is Append Message")
file.close()
```
