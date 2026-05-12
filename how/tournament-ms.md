---
layout: default
title: Tournament micro service
---

This section explains how the tournament micro service has been done along the way

## Dependencies

### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM. Manages dependency versions and build defaults so you don't have to.

- **spring-boot-starter-web** – Enables REST APIs via Spring MVC with an embedded Tomcat server.
- **spring-boot-starter-data-jpa** – Maps Java entities to database tables and provides repository interfaces for CRUD operations.
- **postgresql** – JDBC driver that connects the app to a PostgreSQL database at runtime.
- **spring-cloud-starter-netflix-eureka-client** – Registers this service in the Eureka registry so other services can discover it by name.
- **spring-cloud-starter-openfeign** – Lets you call other microservices using a simple annotated Java interface instead of manual HTTP calls.
- **spring-boot-starter-actuator** – Exposes health check and metrics endpoints (`/actuator/health`, `/actuator/metrics`) for monitoring.
- **spring-boot-micrometer-tracing-brave** – Adds distributed tracing so you can follow a request across multiple services (Zipkin-compatible).
- **lombok** – Generates boilerplate code (getters, setters, constructors) from annotations at compile time.
- **spring-boot-starter-test** – Provides JUnit 5, Mockito, and AssertJ for unit and integration testing.

### Dependency Management
- **spring-cloud-dependencies (BOM)** – Ensures all Spring Cloud libraries (Eureka, Feign, etc.) use compatible versions together.

### Build Plugin
- **spring-boot-maven-plugin** – Packages the app as a runnable fat JAR containing all dependencies (`java -jar tournament-service.jar`).

## Diagram & JPA

The diagram we use for the `JPA` is the following
<img alt="Context-diagram" src="/assets/images/tournament-ms/schema.png"/>

The entities we defined were:
- audit logs
- goals
- matches
- fields
- registrations
- tournaments

Those are the entities for our micro service, other entities are saved as a `UUID` since we have no
reference to them
