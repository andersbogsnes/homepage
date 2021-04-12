---
weight: 10
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=https://images05.military.com/sites/default/files/styles/full/public/media/military-fitness/2014/07/improving-your-pft-run-time-image.jpg?itok=9BfqUpOM >}}

<h1 style="color: white">Docker run</h1>

---

## Hello World

```bash
$ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

---

## Slightly more useful Hello World

```bash
# Remember the -it - it means "I want a terminal"
$ docker run -it python:3.8-slim
Python 3.8.3 (default, Jun  9 2020, 17:49:41)
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.

>>> print("hello world")
hello world
```

---

## Terminology

---

### Image

A Blueprint for a docker container

---

### Container

A running instance of an Image

---

## Setup a database

```bash
docker run -p 5432:5432 postgres
```

---

```bash
Error: Database is uninitialized and superuser password is not specified.
       You must specify POSTGRES_PASSWORD to a non-empty value for the
       superuser. For example, "-e POSTGRES_PASSWORD=password" on "docker run".

       You may also use "POSTGRES_HOST_AUTH_METHOD=trust" to allow all
       connections without a password. This is *not* recommended.

       See PostgreSQL documentation about "trust":
       https://www.postgresql.org/docs/current/auth-trust.html

```

---

## Passing environment variables

- Modern software is often configured with environment variables
- We can pass them into our docker container with the `-e`

```bash
docker run -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres
...
2020-08-12 13:27:45.275 UTC [1] LOG:  starting PostgreSQL 12.3 (Debian 12.3-1.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
2020-08-12 13:27:45.276 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2020-08-12 13:27:45.276 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2020-08-12 13:27:45.277 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2020-08-12 13:27:45.288 UTC [55] LOG:  database system was shut down at 2020-08-12 13:27:45 UTC
2020-08-12 13:27:45.292 UTC [1] LOG:  database system is ready to accept connections
```

---

## Port forwarding

- the `-p` flag indicates what **port** to expose to the your system.
- The application in the container runs on a **port** inside the docker network
- That port needs to be **exposed** to your machine so network traffic is routed inside the container
- We can map whatever port we want from your machine into the container

---

## Let's have a webserver

```bash
# Create a simple webpage
echo "<h1>Hello world</h1>" >> index.html
# Run an nginx container
docker run -d -v ${pwd}/index.html:/usr/share/nginx/html/index.html:ro -p 8080:80 nginx
```

---

## Volume mounting

We have a file on our computer - `index.html` that we want inside our container

- Volume mounting means exposing our local files/directories to the container
- The left side of the `:` is my machine
- The right side is the location inside the container


{{% /section %}}