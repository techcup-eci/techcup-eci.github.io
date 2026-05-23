---
layout: default
title: Competition Service
---

This section explains how the **Competition Service** was built and how it fits into the TechCup platform architecture.

---

## Competition Service

### Dependencies

#### Parent
- **spring-boot-starter-parent** – Base Spring Boot POM used to manage dependency versions and build configuration.

#### Runtime dependencies
- **spring-boot-starter-web** – Enables REST API development with Spring MVC.
- **spring-boot-starter-data-jpa** – Provides ORM support and repository abstraction with JPA/Hibernate.
- **spring-boot-starter-validation** – Enables request validation using annotations such as `@Valid` and `@NotNull`.
- **postgresql** – PostgreSQL JDBC driver used for persistence.
- **spring-cloud-starter-openfeign** – Allows communication with Tournament Service and Teams Service through Feign Clients.
- **spring-boot-starter-actuator** – Exposes health and monitoring endpoints.
- **lombok** – Reduces boilerplate code through annotations.
- **mapstruct** – Generates DTO/entity mappers automatically.
- **springdoc-openapi-starter-webmvc-ui** – Generates Swagger/OpenAPI documentation.
- **jjwt-api / jjwt-impl / jjwt-jackson** – Used to validate JWT tokens forwarded by the API Gateway.

#### Test dependencies
- **spring-boot-starter-test** – Includes JUnit, Mockito and AssertJ.
- **h2** – In-memory database for testing.
- **testcontainers-postgresql** – PostgreSQL integration testing support.

#### Build plugins
- **spring-boot-maven-plugin** – Packages the service as an executable JAR.
- **maven-compiler-plugin** – Configured for Lombok and MapStruct annotation processing.

---

### Diagrams & JPA

The class diagram used for the models is the following:

![Class diagram](/assets/images/competition-ms/class-diagram-competition.png)

The entity-relation diagram used for JPA is the following:

![Entity-relation diagram](/assets/images/competition-ms/entity-relation-competition.png)

---

### Entities

#### `matches`
Stores all scheduled matches for tournaments.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `tournament_id` | `UUID FK` | Tournament reference |
| `home_team_id` | `UUID` | Team reference |
| `away_team_id` | `UUID` | Team reference |
| `scheduled_at` | `TIMESTAMP` | Match date and time |
| `status` | `ENUM` | `SCHEDULED · IN_PROGRESS · FINISHED · CANCELLED` |
| `home_score` | `INTEGER` | Local team score |
| `away_score` | `INTEGER` | Away team score |

#### `standings`
Stores tournament standings and statistics for each team.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `tournament_id` | `UUID FK` | Tournament reference |
| `team_id` | `UUID` | Team reference |
| `points` | `INTEGER` | Total points |
| `wins` | `INTEGER` | Matches won |
| `draws` | `INTEGER` | Matches drawn |
| `losses` | `INTEGER` | Matches lost |
| `goals_for` | `INTEGER` | Goals scored |
| `goals_against` | `INTEGER` | Goals conceded |

#### `goals`
Stores goals scored during matches.

| Column | Type | Notes |
|---|---|---|
| `id` | `UUID PK` | Auto-generated |
| `match_id` | `UUID FK` | Match reference |
| `player_id` | `UUID` | Player reference |
| `team_id` | `UUID` | Team reference |
| `minute` | `INTEGER` | Goal minute |

---

### Endpoints

#### Matches

| Method | Path | Auth required | Description |
|---|---|---|---|
| `POST` | `/api/v1/matches` | Bearer (`ORGANIZER`) | Creates a match schedule. |
| `GET` | `/api/v1/matches/{id}` | No | Returns match details. |
| `GET` | `/api/v1/matches/tournament/{id}` | No | Lists tournament matches. |
| `PATCH` | `/api/v1/matches/{id}/start` | Bearer (`ORGANIZER`) | Starts a match. |
| `PATCH` | `/api/v1/matches/{id}/finish` | Bearer (`ORGANIZER`) | Finishes a match and registers result. |

#### Standings

| Method | Path | Auth required | Description |
|---|---|---|---|
| `GET` | `/api/v1/standings/{tournamentId}` | No | Returns tournament standings. |
| `GET` | `/api/v1/standings/{tournamentId}/teams/{teamId}` | No | Returns team statistics. |

#### Statistics

| Method | Path | Auth required | Description |
|---|---|---|---|
| `GET` | `/api/v1/stats/top-scorers/{tournamentId}` | No | Returns top scorers. |
| `GET` | `/api/v1/stats/matches/{tournamentId}` | No | Returns match history. |

---

### Inter-service communication

The Competition Service communicates directly with other services using **OpenFeign**.

| Called service | Purpose |
|---|---|
| **Tournament Service** | Validate tournament status and dates |
| **Teams Service** | Validate participating teams |
| **Users & Players Service** | Retrieve player information for statistics |

The API Gateway injects authentication headers (`X-User-Id`, `X-User-Role`) into forwarded requests.

---

### Match state machine

```text
SCHEDULED ──► IN_PROGRESS ──► FINISHED
      │
      └──► CANCELLED
```