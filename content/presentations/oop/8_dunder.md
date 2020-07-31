---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# `__dunder__` methods

---

- Short for *double underscore* - also called *magic methods*
- We **override** these methods to change our class behaviour
- Examples include:  
  - `__init__`
  - `__repr__`
  - `__add__`

---

## `__repr__`

What happens if we want to print our `SalesPerson`?

```python
class SalesPerson:
    def __init__(self, name):
        self.name = name
>>> james = SalesPerson("James")
>>> print(james)
<__main__.SalesPerson object at 0x7f479d47db50>
```

---

To make our *SalesPerson* presentable, we need to tell Python what to do when **representing** a Sales person

```python
class SalesPerson:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hi, I'm {self.name}")

    def __repr__(self):
        return f"<SalesPerson {self.name}>"

>>> james = SalesPerson("James")
>>> print(james)
"<SalesPerson James>"
```

--- 

## Exercise - 2 mins

Add a `name` attribute to the `Organization` class we made previously
Add a `__repr__` to the `Organization` class

### It should

- display the name of the organization and how many employees it has when printed

---

## Solution

```python
class Organization:
    def __init__(self, name, num_employees):
        self.name = name
        self.num_employees

    def work(self):
        print(f"{self.num_employees} did some work")

    def __repr__(self):
        return f"<{self.name}: {self.num_employees} employees>"

{{% /section %}}
