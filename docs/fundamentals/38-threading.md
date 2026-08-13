# 38 – Threading

## Sequential Execution

```python
import threading
import time 

start_time = time.perf_counter()

def test(name):
    print(f"starting test {name} ")
    time.sleep(3)
    print(f" test {name} finished")

test('mmd')
test('ali')

end_time = time.perf_counter()

print(end_time-start_time)
```

## Parallel Execution with Threads

```python
import threading
import time 

start_time = time.perf_counter()

def test(name):
    print(f"starting test {name} ")
    time.sleep(3)
    print(f" test {name} finished")

therad1 = threading.Thread(target=test,args=['mmd'])
therad2 = threading.Thread(target=test,args=['ali'])

therad1.start()
therad2.start()

therad1.join()
therad2.join()

end_time = time.perf_counter()

print(end_time-start_time)
```
