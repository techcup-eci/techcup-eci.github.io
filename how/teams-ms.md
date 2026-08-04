---
layout: default
title: Teams Service
---

This section explains how the **Teams Service** was built and how it fits into the TechCup platform architecture.

---

## Teams Service

### Dependencies

#### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM (v3.4.1). Manages dependency versions and build defaults.

#### Runtime dependencies
- **spring-boot-starter-web** – Enables REST APIs via Spring MVC with an embedded Tomcat server.
- **spring-boot-starter-data-jpa** – Maps Java entities to database tables and provides repository interfaces for CRUD operations.
- **spring-boot-starter-validation** – Enables bean validation (`@Valid`, `@NotBlank`, etc.) on request bodies.
- **postgresql** – JDBC driver that connects the service to its own PostgreSQL database at runtime.
- **spring-cloud-starter-openfeign** – Allows communication with external microservices through declarative HTTP clients.
- **spring-boot-starter-actuator** – Exposes monitoring endpoints such as `/actuator/health`.
- **mapstruct** (v1.6.3) – Generates type-safe mappers between entities and DTOs.
- **lombok** – Reduces boilerplate code through annotations.
- **spring-boot-devtools** – Enables automatic restart during development.
- **springdoc-openapi-starter-webmvc-ui** (v2.8.0) – Generates Swagger UI documentation at `/swagger-ui/index.html`.
- **jjwt-api / jjwt-impl / jjwt-jackson** (v0.12.6) – JWT library used for token validation and authentication support.

#### Test dependencies
- **spring-boot-starter-test** – Includes JUnit 5, Mockito, and AssertJ.
- **testcontainers:postgresql** (v1.20.4) – Runs PostgreSQL containers during integration testing.
- **testcontainers:junit-jupiter** (v1.20.4) – JUnit integration for Testcontainers.
- **h2** – In-memory database for lightweight tests.

#### Build plugins
- **spring-boot-maven-plugin** – Packages the microservice as a runnable JAR.
- **maven-compiler-plugin** – Configured for Lombok and MapStruct annotation processing.

#### Dependency management
- **spring-cloud-dependencies BOM** (v2024.0.0) – Ensures compatibility between Spring Cloud libraries.

---

### Diagrams & JPA

The class diagram used for the models is the following:

![Class diagram](/assets/images/teams-ms/class_diagram.png)

The entity-relation diagram used for JPA is the following:

![Entity-relation diagram](/assets/images/teams-ms/entity-relation-diagram.png)

The entities defined for this service are:

#### `teams`
Stores the general information and configuration of each football team.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `name` | `VARCHAR(100)` | Unique team name |
| `logo_url` | `VARCHAR` | Optional team logo |
| `description` | `VARCHAR` | Optional team description |
| `captain_id` | `UUID` | Cross-service reference to Identity Service |
| `created_at` | `TIMESTAMP` | Automatically generated |
| `updated_at` | `TIMESTAMP` | Automatically updated |

#### `team_members`
Stores the players that belong to a team.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `team_id` | `UUID FK` | References `teams.id` |
| `player_id` | `UUID` | Cross-service reference to Users & Players Service |
| `jersey_number` | `INTEGER` | Optional jersey number |
| `position` | `ENUM` | `GOALKEEPER · DEFENDER · MIDFIELDER · FORWARD` |
| `joined_at` | `TIMESTAMP` | Date when the player joined the team |

#### `team_invitations`
Stores invitations sent to players to join teams.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `team_id` | `UUID FK` | References `teams.id` |
| `player_id` | `UUID` | Cross-service reference to Users & Players Service |
| `status` | `ENUM` | `PENDING · ACCEPTED · REJECTED` |
| `sent_at` | `TIMESTAMP` | Invitation creation timestamp |
| `responded_at` | `TIMESTAMP` | Optional response timestamp |

#### `team_requests`
Stores requests sent by players asking to join a team.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `team_id` | `UUID FK` | References `teams.id` |
| `player_id` | `UUID` | Cross-service reference to Users & Players Service |
| `status` | `ENUM` | `PENDING · APPROVED · REJECTED` |
| `requested_at` | `TIMESTAMP` | Request creation timestamp |
| `reviewed_at` | `TIMESTAMP` | Optional review timestamp |

#### `team_statistics`
Stores accumulated statistics for each team.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `team_id` | `UUID FK` | References `teams.id` |
| `matches_played` | `INTEGER` | Default `0` |
| `wins` | `INTEGER` | Default `0` |
| `draws` | `INTEGER` | Default `0` |
| `losses` | `INTEGER` | Default `0` |
| `goals_scored` | `INTEGER` | Default `0` |
| `goals_conceded` | `INTEGER` | Default `0` |

> Cross-service references (`captain_id`, `player_id`) are stored as primitive identifiers because the Teams Service does not own player or identity data. The real information lives in the Identity Service and Users & Players Service.

---

### Endpoints

#### Team management

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/teams` | Bearer (`CAPTAIN`) | Creates a new team. |
| `GET` | `/api/v1/teams` | No | Returns all teams. |
| `GET` | `/api/v1/teams/{id}` | No | Returns a team by ID. |
| `PUT` | `/api/v1/teams/{id}` | Bearer (`CAPTAIN`) | Updates team information. |
| `DELETE` | `/api/v1/teams/{id}` | Bearer (`CAPTAIN`) | Deletes a team. |

#### Team members

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/teams/{id}/members` | Bearer (`CAPTAIN`) | Adds a player to the team. |
| `GET` | `/api/v1/teams/{id}/members` | No | Returns all players in a team. |
| `DELETE` | `/api/v1/teams/{id}/members/{playerId}` | Bearer (`CAPTAIN`) | Removes a player from the team. |

#### Invitations

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/teams/{id}/invitations` | Bearer (`CAPTAIN`) | Sends an invitation to a player. |
| `PATCH` | `/api/v1/teams/invitations/{invitationId}/accept` | Bearer (`PLAYER`) | Accepts a team invitation. |
| `PATCH` | `/api/v1/teams/invitations/{invitationId}/reject` | Bearer (`PLAYER`) | Rejects a team invitation. |

#### Join requests

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/teams/{id}/requests` | Bearer (`PLAYER`) | Sends a request to join a team. |
| `PATCH` | `/api/v1/teams/requests/{requestId}/approve` | Bearer (`CAPTAIN`) | Approves a join request. |
| `PATCH` | `/api/v1/teams/requests/{requestId}/reject` | Bearer (`CAPTAIN`) | Rejects a join request. |

#### Statistics

| Method | Path | Auth required | Description |
|---|---|---|---|
| `GET` | `/api/v1/teams/{id}/stats` | No | Returns statistics for a team. |
| `GET` | `/api/v1/teams/ranking` | No | Returns the global team ranking. |

---

### Inter-service communication

This service communicates directly with other microservices using **OpenFeign** instead of routing internal calls through the API Gateway.

| Called service | When | Purpose |
|---|---|---|
| **Identity Service** | During authenticated operations | Validate user identity and roles |
| **Users & Players Service** | When managing members | Validate player existence and retrieve player information |
| **Tournament Service** | During tournament registration flows | Verify team eligibility and registration state |

The API Gateway injects user information headers (`X-User-Id`, `X-User-Role`, `X-User-Email`) into forwarded requests. The Teams Service uses these headers to identify captains and players performing actions.

---

### Team invitation state machine

```text
PENDING ──► ACCEPTED
    │
    └──► REJECTED