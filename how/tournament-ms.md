---
layout: default
title: Tournament Service
---

This section explains how the **Tournament Service** was built and how it fits into the TechCup platform architecture.

---

## Tournament Service

### Dependencies

#### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM (v3.4.1). Manages dependency versions and build defaults.

#### Runtime dependencies
- **spring-boot-starter-web** – Enables REST APIs via Spring MVC with an embedded Tomcat server.
- **spring-boot-starter-data-jpa** – Maps Java entities to database tables and provides repository interfaces for CRUD operations.
- **spring-boot-starter-validation** – Enables bean validation (`@Valid`, `@NotBlank`, `@Future`, etc.) on request bodies.
- **postgresql** – JDBC driver that connects the service to its own PostgreSQL database at runtime.
- **spring-cloud-starter-openfeign** – Lets you call other microservices using a simple annotated Java interface instead of manual HTTP calls. Used to communicate with the Teams Service and the Identity Service.
- **spring-boot-starter-actuator** – Exposes `/actuator/health` and `/actuator/metrics` endpoints for monitoring.
- **mapstruct** (v1.6.3) – Generates type-safe mappers between entities and DTOs at compile time.
- **lombok** – Generates boilerplate code (getters, setters, constructors) from annotations at compile time.
- **spring-boot-devtools** – Enables automatic restarts during development.
- **springdoc-openapi-starter-webmvc-ui** (v2.8.0) – Generates and serves a Swagger UI at `/swagger-ui/index.html`.
- **jjwt-api / jjwt-impl / jjwt-jackson** (v0.12.6) – JJWT library for reading and validating JWT tokens forwarded by the gateway.

#### Test dependencies
- **spring-boot-starter-test** – Provides JUnit 5, Mockito, and AssertJ for unit and integration testing.
- **testcontainers:postgresql** (v1.20.4) – Spins up a real PostgreSQL instance during integration tests.
- **testcontainers:junit-jupiter** (v1.20.4) – JUnit 5 integration for Testcontainers lifecycle management.
- **h2** – In-memory database used as a lightweight alternative during unit tests.

#### Build plugins
- **spring-boot-maven-plugin** – Packages the service as a runnable fat JAR (`java -jar tournament-service.jar`).
- **maven-compiler-plugin** – Configured with annotation processor paths for Lombok and MapStruct to ensure correct code generation order.

#### Dependency management
- **spring-cloud-dependencies BOM** (v2024.0.0) – Ensures all Spring Cloud libraries (Feign, etc.) use compatible versions together.

---

### Diagrams & JPA

The class diagram used for the models is the following:

![Class diagram](/assets/images/tournament-ms/class_diagram.png)

The entity-relation diagram used for JPA is the following:

![Entity-relation diagram](/assets/images/tournament-ms/entity-relation-diagram.png)

The entities defined for this service are:

#### `tournaments`
Stores the general configuration and lifecycle state of each tournament.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `name` | `VARCHAR(100)` | Unique tournament name |
| `start_date` | `DATE` | Must be today or in the future |
| `end_date` | `DATE` | Must be after start date |
| `registration_close_date` | `DATE` | Must be on or before start date |
| `max_teams` | `INTEGER` | Between 2 and 32 |
| `cost` | `DECIMAL` | Non-negative |
| `status` | `ENUM` | `DRAFT · ACTIVE · IN_PROGRESS · FINISHED` |
| `regulations_url` | `VARCHAR` | Optional URL to the PDF rulebook |
| `created_by` | `UUID` | Cross-service reference to the organizer in Identity Service |
| `created_at` | `TIMESTAMP` | Set automatically on creation |
| `updated_at` | `TIMESTAMP` | Updated automatically on every change |

#### `registrations`
Tracks team registration requests and their payment verification status.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `tournament_id` | `UUID FK` | References `tournaments.id` |
| `team_id` | `BIGINT` | Cross-service reference to Teams Service |
| `captain_id` | `BIGINT` | Cross-service reference to Identity Service |
| `payment_proof_url` | `VARCHAR` | URL of the uploaded payment receipt |
| `status` | `ENUM` | `UNDER_REVIEW · APPROVED · REJECTED · CANCELLED` |
| `submitted_at` | `TIMESTAMP` | When the captain submitted the registration |
| `reviewed_at` | `TIMESTAMP` | When the organizer reviewed it |
| `reviewed_by` | `BIGINT` | Organizer's cross-service reference |
| `created_at` | `TIMESTAMP` | Set automatically on creation |
| `updated_at` | `TIMESTAMP` | Updated automatically on every change |

#### `matches`
Stores each match generated for a tournament, including scheduling and results.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `tournament_id` | `UUID FK` | References `tournaments.id` |
| `round` | `ENUM` | `GROUP · QUARTER_FINAL · SEMI_FINAL · FINAL` |
| `match_order` | `INTEGER` | Position within the round |
| `home_team_id` | `UUID` | Cross-service reference to Teams Service |
| `away_team_id` | `UUID` | Cross-service reference to Teams Service |
| `home_score` | `INTEGER` | Null until result is registered |
| `away_score` | `INTEGER` | Null until result is registered |
| `scheduled_at` | `TIMESTAMP` | Scheduled date and time |
| `status` | `ENUM` | `SCHEDULED · IN_PROGRESS · FINISHED · CANCELLED` |

