---
weight: 30
outputs: ["Reveal"]
---

{{% section %}}

## Setting up Airflow

---

### Installation

In a virtualenv

```bash
pip install apache-airflow[crypto,azure,docker,postgres]
```

---

### Start Airflow

Airflow puts its files into `$AIRFLOW_HOME`, so set that or use the default, `~/airflow`.

```bash
export AIRFLOW_HOME=~/where/i/want

airflow initdb
```

---

## Start two new terminals

### run the webserver

```bash
export AIRFLOW_HOME=~/where/i/want
airflow webserver
```

### run the scheduler

```bash
export AIRFLOW_HOME=~/where/i/want
airflow scheduler
```

---

Navigate to `localhost:8080` and check the UI

---

### Change the config

Start by getting rid of the examples and default connections

```ini
# airflow.cfg
[core]
load_examples = False
load_default_connections = False
```

---

Reset the database to clear the defaults

```bash
airflow resetdb
```

---

Next, we switch to the new UI

```ini
# airflow.cfg
[webserver]
rbac = True
```

---

Add an admin user to the database

```bash
airflow create_user --role Admin --username admin --firstname Anders --lastname Bogsnes --email andersbogsnes@gmail.com
```

---

Restart the webserver and have a look at new, shiny UI! `localhost:8080`

{{% /section %}}