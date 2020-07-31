---
weight: 90
outputs: ["Reveal"]
---

{{% section %}}

# Composition

---

- An object can contain other objects
- We can *compose* components to add behaviour

---

## Redefine Organization

We can say that an Organization is *composed* of salespeople

```python
class Organization:
    def __init__(self, name, sales_people):
        self.name = name
        self.sales_people = sale_people

    def work():
        print(f"{len(self.sales_people)} sales people "
               "did some work")

>>> james = SalesPerson("James")
>>> mike = SalesPerson("Mike")
>>> org = Organization("TiedAgent", sales_people=[james, mike])
>>> org.work()
"2 sales people did some work"
```

---

### We can do *delegation*

- The organization doesn't need to know anything about the SalesPerson class, only the *interface*.

---

We can add a `sales_budget` to each `SalesPerson`

```python
class SalesPerson:
    def __init__(self, name, sales_budget):
        self.name = name
        self.sales_budget = sales_budget

    def greet(self):
        print("Hello, my name is {self.name}")
```

---

Now our organization can easily calculate its salesbudget dynamically

```python
class Organization:
    ...
    def total_budget():
        return sum(sales_person.sales_budget
                   for sales_person
                   in self.sales_people)
```

---

### We can rely on implementation

- The `SalesPerson` class knows how to make an appropriate greeting for that instance
- The `Organization` class doesn't need to know how that is implemented

```python
class Organization:
    ...
    def greet_all():
        for sales_person in sales_people:
            sales_person.greet()
```

---

## Exercise - 5 mins

Create an `Organization` class *composed* of some number of `SalesPerson`

### Criteria

- `SalesPerson` should have a method `work` which prints the person's name and how they're doing work
- The Organization should have a method `work` which *delegates* to `SalesPerson`'s `work` method

---

## Solution

```python
class SalesPerson:
    def __init__(self, name, contact_method)
        self.name = name
        self.contact_method

    def work(self):
        print(f"{self.name} spent the day "
              f"{self.contact_method} customers")

>>> james = SalesPerson("James", "meeting")
>>> jane = SalesPerson("Jane", "calling")
```

---

```python
class Organization:
    def __init__(self, sales_people):
        self.sales_people = sales_people

    def work(self):
        for sales_person in sales_people:
            sales_person.work()
>>> org = Organization([james, jane])
>>> org.work()
"James spent the day meeting customers"
"Jane spent the day calling customers"

```

---

## Recap Composition

- We can *compose* objects together to change how our program works
- We can *delegate* to other objects so our class doesn't need to know
- Leads to less coupling - We can pass any object that has a `work` method to `Organization`

{{% /section %}}
