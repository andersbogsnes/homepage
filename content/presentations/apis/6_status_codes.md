---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

# Status Codes

---

In the Requests-Response cycle, all the responses currently look like this

```https
HTTP/1.1 200 OK

{some data}
```

---

By default, FastAPI handles sets these status codes automatically

- 200 OK on all our responses
- 422 Unprocessable Entity if data fails Pydantic validation
- 500 Internal Server Error if something crashes server side
- 404 Not Found if an invalid URL is found

---

We should be more granular in status codes, but we have to tell FastAPI what is appropriate in each case

[Rest API Tutorials](https://www.restapitutorial.com/httpstatuscodes.html) has an overview of suggestions

---

For convenience, FastAPI has defined status codes as constants to make the code more readable, you can pass an int directly if you prefer

```python
from fastapi import status

@api.post("/listing", 
          response_model=ListingOutSchema, 
          status_code=status.HTTP_201_CREATED)
def create_new_listing(listing: ListingInSchema, 
                       db: Session = Depends(get_db)):
    return create_new_listing(session=db,
                              **listing.dict())
```

---

Some common status codes:

- **201 Created** means the resource was successfully created
- **204 No Content** means the request was OK, but no content is returned
- **202 Accepted** is used when kicking off a background process
- **403 Forbidden** means you passed the authentication, but don't have permission to access the resource
- **401 Unauthorized** means you haven't passed authentication
- **429 Too Many Requests** is used when the API is rate-limited and you've called it too many times

{{% /section %}}