# 13 – Dictionaries

Dictionaries store data as **key-value pairs** and are **mutable**.

## Creating and Accessing

```python
price = {
    "oil": 500000,
    "egg": 350000,
    "frute": {
        "apple": 100000,
        "orange": 120000
    }
}

print(price['oil'])              # 500000
print(price['frute']['apple'])   # 100000
```

## Dictionary Methods

```python
print(price.keys())    # dict_keys(['oil', 'egg', 'frute'])
print(price.values())  # dict_values([500000, 350000, {...}])
print(price.items())   # key-value pairs

print(price.get('egg'))           # 350000
print(price.get('water', -1))     # -1 (default if key missing)
```

## Modifying

```python
price['milk'] = 200000   # add or update
del price['egg']         # remove key
```

## Looping Over a Dictionary

### Method 1: Looping Over Keys

```python
for name in price:
    print(name, price[name])
```

### Method 2: Using .items()

```python
for name, pr in price.items():
    print(name, pr)
```

`.items()` is more readable than accessing `price[name]` inside the loop.
