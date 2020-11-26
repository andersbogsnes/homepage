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
pip install fastapi[all]
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

Note how the API is defined

A method + route is mapped to a function that gets executed when the Server receives a request for that combination

---

Navigate to http://127.0.0.1:8000

<p class="fragment">Rememember, your browser will create a GET HTTP message for <code>/</code></p>
<p class="fragment">The request triggers the mapped function to generate a reponse</p>

---

FastAPI handles calling the function and converting the return value to something the browser can understand

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

The mapped endpoint now takes a named argument, which FastAPI will give us as a variable and we can use it in our function.

---

:warning:

FastAPI matches by name, so the placeholder name and the function argument name must be the same

---

## Multiple levels

```python
@api.get("/hello/{name}/{greeting}")
def hello_world_name_and_greeting(name: str, greeting: str):
    return f"{greeting} {name}"
```

---

## Posting data

One of the features of FastAPI is using the `pydantic` library to do data validation.

---

### Pydantic

Pydantic is a FastAPI dependency, and is used to do data validation using python typehinting

Let's define a User schema

```python
from pydantic import BaseModel

class UserSchema(BaseModel):
    username: str
    email: str
```

---

We can now use that in a `post` request handler

```python
@api.post("/user")
def create_user(user: UserSchema):
    return "Thank you new user"
```

---

### Try it out

How do we make a POST request?

---

#### OpenAPI

Navigate to `127.0.0.1:8000/docs`

<p class="fragment">FastAPI automatically creates an OpenAPI spec for the API</p>

<p class="fragment">Find the endpoint and <code>Try it out</code></p>

<p class="fragment">Notice that any docstrings you include in your handler function gets included here</p>

---

#### Curl

On the commandline we can use curl

```bash
curl -X POST localhost:8000/user -d '{"username": "Anders", "email": "andersbogsnes@gmail.com"}'
```

---

#### Pycharm

Pycharm has a http request feature

Right-click -> New File -> HTTP Request.

The syntax for a POST looks like this:

```https
POST http://localhost:8000/user
Content-Type: application/json

{"username":  "anders", "email":  "andersbogsnes@gmail.com"}
```

---

Choose one and try it!

---
### JSON

Note the data payload used here is JSON format

The lingua franca of APIs these days is JSON - **J**ava**S**cript **O**bject **N**otation

---

JSON is written using two elements

- Array (equivalent to python list)
- Key: Value mapping (equivalent to python dict)

---

The example data is a key-> value mapping

```json
{"username": "Anders", "email": "andersbogsnes@gmail.com"}
```

---

It could also be an array of multiple users:

```json
[
    {"username": "Anders", "email": "andersbogsnes@gmail.com"},
    {"username": "Tom", "email": "tomhanks@gmail.com"}
]
```

---

## Response Model

We can return a dictionary and FastAPI will convert it to JSON and return it

```python
@api.post("/user")
def create_new(user: UserSchema):
    # This is a python dictionary, not JSON!
    return {
        "name": user.username,
        "email": user.email
    }
```


<p class="fragment">Since we are using pydantic, we can see what data is available on the object!</p>

---

We can also define a response model to validate the outgoing data - maybe we only want the email to be returned?

```python
class UserOutSchema(BaseModel):
    email: str

@api.post("/user", response_model=UserOutSchema)
def create_new_user(user: UserSchema):
    return {
        # What happens if the key is 'name'?
        "username": user.username,
        "email": user.email
    }
```

<p class="fragment">What is the response from the API now?</p>

---

## Validation

The typehints we've used in pydantic helps to validate the data, but `str` is generic - anything can be a string.

Pydantic comes with built-in validators we can use. Let's validate that the email is actually an email

---

Change the UserSchema to be of type EmailStr instead

```python
from pydantic import EmailStr, BaseModel

class UserSchema(BaseModel):
    username: str
    email: EmailStr
```

<p class="fragment">What happens if you try to post a non-email?</p>

---

## Exercise 1

Let's build an AirBnB-alike API.

- Write an ListingSchema with username, email, address and price
- Write a ListingOutSchema with username, address and price
- Write a POST endpoint to submit a new listing
  - POST to "/listing"
- Write a GET endpoint with an id to get a listing - hardcode a dummy listing for now
  - GET to "/listing/0"
- Test it out!

---

## Solution

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr


class ListingOutSchema(BaseModel):
    username: str
    address: str
    price: float


class ListingSchema(ListingOutSchema):
    email: EmailStr


api = FastAPI()

LISTINGS = [{
    "email": "test@test.com",
    "username": "test",
    "address": "Nørrebrogade 20",
    "price": 700
}]


@api.post("/listing", response_model=ListingOutSchema)
def create_new_listing(listing: ListingSchema):
    return listing


@api.get("/listing/{listing_id}", response_model=ListingOutSchema)
def get_listing(listing_id: int):
    return LISTINGS[listing_id]
```

{{% /section %}}
