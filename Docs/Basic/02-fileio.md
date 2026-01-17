## Python File Handling Operations

This document explains common operations performed on files in Python.

### Opening a File

The `open()` function is used to open a file. It takes two main arguments: the file name and the mode.

```python
f = open("file.txt")
```

### Reading File Content

There are several ways to read the content of a file:

1.  **`f.read()`**: Reads the entire content of the file as a single string.
2.  **`f.readlines()`**: Reads all lines of the file and returns them as a list of strings.
3.  **`f.read()` followed by `f.read()`**: This sequence reads the entire file content twice, the second time returning an empty string.
4.  **`with open(...) as f:`**: This context manager automatically closes the file after the block of code is executed, even if errors occur. This is the recommended way to work with files.

```python
with open("file.txt") as f:
    lines = f.readlines()
    print(lines[-1])
```

This code opens the file "file.txt", reads all lines into a list called `lines`, and then prints the last line of the file.

### Closing a File

It's important to close the file after you are done with it to release system resources.

```python
file = open("file.txt" , 'r') # read
file.read()
file.close()
```

### Writing to a File

You can write to a file using the `open()` function with the `'w'` (write) or `'a'` (append) modes.

1.  **`'w'` (write)**: Opens the file for writing. If the file exists, its content is overwritten. If the file does not exist, it creates a new file.
2.  **`'a'` (append)**: Opens the file for appending. New data is added to the end of the file. If the file does not exist, it creates a new file.

```python
file = open("file.txt" , 'r') # read
file.read()
file.close()

file = open("file.txt" , 'w') # write
file.write("This Is Write Message")
file.close()

file = open("file.txt" , 'a') # append
file.write("This Is Append Message")
file.close()
```

This code first reads the file, then opens it in write mode and writes the string "This Is Write Message", overwriting any existing content. Finally, it opens the file in append mode and adds the string "This Is Append Message" to the end of the file.

