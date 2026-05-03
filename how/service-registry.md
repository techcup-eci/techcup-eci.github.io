---
layout: default
title: Service registry
---

> Why use Netflix Eureka in Microservices

## Problem

In a microservices architecture, services need to communicate with each
other.

A naive approach is to hardcode URLs:

`http://localhost:8081\`

`http://192.168.1.10:8082`

This quickly breaks when: - Services scale (multiple instances) - IPs or
ports change - Services restart or move - Different environments are
used (dev, test, prod)


## What Eureka Solves

Netflix Eureka is a service registry that allows services to dynamically
discover each other.

Instead of using fixed addresses, services use logical names.


## How It Works

1.  Each service registers itself in Eureka
2.  Eureka keeps a list of available instances
3.  Other services query Eureka to find where to send requests

Example:

USER-SERVICE → \[`10.0.0.1:8080`, `10.0.0.2:8080`\]\
ORDER-SERVICE → \[`10.0.0.3:9090`\]


## Benefits

-   Dynamic service discovery
-   Loose coupling
-   Load balancing support
-   Fault tolerance
-   Easier scaling


## Summary

Eureka removes the need for hardcoded service locations and replaces
them with dynamic discovery using service names.

---
---

<img width="800" height="300" alt="Context-diagram" src="/assets/images/service-reg/ejemplo-1.png"/>

This is an example on how eureka is recognizing `tournament-service` micro service at port `8080`. Here
we can see what services are active and running and how many instances of them there are.

Hopefully, when we get the Azure thing working, all microservices will be connected here so we can track
them :)
