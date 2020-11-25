---
weight: 70
outputs: ["Reveal"]
---

{{% section %}}

# Headers

---

There are some other ways a client can pass information to the server and one important one is the Header.

Remember the HTTP Request Message - it actually looks something like this

```https
GET / HTTP/1.1
Host: github.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:83.0) Gecko/20100101 Firefox/83.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache
```

---

The rest of the message is passing Headers, which the server can interpret and modify it's behaviour accordingly.

Headers are simple key-value pairs passed in HTTP messages and can be found in both response and request messages

---

In FastAPI, it's simple to get header values

```python
from fastapi import Header

@api.get("/user-agent")
def get_user_agent(user_agent: str = Header(None)):
    return f"Your user agent is {user_agent}"
```

---

The most common header we have to worry about is authentication. A typical usecase is `Bearer Tokens`

The client can send its authentication as a header with the format `Authorization: Bearer mytoken`


{{% /section %}}