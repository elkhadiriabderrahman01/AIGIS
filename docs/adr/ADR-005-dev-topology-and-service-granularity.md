# ADR-005: Service Granularity and Development Topology

- **Status:** Proposed
- **Date:** 2026-11-17
- **Decision Owners:** AIGIS Project Team
- **Amends:** ADR-004 section 3.1 (containerization granularity) and ADR-003 section 9 (development environment)

## 1. Context

ADR-001 defines AIGIS as a set of modules. ADR-004 states that each independently deployable service has its own container image. ADR-003 places monitored endpoints inside the Docker Compose laboratory.

The primary development machine is a Windows 11 laptop with 16 GB of RAM running Docker Desktop on WSL2. The team is one developer. Three problems follow:

1. Many separate services plus a Wazuh stack compete for limited memory and add operational overhead (images, health checks, service-to-service authentication, versioning, debugging) before any product feature exists.
2. A container is not a realistic monitored endpoint. It has no real service manager, and file integrity and system inventory behave differently from a real host.
3. Attack simulations (for example Atomic Red Team) must never run on the primary workstation.

## 2. Decision

### 2.1 Modular monolith first

The AIGIS backend starts as one FastAPI codebase organized into modules matching the repository layout under backend/ (authentication, tenants, core, services, api) plus ingestion/, detection/ and automation/.

Rules:

- Each module exposes a defined interface. Other modules call that interface, not its internals.
- Each module owns its database tables. No module reads or writes another module's tables directly.
- Every tenant-owned object carries tenant_id, and tenant context is passed explicitly across module interfaces.
- The same codebase may run in more than one process (for example an API process and a background worker process). This is one codebase, not separate services.

### 2.2 Criteria for extracting a service

A module becomes a separate service only when there is evidence of at least one of:

- It needs to scale independently, measured by load or latency data.
- It needs stronger isolation (for example it handles tenant credentials or executes response actions).
- It needs a different runtime or language.
- Its failure must not take down the rest of the platform.

Each extraction is recorded in a new ADR.

### 2.3 Compose profiles

Docker Compose profiles control which containers start:

| Profile | Contents |
|---|---|
| core | PostgreSQL, Redis, AIGIS API, AIGIS worker |
| wazuh | Wazuh manager, indexer, dashboard |
| full | core plus wazuh (plus frontend when it exists) |

### 2.4 Resource budget

WSL2 memory is capped through the .wslconfig file in the user profile. The initial cap and the per-container limits are targets only. They must be validated by measurement (docker stats) in the environment baseline task before being treated as facts.

### 2.5 Monitored endpoints

- A small, disposable Linux virtual machine acts as the monitored endpoint and attack target. A snapshot is taken before every attack test.
- Attack simulations run only inside such virtual machines, never on the host workstation.
- The Windows endpoint approach (host agent for telemetry only, or a Windows virtual machine) is decided in the Telemetry Core phase after the resource baseline is measured.
- VirtualBox and WSL2 coexistence on Windows 11 is verified during the baseline task. If it is unreliable, the hypervisor choice is revisited in a new ADR.

### 2.6 Wazuh exposure in development

The Wazuh dashboard and indexer ports are bound to the local host only. They are never exposed to tenants. Tenants access telemetry only through AIGIS.

## 3. Alternatives Considered

| Option | Evaluation |
|---|---|
| One container per module from the start | Rejected for v1: memory and operational overhead without evidence of need |
| Modular monolith, extract on evidence | Selected |
| Full virtual machines for every component | Rejected: heavier than containers and harder to reproduce |
| Endpoints as containers only | Rejected: unrealistic telemetry and unsafe for attack simulation |

## 4. Consequences

### Positive

- Lower memory use and faster development on the target machine.
- Simpler debugging and testing, with one codebase and one test suite.
- Module boundaries stay explicit, so later extraction is possible.
- Realistic telemetry from a real endpoint, with safe attack testing.

### Negative

- Module boundary discipline must be enforced by review and automated checks, otherwise the monolith becomes tightly coupled.
- A defect in one module can affect the whole process until extraction.
- Independent scaling is not available until a module is extracted.

## 5. Verification

This ADR is verified when:

1. A Compose file starts the core profile and the wazuh profile separately.
2. docker stats output with the full profile running is recorded and fits within the WSL2 cap.
3. The endpoint virtual machine reports to Wazuh and can be restored from a snapshot.
4. An automated check (to be defined) detects cross-module imports of internals.

## 6. Implementation Scope

This ADR records a decision. It does not mean the Compose file, profiles, endpoint virtual machine or module boundaries have been implemented.
