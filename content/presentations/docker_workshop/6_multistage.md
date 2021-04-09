---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=https://ohnorocketscience.files.wordpress.com/2015/08/staging.jpg >}}

<h1 style="color: white">Multi-stage builds</h1>

---

## Multi-stage builds slim down our builds

- Docker images can get big

```bash
docker image ls | grep python
python              3.8                 79cc46abd78d        7 days ago          882MB
python              3.8.5-slim          07ea617545cd        2 weeks ago         113MB
python              3.8-slim            9d84edf35a0a        2 months ago        165MB
python              3.7.5               fbf9f709ca9f        9 months ago        917MB
```

The difference between `python:3.8` and `python:3.8-slim` is 717 mb!

---

## Size matters

Every mb of a Docker image has to be transmitted over the network - multiple times

- Slow startup
- Slow replication
- Storage costs

---

## Why is it so big?

Docker has a concept of `layers` - each statement in a Dockerfile creates a new layer on top

{{< figure src=https://static.packt-cdn.com/products/9781788992329/graphics/0ee3d4cf-2133-4143-a7c4-690274483841.png >}}

---

## Slimming down

Since the layers are read-only, we can't delete anything from a previous layer

### Some tips and tricks

- Do many operations in a single layer
- Any deletion of files must happen in the same layer as they are created
- Choose smaller base images

---

## The build context

Did you notice this line?

```bash
$ docker build -t test_nginx .
Sending build context to Docker daemon  3.072kB
```

---

Docker sends the contents of your as a zip file to the `Docker daemon` - the thing that actually does `docker`

If this gets big, it can slow down your builds and make them much bigger - everything that is sent gets included.

---

## The solution: .dockerignore

- If you have large files you don't need in your build, you can add them to `.dockerignore`, much like a `.gitignore`
- They're not quite the same, so if it's not doing what you want - look up the [docs](https://docs.docker.com/engine/reference/builder/#dockerignore-file)

---

## Multi-stage builds

Multi-stage builds means passing **artifacts** between stages in a Dockerfile

In essence, it let's us start from scratch with a new base image, with added files

---

## An example python build

```Dockerfile
FROM python:3.8 as build

# Setup a virtualenv we can install into
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Install requirements into the virtualenv
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

FROM python:3.8-slim as run
# Copy the virtualenv from the build stage
COPY --from=build /opt/venv /opt/venv

ENV PATH="/opt/venv/bin:$PATH"
COPY . .

CMD ["python", "run.py"]
```

---

## Exercise

- Check how big your docker-compose app is (`docker images`)
- Implement multi-stage in the app
- Build the new images
- Check how big your docker-compose app is now

{{% /section %}}