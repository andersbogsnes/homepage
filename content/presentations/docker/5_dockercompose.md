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
- ports
- volumes
- image
- environment

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
postgres:
    build: 
        context: ./build


{{% /section %}}