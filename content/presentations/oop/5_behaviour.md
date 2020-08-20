---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Behaviour

---

- Storing data is nice, and a common usecase
- But we usually want to *do* something with data

---

## Add behaviour

We can add **behaviour** to our class - these usually act on **attributes**

```python
class SalesPerson:
    company = "Alm Brand"

    def __init__(self, name):
        self.name = name
        self.age = age

    def greet(self):
        print(f"Hi, I'm {self.name} from {self.company}")

>>> james = SalesPerson("James")
>>> james.greet()
"Hi, I'm James from Alm Brand"
```

---

We have added the `greet` *behaviour* to our class - Our `SalesPerson` now knows how to greet someone

---

## Exercise - 2 mins

Create an Organization class.

---

### It should

- have a `num_employees` attribute showing how many employees there are
- have a method `work` which prints out that `num_employees` did some work

---

## Solution

```python
class Organization:
    def __init__(self, num_employees):
        self.num_employees = num_employees

    def work(self):
        print(f"{self.num_employees} did some work")
```



{{% /section %}}
