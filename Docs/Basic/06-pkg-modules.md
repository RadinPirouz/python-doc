```python
import emoji
print(emoji.emojize("abbas is :red_heart:"))
```

```python
from emoji import emojize
print(emojize("abbas is :red_heart:"))
```

---

create modules

hi.py
```python
def hi():
    print("Hi :)")
```
main.py
```python
import hi
hi.hi()
```


--- 

create package

honor/hi.py
```python
def hello():
    print("Hi :)")
```
honor/__init__.py
```python
```

Can Be Empty But Must Be Exist

main.py
```python
from honor import hi
hi.hello()
```
or
main.py
```python
from honor.hi import hello
hello()
```


__name__ conecpt :

```python
print(__name__)
```

if we run dirctly for example python3 abbas.py the output is __main__ but if import it that is diffrent