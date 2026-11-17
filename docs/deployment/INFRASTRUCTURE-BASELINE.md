# AIGIS Infrastructure Baseline

**Document:** Infrastructure Baseline v1.0
**Milestone:** M9 - Infrastructure Foundation
**Issue:** M9-001 - Define Docker Compose Infrastructure Baseline
**Status:** Approved for implementation
**Architecture Decision:** ADR-004

## 1. Purpose

This document defines the initial infrastructure requirements for AIGIS v1.

The objective is to establish a consistent, secure, maintainable, and reproducible infrastructure foundation using Docker and Docker Compose.

This document complements ADR-004. ADR-004 explains the architectural decision; this document defines the engineering requirements.

## 2. Initial Architecture

The initial Docker Compose environment will contain two services:

- PostgreSQL: primary relational database.
- Redis: caching and background task/queue infrastructure.

The Backend API (FastAPI) and Frontend (React) will be introduced in later development milestones.

### 2.1 PostgreSQL

Requirements:

- Use an official PostgreSQL image with an explicitly pinned version.
- Use a named Docker volume for database persistence.
- Configure database name, username, and password through environment variables.
- Provide a database readiness health check using `pg_isready`.
- Do not expose the database publicly.
- Allow local development access only when explicitly required.

### 2.2 Redis

Requirements:

- Use an official Redis image with an explicitly pinned version.
- Configure Redis access according to the selected development security model.
- Provide a health check using `redis-cli ping`.
- Document the persistence policy.
- Do not expose Redis publicly.
- Define its future use for caching and background task/queue support.

Redis persistence settings must be selected and documented before implementation. Cache data and critical queued jobs must not be treated as equivalent data.

## 3. Containerization

- Use Docker Compose to manage the initial services.
- Keep service definitions independent.
- Pin image versions; do not use the `latest` tag.
- Define appropriate restart policies.
- Avoid privileged containers and unnecessary capabilities.
- Keep configuration reproducible across development environments.

## 4. Networking

- Create a dedicated Docker bridge network.
- Connect PostgreSQL and Redis to this network.
- Use Compose service names for internal communication.
- Do not expose database services to public interfaces.
- If host access is required during development, bind published ports to `127.0.0.1`.
- Allow future AIGIS services to join the infrastructure network.

## 5. Configuration and Secrets

- Store actual local development configuration in an untracked `.env` file.
- Provide an `.env.example` containing placeholder values only.
- Ensure `.env` is excluded from Git.
- Never commit passwords, API keys, tokens, or other secrets.
- Separate application configuration from infrastructure definitions.
- Document required environment variables.

## 6. Persistent Storage and Backups

- Use a named Docker volume for PostgreSQL data.
- Ensure PostgreSQL data survives container recreation.
- Define Redis persistence behavior explicitly.
- Document volume naming and ownership considerations.
- Define backup and restoration procedures in the M9.4 work package.
- Never assume that deleting or recreating a container is equivalent to deleting its persistent volume.

## 7. Health Checks and Observability

- PostgreSQL health check: `pg_isready`.
- Redis health check: `redis-cli ping`.
- Configure suitable intervals, timeouts, and retries.
- Distinguish container startup from service readiness.
- Provide basic instructions for viewing service logs and health status.

## 8. Proposed Directory Structure

```text
infrastructure/
├── docker/
│   ├── docker-compose.yml
│   └── .env.example
└── README.md
```

This structure is the proposed initial layout. Any existing repository conventions must be checked before implementation.

## 9. Security Requirements

- Follow the principle of least privilege.
- Do not expose PostgreSQL or Redis publicly.
- Do not commit real credentials.
- Use trusted official container images.
- Avoid privileged containers.
- Keep development configuration separate from production secrets.
- Review exposed ports and persistent volumes before deployment.

## 10. Acceptance Criteria

- [ ] Docker Compose configuration exists and is documented.
- [ ] PostgreSQL starts successfully.
- [ ] Redis starts successfully.
- [ ] Both services join the dedicated Docker network.
- [ ] PostgreSQL data persists after container recreation.
- [ ] Redis persistence behavior is explicitly configured and documented.
- [ ] Both services have working health checks.
- [ ] Development configuration is separated from committed example configuration.
- [ ] No secrets are committed to Git.
- [ ] Infrastructure setup instructions are documented.

## 11. Implementation Sequence

1. Validate the infrastructure directory structure.
2. Select and pin PostgreSQL and Redis image versions.
3. Create the Docker Compose configuration.
4. Configure networking and persistent volumes.
5. Configure environment variables and secrets protection.
6. Configure health checks.
7. Start and validate both services.
8. Test PostgreSQL persistence.
9. Document startup, shutdown, and troubleshooting procedures.

## 12. Future Extensions

The initial infrastructure will be extended progressively:

- FastAPI Backend.
- React Frontend.
- Background workers.
- Additional monitoring and observability services.
- External Wazuh integration.
- External AI provider integration.

These services are outside the initial M9-001 implementation scope.
