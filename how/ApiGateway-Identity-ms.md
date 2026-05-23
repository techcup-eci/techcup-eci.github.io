---
layout: default
title: Identity Service & API Gateway
---

This section explains how the **Identity Service** and the **API Gateway (Orchestrator)** were built and how they work together to secure the TechCup platform.

---

## Container diagram

The following diagram shows the containers that make up both services and their relationships with the rest of the microservices and the frontend.

![Container diagram](contIdeApi/techcup_container_diagram.svg)

---

## Identity Service

### Dependencies

#### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM (v3.4.5). Manages dependency versions and build defaults.

#### Runtime dependencies
- **spring-boot-starter-web** – Enables REST APIs via Spring MVC with an embedded Tomcat server.
- **spring-boot-starter-data-jpa** – Maps Java entities to database tables and provides repository interfaces for CRUD operations.
- **spring-boot-starter-security** – Integrates Spring Security for authentication and authorization.
- **spring-boot-starter-validation** – Enables bean validation (`@Valid`, `@NotBlank`, etc.) on request bodies.
- **spring-boot-starter-webflux** – Provides the reactive `WebClient` used to call the Users & Players microservice.
- **postgresql** – JDBC driver that connects the service to its own PostgreSQL database at runtime.
- **jjwt-api / jjwt-impl / jjwt-jackson** (v0.12.6) – JJWT library for generating and validating JWT access tokens and hashing refresh tokens with SHA-256.
- **springdoc-openapi-starter-webmvc-ui** (v2.8.6) – Generates and serves a Swagger UI at `/api/identity/swagger-ui.html`.

#### Test dependencies
- **spring-boot-starter-test** – Provides JUnit 5, Mockito, and AssertJ.
- **spring-security-test** – Helpers for testing secured endpoints.
- **h2** – In-memory database used only during tests.

#### Build plugins
- **spring-boot-maven-plugin** – Packages the service as a runnable fat JAR (`java -jar techcup-identity-service.jar`).
- **jacoco-maven-plugin** (v0.8.11) – Generates code-coverage reports in XML format for SonarQube integration.

---

### Diagrams & JPA

The entities managed by this service are:

#### `users`
Stores authentication credentials and the system role. The `users_ms_user_id` column links each record to its extended profile in the Users & Players microservice.

| Column | Type | Notes |
|---|---|---|
| `id` | `BIGINT PK` | Auto-generated |
| `email` | `VARCHAR UNIQUE` | Used as login identifier |
| `password` | `VARCHAR` | BCrypt-hashed |
| `role` | `ENUM` | `INVITED · PLAYER · CAPTAIN · ORGANIZER · REFEREE · ADMIN` |
| `status` | `ENUM` | `ACTIVE · INACTIVE` |
| `users_ms_user_id` | `BIGINT` | Cross-service link to users-ms |

#### `refresh_tokens`
Stores token rotation records. The raw UUID token is **never** persisted — only its SHA-256 hash.

| Column | Type | Notes |
|---|---|---|
| `id` | `BIGINT PK` | Auto-generated |
| `token_hash` | `VARCHAR(64)` | SHA-256 of the raw UUID sent to the client |
| `user_id` | `BIGINT FK` | References `users.id` |
| `users_ms_user_id` | `BIGINT` | Cross-service link |
| `created_at` | `TIMESTAMP` | Creation time |
| `expires_at` | `TIMESTAMP` | `created_at + 7 days` |
| `revoked` | `BOOLEAN` | Set to `true` on logout or rotation |

#### `audit_logs`
Append-only log of security-relevant actions.

| Column | Type | Notes |
|---|---|---|
| `id` | `BIGINT PK` | Auto-generated |
| `action` | `VARCHAR` | `LOGIN · LOGOUT · REGISTER · STATUS_CHANGE` |
| `user_email` | `VARCHAR` | Email of the actor |
| `details` | `VARCHAR` | Human-readable description |
| `ip_address` | `VARCHAR` | Remote IP of the request |
| `timestamp` | `TIMESTAMP` | Defaults to `NOW()` |

---

