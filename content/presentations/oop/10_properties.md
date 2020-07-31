---
weight: 100
outputs: ["Reveal"]
---



{{% section %}}

# Properties

---

Access calculations as if it was an attribute

---

Let's define a `Person` class with a first name and last name

```python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name
```

---

We can now add a **property** to our person to get their `full_name`

```python
class Person:
    ...
    @property
    def full_name(self):
        return f"{self.first_name} {self.last_name}"
```

---

We now have access to an attribute which does a calculation on the fly

```python
>>> anders = Person(first_name="Anders", last_name="Bogsnes")
>>> print(anders.full_name) # No parentheses
"Anders Bogsnes"
```

---

## Exercise - 2 mins

Change the implementation of Organization to use a property for `num_employees`

```python
class Organization:
    def __init__(self, sales_people):
        self.sales_people = sales_people

```

---

## Solution

```python
class Organization:
    def __init__(self, sales_people):
        self.sales_people = sales_people

    @property
    def num_employees(self):
        return len(self.sales_people)
```

---

## Advanced Bonus

Can also use a `setter` if we need to modify or validate data when setting an attribute.

```python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @property
    def full_name(self):
        return f"{self.first_name} {self.last_name}"

    @full_name.setter
    def full_name(self, value):
        self.first_name, self.last_name = value.split(" ")
```

---

We can now change `full_name` and update `first_name` and `last_name` are updated automatically

```python
>>> anders = Person("Anders", "Bogsnes")
>>> anders.full_name
"Anders Bogsnes"
>>> anders.full_name = "Thomas Bogsnes"
>>> anders.first_name
"Thomas"
```

{{% /section %}}
