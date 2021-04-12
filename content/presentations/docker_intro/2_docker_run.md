---
weight: 20
outputs: ["Reveal"]
---

{{% section %}}

# Hello World

<pre class="fragment">
    <code class="bash">docker run hello-world</code>
</pre>    

<pre class="fragment">
    <code class="bash">
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
</code>
</pre>

---

# Args

<pre class="fragment">
    <code class="bash">docker run andersbogsnes/cowsay hello world</code>
</pre>    
<pre class="fragment"><code class="bash"> -------------
< hello world >
 -------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||</code></pre>


---

# Starting a database

<pre class="fragment">
    <code class="bash">docker run -d \
-e ACCEPT_EULA=Y \
-e MSSQL_SA_PASSWORD=Secure.Password1234 \
-p 1433:1433 \
mcr.microsoft.com/mssql/server:2019-latest</code>
</pre>


---

# Keeping tabs

<pre class="fragment"><code class="bash">docker ps</code></pre>

<pre class="fragment">
<code class="bash">CONTAINER ID   IMAGE                                        COMMAND                  CREATED          STATUS          PORTS                    NAMES
736c8af768c3   mcr.microsoft.com/mssql/server:2019-latest   "/opt/mssql/bin/perm…"   25 seconds ago   Up 24 seconds   0.0.0.0:1433->1433/tcp   interesting_kalam
</code>
</pre>

---

# Webserver

<pre class="fragment">
<code class="bash"># Create a simple webpage
echo "<h1>Hello world</h1>" >> index.html
</code>
<code class="bash fragment"># Run an nginx container
docker run -d \
-v ${PWD}/index.html:/usr/share/nginx/html/index.html:ro \
-p 8080:80 \
nginx
</code>
</pre>
{{% /section %}}