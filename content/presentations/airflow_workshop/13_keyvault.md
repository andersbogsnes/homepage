---
weight: 130
outputs: ["Reveal"]
---

{{% section %}}

# Key Vaults

---

## Storing secrets

So far, we've been passing secrets as environment variables

This works, but there are alternatives!

---

## Passing secrets to a docker container

There are many ways of passing secrets to a docker container

- Environment variables
- Mounting files
- Secrets backend

---

## Environment variables

Environment variables are passed to a container using the `-e` flag when using `docker run`

---

Environment variables are available to any process running on a machine and are not secure, given that someone can access the machine.

<p class="fragment">Logging systems can dump the ENVs when logging - :cold_sweat:</p>

---

They are, however, great for passing configuration, and a better option than hardcoding secrets

---

## Mounting a file

We can mount a file containing our secrets into the container

`docker run -v /path/to/secret:/tmp/secret`

---

Inside the container, we can now do

```python
with open("/tmp/secret") as f:
    credentials = f.read()
```

---

Now an attacker needs access to the running container, not just the docker host

---

However, now we need a safe place to store the credentials on our machine! :worried:

---

## Secrets Backend

The best option is a secrets Backend, such as Azure Keyvault, AWS Secrets Manager or GCP Secret Manager.

---

These serve as a secure place to store credentials, where we can grant authenticated access to them.

---

Backends allow us just-in-time access to secrets, which we can then manage separately from our code

---

## Accessing Azure Keyvault

To interact with Azure Keyvaults, run `pip install azure-keyvault-secrets`.

To authenticate, we also need to `pip install azure-identity` 

---

We need the URL to the keyvault - it's in the format `https://{my_key_vault}.vault.azure.net` and would be given to you

```python
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultCredentials

client = SecretClient(vault_url="my_vault_url", credential=DefaultCredentials())
my_secret = client.get_secret("DatabasePassword")
# Use `.value` to access the actual secret value
print(mysecret.value)
```

{{% /section %}}