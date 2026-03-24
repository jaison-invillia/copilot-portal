---
description: "Use when editing backend source code. Enforces Clean Architecture layer boundaries, dependency direction, and naming conventions."
applyTo: "backend/src/**/*.ts"
---

# Backend Architecture Guidelines

## Clean Architecture layers (dependency direction: outer → inner)

```
Main → Interfaces → Application → Domain
       Infrastructure ↗
```

### Domain (`domain/`)
- Pure business logic: entities, value objects, domain errors
- ZERO framework or library imports
- No imports from application, interfaces, or infrastructure

### Application (`application/`)
- Use cases (commands/queries) and port interfaces
- Depends only on Domain
- Never imports from interfaces or infrastructure
- External access only through ports (abstractions)

### Interfaces (`interfaces/`)
- HTTP controllers, route DTOs, routes, middlewares
- Only HTTP concerns: parsing, validation, response formatting
- Never contains business logic
- Calls use cases from application layer only

### Infrastructure (`infrastructure/`)
- Implements ports defined in application
- Repository implementations, external service adapters, logging, DB connections
- May use frameworks, libraries, and DB drivers

### Main (`main/`)
- Composition root: dependency injection wiring only
- No business logic, no HTTP handling

## Naming conventions

- **Files/folders**: `kebab-case` (e.g., `create-order.ts`, `order-items.ts`)
- **Classes**: `PascalCase` (e.g., `CreateOrderUseCase`, `OrderRepository`)
- **Functions/variables**: `camelCase`
- **Constants**: `UPPER_SNAKE_CASE`
- **Ports**: prefix with interface purpose (e.g., `OrderRepository`, `TokenProvider`)

## DTOs vs Domain entities

- DTOs live in `interfaces/http/dto/` or `application/dto/`
- Domain entities live in `domain/entities/`
- Never pass domain entities directly to/from HTTP layer
- Map between DTOs and entities at the boundary
