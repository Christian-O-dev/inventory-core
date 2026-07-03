# ADR-0002: Use TypeScript and Docker

## Status

Accepted.

## Context

InventoryCore is planned as a scalable inventory and sales system. The project will include backend, frontend, PostgreSQL, API contracts, domain models, validation, permissions, inventory movements, purchases, sales and audit logs.

Because the project can grow in size and complexity, the development environment must reduce errors and be reproducible across different machines.

## Decision

InventoryCore will use:

- TypeScript for backend.
- TypeScript for frontend.
- Docker for local infrastructure.
- PostgreSQL running through Docker during local development, unless a specific environment requires otherwise.

## Consequences

### Positive

- Better type safety across backend and frontend.
- Easier maintenance as modules grow.
- Better support for DTOs, API contracts and form models.
- More reliable local setup with Docker.
- Easier development across home and institute machines.
- PostgreSQL environment can be recreated consistently.

### Trade-offs

- Slightly more setup at the beginning.
- TypeScript requires more discipline than plain JavaScript.
- Docker must be installed and running locally.

## Rules

1. New backend code should be written in TypeScript.
2. New frontend code should be written in TypeScript.
3. Local database should run with Docker by default.
4. Real environment files must not be committed.
5. `.env.example` files must be committed as templates.
6. Docker configuration must be simple at first and grow only when necessary.

## Related documents

- `docs/implementation/phase-0-setup.md`
- `docs/data-model/postgresql-v0.1.md`
- `docs/api/backend-modules-v0.1.md`
- `docs/frontend/frontend-modules-v0.1.md`
