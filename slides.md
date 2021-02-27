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


Server-side service discovery

Client-side service discovery
    

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
