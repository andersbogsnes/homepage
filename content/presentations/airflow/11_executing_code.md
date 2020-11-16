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

In general, the preferred approach is to run docker images for each task to the extent that is possible.

---

It means you don't have to worry about installing all the airflow dependencies in each worker, and lets you write each task's logic independently.

---

These images can run on a Kubernetes Cluster, or managed container runtimes like Azure Container Instances, Amazon ECS or Google Cloud Run.

---

That way our airflow instance can be a small LocalExecutor and the heavy work is done by a managed service.

---

## Exercise

Build a docker image to calculate beds per person in the Airbnb Data.
We want to calculate this by dividing the `accommodates` column by the `beds` column

- Read the data from your datalake
- Perform the calculation
- Write the result back to the datalake
- Write a Dockerfile to run this code
- Build the image and verify that it runs

---

## Running a Dockerimage DAG

The simplest way to run an image is to use the [DockerOperator](https://airflow.apache.org/docs/stable/_api/airflow/operators/docker_operator/index.html)

But how do we get access to configuration from Airflow inside the image?

<p class="fragment">Any runtime config must be passed as env to the container instance</p>

---

## Grabbing connections

To use the connections defined in the UI, we can use the BaseHook class

```python
>>> conn = BaseHook.get_connection('my_connection_id')
>>> conn.login
"abc123"
```

<p class="fragment">We can write our own Operator inheriting from DockerOperator if we want to run this inside our Operator instance</p>
<p class="fragment">:warning: Remember, don't write code that runs every time the DAG is parsed!</p>

---

## Variables

In addition to connections, we can also use the Variables backend to store configuration. This is simpler to use and can be accessed in jinja templating.

![variables](/images/airflow/add_variable.png)

---

```jinja
{{ var.variable_name }}
```

If you have a JSON stored in your variable, Airflow can automatically convert it

```jinja
{{ var.json.variable_name.json_property }}
```

---

## Using the Docker Operator

We can use Jinja to dynamically generate commands and configuration, such as the environment variables passed to our docker image.

```python
from airflow.operators import DockerOperator
with dag:
    ...
    transform_task = DockerOperator(
        task_id="transform_data",
        image="test_upload:latest",
        command="transform -d 2020-06-26",
        environment={
            "ACCOUNT_KEY": "{{ var.json.storage_account.account_key }}",
            "ACCOUNT_NAME": "{{ var.json.storage_account.account_name }}"
        })
```

---

This works, because we've built the image locally - we want to be able to access the image from anywhere

<p class="fragment">We need to push the image to a container registry</p>

---

## Pushing to a registry

Pushing to a docker registry is simple - we need to have docker login first

```bash
docker login <login server>
```

---

Now we can tag our image with the name of the registry

```bash
docker build -t <login server>/<name of repo>/<image name>:<image tag> .
```

---

We can now push the tagged image

```bash
docker push <login server>/<name of repo>/<image name>:<image tag>
```

Now that image ais available to anyone who can log in to your registry

---

## Exercise

Now we have a private registry set up, Airflow needs a connection to run our DockerOperator

- Create a connection in the Airflow connections UI
- Set the DockerOperator to use that connection id
- Delete the image from local 
    - `docker rmi <name_of_image>`
- Try to rerun the pipeline with the pushed image!



{{% /section %}}