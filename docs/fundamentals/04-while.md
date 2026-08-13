# 04 – While Loops

A `while` loop runs as long as a condition is `True`.

## Basic While Loop

```python
count = 0
while count < 5:
    print(f"count: {count}")
    count += 1
```

**Output:**

```
count: 0
count: 1
count: 2
count: 3
count: 4
```

## Infinite Loop with Break

Use `break` to exit early — see [08 – Break, Continue, Pass](08-break-continue-pass.md).

```python
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == "quit":
        break
    print(f"You typed: {user_input}")
```

## While vs For

| Loop | Best for |
|------|----------|
| `while` | Unknown number of iterations, condition-driven |
| `for` | Iterating over a known sequence — see [06 – For Loops](06-for.md) |
