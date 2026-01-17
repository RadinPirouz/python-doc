Data Types In Python : 


int : normal numbers like (1 , -100 , 20326 ,0 , ..)
float : 
string: txts in ""
list [1,2,3,4,5] or ['abbas', 2 ,-10 , 2.65]
dictionary: {'name': abbas , 'l_name': 'gholi'}
set: like set but dont have duplications  

```python
print(type(var)) # Return Varible Type
```
strings:

strings is imutable (can,t change after set string with str[0]='i')

```python
string="this is string about abbas gholi #1"
print(string[0]) # --> Return t
print(string[1:3]) # --> Return his
print(string[-1]) # --> Return 1
# [start : end]
```

```python
string='012345678'
print(string[5]) # --> return '5'
print(string[1,5]) # --> return '1234'
print(string[1,4,2]) # --> return 13
print(string[::-1]) # --> return 876543210
```
```python
# String With Enter Every Char
"""
Hi This Is String Without Any Limits Like
enter 
" "
and any more 
"""
```


string methods:


```python
string="AbbAsGholi
print(string.upper()) # Return ABBASGHOLI
print(string.lower()) # Return abbasgholi
print(string.islower()) # Return False
print(string.index('i')) # Return 8
```

```python
string = "abbas,mmd,asghar"
list_strings = string.split(",")
print(list_strings) # out put is : ['abbas','mmd','asghar'] 
```



```python
f_name = 'abbas'
l_name = 'gholi'
age = 25
final_data = " Information: first_name: %s , last_name: %s , age : %i" % (f_name,l_name,age)
final_data = "Information: first_name: {} , last_name: {} , age : {}".format(f_name,l_name,age)
final_data = "Information: first_name: {0} , last_name: {1} , age : {2}".format(f_name,l_name,age)
final_data = "Information: first_name: {f} , last_name: {l} , age : {a}".format(f=f_name,l=l_name,a=age)
final_data = f"Information: first_name: {f_name} , last_name: {l_name} , age : {age}"
```



lists:

```python
list = [1,2,3,'pizze']
print(list[0]) # Return 1
print(list[1:3]) # Return 2,3
print(list[-1]) # Return 'pizze'
print(len(list)) # Return 4
print(list.count(3)) # return 3
list_last_value = list.pop()
print(list_last_value,list) # return 'pizze' , [1,2,3]
``` 

```python
list = [9,5,1,10]
list.sort()
print(list) # [1,5,9,10]
```

```python
list = [9,5,1,10]
list[0] = 2
print(list) # [2,5,1,10]
list.reverse()
print(list) # [10,2,5,2]
```


dictionary:


```python
price = {
    "oil": 500000,
    "egg": 350000
    "frute" : {
        "apple": 100000
        "orange": 120000
    }
}

print(price['oil'])
print(price['frute']['apple'])
print(price.keys()) # show keys
print(price.values()) # show values
print(price.items()) # show all key , values
print(price.get('egg')) # Return egg
print(price.get('water'),-1) # return -1
```


boolian:
