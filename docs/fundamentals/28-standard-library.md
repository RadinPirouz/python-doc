# 28 – Python Standard Library

This document introduces some of the most commonly used **Python standard library** modules:

* `datetime`
* `math`
* `random`
* `decimal`

These modules come bundled with Python and require no external installation.

---

## 1. Date and Time with `datetime`

The `datetime` module provides classes for working with dates and times.

---

### Working with Dates

#### Code

```python
import datetime

date_1 = datetime.date(2026, 1, 1)

print(date_1.year)
print(date_1.month)
print(date_1.day)

print(date_1.weekday)
print(date_1.ctime)
```

#### Explanation

* `datetime.date(year, month, day)` creates a date object.
* `.year`, `.month`, `.day` access individual components.

#### Important Note

```python
date_1.weekday()
```

* Returns the day of the week as an integer:

  * Monday = 0
  * Sunday = 6

```python
date_1.ctime()
```

* Returns a human-readable string representation of the date.

---

### Working with Time

#### Code

```python
time_1 = datetime.time(12, 12)

print(time_1.hour)
print(time_1.min)
```

#### Explanation

* `datetime.time(hour, minute)` creates a time object.
* `.hour` returns the hour.
* `.minute` returns the minute.

---

### Working with Date and Time Together

#### Code

```python
abbas_birth = datetime.datetime(2026, 1, 1, 12, 12)
today = datetime.date.today()
now = datetime.datetime.now()

diff_time = now - abbas_birth
```

#### Explanation

* `datetime.datetime` includes both date and time.
* `date.today()` returns today’s date.
* `datetime.now()` returns the current date and time.
* Subtracting two `datetime` objects returns a `timedelta`.

---

## 2. Mathematical Operations with `math`

The `math` module provides advanced mathematical functions and constants.

---

### Mathematical Constants

```python
import math

print(math.pi)
print(math.e)
print(math.inf)
```

* `math.pi`: π constant
* `math.e`: Euler’s number
* `math.inf`: infinity

---

### Power and Rounding

```python
print(math.pow(2, 3))

print(round(4.2))
print(round(4.8))
```

* `math.pow(a, b)` returns `a` raised to the power of `b`.
* `round()` rounds to the nearest integer.

---

### Floor and Ceil

```python
print(math.floor(4.2))
print(math.floor(4.9))

print(math.ceil(4.2))
print(math.ceil(4.9))
```

* `floor`: rounds down
* `ceil`: rounds up

---

### Logarithms

```python
print(math.log(100, 10))
```

* Returns the logarithm of 100 with base 10.

---

## 3. Random Values with `random`

The `random` module is used to generate pseudo-random values.

---

### Random Numbers

```python
import random

print(random.randint(1, 6))
print(random.random())
```

* `randint(a, b)`: random integer between `a` and `b` (inclusive)
* `random()`: random float between `0` and `1`

---

### Random Selection

```python
number_list = list(range(15))
print(random.choice(number_list))

char_list = ['a', 'm', 's']
print(random.choice(char_list))
```

* `choice()` selects a random element from a sequence.

---

### Shuffling

```python
random.shuffle(number_list)
print(number_list)
```

* `shuffle()` randomly rearranges the list in place.

---

## 4. Decimal Precision with `decimal`

The `decimal` module provides precise decimal arithmetic, avoiding floating-point errors.

---

### Decimal Context

```python
import decimal

print(decimal.getcontext())
```

* Shows current precision and rounding settings.

---

### Float vs Decimal

```python
print(decimal.Decimal(0.1))
print(decimal.Decimal('0.1'))
```

* Passing a float carries floating-point error.
* Passing a string preserves exact value.

---

### Precision Comparison

```python
print(0.1 + 0.2 == 0.3)
```

Returns `False` due to floating-point precision issues.

```python
print(decimal.Decimal(0.1) + decimal.Decimal(0.2) == decimal.Decimal(0.3))
```

Still `False` because the floats are imprecise.

```python
print(decimal.Decimal('0.1') + decimal.Decimal('0.2') == decimal.Decimal('0.3'))
```

Returns `True` because strings preserve precision.

---

## Summary

* `datetime` handles dates and times
* `math` provides mathematical constants and functions
* `random` generates pseudo-random values
* `decimal` solves floating-point precision problems
* Always use strings when creating `Decimal` values

