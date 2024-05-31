# REST, OpenAPI, egress vs ingress

## Table of contents

- [REST, OpenAPI, egress vs ingress](#rest-openapi-egress-vs-ingress)
  - [Table of contents](#table-of-contents)
  - [REST](#rest)
  - [OpenAPI specification](#openapi-specification)
  - [Egress vs ingress](#egress-vs-ingress)

## REST

**REST** is an IPC mechanism that almost always uses HTTP.

A key concept in REST is a *resource*, which typically represents a business object such as a Customer or Product, or a collection of business objects.

REST uses the HTTP verbs for manipulating resources, which are referenced using a URL. For example, a GET request returns the representation of a resource, which might be in the form of an XML document or JSON object. A POST request creates a new resource and a PUT request updates a resource.

## OpenAPI specification

The **OpenAPI Specification**, previously known as the **Swagger Specification**, is a specification for machine-readable interface files for describing, producing, consuming, and visualizing RESTful web services.

[↑ Swagger](https://swagger.io) is an interface description language, IDL, for describing RESTful APIs expressed using JSON.

Examples of IDLs:

- Protocol Buffers
- Avro

## Egress vs ingress

An **egress** is a traffic that exits an entity or a network boundary.

Since traffic often is translated using NAT in and out of a private network like the cloud, a response back from a public endpoint to a request that was initiated inside the private network is not considered ingress. If a request is made from the private network out to a public IP, the public server/endpoint responds back to that request using a port number that was defined in the request, and firewall allows that connection since its aware of an initiated session based on that port number.

An **ingress** is a traffic that enters the boundary of a network.

Ingress more specifically refers to unsolicited traffic sent from an address in public internet to the private network — it is not a response to a request initiated by an inside system. In this case, firewalls are designed to decline this request unless there are specific policy and configuration that allows ingress connections.

[↑ The definitions of Egress and Ingress for the cloud](https://aviatrix.com/learn-center/cloud-security/egress-and-ingress/).
