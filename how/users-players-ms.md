---
layout: default
title: Users & Players Service
---

This section explains how the **Users & Players Service** was built and how it fits into the TechCup platform architecture.

---

## Users & Players Service

### Dependencies

#### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM (v3.4.1). Manages dependency versions and build defaults.

#### Runtime dependencies
- **spring-boot-starter-web** – Enables REST APIs using Spring MVC with an embedded Tomcat server.
- **spring-boot-starter-data-jpa** – Provides ORM mapping and CRUD repositories for PostgreSQL entities.
- **spring-boot-starter-validation** – Enables DTO validation using annotations such as `@Valid`, `@NotBlank`, and `@Size`.
- **spring-boot-starter-data-mongodb** – Allows storing and retrieving player profile photos in MongoDB.
- **postgresql** – JDBC driver used for structured relational data persistence.
- **spring-cloud-starter-openfeign** – Used for communication with external microservices such as Teams Service.
- **spring-boot-starter-actuator** – Exposes monitoring endpoints such as `/actuator/health`.
- **mapstruct** – Generates type-safe DTO/entity mappers at compile time.
- **lombok** – Reduces boilerplate code using annotations.
- **springdoc-openapi-starter-webmvc-ui** – Generates Swagger/OpenAPI documentation.
- **jjwt-api / jjwt-impl / jjwt-jackson** – Used to validate JWT tokens forwarded by the gateway.

#### Test dependencies
- **spring-boot-starter-test** – Includes JUnit 5, Mockito, and AssertJ for testing.
- **testcontainers:postgresql** – Runs PostgreSQL containers during integration tests.
- **testcontainers:junit-jupiter** – Integrates Testcontainers with JUnit 5 lifecycle.
- **h2** – In-memory database for lightweight tests.

#### Build plugins
- **spring-boot-maven-plugin** – Packages the service as an executable JAR.
- **maven-compiler-plugin** – Configured for Lombok and MapStruct annotation processing.

#### Dependency management
- **spring-cloud-dependencies BOM** – Ensures compatibility between Spring Cloud libraries.

---

### Diagrams & Persistence

The class diagram used for the models is the following:

![Class diagram](../images/Diagrama%20de%20contexto%20Users%20and%20Players.png)

The container diagram used for the architecture is the following:

![Container diagram](../images/Diagrama%20contenedores%20Usuers%20and%20Players.png)

The entity-relation diagram used for JPA is the following:

![Entity-relation diagram](../images/Diagrama%20ER%20users%20and%20players.png)

---

### Entities

#### `users`
Stores the personal and academic information of platform users.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `full_name` | `VARCHAR(120)` | User full name |
| `email` | `VARCHAR(120)` | Unique institutional email |
| `school_relation` | `ENUM` | `STUDENT · PROFESSOR · GRADUATE · STAFF · FAMILY_MEMBER · ADMIN` |
| `academic_program` | `VARCHAR(120)` | Optional depending on relation |
| `semester` | `INTEGER` | Only valid for students |
| `status` | `ENUM` | `ACTIVE · INACTIVE` |
| `birth_date` | `DATE` | User birth date |
| `identification` | `VARCHAR(30)` | Government identification |
| `created_at` | `TIMESTAMP` | Automatically generated |
| `updated_at` | `TIMESTAMP` | Automatically updated |

#### `player_profiles`
Stores sports-related information for users with Player role.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `user_id` | `UUID FK` | References `users.id` |
| `position` | `ENUM` | `GOALKEEPER · DEFENDER · MIDFIELDER · FORWARD` |
| `jersey_number` | `INTEGER` | Player jersey number |
| `photo_id` | `VARCHAR` | MongoDB document reference |
| `availability_status` | `ENUM` | `AVAILABLE · LINKED` |
| `created_at` | `TIMESTAMP` | Automatically generated |
| `updated_at` | `TIMESTAMP` | Automatically updated |

#### `team_requests`
Stores player requests to join teams.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `player_id` | `UUID FK` | References `player_profiles.id` |
| `team_id` | `UUID` | Cross-service reference to Teams Service |
| `status` | `ENUM` | `PENDING · ACCEPTED · REJECTED · CANCELED` |
| `created_at` | `TIMESTAMP` | Automatically generated |
| `updated_at` | `TIMESTAMP` | Automatically updated |

#### `team_invitations`
Stores invitations sent from teams to players.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `player_id` | `UUID FK` | References `player_profiles.id` |
| `team_id` | `UUID` | Cross-service reference to Teams Service |
| `status` | `ENUM` | `PENDING · ACCEPTED · REJECTED` |
| `created_at` | `TIMESTAMP` | Automatically generated |
| `updated_at` | `TIMESTAMP` | Automatically updated |

#### `audit_logs`
Stores traceability information for relevant actions.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `entity_type` | `ENUM` | `USER · PLAYER_PROFILE · REQUEST · INVITATION` |
| `entity_id` | `UUID` | ID of affected entity |
| `action` | `VARCHAR` | `CREATED · UPDATED · STATUS_CHANGED` |
| `performed_by` | `UUID` | User performing the action |
| `details` | `JSON` | Additional context |
| `timestamp` | `TIMESTAMP` | Defaults to `NOW()` |

> Cross-service references to teams are stored as primitive UUIDs because the Users & Players Service does not own team entities.

---

### Endpoints

#### User management

| Method | Path | Auth required | Description |
|---|---|---|---|
| `GET` | `/api/v1/users/{id}` | Bearer | Returns a user profile. |
| `PUT` | `/api/v1/users/{id}` | Bearer | Updates user personal information. |
| `PATCH` | `/api/v1/users/{id}/status` | Bearer (`ADMIN`) | Changes user status to ACTIVE or INACTIVE. |
| `GET` | `/api/v1/users` | Bearer (`ADMIN`) | Returns paginated and filtered users. |

#### Sports profiles

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/players/profile` | Bearer (`PLAYER`) | Creates a sports profile. |
| `GET` | `/api/v1/players/profile/{userId}` | Bearer | Returns a sports profile. |
| `PUT` | `/api/v1/players/profile` | Bearer (`PLAYER`) | Updates sports profile data. |
| `DELETE` | `/api/v1/players/profile/{userId}` | Bearer | Returns 405 Method Not Allowed. |

#### Team requests

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/players/requests` | Bearer (`PLAYER`) | Sends a request to join a team. |
| `GET` | `/api/v1/players/requests` | Bearer (`PLAYER`) | Returns request history. |
| `PATCH` | `/api/v1/players/requests/{id}/cancel` | Bearer (`PLAYER`) | Cancels a pending request. |

#### Team invitations

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/players/invitations` | Internal service | Registers a team invitation. |
| `GET` | `/api/v1/players/invitations` | Bearer (`PLAYER`) | Returns received invitations. |
| `PATCH` | `/api/v1/players/invitations/{id}` | Bearer (`PLAYER`) | Accepts or rejects an invitation. |

#### Audit

| Method | Path | Auth required | Description |
|---|---|---|---|
| `GET` | `/api/v1/audit` | Bearer (`ADMIN`) | Returns filtered audit logs. |

---

### Inter-service communication

This service communicates directly with external services using **OpenFeign**.

| Called service | When | Purpose |
|---|---|---|
| **Teams Service** | Requests and invitations | Validate team linkage and player availability |
| **Identity Service** | Authentication and authorization | Validate JWT roles and identities |

The gateway injects authentication headers such as `X-User-Id` and `X-User-Role` into forwarded requests.

---

### Player request state machine

```text
PENDING ──► ACCEPTED
   │
   └──► REJECTED
   │
   └──► CANCELED