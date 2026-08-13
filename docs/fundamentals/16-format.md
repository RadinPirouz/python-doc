# 16 – String Formatting

Ways to embed variables into strings before f-strings (still useful in legacy code).

## % Formatting

```python
f_name = "abbas"
l_name = "gholi"
age = 25

final_data = "Information: first_name: %s , last_name: %s , age : %i" % (f_name, l_name, age)
print(final_data)
```

| Code | Type |
|------|------|
| `%s` | String |
| `%d` / `%i` | Integer |
| `%f` | Float |

## .format() Method

### Positional

```python
final_data = "Information: first_name: {} , last_name: {} , age : {}".format(f_name, l_name, age)
```

### Indexed

```python
final_data = "Information: first_name: {0} , last_name: {1} , age : {2}".format(f_name, l_name, age)
```

### Named

```python
final_data = "Information: first_name: {f} , last_name: {l} , age : {a}".format(f=f_name, l=l_name, a=age)
```

## Recommendation

Prefer [15 – F-Strings](15-fstring.md) for new code — they are shorter and faster.
