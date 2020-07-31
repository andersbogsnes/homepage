---
title: "OOP"
description: "An introduction to Object-Oriented programming in Python"
date: 2020-07-30T17:55:21+02:00
outputs: ["Reveal"]
---

{{< slide background-image="/images/python_code.jpg" >}}

# OOP in Python

---

OOP is a style of programming that models Objects in code

---
{{% section %}}

## What is an Object?

An object typically has *attributes* and *behaviour*

---

Attributes is something we have

- Age
- Address
- Height

---

Behaviour is something we do

- Walk
- Talk
- Run

---

## Other types of programming

- Imperative
- Functional

{{% /section %}}

---

{{% section %}}

## Representing the world in code

How do we represent the world in Python code?

---

If we want to represent a salesperson, we could represent them as a list of data

```python
james = ["James", 32, 1000]
```

---
We can extract data about our salesperson James

```python
name = james[0]
age = james[1]
sales_budget = james[3]
```

---

How does that make you feel?

{{% /section %}}

---

{{% section %}}

## Defining a class

```python
class SalesPerson:
    pass

james = SalesPerson()
```

---

### Terminology

- `james` is an **instance** of `SalesPerson`
- we have **instantiated** a new **instance** of `SalesPerson`

---

## Add attributes

We can *initialize* the class with some data

```python
class SalesPerson:
    def __init__(self, name, age, sales_budget):
        self.age = age
        self.name = name
        self.sales_budget = sales_budget

>>> james = SalesPerson(name="James", age=32, sales_budget=1000)
>>> james.age
32
```

---

Now we can make as many salespeople as we want

```python
>>> mike = SalesPerson(name="Mike", age=40, sales_budge=2000)
>>> mike.age
40
```

---

### We can change attributes

> We're all consenting adults
>
> -- <cite>Guido van rossum</cite>

```python
>>> james.name = "Jamie"
>>> james.greet()
"Hi, I'm Jamie from Alm Brand"
```

{{% /section %}}

---
{{% section %}}

## The mystical `self`

- A class is a **blueprint** of something. 
- A blueprint can create many **instances** with it's own unique data.

---

- When we need to refer to an **instance** of a blueprint `self` is the placeholder we use
- Python passes it as the first argument to any **method** defined on the class


{{% /section %}}

---

{{% section %}}

## Class attributes

- Classes have **instance** attributes, which we define in the `__init__`
- Classes also have **class** attributes, which are shared between all instances

---

### Class attribute

```python
class SalesPerson:
    company = "Alm Brand"

    def __init__(self, name, age, sales_budget)
        self.name = name
        self.age = age
        self.sales_budget = sales_budget

>>> james = SalesPerson("James", 32, 1000)
>>> mike = SalesPerson("Mike", 40, 2000)
>>> james.company
"Alm Brand"
>>> mike.company
"Alm Brand"
```

---

## Add behaviour

We can add **behaviour** to our class - these usually act on **attributes**

```python
class SalesPerson:
    company = "Alm Brand"

    def __init__(self, name, age, sales_budget):
        self.name = name
        self.age = age
        self.sales_budget

    def greet(self):
        print(f"Hi, I'm {self.name} from {self.company}")

>>> james = SalesPerson("James", 32, 1000)
>>> james.greet()
"Hi, I'm James from Alm Brand"
```


{{% /section %}}

---

{{% section %}}

### \_\_repr\_\_

If we print `james`, it's not very informative

```python
>>> print(james)
<__main__.SalesPerson object at 0x7f479d47db50>
```

---

## dunder methods

- Short for *double underscore* - also called *magic methods*
- Used to change how an object interacts with python
- Examples include:  
  - `__init__`
  - `__repr__`
  - `__add__`

---

To make our *SalesPerson* presentable, we need to tell Python what to do when **representing** a Sales person

