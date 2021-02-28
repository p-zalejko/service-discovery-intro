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

* reduce effort

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

* DNS Service Discovery (DNS-SD)
* Service Location Protocol (SLP).
* Apache Zookeeper, Consul, etcd, Netflix Eureka


---

## k8s - basics

- deplpyment
- pod
- service
- ingress (exposes services!)

---

## k8s - basics


![title](assets/img/k8s.png)

---

## Service

- server-side service discovery
- load balancer
- respect readiness ~~and liveness~~ probes 
- use selectors

---

## Service - example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp   (1)
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

---

## Netflix Eureka

![title](assets/img/eureka_arch.png)

[source](https://www.codeprimers.com/client-side-service-discovery-in-spring-boot-with-netflix-eureka/)

---

## Netflix Eureka - core elements

* Service registry
* Eureka client

---

## Ribbon

![title](assets/img/ribbon.jpg)

[source](https://www.javaxp.com/2020/06/client-side-load-balancing-using-eureka.html)


---

## Spring + Netflix Eureka + Ribbon


---

## Spring + Netflix Eureka + Ribbon + k8s


---

## Lessons Learned

---