#### `goals`
Records each goal scored during a match, linked to the player who scored it.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `match_id` | `UUID FK` | References `matches.id` |
| `player_id` | `UUID` | Cross-service reference to Users & Players Service |
| `team_id` | `UUID` | Cross-service reference to Teams Service |
| `minute` | `INTEGER` | Minute of the match when the goal was scored |

#### `fields`
Stores the available fields (canchas) where matches can be played.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `tournament_id` | `UUID FK` | References `tournaments.id` |
| `name` | `VARCHAR` | Field name |
| `description` | `VARCHAR` | Optional description |
| `image_url` | `VARCHAR` | URL to the field image stored in MongoDB |

#### `audit_logs`
Append-only log of all relevant actions performed on tournaments and registrations.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `entity_type` | `ENUM` | `TOURNAMENT · REGISTRATION` |
| `entity_id` | `UUID` | ID of the affected entity |
| `action` | `VARCHAR` | `CREATED · STATUS_CHANGED · UPDATED · DELETED` |
| `performed_by` | `BIGINT` | Cross-service reference to the user who performed the action |
| `previous_status` | `VARCHAR` | Status before the change (if applicable) |
| `new_status` | `VARCHAR` | Status after the change (if applicable) |
| `details` | `JSON` | Additional context about the action |
| `timestamp` | `TIMESTAMP` | Defaults to `NOW()` |

> Cross-service references (team IDs, user IDs, captain IDs) are stored as primitive types (`UUID`, `BIGINT`) since the Tournament Service does not own those entities. The actual data lives in the Teams Service, Identity Service, or Users & Players Service respectively.

---

### Endpoints

#### Tournament lifecycle

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/tournaments` | Bearer (`ORGANIZER`) | Creates a tournament in `DRAFT` state. |
| `GET` | `/api/v1/tournaments` | No | Returns all tournaments. |
| `GET` | `/api/v1/tournaments/{id}` | No | Returns a tournament by ID. |
| `PATCH` | `/api/v1/tournaments/{id}/activate` | Bearer (`ORGANIZER`) | Transitions state from `DRAFT` to `ACTIVE`. |
| `PATCH` | `/api/v1/tournaments/{id}/start` | Bearer (`ORGANIZER`) | Transitions state from `ACTIVE` to `IN_PROGRESS`. Start date must equal today. |
| `PATCH` | `/api/v1/tournaments/{id}/finish` | Bearer (`ORGANIZER`) | Transitions state from `IN_PROGRESS` to `FINISHED`. End date must be today or earlier. |
| `DELETE` | `/api/v1/tournaments/{id}` | Bearer (`ORGANIZER`) | Deletes a tournament. Only allowed in `DRAFT` state. |

#### Registrations

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/tournaments/{id}/registrations` | Bearer (`CAPTAIN`) | Registers a team in a tournament and uploads payment proof. |
| `GET` | `/api/v1/tournaments/{id}/registrations` | Bearer (`ORGANIZER`) | Lists all registrations for a tournament. |
| `GET` | `/api/v1/tournaments/{id}/registrations/{regId}` | Bearer | Returns a single registration. |
| `PATCH` | `/api/v1/tournaments/{id}/registrations/{regId}/approve` | Bearer (`ORGANIZER`) | Approves a registration. |
| `PATCH` | `/api/v1/tournaments/{id}/registrations/{regId}/reject` | Bearer (`ORGANIZER`) | Rejects a registration. |
| `PATCH` | `/api/v1/tournaments/{id}/registrations/{regId}/cancel` | Bearer (`CAPTAIN`) | Cancels a registration. Only allowed while `UNDER_REVIEW`. |

#### Statistics

| Method | Path | Auth required | Description |
|---|---|---|---|
| `GET` | `/api/tournaments/{id}/stats/top-scorers` | No | Returns the top scorers for a tournament. |
| `GET` | `/api/tournaments/{id}/stats/matches` | No | Returns the full match history for a tournament. |
| `GET` | `/api/tournaments/{id}/stats/teams/{teamId}` | No | Returns stats for a specific team in a tournament. |

---

### Inter-service communication

This service communicates directly with other microservices using **OpenFeign** — not through the API Gateway. This is intentional: inter-service calls are internal and should not pass through the public entry point.

| Called service | When | Purpose |
|---|---|---|
| **Teams Service** | On team registration | Verify the team exists before creating the registration |
| **Identity Service** | On state transitions | Validate the requesting user has the `ORGANIZER` role |

The gateway injects `X-User-Id`, `X-User-Role`, and `X-User-Email` headers into every forwarded request. The service reads `X-User-Id` to identify the organizer or captain performing each action.

---

### Tournament state machine

```
DRAFT ──► ACTIVE ──► IN_PROGRESS ──► FINISHED
  │
  └──► (deleted)
```

| Transition | Condition |
|---|---|
| `DRAFT → ACTIVE` | No additional condition required |
| `ACTIVE → IN_PROGRESS` | `startDate == today` |
| `IN_PROGRESS → FINISHED` | `endDate <= today` |
| `DRAFT → deleted` | Tournament must still be in `DRAFT` |

---

### Registration state machine

```
UNDER_REVIEW ──► APPROVED
     │
     └──► REJECTED
     │
     └──► CANCELLED  (captain only, before review)
```

---