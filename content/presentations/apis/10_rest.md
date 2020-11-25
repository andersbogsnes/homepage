---
weight: 100
outputs: ["Reveal"]
---
{{% section %}}

## REST Api

In the context of "Talking to Webservers", there are many methods to do that.

The most common among modern APIs is REST, **RE**presentational **S**tate **T**ransfer

---

## Defining REST

The term REST was first defined in 2000 to describe a way of modelling data using HTTP

The original tenets of REST were:

- Client–server architecture
- Statelessness
- Cacheability
- Layered system
- Code on demand (optional)
- Uniform interface

---

### Client-server architecture

Allowing the client - the consumer of data - to be separate from the server - the producer of data.
Also known as frontend + backend

---

### Statelessness

One of the most important principles - there is no state stored. Any API call is completely self-sufficient and does not rely on anything else

---

### Cacheability

Responses from the API should be able to be cached

---

### Layered System

The API should be able to handle any number of intermediaries between itself and the client. E.g. load balancers, proxies.

---

### Code on demand (optional)

Probably the least common, but the API should be able to extend server behaviour, by accepting executable code to run server-side

---

### Uniform interface

The REST spec identifies a Uniform Interface to be as follows:

#### Resource identification in requests

The request identifies the resource the client needs, but the representation of that resource is not constrained by the server's internal representation.
For example, the server could send data from its database as HTML, XML or as JSON—none of which are the server's internal representation.

---

#### Resource manipulation through representations

A response for a resource contains all the information needed to manipulate that resource

#### Self-descriptive messages

A response should include information about how to process the response

#### Hypermedia as the engine of application state (HATEOAS)

Just like navigating a webpage, a response should contain data about how to proceed with manipulating the data

---

It is rare to find an API that lives up to REST completely, so *RESTful* is often used to describe APIs that generally stick to REST principles

---

Most modern APIs use HTTP methods, pass data back and forth via JSON (this is not a REST requirement!) and is generally stateless

{{% /section %}}
