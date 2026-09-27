---
description: 'Generate a service and its unit test from the Spring Boot or Angular instruction file.'
name: 'generate-service'
argument-hint: 'Service name and target path'
agent: 'agent'
---

Generate a service from the request that follows this prompt. If the name, responsibility, or target path is missing, ask for it before writing files.

1. Choose the stack from the target path. `.java` is Spring Boot. Angular services are `.ts`.
2. Follow the matching instruction file. Apply those rules, and do not copy them into comments in the new files.
   - Spring Boot: [spring-boot.instructions.md](../instructions/spring-boot.instructions.md)
   - Angular: [angular.instructions.md](../instructions/angular.instructions.md)
3. Also follow [security.instructions.md](../instructions/security.instructions.md).
4. For Spring Boot, add the service, a DTO when the operation crosses HTTP, and a unit test that mocks the repository.
5. For Angular, add an injectable service. Keep local state in signals. Use RxJS only when the service owns a real async stream. Add a colocated unit test.
6. Stop once the new files satisfy those instruction files.