### Endpoints

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/identity/register` | No | Creates credentials in identity-ms and full profile in users-ms. Returns access token + sets refresh cookie. |
| `POST` | `/api/identity/login` | No | Validates credentials. Returns access token + sets `refresh_token` httpOnly cookie. |
| `POST` | `/api/identity/refresh` | Cookie | Rotates the refresh token and issues a new access token. |
| `POST` | `/api/identity/logout` | Bearer | Revokes all refresh tokens for the user and clears the cookie. |
| `GET` | `/api/identity/validate` | Bearer | Validates the access token and returns user info. |
| `GET` | `/api/identity/me` | Bearer | Alias of `/validate`. Returns the current user's profile. |
| `GET` | `/api/identity/users` | Bearer (ADMIN) | Lists all users in the system. |
| `PATCH` | `/api/identity/users/{id}/role` | Bearer | Updates a user's role. Self-promotion: `INVITED→PLAYER` or `PLAYER→CAPTAIN`. ADMIN can set any role. |
| `PATCH` | `/api/identity/users/{id}/status` | Bearer (ADMIN) | Activates or inactivates a user. Blocked if the user is enrolled in an active tournament. |

---

### Security design

- **Access token** – JWT signed with HMAC-SHA256. Claims: `sub` = userId, `email`, `role`, `name`. Expiration configurable via `jwt.expiration` (default 15 min).
- **Refresh token** – Random UUID sent to the client as an `httpOnly` cookie. Only the SHA-256 hash is stored in the database. Each use triggers token rotation (old token revoked, new one issued).
- **Password storage** – BCrypt via `PasswordEncoder`.
- **Internal requests** – The gateway adds `X-Internal-Secret` to every proxied request. The service can verify the header to reject direct external calls.

---

## API Gateway

### Dependencies

#### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM (v3.4.5).

#### Runtime dependencies
- **spring-cloud-starter-gateway** – Core Spring Cloud Gateway library. Provides reactive routing, predicate matching, and the filter chain used to proxy requests to each microservice.
- **spring-boot-starter-security** – Adds the reactive security context required by the gateway filter chain.
- **jjwt-api / jjwt-impl / jjwt-jackson** (v0.12.3) – Used by `AuthFilter` and `JwtUtil` to validate the JWT signature and expiration before forwarding requests.
- **lombok** – Generates boilerplate at compile time (excluded from the final JAR).
- **spring-boot-starter-actuator** – Exposes `/actuator/health` and `/actuator/info` for monitoring.
- **springdoc-openapi-starter-webflux-ui** (v2.8.4) – Aggregates Swagger docs from all microservices into a single UI at `/swagger-ui.html`.

#### Test dependencies
- **spring-boot-starter-test** – Provides JUnit 5 and Mockito for unit testing filters.

#### Dependency management
- **spring-cloud-dependencies BOM** (v2024.0.1) – Ensures all Spring Cloud libraries use compatible versions.

#### Build plugin
- **spring-boot-maven-plugin** – Packages the gateway as a runnable fat JAR. Lombok is excluded from the final artifact.

---

### Routing table

| Route id | Path pattern | Auth filter | Upstream service |
|---|---|---|---|
| `identity-public` | `POST /api/identity/login`, `/register`, `/refresh` | No | Identity Service (`:8081`) |
| `identity-protected` | `/api/identity/**` | Yes | Identity Service (`:8081`) |
| `users-register` | `POST /api/users/register` | No | Users & Players MS (`:8082`) |
| `users-service` | `/api/users/**` | Yes | Users & Players MS (`:8082`) |
| `teams-service` | `/api/teams/**` | Yes | Teams MS (`:8083`) |
| `tournaments-service` | `/api/tournaments/**` | Yes | Tournaments MS (`:8084`) |
| `competition-service` | `/api/competition/**` | Yes | Competition MS (`:8085`) |

Every proxied request receives the `X-Internal-Secret` header so downstream services can reject calls that bypass the gateway.

---

### Security design

The gateway is the **single entry point** for all external traffic. Its security responsibility is different from the identity service's:

| Concern | Identity Service | API Gateway |
|---|---|---|
| Login / token generation | ✅ | ❌ |
| JWT generation | ✅ | ❌ |
| JWT full validation | ✅ | ✅ basic (signature + expiration) |
| Roles & permissions logic | ✅ | ❌ |
| Block requests without token | ❌ | ✅ |
| CORS | ❌ | ✅ single source of truth |
| Propagate user context | ❌ | ✅ via `X-User-Id`, `X-User-Role`, `X-User-Email` |

The `AuthFilter` validates the Bearer token on every protected route, extracts `userId`, `role`, and `email` from the JWT claims, and injects them as request headers before forwarding. Microservices trust these headers instead of re-validating the token themselves.