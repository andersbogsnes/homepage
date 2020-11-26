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

## The Request - Response cycle

---

HTTP is a protocol that defines how messages are passed between a client - your machine - and the webserver.

![Request Response](/images/apis/request_response.png)

---

For example, the message for requesting Google's homepage looks like this:

```https
GET / HTTP/1.1
Host: www.google.com
```

---

### Methods

```https
GET # Method / # Resource HTTP/1.1 # Protocol
```

- What action do we want to perform?
- We asked to `GET` the `/` resource from google.

---

We will focus on the methods that get used most commonly:

- GET
- HEAD
- POST
- PUT
- DELETE
- PATCH

---

#### GET

The GET request simply requests a resource. Should only ever retrieve data

---

#### HEAD

A GET that only returns the header and not the body. Commonly used for health checks

---

#### POST

Submit a resource to the server. Usually used to change state on the server, e.g create a new resource

---

#### PUT

Submit a resoruce intended to replace an existing resource.

---

#### DELETE

Delete a given resource from the server

---

#### PATCH

Modify part of an existing resource

---

## Response

Google's webserver will respond to our Request with a Response message

```https
HTTP/1.1 200 OK

<html>...</html>
```

---

In the Response, the header contains the *protocol* and the *status code*

```https
HTTP/1.1 # HTTP Protocol 200  OK # Status Code
```

---

### Status Codes

Status codes are the server's main method of communicating the result of the Request.

There are many status codes with lots of nuance, but a few you might know:

<ul>
    <li class="fragment">500 - Internal Server Error</li>
    <li class="fragment">404 - Not Found</li>
    <li class="fragment">200 - OK</li>
</ul>

---

# API

Usually, when you work with HTTP, you're in the browser, asking for webpages.

---

When you navigate to google.com, your browser does a GET for the website.

The Response is the text of the website HTML which your browser knows how to render.

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
