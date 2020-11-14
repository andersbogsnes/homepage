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

## Upload a file

```python
fs = fsspec.filesystem("az", account_name="mytestaccount", account_key="mytoken")

## equivalent to
# from adlfs import AzureBlobFileSystem
# fs = AzureBlobFileSystem(account_name="mytestaccount", account_key="mytoken")

fs.put("mylocalfile.csv", "/raw/myblobstoragefile.csv")
```

---

## Download a file

```python
fs.download("raw/myblobstoragefile.csv", "mylocalfile.csv")
```

---

## Read and write directly from pandas

One nice feature of fsspec is being able to treat all files the same

```python
import fsspec
import pandas as pd

with fsspec.open("az://raw/test.csv", account_name="mytestaccount", account_key="mytoken") as f:
    df = pd.read_csv(f)

with fsspec.open("az://raw/uploaded.csv", mode="w", account_name="mytestaccount", account_key="mytoken") as f:
    df.to_csv(f)
```

---

## Filesystem operations

With an fsspec-compatible Filesystem, we can do many of the things we expect from a filesystem.

### List files

```python
>>> fs.ls("/raw")
["/raw/uploaded.csv"]
```

### Delete files

```python
>>> fs.rm("/raw/uploaded.csv")
```

### Rename files

```python
>>> fs.rename("raw/uploaded.csv", "raw/myfile.csv")
```

---

## Exercise

- Upload your data from the previous exercise using fsspec instead
- Delete the data using fsspec

{{% /section %}}