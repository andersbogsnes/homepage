---
weight: 70
outputs: ["Reveal"]
---

{{% section %}}

# Fsspec

---

Fsspec is a project started by the Dask people to standardise working on remote filesystems - a core requirement for Dask.

Fsspec helps simplify common operations, and gives a higher-level abstraction

---

> Filesystem Spec (FSSPEC) is a project to unify various projects and classes to work with remote filesystems and file-system-like abstractions using a standard pythonic interface.

---

## Setup

In the same venv as before

```bash
pip install fsspec adlfs
```

---

## Read directly into pandas

One nice feature of fsspec is being able to treat all files the same

```python
import fsspec
import pandas as pd

with fsspec.open("az://raw/test.csv", account_name="mytestaccount", account_key="mytoken") as f:
    df = pd.read_csv(f)

```

---

## Upload a file

```python
fs = fsspec.filesystem("az", account_name="mytestaccount", account_key="mytoken")

## equivalent to
# from adlfs import AzureBlobFileSystem
# fs = AzureBlobFileSystem(account_name="mytestaccount", account_key="mytoken")

fs.put("mylocalfile.csv", "/raw/myblobstoragefile.csv")
```


{{% /section %}}