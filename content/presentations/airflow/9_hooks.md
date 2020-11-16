---
weight: 90
outputs: ["Reveal"]
---

{{% section %}}

# Hooks

---

## Interfaces

Hooks serve as reusable interfaces to various services, allowing for code reuse

---

## Example - inserting into a database

If we have a postgres database, we can use the PostgresHook to simplify talking to the database.

```python
from airflow.providers.postgres.hooks.postgres import PostgresHook

# Refer to the connection in the Airflow database
hook = PostgresHook("postgres_conn_id")

hook.bulk_load("mytable", "mydata.csv")
```

---

## :warning: Using Hooks with Azure Blob Storage

One of the current issue with Azure Blob Storage SDKs, is that Microsoft recently decided to implement breaking changes to the Blob Storage API.

<p class="fragment">Downstream, there is still plenty of work left to do to update libraries to work with this breaking change.</p>

---

Airflow has been focused on the 2.0 release, and still does not support the new API

This will not be an issue, when we switch to Docker images, but know that this can be an issue

---

## WASB protocol

Microsoft tried to introduce the WASB protocol, which is the legacy way of interacting with Azure Blob Storage.

Airflow currently implements the legacy method using WASB connection and the WASB hooks

<p class="fragment">We will try them in an exercise, but in general we won't use many operators.</p>

---

# Exercise

Write a DAG that downloads a month's worth of data from [Inside Airbnb](http://insideairbnb.com/get-the-data.html), unzips it and uploads it 
to your storage account.

- Find an operator that helps with the uploading of the file to WASB
- Setup a connection id in the Airflow UI
- Schedule it to run only once [hint](https://airflow.apache.org/docs/stable/dag-run.html)
- Some helpful bash commands - wget, gunzip

---

# Solution

```python
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from airflow.contrib.operators.file_to_wasb import FileToWasbOperator
from airflow.operators.dummy_operator import DummyOperator
from airflow.operators.sensors import HttpSensor
from airflow.utils.dates import days_ago

default_args = {
    "owner": "me",
    "start_date": days_ago(2)
}

dag = DAG("upload_test_file",
          description="uploading a test file",
          default_args=default_args,
          schedule_interval="@once")

with dag:
    download_task = BashOperator(
        task_id="download_file",
        bash_command="wget "
                     "http://data.insideairbnb.com/denmark/hovedstaden/copenhagen/2020-06-26"
                     "/data/listings.csv.gz "
                     "-O /tmp/listings.csv.gz"
    )

    unzip = BashOperator(
        task_id="unzip",
        bash_command="gunzip -f /tmp/listings.csv.gz"
    )

    upload_task = FileToWasbOperator(task_id="test_upload",
                                     file_path="/tmp/listings.csv",
                                     container_name="raw",
                                     blob_name="my_test.csv")

    download_task >> unzip >> upload_task
```

{{% /section %}}