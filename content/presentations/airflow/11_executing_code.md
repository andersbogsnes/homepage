---
weight: 110
outputs: ["Reveal"]
---

{{% section %}}

# Executing Code

---

## Executors

Airflow is split into 3 main components - 4 if we include the metadata database.

- Scheduler
- Webserver
- Executor

---

## Scheduler

The scheduler is in charge of ensuring that tasks are given to the Executor at the correct time - it doesn't perform any "work"

---

## Webserver

The webserver is a UI on top of the Scheduler API, and also doesn't actually do any work

---

## Executor

The executor is the part that actually does the work - Airflow currently has the following executors:

- SequentialExecutor
- LocalExecutor
- DebugExecutor
- KubernetesExecutor
- DaskExecutor
- CeleryExecutor

---

## SequentialExecutor

The SequentialExecutor is the default when running Airflow - it only runs one task at a time and is the most limited executor.

Use for testing, and switch to another in prod

---

## LocalExecutor

LocalExecutor runs tasks in parallel on the machine - If you have a small airflow installation, with only one machine, this is the option to use

---

## DebugExecutor

Designed to use for debugging in your IDE, check details of how to use it [here](https://airflow.apache.org/docs/stable/executor/debug.html)

---

## KubernetesExecutor

Runs each task in a Kubernetes pod - generally the way to run Airflow in a modern setup

---

## DaskExecutor

Runs each task using dask.distributed workers. Great if you already have a dask cluster setup

---

## CeleryExecutor

The "old" main way to run Airflow workers. Celery is a python framework to distribute work to various distributed workers and has been in use for
a long time. Still works great, but requires dedicated machines.

---

## Preferred approach

In general, the preferred approach is to run docker images for each task to the extent that is possible. It means you don't have to worry about installing all the airflow dependencies in each worker, and let's you write each task's logic independently.

These images can run on a Kubernetes Cluster, or managed container runtimes like Azure Container Instances, Amazon ECS or Google Cloud Run.

That way our airflow instance can be a small LocalExecutor and the heavy work is done by a managed service.

---

## Exercise

Build a docker image to calculate beds per person in the Airbnb Data. 
We want to calculate this by dividing the `accomodates` column by the `beds` column

- Read the data from your datalake
- Perform the calculation
- Write the result back to the datalake
- Write a Dockerfile to run this code
- Build the image and verify that it runs

---

## Running a Dockerimage DAG

The simplest way to run an image is to use the [DockerOperator](https://airflow.apache.org/docs/stable/_api/airflow/operators/docker_operator/index.html)

---

## Grabbing connections

To use the connections defined in the UI, we can use the BaseHook class

```python
>>> conn = BaseHook.get_connection('my_connection_id')
>>> conn.login
"abc123"
```

<p class="fragment">We can write our own Operator inheriting from DockerOperator if we want to run this inside our Operator instance</p>

---

## Variables

In addition to connections, we can also use the Variables backend to store configuration. This is simpler to use and can be accessed in jinja templating

```jinja
{{ var.variable_name.json.json_property }}
```

---

## Jinja templated commands

We can use Jinja to dynamically generate commands and configuration, such as the environment variables passed to our docker image

{{% /section }}