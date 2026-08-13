# 08 – Break, Continue, and Pass

Control flow keywords used inside loops.

## break

Exits the loop immediately.

```python
for i in range(10):
    if i == 5:
        break
    print(i)  # 0, 1, 2, 3, 4
```

## continue

Skips the rest of the current iteration and moves to the next.

```python
for i in range(5):
    if i == 2:
        continue
    print(i)  # 0, 1, 3, 4
```

## pass

A no-op placeholder — does nothing. Used when syntax requires a block but you have no code yet.

```python
for item in [1, 2, 3]:
    pass  # TODO: implement later
```

```python
def not_implemented_yet():
    pass
```

## Summary

| Keyword | Effect |
|---------|--------|
| `break` | Exit the loop |
| `continue` | Skip to next iteration |
| `pass` | Do nothing (placeholder) |
