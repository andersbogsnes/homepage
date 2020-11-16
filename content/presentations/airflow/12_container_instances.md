---
weight: 120
outputs: ["Reveal"]
---

{{% section %}}
# Azure Container Instances

---

In production, we don't want to use our local machine to run a container

That's Azure's job!

---

Luckily, Airflow already has built in support for running Azure Container Instances 

- the AzureContainerInstancesOperator

---

## Exercise

We need to replace our DockerOperator with the AzureContainerInstance

- Create a new connection that is allowed to run a ContainerInstance
- Replace the DockerOperator with AzureContainerInstance
- Rerun pipeline!

{{% /section %}}