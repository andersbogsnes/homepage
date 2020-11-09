---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

## Defining an Airflow DAG

DAGs in Airflow are defined by python code

<p class="fragment">This gives us a lot of flexibility when defining DAGs</p>

---

## The DAG

```python
from airflow import DAG
from airflow.utils.dates import days_ago
from datetime import timedelta

default_args = {
    'owner': 'me',
    'start_date': days_ago(2),
}

dag = DAG(
    "my_dag",
    default_args=default_args,
    description="My Awesome DAG",
    schedule_interval=timedelta(days=1),
)
```

---

## Tasks

The DAG contains the tasks needed to fulfill the business logic.

Airflow includes many such tasks - they are called *Operators*

---

```python
from airflow.operators.bash_operator import BashOperator

t1 = BashOperator(
    task_id='print_hello_world',
    bash_command='echo hello_world',
    dag=dag
)
```

---

```python
from airflow.operators.python_operator import PythonOperator

def hello_world():
    print("Hello World from Python")

t2 = PythonOperator(
    task_id='python_hello_world',
    python_callable=hello_world,
    dag=dag
)

```
---

```python
from airflow.operators.docker_operator import DockerOperator

t3 = DockerOperator(
    task_id="docker_hello_world",
    image="hello-world",
    dag=dag
)
```

---

## Setting dependencies

Define dependencies between tasks with `>>` or the `.set_upstream()/.set_downstream()` method. Generally `>>` is preferred

```python

t1 >> t2 >> t3

```

---

## The DAG Bag

Airflow automatically loads all DAGs from the `dag` folder in `$AIRFLOW_HOME`. 


---

## Exercise

Make a DAG in the dag folder with the following tasks

- Write a python function which takes a text
- The function should take a keyword arg `n` which specifies how many times to repeat the text
- The function should write the input text `n` times to a local file in `/tmp`
- Create a DAG
- Create a PythonOperator to run the function
- Create a BashOperator to read the results of the file
- Add them to the DAG
- Execute them in the UI

{{% /section %}}
