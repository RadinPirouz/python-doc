```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def home_dir():
    return {"message": "Home Pag"}
```

```bash
fastapi dev main.py
```
use uvicorn as defualt runner 
or
we can run with uvicorn command
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 1234
```

