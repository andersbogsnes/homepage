---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=https://developers.redhat.com/blog/wp-content/uploads/2017/11/dockerfile.png background-size=30% >}}

---

## Anatomy of a Dockerfile

A Dockerfile has a handful of commands to know

<p class="fragment">FROM</p>
<p class="fragment">RUN</p>
<p class="fragment">COPY</p>
<p class="fragment">LABEL</p>
<p class="fragment">CMD</p>
<p class="fragment">ENTRYPOINT</p>
<p class="fragment">WORKDIR</p>


---

### FROM

What image are we basing our image off of?

Docker images are extensible, so it's simple to build off of other people's work

```docker
FROM python:3.8.3-slim
```

---

### RUN

Run a command inside the container at build time - install some dependency, setup some data etc.

```docker
RUN pip install psycopg2
```

---

### COPY

Copy a file from your machine into the container when building

- left side == your computer
- right side == docker container

```docker
COPY requirements.txt requirements.txt
```

---

### LABEL

Labels add metadata to an image that can be used by tooling or looked up with inspect

```docker
LABEL maintainer="maintainer@mycompany.org"
```

---

### CMD

The command that is executed when we do `docker run` without any arguments


```docker
CMD ["flask", "run", "-p", "8080"]
```

Notice the syntax - this makes it safer to parse

---

We can override the CMD by passing the command we want to run

```bash
$ docker run -it python:3.8-slim /bin/bash
root@2e321b156c7b:/# 
```

---

### ENTRYPOINT

Sometimes we want to run the container with arguments. 

If ENTRYPOINT is specified, any arguments passed when doing docker run, they will be passed to the ENTRYPOINT

```docker
ENTRYPOINT ["python", "-i"] # docker run arguments will be appended
```

---

### WORKDIR

Sets the working directory inside the container - the docker version of `cd`

Useful if you want everything to happen in a subfolder

```docker
WORKDIR /app
```

---

## Exercise

- Create a new folder named `db`
- Find the postgres docker image [docs](https://hub.docker.com/_/postgres)
- Write a Dockerfile in the `db` directory to start a postgres 12 database
- Initialize the database with a table matching the datafile found [here](https://www.kaggle.com/benroshan/factors-affecting-campus-placement)
- Build the image with a tag
- Run the image
- Connect to the database with Pycharm

{{% /section %}}