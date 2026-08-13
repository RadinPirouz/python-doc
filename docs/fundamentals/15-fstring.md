# 15 – F-Strings

**F-strings** (formatted string literals) are the recommended way to embed variables in strings (Python 3.6+).

## Basic Usage

```python
f_name = "abbas"
l_name = "gholi"
age = 25

final_data = f"Information: first_name: {f_name} , last_name: {l_name} , age : {age}"
print(final_data)
```

## Expressions Inside F-Strings

```python
x = 10
print(f"Double: {x * 2}")       # Double: 20
print(f"Upper: {'abbas'.upper()}")  # Upper: ABBAS
```

## Formatting Numbers

```python
price = 500000
print(f"Price: {price:,}")       # Price: 500,000
print(f"Ratio: {0.333:.2f}")     # Ratio: 0.33
```

## Multi-line F-Strings

```python
name = "abbas"
message = f"""
Hello {name},
Welcome to the course!
"""
```

For older formatting methods, see [16 – Format](16-format.md).
