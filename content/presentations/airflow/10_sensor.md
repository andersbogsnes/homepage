---
weight: 100
outputs: ["Reveal"]
---

{{% section %}}

# Sensors

---

## Wait for something to happen

A sensor is an operator that runs continously, succeeding only when a condition is met.

These can be used for any task that needs to wait on some external input.

Could be a file landing on a FTP server or some data to land in a database or just wait 10 mins before proceeding.

---

## Example - wait 10 mins

```python
from airflow import DAG
from airflow.sensors.time_delta_sensor import TimeDeltaSensor
from datetime import timedelta

dag = ...


with dag:
    t1 = ...
    wait_10_mins = TimeDeltaSensor(timedelta(min=10))
    t2 = ...
    t1 >> wait_10_mins >> t2
```

---

## Exercise

Add a HttpSensor to the previous workflow that will cause the pipeline to wait if the file is not present

---

## Solution

```python
    file_available = HttpSensor(http_conn_id="airbnb_file",
                                task_id="check-is-available",
                                endpoint="denmark/hovedstaden/copenhagen/2020-10-27/data/listings.csv.gz"
                                )
```

{{% /section %}}