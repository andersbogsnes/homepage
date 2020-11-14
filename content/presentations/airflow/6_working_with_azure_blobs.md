---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=/images/airflow/azure_blob_storage.png background-size=60% >}}

---

# Working with Azure Blob Storage

---

Azure Blob Storage is basically the same as a network drive that is attached to the internet instead of being mounted to your
computer

---

This is important for two main reasons

- Ease of access - can access from anywhere with internet
- Credentials are handled on-demand

---

## Setup

Setup a new folder and new venv

Start by installing the azure python SDK

```bash
pip install azure-storage-blob
```

---

## Important Concepts

- The Account
- The Container
- The Blob

{{< figure src=/images/airflow/blob_storage_model.png class=fragment >}}

---

### The Account

The account name is given when we create the Blob Storage and doesn't change, similar to a youtube account or dropbox account

---

### The Container

In the account, we can create containers - akin to directories to hold various files.

---

### The Blob

Contains the actual data - is a file of any shape or size

---

## Credentials

To be allowed access to the data, we need credentials.

There are a few different ways of being given access to data.

- Access keys
- SAS token
- User credentials

---

### Access keys

Access keys are a fixed password that gives full access to the storage account - basically admin access

Can be regenerated but can't set expiration date

---

### SAS token

A Shared Access Signature token - generate a time-limited, scope-limited token to grant access.

Generated using an access key so if the access key is rotated, all SAS tokens generated that way will also expire

---

### User Credentials

A user can also be given access to a storage account through the Active Directory - this could be a Service Principal, your personal login or a ManagedIdentity

These can be managed with the `azure-identity` python package

---

## Uploading data

Assuming we have a storage account named "myteststorage" with a container named "raw".

```bash
myteststorage/
|-- raw/
```

Let's upload a file named `test.csv` to the container

---

```python
from azure.blob.storage import BlobClient

# Credentials is whatever credentials of the above you are using
>>> client = BlobClient("https://myteststorage.blob.core.windows.net", 
                        container_name="raw",
                        blob_name="test.csv",
                        credential="mytoken")
>>> client.exists()
False

>>> with open("test.csv", mode="rb") as f:
...    client.upload_blob(f)

>>> client.exists()
True
```

---

## Downloading Data

```python
>>> client.exist()
True

>>> with open("local_file.csv", mode="wb") as f:
...     stream = client.download_blob()
...     f.write(stream.readall())

```

---

## Exercise

In your assigned storage accounts, upload and download a test file

- In your local folder, create a new file with some text
- Using the Azure SDK client library, create a new container named "{your_name}_demo"
- Upload the text file to the container
- Download the file into a test2.txt file locally
- Delete your container

---

{{% /section %}}