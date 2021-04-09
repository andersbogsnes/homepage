---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# Connections

---

Notice that we needed access to some sensitive information in order to make the connection to the Storage Blob. 

This is generally the case when working with non-public data or databases

---

## Abstract it away

Airflow provides an abstraction, by storing that sensitive connection info encrypted in the metadata database and referring to it in code via the id

---

## Adding a connection

In the UI, the admin can add a Connection to any backend - Airflow supports a number out of the box. For each operator we want to use, we generally need to add a connection.

{{< figure src=/images/airflow/connection_view.png >}}

{{% /section %}}