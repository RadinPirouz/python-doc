# 25 – Access Modifiers

Python does not enforce strict private access like Java or C++, but uses naming conventions to signal intent.

## Public (Default)

All attributes and methods are public by default.

```python
class Person:
    def __init__(self, name):
        self.name = name

p = Person("abbas")
print(p.name)  # abbas — accessible
```

## Protected (_single underscore)

A single leading underscore signals "internal use" — convention only, not enforced.

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        self._balance += amount
```

## Private (__double underscore)

Double underscore triggers **name mangling** — Python renames the attribute to `_ClassName__attr`.

```python
class Secret:
    def __init__(self):
        self.__key = "hidden"

    def get_key(self):
        return self.__key

s = Secret()
print(s.get_key())       # hidden
# print(s.__key)         # AttributeError
print(s._Secret__key)    # hidden (mangled name — avoid in normal code)
```

## Summary

| Convention | Meaning | Enforced? |
|------------|---------|-----------|
| `name` | Public | — |
| `_name` | Protected (internal) | No |
| `__name` | Private (name mangling) | Partially |

Use [26 – Property](26-property.md) for controlled access to attributes.
