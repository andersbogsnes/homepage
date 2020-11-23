---
weight: 30
outputs: ["Reveal"]
---

{{% section %}}

# FastAPI

---

## Building an API

Let's write some code - install fastapi + dependencies in a new virtualenv

```bash
pip install fastapi uvicorn
```

---

## Fastapi Hello World

```python
from fastapi import FastAPI

api = FastAPI()


@api.get("/")
def hello_world():
    return "Hello world"
```

---

## Start the API

```bash
>>> uvicorn --reload hello_world:api
INFO:     Started server process [18657]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

---

Note how the API is defined -> method + route is mapped to a function that gets executed when the API receives a request for that combination

---

Navigate to http://127.0.0.1:8000

<p class="fragment">Rememember, your browser will create a GET HTTP message for <code>/</code></p>
<p class="fragment">The request triggers the mapped function to generate a reponse</p>

---

Note that FastAPI handles calling the function and converting the return value to something the browser can understand

---

## Dynamic endpoints

We can create a dynamic endpoint by creating a route placeholder

```python
@api.get("/hello/{name}")
def hello_world_name(name: str):
    return f"Hello world {name}!"
```

---

Try navigating to http://127.0.0.1:8000/hello/foo

---

The mapped function now takes a named argument, which FastAPI will give us as a variable and we can use it in our function.

Note that it is matched by name, so the placeholder name and the function name must be the same

---

```python
@api.get("/hello/{name}/{greeting}")
def hello_world_name_and_greeting(name: str, greeting: str):
    return f"{greeting} {name}"
```

---


{{% /section %}}