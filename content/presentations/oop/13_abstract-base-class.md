---
weight: 130
outputs: ["Reveal"]
---

{{% section %}}

# Abstract Base Classes (ABC)

---

- How do we define the *interface* that all our inherited classes need to have?
- How do we make sure that if someone else creates a new class, that it has all the *behaviour* we rely on?

---

We need all `SalesPeople` to have a `work` method so our `Organization` can call it

`work` is an **interface** - a contract of sorts

---

Any class that inherits from our AbstractSalesPerson will not work unless it implements a `work` method

```python

import abc

class AbstractSalesPerson(abc.ABC):
    @abc.abstractmethod
    def work()
        pass
```

---

```python
class LazySalesPerson(AbstractSalesPerson):
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hi, I'm {self.name}")

    # Oops - I don't know how to work!

>>> mike = LazySalesPerson("Mike")
"TypeError: Can't instantiate abstract class LazySalesPerson with abstract methods work"
```

---

Let's fix our LazySalesPerson

```python
class LazySalesPerson(AbstractSalesPerson):
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hi, I'm {self.name}")

    def work(self):
        print("I know how to work")

>>> mike = LazySalesPerson("Mike")
>>> mike.work()
"I know how to work"
```
