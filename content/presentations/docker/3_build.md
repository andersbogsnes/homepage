---
weight: 30
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=https://searchengineland.com/figz/wp-content/seloads/2017/01/bricks-construction-building-foundation-ss-1920.jpg >}}

<h1 style="color: white">Docker build</h1>

---

## Docker hub

All the images we've been using come from [Docker hub](https://hub.docker.com/) - a central, open repository for docker images

- These images have all been created by other people
- Some are official images
- You can upload your own images as well


---

## Defining an image

A docker image is defined by a `Dockerfile` - a step by step definition of how the container should work

---

A `Dockerfile` can be simple - let's remake our nginx server as a Dockerfile

```docker
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
```

---

Now we can `build` the `Dockerfile` and create an **image**

```bash
>>> docker build -t test_nginx .
Sending build context to Docker daemon  3.072kB
Step 1/2 : FROM nginx
 ---> 2622e6cca7eb
Step 2/2 : COPY index.html /usr/share/nginx/html/index.html
 ---> 01df86f8a47f
Successfully built 01df86f8a47f
Successfully tagged test_nginx:latest
```

---

## Tagging images

- Just like in git - Docker uses `SHA` to keep track of objects
- We can name our images by **tagging** them
- We can also specify versions - like in the python hello-world

---

We can add a version tag to our previous build

```bash
>>> docker tag test_nginx:latest test_nginx:0.1.0
>>> docker images
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test_nginx          0.1.0               01df86f8a47f        7 minutes ago       132MB
test_nginx          latest              01df86f8a47f        7 minutes ago       132MB
```

<p class="fragment">Notice they have the same IMAGE_ID</p>


{{% /section %}}