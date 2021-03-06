---
weight: 30
outputs: ["Reveal"]
---

{{% section %}}

# What is a DAG?

---

A DAG is a *D*irected *A*cyclic *G*raph

---

## Directed

Goes in one direction only

---

## Acyclic

No cycles, loops

---

## Graph

An order of operations. First A then B then C

---

## Why does it matter?

- DAGs are very useful for pipelines
- They let us describe a workflow programatically, that is easy for an orchestrator to execute.

<p class="fragment">No matter where we are in the graph, the orchestrator knows what are the next possible options.</p>

---

## A DAG

![dag](/images/airflow/DAG.png)

<p class="fragment">2 and 3 can run in parallel</p>
<p class="fragment">4 and 5 can't start until 2 has finished</p>

---

## The Dag View

In the Airflow UI, the Dag View looks like this:

![dagui](/images/airflow/dags_view.png)

{{% /section %}}
