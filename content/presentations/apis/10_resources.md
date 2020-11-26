---
weight: 100
outputs: ["Reveal"]
---

{{% section %}}

# API design

---

## Naming

When designing something, the first thing to think about is naming

- A resource is a noun
- Collections should be plural nouns (listing vs listings)
- Use lowercase + `-`
- Use hierarchies - `/user/{user_id}/address`
- Be consistent!

---

## Actions

Think about what actions the user can do

Write them down and make sure you have an endpoint that let's them do that

- Query parameters are great for searching collections
- Provide detailed documentation for the user in the docstrings
- Think about what method best represents the action

---

## Validation

The API is getting unsanitized input from the internet - never trust the internet!

Assume data is wrong and handle it - your server should never crash from bad data

- What if the client asks for userid `BOTUS`? What should the response be?
- What if the database doesn't have the data? What should the response be?

{{% /section %}}
