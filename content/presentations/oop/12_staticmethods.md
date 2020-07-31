---
weight: 120
outputs: ["Reveal"]
---

{{% section %}}

# Staticmethods

---

- Static methods are methods that don't require any of the class attributes or methods
- Formally, they don't require `self`
- Used to keep methods that make sense together instead of as a separate function

---

```python
class Phoner:
    ...
    @staticmethod
    def lookup_phonenumber(phone_number):
        # Lookup who the phone number belongs to
        print(f"Phone number {phone_number} belongs to John")

>>> Phoner.lookup_phonenumber(35477777)
"Phone number 35477777 belongs to John"
>>> phoner = Phoner("Joe")
>>> phoner.lookup_phonenumber(35477777)
"Phone number 35477777 belongs to John"
```

It could easily be a function, but it's *grouped* together with the Phoner, since it's often used in that context

{{% /section %}}
