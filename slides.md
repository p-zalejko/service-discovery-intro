# Service discovery 

### the good the bad and the k8s

---

## The definition


> Service discovery is how applications and (micro)services locate each other on a network

[source](https://www.getambassador.io/resources/service-discovery-microservices/)


> Service discovery is the automatic detection of devices and services offered by these devices on a computer network.

[source](https://en.wikipedia.org/wiki/Service_discovery)


---

## The problem

<img src="assets/img/service_discovery_problem.png" width="400">

[source](https://www.nginx.com/blog/service-discovery-in-a-microservices-architecture/)

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
* Service Location Protocol (SLP)
* Apache Zookeeper, Consul, etcd, Netflix Eureka etc.


---

## k8s - basics

- Deplpyment
- ReplicaSet
- Pod
- Service
- Ingress

Note: 
ReplicaSet ensures that a specified number of pod replicas are running
Deployment manages ReplicaSets, rollback to an earlier Deployment revision
Ingress exposes services

---

## k8s - basics

> etcd: Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.
 

<img src="assets/img/k8s_cluster.png" width="700">

---

## k8s - basics

![title](assets/img/k8s.png)

---

## Service

- server-side service discovery
- load balancer
- relies on readiness ~~and liveness~~ probes 
- uses selectors

Note:
k8s uses etcd

---

## Service - example

```yaml
apiVersion: v1
kind: Service                      (1)
metadata:
  name: my-service
spec:
  selector:
    app: MyApp                     (2)
  ports:
    - protocol: TCP
      port: 80                     (3)
      targetPort: 9376
```

---

## Netflix Eureka

![title](assets/img/eureka_arch.png)

[source](https://www.codeprimers.com/client-side-service-discovery-in-spring-boot-with-netflix-eureka/)

Note:
core elements: registry and client

---

## Ribbon

![title](assets/img/ribbon.jpg)

[source](https://www.javaxp.com/2020/06/client-side-load-balancing-using-eureka.html)


---

## Spring + Eureka + Ribbon

![title](assets/img/spring_eureka_ribbon.png)

[read this: Client cache refresh + LoadBalancer refresh](https://github.com/spring-cloud/spring-cloud-netflix/issues/373#issuecomment-110331739)

Note:
ribbon has a cache
eureka client has a cache
ribbon calls eureka client :-)
caches are not sychronized! refresh can take event 30s+30s = 1min!

---

## Spring + Eureka + Ribbon + k8s

![title](assets/img/spring_eureka_ribbon_k8s.png)

---

# Lessons Learned

---

## Understand technologies

* eureka (registry, client)
* ribbon
* spring (integration)
* caches
* k8s
* ...

---

## Refresh interval is crucial

* eureka server (many parameters)
* eureka client (`registryFetchIntervalSeconds`)
* ribbon (`ServerListRefreshInterval`)

Note: 
client-side load balancers need to get the  new list of services

---

## k8s readiness and liveness probes vs Eureka

* Eureka has its own solutions
* k8s solutions control Pods, impact Services and Ingresses

---

## Eureka self-preservation mode and k8s can be tricky

> The mechanism that stops evicting the instances when the heartbeats are below the expected threshold is called self-preservation. This might happen in the case of a poor network partition, where the instances are still up, but just can't be reached for a moment or in the case of an abrupt client shutdown.

[source](https://www.baeldung.com/eureka-self-preservation-renewal)

---

## k8s rolling update 

* imagine you have X services
* imagine you have Y instances of each service...
* imagine you update Z services...
* ... and you have eureka with self-preservation mode

Note:
low number of instances is a problem (early stage of the project)
x=5
y=2
z=2

---


## Graceful shutdown
 
always...

Note: 
deregister from external systems, brokers, databases etc.
create a proper dockerfile configuration
use readiness and liveness

---

## Ugly solution 
### "Delayed" shutdown

* have a custom shutdown endpoint
* check your configuration and count time
* deregister the service but do not stop it yet
* wait for refreshing list of services by all instances
* stop it now

Note:
Count time of refresh, all parts.
Ribbon, eureka client and server

---

## What if you need something more than pure SD?

* [blue-green or canary deployment](https://opensource.com/article/17/5/colorful-deployments)
* [weighted routing, OpenShift example](https://docs.openshift.com/container-platform/3.9/architecture/networking/routes.html#alternateBackends)
* [routing/LB to external services](https://medium.com/@ManagedKube/kubernetes-access-external-services-e4fd643e5097)
* [Observability: From Service Discovery to Service Mesh](https://thenewstack.io/observability-from-service-discovery-to-service-mesh/)


---

# Q&A

---
