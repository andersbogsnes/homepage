---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

# Attributes

---

## Add attributes

We can *initialize* the class with some data

```python
class SalesPerson:
    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> james = SalesPerson(name="James", age=32)
>>> james.age
32
```

---

## Aside: The mystical `self`

- A class is a **blueprint** of something
- A blueprint can create many **instances** with it's own unique data

---

- When we need to refer to an **instance** of a blueprint `self` is the placeholder we use
- Python passes it as the first argument to any **method** defined on the class

---

Now we can make as many salespeople as we want

```python
>>> mike = SalesPerson(name="Mike", age=40)
>>> mike.age
40
```

---

### We can change attributes

> We're all consenting adults
>
> -- <cite>Guido van rossum</cite>

```python
>>> james.name = "Tony"
>>> james.name
"Tony"
```

---

## Two types of attributes

- Classes have **instance** attributes, which we define in the `__init__`
- Classes also have **class** attributes, which are shared between all instances

---

## An example of class attributes

We have been using **instance** attributes - let's try a **class** attribute

---

Let's set Alm Brand be the default company to work for

```python
class SalesPerson:
    company = "Alm Brand"

    def __init__(self, name)
        self.name = name

>>> james = SalesPerson("James")
>>> mike = SalesPerson("Mike")
>>> james.company
"Alm Brand"
>>> mike.company
"Alm Brand"
>>> james.company = "Tryg"
>>> mike.company
"Tryg"
```

{{% /section %}}
