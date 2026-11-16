# ADR-004: Infrastructure and Containerization Strategy



- **Project:** AIGIS

- **Status:** Accepted

- **Date:** 2026-11-16

- **Decision owner:** AIGIS Engineering Team

- **Phase:** Phase 1 — Requirements Validation



## 1. Context



AIGIS is an all-in-one MSSP SOC platform supporting both cloud and on-premises deployment.



The platform will contain multiple independent services, including identity management, customer management, security data ingestion, detection, incident management, AI integration and SOAR.



We need a containerization and deployment strategy that supports:



- Consistent development and production environments.

- Deployment on cloud and on-premises infrastructure.

- Independent service management.

- Persistent data storage.

- Secure service-to-service communication.

- Future scalability.



## 2. Decision



AIGIS will use **Docker and Docker Compose as its primary containerization and orchestration solution for v1**.



Kubernetes will remain a potential future option when deployment complexity and scalability requirements justify it.



## 3. Technical Strategy



### 3.1 Containerization

- Each independently deployable AIGIS service will have its own container image.

- Services will use Dockerfiles.

- Images should be reproducible and versioned.



### 3.2 Orchestration

- Docker Compose will define and manage the application services.

- Service dependencies and startup configurations will be documented.

- Health checks will be implemented where appropriate.

- Restart policies will be defined.



### 3.3 Networking

- Docker networks will manage internal service communication.

- Only necessary services and ports will be exposed externally.

- Internal services should not be unnecessarily exposed to the host network.



### 3.4 Data Persistence

- Docker volumes will support persistent application data.

- Database data must survive container restarts and recreation.

- Backup and recovery procedures will be defined separately.



### 3.5 Configuration and Secrets

- Environment-specific configuration will be separated from application code.

- An `.env.example` file will document required variables.

- Sensitive credentials must never be committed to Git.

- Production secrets must use an appropriate secrets-management mechanism.



### 3.6 Deployment

The same application architecture should support:

- On-premises deployment.

- Cloud deployment.



Initial Compose deployments will target a single host per deployment environment. Multi-node high availability is outside the initial orchestration scope.



## 4. Alternatives Considered



| Option | Evaluation |

|---|---|

| Docker Compose | Selected: simple and suitable for v1 |

| Kubernetes | Deferred: greater operational complexity |

| Manual deployment | Rejected: inconsistent and difficult to maintain |



## 5. Consequences



### Positive

- Simplified development and deployment.

- Consistent environments.

- Easier service isolation and management.

- Persistent data through volumes.

- Support for cloud and on-premises installations.



### Negative

- Docker Compose does not provide full multi-node orchestration.

- Scaling and high availability will require additional engineering.

- Container networking and persistent storage require careful configuration.



## 6. Security Considerations



- Apply least-privilege container permissions.

- Avoid privileged containers unless strictly necessary.

- Protect secrets and credentials.

- Restrict exposed ports.

- Use trusted and maintained container images.

- Implement appropriate container and image security checks.



## 7. Future Considerations



Kubernetes may be introduced when AIGIS requires:

- Multi-node orchestration.

- Advanced autoscaling.

- High availability across multiple nodes.

- More complex production deployment management.



Migration must preserve the modular service architecture.



## 8. Final Decision



Docker Compose is the approved orchestration solution for AIGIS v1.



Kubernetes is deferred until future operational and scalability requirements justify its adoption.



