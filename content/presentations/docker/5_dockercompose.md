---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=http://openwhisk.apache.org/images/deployments/logo-docker-compose-text.svg background-size=60% >}}

---

## What is it?

- Docker compose coordinates multiple docker containers

---

## In Docker

Imagine we have a python API that needs a Redis cache and a Postgres database. With only docker we would do

<ul>
<li class="fragment"><pre><code class="bash">docker build webapp</code></pre></li>
<li class="fragment"><pre><code class="bash">docker run webapp</code></pre></li>
<li class="fragment"><pre><code class="bash">docker run redis</code></pre></li>
<li class="fragment"><pre><code class="bash">docker run postgres</code></pre></li>
<li class="fragment">Add them all to the same docker network, so they can talk to each other</li>
<li class="fragment">Make sure each one has the correct environment variables to connect to each other</li>

</ul>

---

## In docker-compose

<pre class="fragment"><code data-trim class="bash">
docker-compose up
</code></pre>

<p class="fragment">Seems a bit shorter?</p>

---

## The yaml file

Docker-compose specifies all the configuration necessary in the docker-compose.yaml file

---

### The most important sections

- build
- environment
- ports
- volumes
- image

---

## Building a docker-compose file

We always need to specify a `version` number to ensure that docker-compose knows if it is compatible

The version numbers can be found [here](https://docs.docker.com/compose/compose-file/) and is at `3.8`
at the time of writing

```yaml
version: "3.8"
```
---

## Define services

- `docker` deals with one service
- `docker-compose` deals with multiple, so we need to define each service in its own block

```yaml
version: "3.8"
services:
    service1:
        ...

    service2:
    ...
```

---

## build

Let's add our postgres service from before

```yaml
version: "3.8"
services:
  db:
    build:
      context: ./db
      dockerfile: Dockerfile
```

And run it

```bash
docker-compose up
```

---

## environment

We want to add the `POSTGRES_PASSWORD` environment variable to the build - else it complains

```yaml
version: "3.8"
services:
  db:
    build:
      context: ./db
      dockerfile: Dockerfile
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
```

---

## Exercise - 5 mins

- Create a new folder nginx
- Create an index.html file in that folder
- Add a `<h1>Hello world</h1>` to the index.html
- Write a Dockerfile to run nginx and show that file
- Add that service to the yaml file

---

## Solution

```yaml
version: "3.8"
services:
    db:
        build:
            context: ./db
            dockerfile: Dockerfile
        environment: 
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=postgres
    nginx:
        build:
            context: ./nginx
            dockerfile: Dockerfile
```

---

## ports

In the exercise, we added the application - but we can't actually access our website

We need a port mapping

```yaml
...
    nginx:
        build:
            context: ./nginx
            dockerfile: Dockerfile
        ports:
            - "8080:80"
```

Now we can go to `localhost:8080` and see our website

---

## volumes

We can also mount files in docker-compose - we can add a "hot reload" to our website.

```yaml
...
    nginx:
        build:
            context: ./nginx
            dockerfile: Dockerfile
        ports:
            - "8080:80"
        volumes:
            - "./index.html:/usr/share/nginx/html/index.html"
```

---

We can now add a `index.html` file to the same directory as the docker-compose file and look at the website

<p class="fragment">Try changing the contents of index.html</p>

---

## image

We can also specify an image to load

Let's add a `redis` instance

```yaml
...
    redis:
        image: redis
```

`Image` replaces build - everything else is the same

---

## networking

Docker-compose automatically puts all the services in the same network

When you are inside a container, all you have to do is refer to the service name instead of the ip

---

## Connecting to postgres

- User `postgres`
- Password `postgres`

```python
import sqlalchemy as sa

engine = sa.create_engine("postgresql://postgres:postgres@postgres:5432")
```

---

## depends_on

When we are creating multiple services, we can set one to depend on another

Our python app is dependant on the DB being up, so we can add `depends_on`

```yaml
...
  app:
    depends_on:
      - db
```

This will wait until the container has started - **not until the application inside the container is ready**

---

## Exercise - 10 min

Write a python application to insert the CSV you downloaded previously into the database

- Make a new folder `app`
- Take a connection string from an environment variable
- Read the csv
- Insert the data into the database (hint: use psycopg2)
- You might need to install dependencies, you can always use `apt-get install`

---

## Exercise - 10 mins

- Write a Dockerfile for the application
- Add it to the docker-compose

---

## Solution - Python app

```python
import os
import psycopg2
import time
import logging

logging.basicConfig(level=logging.INFO, format="%(asctime)s - %(message)s")

logger = logging.getLogger("data")

DB_URL = os.environ["DB_URL"]

time.sleep(2)

logger.info("Opening connection to database")
conn = psycopg2.connect(DB_URL)

with open("placement_data_full_class.csv") as f:
    logger.info("Opening data file")
    next(f)

    with conn.cursor() as curs:
        logger.info("Copying data")
        curs.copy_from(f, table="placement_data", sep=",", null="")
    conn.commit()

logger.info("Done copying data")
```

---

## Solution - docker-compose

```yaml
    app:
        build:
            context: ./app
            dockerfile: Dockerfile
        environment: 
            - DB_URL=postgresql://postgres:postgres@db:5432
        depends_on:
            - db
```

{{% /section %}}