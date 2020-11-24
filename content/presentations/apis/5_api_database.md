---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Connecting FastAPI and the Database

---

Now we have the basic CRUD setup, we need to hook up our handler functions to use the database

---

## Dependency Injection

Remember, we want our connections to be open for the duration of the request and then close it.

FastAPI lets us `Depend` on a function to have run when we execute, so FastAPI can keep track of what has run

---

First we define a `get_session` function

```python
def get_session():
    with Session(engine) as session:
        yield session
```

---

Change the endpoint

```python


{{% /section %}}