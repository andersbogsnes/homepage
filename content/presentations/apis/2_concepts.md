---
weight: 20
outputs: ["Reveal"]
---


{{% section %}}

# Important Concepts

---

## HTTP

HTTP (Hypertext Transfer Protocol) is the backbone of the internet. Every time you visit a webpage, you use http

---


### Request - Response cycle

HTTP defines how messages are passed between a client - your machine - and the webserver.

For example, the message for requesting google.com from your browser looks like this:

```
GET / HTTP/1.1
Host: www.google.com
```

---

### Response

Google's webserver will respond to our Request with it's own message

```
HTTP/1.1 200 OK

(google homepage html)
```

---

### Verbs

If we look at our request, the first part is called the HTTP verb - what action do we want to perform

In the case of fetching google's homepage, we asked to `GET` some data from google - there are a number of others, but we will focus on those that get used most commonly:

- GET
- HEAD
- POST
- PUT
- DELETE
- PATCH

---

## GET

The GET request simply requests a resource. Should only ever retrieve data

---

## HEAD

A GET that only returns the header and not the body. Commonly used for health checks

---

## POST

Submit a resource to the server. Usually used to change state on the server, e.g create a new resource

---

## PUT

Submit a resoruce intended to replace an existing resource. 
{{% /section %}}