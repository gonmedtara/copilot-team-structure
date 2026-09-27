---
description: 'Spring Boot conventions for layers, DTO boundaries, constructor injection, and tests. Use when editing Java application code.'
applyTo: '**/*.java'
---

# Spring Boot

- Keep controller, service, and repository in separate layers. Put business decisions in the service.
- Accept and return DTOs at the controller. Map between entities and DTOs in the service or a mapper.
- Keep JPA entities inside the persistence layer. A controller response must not be an entity.
- Inject dependencies through the constructor only. Leave `@Autowired` off fields.
- Write a unit test for each service behavior. Mock the repository. Do not boot the full application for a service unit test.
- Write integration tests with Testcontainers when the test needs a real database. Point those tests at the container, not at a shared environment.
