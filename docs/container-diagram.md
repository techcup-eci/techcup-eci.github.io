---
layout: default
title: Container diagram 
---

<img width="800" height="800" alt="Context-diagram" src="/assets/images/context/container_diagram.png"/>

## Container diagram (C4) description

The container diagram presents the TECHCUP Football Platform as a web-based system composed of multiple interconnected containers that support tournament management.

Users interact with the system through a frontend web application developed in React and TypeScript, which communicates with a backend API built using Spring Boot. The backend exposes REST endpoints and handles the core business logic of the platform.

Authentication and authorization are managed through a dedicated identity component using JWT and Spring Security, ensuring secure access control for all users.

The system relies on a PostgreSQL database for structured data such as users, teams, tournaments, and matches, while MongoDB is used for storing images such as team logos and payment proofs.

An external payment coordinator is integrated into the system to validate payments performed outside the platform. Additionally, a CI/CD pipeline automates the build and deployment process for both frontend and backend components.

---