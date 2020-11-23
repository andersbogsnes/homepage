---
weight: 20
outputs: ["Reveal"]
---


{{% section %}}

# Important Concepts

---

## HTTP

HTTP (Hypertext Transfer Protocol) is the backbone of the internet. Every time you visit a webpage, you use HTTP

---

## Resources

In HTTP lingo, a URL (Uniform Resource Locator) points to a given resource.

For example: `www.github.com/andersbogsnes` has two parts

<p class="fragment">The host or name of the server <code>www.github.com</code></p>
<p class="fragment"> and the resource <code>/andersbogsnes</code></p>

---

## Accessing resources

A resource has a representation on the server - it could be a file, such as an HTML page, or it could be information stored in a database.

The requested resource is sent back to the client in a representation - usually some form of text.

---

### The Request - Response cycle

---

HTTP is a protocol that defines how messages are passed between a client - your machine - and the webserver.

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


In the case of fetching google's homepage, we asked to `GET` some data from google.

---

there are a number of others, but we will focus on those that get used most commonly:

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

---

## DELETE

Delete a given resource from the server

---

## PATCH

Modify part of an existing resource

---

# API

Usually, when you work with HTTP, you're in the browser, asking for webpages.

---

When you navigate to google.com, your browser does a GET for the website. The response is the text of the website HTML which your browser knows how to render.

---

You might want to create a new user - the form you fill out gets POSTed to google.com, which creates a new resource - your new user account

---

An API is designed to use the same methods to manipulate data.

---

Take the Twitter API - a tweet can be thought of as a resource!

- you can GET tweet #123
- you can POST a new tweet
- you can PATCH tweet #123 to update the content
- you can DELETE tweet #123

{{% /section %}}