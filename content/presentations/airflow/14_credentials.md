---
weight: 140
outputs: ["Reveal"]
---

{{% section %}}

# Credentials

---

## Authenticating to Azure

Azure needs to know what permissions you or your application has when trying to perform an action

---

In Python, these are represented as credentials in the `azure.identity` module

---

We have access to many variations

- AuthorizationCodeCredential
- AzureCliCredential
- CertificateCredential
- ClientSecretCredential
- DeviceCodeCredential
- EnvironmentCredential
- InteractiveBrowserCredential
- ManagedIdentityCredential
- UsernamePasswordCredential
- VisualStudioCodeCredential

---

### DefaultCredentials

The `identity` module has a `ChainedTokenCredential` which is a way of specifying "Try these credentials in order".

---

`DefaultAzureCredentials` is a pre-packaged one which tries

- EnvironmentCredential
- ManagedIdentityCredential
- VisualStudioCodeCredential
- AzureCliCredential

This is usually a good default

---

### EnvironmentCredential

Looks up environment variables to try to authenticate. Need to set the following:

---

#### Service principal with secret:

- *AZURE_TENANT_ID*: ID of the service principal's tenant. Also called its 'directory' ID.
- *AZURE_CLIENT_ID*: the service principal's client ID
- *AZURE_CLIENT_SECRET*: one of the service principal's client secrets

---

#### User with username and password:

- *AZURE_CLIENT_ID*: the application's client ID
- *AZURE_USERNAME*: a username (usually an email address)
- *AZURE_PASSWORD*: that user's password
- *AZURE_TENANT_ID*: (optional) ID of the service principal's tenant. Also called its 'directory' ID.

---

### ManagedIdentityCredential

In Azure, we can assign an identity to a service, such as a VM or webapp.

We can then assign permissions to that identity and allow the webapp to do things

---

Whenever possible, this is what we want - no storing of credentials! This doesn't work for local development though

---

## User Authentication

When we want a user to login, we have a few different options

---

### AzureCliCredential

Login with the user who is logged in to the `az` CLI - works for developers

---

### InteractiveBrowserCredential

Open a browser window and allow the user to login via the browser. This is what Office365 uses

---

### DeviceCodeCredential

Will give the user a code they have to navigate to `http://microsoft.com/devicelogin` and type in. 

---

### ClientSecretCredential

Login using a service principal

{{% /section %}}