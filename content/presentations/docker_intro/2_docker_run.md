---
weight: 20
outputs: ["Reveal"]
---

{{% section %}}

# Hello World

```
docker run hello-world
```

---

# Args

```
docker run andersbogsnes/cowsay hello world
```

# Starting a database

```
docker run -d -e ACCEPT_EULA=Y -e MSSQL_SA_PASSWORD=Secure.Password1234 -p 1433:1433 mcr.microsoft.com/mssql/server:2019-latest
```

#
{{% /section %}}