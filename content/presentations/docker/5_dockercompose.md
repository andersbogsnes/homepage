---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Docker Compose

---

## What is it?

- Docker compose coordinates multiple docker containers

--- 

## In docker

Imagine we have a python API that needs a Redis cache and a Postgres database. With only docker we would do

<ul>
<li class="fragment">Docker build webapp</li>
<li class="fragment">Docker run webapp</li>
<li class="fragment">Docker run Redis</li>
<li class="fragment">Docker run postgres</li>
<li class="fragment">Add them all to the same docker network, so they can talk to each other</li>
<li class="fragment">Make sure each one has the correct environment variables to connect to each other</li>

</ul>

---

## In docker-compose

<pre class="fragment"><code data-trim class="bash">
>>> docker-compose up
</code></pre>

<p class="fragment">Seems a bit shorter?</p>

---

## The yaml file

Docker-compose specifies all the configuration necessary in the docker-compose.yaml file

{{% /section %}}