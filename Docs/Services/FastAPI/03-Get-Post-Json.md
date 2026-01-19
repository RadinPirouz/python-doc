```python

from fastapi import FastAPI

app = FastAPI()

users = [
    {'name': 'abbas ' , 'age': '20'},
    {'name': 'mmd ' , 'age': '37'},
    {'name': 'asghar ' , 'age': '19'}
]

@app.get("/")
def root_dir:
    return {'message' , 'api is working'}

@app.get("/users")
def home_dir():
    return users

@app.get("/user/{name_input}")
def read_item(name_input: str):
   for item in users:
        if item["name"] == name_input:
            return {"information": item}
    return {"message" , "not worked"}
```