---
weight: 110
outputs: ["Reveal"]
---

{{% section %}}

# Type Hinting

---

## Static Typing

- Other languages have a concept of static typing
- When declaring a variable, we must declare a type as well and that type does not change

```scala
// This is Scala code
var s: String = "abc"
// This will error
s = 1
```

---

## Dynamic Typing

- Python is `dynamically typed` 
- We can reassign variables as we want

```python
a = "abc"
a = 1
```

---

## Advantages of Static typing

- Static typing makes it easy to verify the correctness of our program

```python
a = "abc"
a = 1
print(a.upper())
```

<p class="fragment">These errors will throw an exception at runtime</p>

---

{{< figure src=https://toytales.ca/wp-content/uploads/2014/12/time-bomb.jpg >}}

---

## Advantages of Dynamic typing

- Part of the simplicity of Python
- We might not know up-front what types we can work with
- "Duck typing"

---

## The best of both worlds?

- Python 3.5 introduced the `typing` module, allowing us to `annotate` our types
- Annotated types have no effect on the runtime - it helps tools be able to verify your code
- Pycharm uses typehints to warn you if your types are incompatible
- `mypy` is a CLI tool that verifies if there are any typing issues


---

## How to type

```python
a: str = "abc"
b: int = 1

def my_func(a: str, b: int) -> str:
    ...
```

---

## The typing module

```python
from typing import List, Tuple

def process_ints(list_of_ints: List[int]) -> Tuple[int, int]:
    ...
```

---

## Classes are types too

```python

class A:
    ...

class B:
    ....

def process_a_and_b(a: A, b: B) -> int
    ...

```

---

## Multiple Types

We can Union types to say "either or"

```python
from typing import Union

def handle_data(c: Union[pd.DataFrame, pd.Series]):
    ...
```

---

## Supporting Ducktyping

Ducktyping can be implemented with a `Protocol`

```python
from typing import Protocol

class GreetType(Protocol):
    def greet(self) -> None
        ... # This is a literal ellipsis -> "..."


class Greeter:
    def greet():
        print("hello")

# Accepts any type that has a `greet` method
def greet_everyone(to_greet: List[GreetType]):
    ...

```

---

There are number of built-in Protocols - see them [here](https://mypy.readthedocs.io/en/latest/protocols.html#protocol-types)

---

## Implement Types

- Typing helps you document your code
- Typing helps catch errors before they blow up
- Typing makes Pycharm smarter

{{% /section %}}