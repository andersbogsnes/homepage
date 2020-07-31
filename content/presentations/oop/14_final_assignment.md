---
weight: 140
outputs: ["Reveal"]
---

{{% section %}}

# Final assignment

---

## We are going to simulate a month of sales using OOP

We want to simulate different strategies for allocating leads to Tied Agents or to Customer Service representatives.

---

## Output

At the end of the month, we want to get a summary for that simulation:

- How much did we sell for?
- What did it cost to sell that amount?
- How many leads were used?

---

## Parameters

We want to set some parameters for the simulation:

- How many leads are available?
- How many Tied Agents are available?
- How many Customer Service representatives are available?

---

## Implementation

- We need an `Organization` to contain the sales people
- We need a `TiedAgent` class and a `CustomerServiceRep` class, both inheriting from a `SalesPerson` class
- We need a `Lead` class

---

## SalesPerson

- Each salesperson has a hitrate assigned to them, depending on the type of salesperson
- Each salesperson has a per-sales cost associated with them, depending on the type of salesperson
- Each saleperson can consume some number of leads per day
- Each salesperson keeps track of what leads it has used and which ones it converted

---

## Leads

- A lead has a potential sales sum
- A lead can be "converted" into a sale
- A lead has a lead cost associated with it

---

## Organization

- The Organization must implement the reporting functionality
- The Organization must be able to run a day of the simulation
- The Organization must be able to be constructed from a JSON list of scenarios
