## Service discovery 
### the good the bad and the k8s

---

## The definition


> Service discovery is how applications and (micro)services locate each other on a network

[source](https://www.getambassador.io/resources/service-discovery-microservices/)



> Service discovery is the automatic detection of devices and services offered by these devices on a computer network.

[source](https://en.wikipedia.org/wiki/Service_discovery)


---

    
## Why do we need service discovery? 

* simplify the configuration and reduce effort

* discover each other dynamically

* health monitoring

* load balancing


---

## Kinds of Service Discovery

* Server-side service discovery
* Client-side service discovery

---

## Server-side service discovery

> When making a request to a service, the client makes a request via a router (a.k.a load balancer) that runs at a well known location. The router queries a service registry, which might be built into the router, and forwards the request to an available service instance.

[source](https://microservices.io/patterns/server-side-discovery.html)


---

## Server-side service discovery

![title](assets/img/server-side-discovery.jpg)


---

## Client-side service discovery
> When making a request to a service, the client obtains the location of a service instance by querying a Service Registry, which knows the locations of all service instances.

[source](https://microservices.io/patterns/client-side-discovery.html)

---

## Client-side service discovery

![title](assets/img/client-side-discovery.jpg)

---

## Implementations

Service registry
Service Discovery (DNS-SD) 
Service Location Protocol (SLP).


Apache Zookeeper, Consul, or etcd, Netflix Eureka


---

## k8s

- deplpyment
- pod
- service
- ingress (exposes services!)

---

## k8s - c.d.
- pods have random IP addresses
- pods have random* hostnames, but they are not in DNS
- pods' hostnames and IPs change
- servoces have IPs that do not change*

---

## Service

- server-side service discovery
- load balancer
- respect readiness and liveness probes (mainly readiness)
- use selectors

## Service - sample

IMAGE here



## Challenges when eureka and/or ribbon used
