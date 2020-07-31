---
weight: 110
outputs: ["Reveal"]
---

{{% section %}}

# Classmethods

---

## Classmethods construct classes

Most often used as an alternate constructor for a class.

What if the data for our sales people was in a JSON file?

```json
{
    "name": "John",
    "sales_budget": 2000
}
```

---

We could read the file first and then construct a `SalesPerson` instance

```python

import json
import pathlib

data = json.loads(pathlib.Path("john.json").read_text())

john = SalesPerson(name=data["name"],
                   sales_budget=data["sales_budget"])
```

---

This works, but what if we need to add a new parameter, `contact_method`?

We would need to go through all our code and update every location we create a SalesPerson

:cold_sweat:

---

A better way is to add a `classmethod` constructor

```python
class SalesPerson:
    ...

    @classmethod
    def from_json(cls, json_file):
        data = json.loads(pathlib.Path(json_file).read_text())
        return cls(name=data["name"],
                   age=data["age"])
```

---

We can then use it to create a `SalesPerson` from our json file

```python
>>> john = SalesPerson.from_json("john.json")
>>> john.name
"John"
```
---

Now if we want to add our `contact_method` attribute, we just need to add it to the constructor!

:smile:


---

## Exercise - 5 mins

Modify the Organization class to have an alternate constructor which accepts a JSON file containing a list of sales people

```json
[
    {"name": "John", "sales_budget": 2000},
    {"name": "Mike", "sales_budget": 1000}
]
```

---

### It should

- have a classmethod `from_json` which will construct a list of SalesPeople and create an instance of Organization with that list 

---

## Solution

```python
import json
import pathlib

class Organization:
    def __init__(self, sales_people):
        self.sales_people = sales_people

    @property
    def num_employees(self):
        return len(self.sales_people)

    @classmethod
    def from_json(cls, file_path):
        data = json.loads(pathlib.Path(file_path).read_text())
        return cls(sales_people=[
            SalesPerson(name=datum["name"],
                        sales_budget=["sales_budget"])
            for datum in data])
```

{{% /section %}}
