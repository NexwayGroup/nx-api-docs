
# Understanding Nexway REST APIs

Nexway Monetize is a RESTFull API platform.

## What is REST

As written in the [REST API Tutorial](https://restfulapi.net/):
> REST is an acronym for REpresentational State Transfer. It is an architectural style for distributed hypermedia systems, and was first presented by Roy Fielding in 2000 in his [famous dissertation.](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

All APIs use JSON format for requests and responses.

## Supported HTTP verbs

We have two kinds of APIs.:
* Resource CRUD (**C**reate, **R**ead, **U**pdate, **D**elete; see [Using HTTP Methods for RESTful Services](http://www.restapitutorial.com/lessons/httpmethods.html) for more details)
    * **GET** - Get a resource or a collection of resources
    * **POST** - Create a new resource
    * **PATCH** - Partially update a resource
    * **DELETE** - Delete a resource

* Action
    * **POST** - Launch an action


## Security
All connections are done through HTTPS (by default TLS 1.2;).

All APIs are secured using [JWT Authentication](/nexway-monetize/reference/jwt-authentication) except for [Shopper APIs](/nexway-monetize/reference/shopper-apis).

This is a test
