---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# Query params

---

The second most common way of passing parameters is via `query params`

You might have noticed this in various queries you've made on the internet.

Let's look at [DAWA's Autocomplete API](https://dawa.aws.dk/dok/api/autocomplete#autocomplete) (Danmarks Adresser Web API)

---

To autocomplete an address in zipcode 2720 starting with `Bog`
To query a given address, we have to use the following URL
https://dawa.aws.dk/autocomplete?q=Bog&postnr=2720

---

- `?` marks the beginning of the query parameters
- `q=Bog` is now a query parameter - the text we are searching for accordin to the docs
- `&` marks a new query parameter
- `postnr=2720` passes 2720 to the query parameter `postnr`

The documentation notes several other query parameters we could have passed

---

In FastAPI, it's very simple to get query params.

If we wanted to recreate the DAWA API

```python
@api.get("/autocomplete")
def find_suggestions(q: str, postnr: Optional[int] = None):
    ...
```

---

FastAPI will automatically parse the query parameters for you. It can also figure out what parts are path parameters and which ones are query parameters
 
{{% /section %}}