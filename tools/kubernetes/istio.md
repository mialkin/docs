# Istio

[↑ Istio](https://istio.io) is a [service mesh](#service-mesh) which is logically split into a *data plane* and a *control plane*:

- The **data plane** is composed of a set of intelligent [↑ Envoy](https://www.envoyproxy.io) proxies deployed as sidecars. These proxies mediate and control all network communication between microservices. They also collect and report telemetry on all mesh traffic.
- The **control plane** manages and configures the proxies to route traffic.

Working with both Kubernetes and traditional workloads, Istio brings standard, universal traffic management, telemetry, and security to complex deployments.

## Table of contents

- [Istio](#istio)
  - [Table of contents](#table-of-contents)
  - [Envoy](#envoy)
  - [Istiod](#istiod)
  - [Gateway](#gateway)
  - [Pod](#pod)
  - [Service](#service)
  - [Service endpoint](#service-endpoint)
  - [Service mesh](#service-mesh)
  - [Service registry](#service-registry)
  - [Virtual service](#virtual-service)
  - [Workload](#workload)
  - [Workload instance](#workload-instance)
  - [Kubernetes Ingress vs Istio Gateway](#kubernetes-ingress-vs-istio-gateway)

## Envoy

[↑ Envoy](https://www.envoyproxy.io) is a high-performance proxy developed in C++ to mediate all inbound and outbound traffic for all services in the service mesh.

Envoy proxies are deployed as sidecars to services, logically augmenting the services with Envoy's many built-in features, for example:

- Dynamic service discovery
- Load balancing
- TLS termination
- HTTP/2 and gRPC proxies
- Circuit breakers, retries, failovers, and fault injection.
- Health checks
- Staged rollouts with %-based traffic split
- Fault injection
- Rich metrics

## Istiod

Istiod provides service discovery, configuration and certificate management.

Istiod converts high level routing rules that control traffic behavior into Envoy-specific configurations, and propagates them to the sidecars at runtime.

Istiod acts as a certificate authority and generates certificates to allow secure mTLS communication in the data plane.

## Gateway

A **gateway** is a standalone Istio proxy deployed at the edge of the [mesh](#service-mesh). Gateways are used to route traffic into or out of the mesh.

## Pod

A **pod** is a group of one or more containers, with shared storage and network, and a specification for how to run the containers. Pods are the [workload instances](#workload-instance) in a Kubernetes deployment of Istio.

## Service

A **service** is a delineated group of related behaviors within a [service mesh](#service-mesh).

A service is typically materialized by one or more [service endpoints](#service-endpoint), and may consist of multiple service versions.

## Service endpoint

A **service endpoint** is a network-reachable manifestation of a [service](#service).

## Service mesh

A **service mesh** or simply **mesh** is an infrastructure layer that enables managed, observable and secure communication between workload instances.

## Service registry

Istio maintains an internal **service registry** containing the set of [services](#service), and their corresponding [service endpoints](#service-endpoint), running in a [service mesh](#service-mesh). Istio uses the service registry to generate Envoy configuration.

## Virtual service

A **virtual service** is an object that defines a set of traffic routing rules to apply when a host is addressed. Each routing rule defines matching criteria for traffic of a specific protocol. If the traffic is matched, then it is sent to a named destination service (or subset/version of it) defined in the [service registry](#service-registry).

## Workload

A **workload** is a binary deployed by operators to deliver some function of a service mesh application.

In Kubernetes, a workload typically corresponds to a Kubernetes deployment, while a [workload instance](#workload-instance) corresponds to an individual pod managed by the deployment.

## Workload instance

A **workload instance** is a single instantiation of a workload's binary. A workload instance can expose zero or more service endpoints, and can consume zero or more services.

Workload instances have a number of properties:

Name and namespace
Unique ID
IP Address
Labels
Principal

## Kubernetes Ingress vs Istio Gateway

Any application running at production scale should have an *ingress* to expose itself to the outside world. While Kubernetes provides the `Ingress` resource for this purpose, its feature set is limited depending on the kind of ingress controller, usually Nginx, being used. Alternatively, you can leverage Istio and take advantage of its more feature-rich `Gateway` resource, even if your application pods themselves are not running purely Kubernetes.