```python
class SalesPerson:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print(f"Hi, I'm {self.name}")

    def __repr__(self):
        return f"SalesPerson {self.name}: {self.age} years old"

>>> james = SalesPerson("James", 32, 1000)
>>> print(james)
"SalesPerson James: 32 years old"
```

---

## Exercise

Create an Organization class.

### It should

- have a `num_employees` attribute showing how many employees there are
- have a `name` attribute
- have an informative `__repr__`
- have a method `work` which prints out that `num_employees` did some work

---

## Solution

```python
class Organization:
    def __init__(self, name, num_employees):
        self.name = name
        self.num_employees = num_employees

    def __repr__(self):
        return f"{self.name}: {self.num_employees} employees"

    def work(self):
        print(f"{self.num_employees} did some work")
```

{{% /section %}}

---

{{% section %}}

## Composition and Inheritance

OOP has two powerful concepts that allow objects to interact with each other:

---

## Composition

**Compose** objects together

Objects can contain other objects and call their methods and attributes

---

### Inheritance

**Inherit** from other objects 

Objects can inherit from other objects and call its parents methods and attributes

Used to **specialize** an existing class - we usually call this *subclassing*

{{% /section %}}

---

{{% section %}}

### Inheritance

These are **specialized** versions of a SalesPerson and can have specific **behaviour**

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
>>> mike = Phoner("Mike", 40, 2000)
>>> mike.greet()
"Hi, I'm Mike"
```

---

We can also overwrite methods from the parent

```python
class Phoner(SalesPerson):
    def greet(self):
        print(f"Hi, I'm {self.name} and I'm a phoner")
```

---

Or extend methods

```python

class Phoner(SalesPerson):
    def __init__(self, name, age, sales_budget, phone_number):
        super().__init__(name, age, sales_budget)
        self.phone_number = phone_number

>>> jane = Phoner(name="Jane", age=20, sales_budget=0, phone_number=35477777)
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
class CustomerServiceCenter(Organization):
    def __init__(self, num_employees, contact_method):
        super().__init__("KSC", num_employees)
        self.contact_method = contact_method

    def work(self):
        print(f"{self.num_employees} contacted customers "
              f"via {self.contact_method}")
```

---

```python
class FranchiseOffice(Organization):
    def __init__(self, num_employees, contact_method):
        super().__init__("Franchise", num_employees)
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

---
{{% section %}}

## Composition

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

>>> james = SalesPerson("James", 32, 1000)
>>> mike = SalesPerson("Mike", 40, 2000)
>>> org = Organization("TiedAgent", sales_people=[james, mike])
>>> org.work()
"2 sales people did some work"
```

---

### We can do *delegation*

- The organization doesn't need to know anything about the SalesPerson class, only the *interface*


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

---

{{% section %}}

## Properties

Access calculations as if it was an attribute

---

Define a Person class with a first name and last name

```python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @property
    def full_name(self):
        return f"{self.first_name} {self.last_name}"
```

---

We now have access to an attribute which does a calculation on the fly

```python
>>> anders = Person(first_name="Anders", last_name="Bogsnes")
>>> print(anders.full_name)
"Anders Bogsnes"
```

---

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

{{% /section %}}

---

{{% section %}}

## Classmethods

Most often used as an alternate constructor for a class. 

What if the data for our sales people was in a JSON file? 

```json
{
    "name": "John",
    "age": 40,
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
                   age=data["age"],
                   sales_budget=data["sales_budget"])
```

---

This works, but what if we need to add a new parameter, `contact_method`?

A better way is to add a `classmethod` constructor

```python
class SalesPerson:
    ...

    @classmethod
    def from_json(cls, json_file):
        data = json.loads(pathlib.Path(json_file).read_text())
        return cls(name=data["name"],
                   age=data["age"],
                   sales_budget=data["sales_budget"])
```

---

We can then use it to create a `SalesPerson` from our json file

```python
>>> john = SalesPerson.from_json("john.json")
>>> john.name
"John"
```

{{% /section %}}

---

## Staticmethods