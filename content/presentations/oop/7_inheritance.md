---
weight: 70
outputs: ["Reveal"]
---

{{% section %}}

# Inheritance

---
We can **inherit** from SalesPerson and create a customized version with more specific attributes and behaviour

```python
class Phoner(SalesPerson):
    def call(self, telephone_number):
        ...

class TiedAgent(SalesPerson):
    def meet(self, address):
        ...
```

---

Inherited classes still have access to its parents methods

```python
class Phoner(SalesPerson):
    def call(self, telephone_number):
        ...

>>> mike = Phoner("Mike")
>>> mike.greet()
"Hi, I'm Mike"
```

Note we didn't define a `greet` method on our `Phoner` class

---

We can also overwrite methods from the parent

```python
class Phoner(SalesPerson):
    def greet(self):
        print(f"Hi, I'm {self.name} and I'm a phoner")

>>> mike = Phoner("Mike")
>>> mike.greet()
"Hi, I'm Mike and I'm a phoner"
```

---

Or extend methods

```python

class Phoner(SalesPerson):
    def __init__(self, name, phone_number):
        super().__init__(name)
        self.phone_number = phone_number

>>> jane = Phoner(name="Jane", phone_number=35477777)
>>> jane.phone_number
35477777
```

---

## Super == :point_up:

`super` is used when we want to call some method from the parent - usually when we *override* a method from the parent class

---

## Exercise - 5 mins

Create two new classes inheriting from Organization:

- `CustomerServiceCenter`
- `FranchiseOffice`

They should:

- accept a `contact_method` parameter in their `__init__`

- override the `work` method to specify what work they did using the `contact_method` parameter

---

## Solution

```python
class Organization:
    def __init__(self,num_employees):
        self.num_employees = num_employees

```

---

```python
class CustomerServiceCenter(Organization):
    def __init__(self, num_employees, contact_method):
        super().__init__(num_employees)
        self.contact_method = contact_method

    def work(self):
        print(f"{self.num_employees} contacted customers "
              f"via {self.contact_method}")
```

---

```python
class FranchiseOffice(Organization):
    def __init__(self, num_employees, contact_method):
        super().__init__(num_employees)
        self.contact_method = contact_method

    def work(self):
        print(f"{self.num_employees} contacted customers "
              f"via {self.contact_method}")
```

---

## Recap Inheritance

- A class can inherit code from a parent class
- Inheritance is used to *specialize* a class
- `super()` calls a method from the parent
- Think inheritance when you can describe the relationship as "is-a"

{{% /section %}}
