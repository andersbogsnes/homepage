---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

# Dockerfile

{{< figure src=/images/docker/dockerfile.jpeg >}}


---

## Define what the Container looks like

```Dockerfile
# The python maintainers have already defined an image
# with python installed. I want to build on top of that
FROM ubuntu

# Copy files from my local filesystem into the container
COPY my_code.py my_code.py

# Run an arbitrary command
RUN echo "This is executed when building this image"

# What command should my image run by default?
CMD ["echo", "This is executed when I run the image"]
```

---

## Build the image

<pre class="fragment">
    <code class="bash">docker build .</code>
</pre>

<pre class="fragment">
    <code class="bash">docker images</code>
</pre>


{{% /section %}}