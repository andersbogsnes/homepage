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

## Using Hooks with Azure Blob Storage

One of the current issue with Azure Blob Storage SDKs, is that Microsoft recently decided to implement breaking changes to the Blob Storage API.

Downstream, there is still plenty of work left to do to update libraries to work with this breaking change.

Airflow has been focused on the 2.0 release, and still does not support the new API

This will not be an issue, when we switch to Docker images, but know that this can be an issue

---

## WASB protocol

Microsoft tried to introduce the WASB protocol, which is the legacy way of interacting with Azure Blob Storage.

This is reflected in Airflow, as there is the WASB connection and the WASB hooks

Feel free to try them, but with our preferred approach, we don't need to use Airflow hooks directly

---

# Exercise

Write a DAG that downloads a month's worth of data from [Inside Airbnb](http://insideairbnb.com/get-the-data.html), unzips it and uploads it 
to your storage account.

- Find an operator that helps with the uploading of the file to WASB
- Setup a connection id in the Airflow UI
- Schedule it to run only once [hint](https://airflow.apache.org/docs/stable/dag-run.html)
- Some helpful bash commands - wget, gunzip

{{% /section %}}