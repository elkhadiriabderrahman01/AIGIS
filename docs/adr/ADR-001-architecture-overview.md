# ADR-001: AIGIS Architecture Overview



- **Status:** Accepted

- **Date:** 2026-11-16

- **Decision Owners:** AIGIS Project Team



## 1. Context



AIGIS is an all-in-one, multi-tenant MSSP SOC platform designed to help security teams monitor, investigate, manage, and respond to security incidents across multiple customer environments.



The platform must support cloud and on-premises deployments, scalable architecture, secure tenant isolation, and human-approved security automation.



AIGIS will integrate with existing security tools instead of rebuilding established SIEM and EDR capabilities.



## 2. Decision



AIGIS will use a modular, API-first architecture.



The initial architecture will consist of:



- **Backend:** Python 3.12 and FastAPI.

- **Frontend:** React and TypeScript using Vite.

- **Primary database:** PostgreSQL.

- **Caching and background processing:** Redis.

- **Security monitoring:** Wazuh.

- **Deployment:** Docker and Docker Compose.

- **AI integration:** External Large Language Model (LLM) APIs.

- **Version control:** Git and GitHub.



## 3. Main Architectural Modules



| Module | Responsibility |

|---|---|

| Authentication | User authentication and session management |

| RBAC | Role-based access control |

| Tenant Management | Customer organization management |

| Ingestion | Collecting and normalizing security events |

| Detection | Managing and validating detection rules |

| Alert Management | Alert lifecycle and investigation |

| Case Management | Incident investigation and tracking |

| Threat Intelligence | External IOC enrichment |

| Automation | Playbook execution with approval controls |

| AI Assistance | Alert triage and investigation support |

| Reporting | Security reports and operational metrics |

| Dashboard | User interface and visualization |



## 4. Deployment Strategy



AIGIS must support two deployment models:



1\. **Cloud deployment:** AIGIS services operate in a cloud-hosted environment.

2\. **On-premises deployment:** AIGIS services operate within customer-controlled infrastructure.



Docker Compose will be used for the initial development and testing environment.



The architecture must avoid unnecessary dependencies on a single deployment model.



## 5. Data Storage Strategy



AIGIS will use a hybrid storage approach.



- PostgreSQL stores platform data, users, tenants, alerts, cases, configurations, and audit records.

- Redis provides caching and supports background processing.

- Wazuh manages security monitoring data and associated indexing infrastructure.

- Additional storage systems may be introduced when justified by scale or requirements.



Tenant-owned data must be associated with a tenant identifier and protected by tenant isolation controls.



## 6. Security Architecture



Security must be integrated into the architecture from the beginning.



The following principles apply:



- Role-based access control.

- Server-side authorization.

- Tenant isolation.

- PostgreSQL Row-Level Security where appropriate.

- Secure credential and secret management.

- Audit logging for privileged operations.

- API input validation.

- Secure communication between services.

- No unauthorized cross-tenant data access.



## 7. AI Integration



AI capabilities will initially use external LLM APIs.



AI may assist with:



- Alert triage.

- Alert summarization.

- Investigation notes.

- Threat explanation.

- Response recommendations.



AI must not autonomously execute destructive or sensitive response actions.



AI requests must respect tenant boundaries and avoid exposing unnecessary sensitive information.



## 8. Automation Strategy



AIGIS will initially use an internal playbook engine.



Security response actions must follow a human approval workflow.



The platform must record:



- Requested action.

- Requesting user.

- Approval decision.

- Execution result.

- Timestamp.

- Associated alert or case.



## 9. Architectural Principles



AIGIS development will follow these principles:



1\. Modular design.

2\. API-first development.

3\. Security by design.

4\. Multi-tenancy from the beginning.

5\. Separation of responsibilities.

6\. Reuse of established security tools.

7\. Human oversight of sensitive automation.

8\. Incremental implementation and testing.

9\. Evidence-based progression between development phases.



## 10. Consequences



### Positive consequences



- Clear separation of platform responsibilities.

- Easier maintenance and testing.

- Support for cloud and on-premises deployments.

- Integration with established security technologies.

- A foundation for future scalability.



### Trade-offs and challenges



- Multi-tenancy introduces additional security complexity.

- Hybrid storage requires careful data management.

- External AI APIs introduce cost, privacy, and availability considerations.

- Docker-based deployments require resource management.

- Integration with Wazuh introduces operational dependencies.



## 11. Implementation Scope



This ADR establishes the initial architectural direction.



It does not mean that all listed components have been implemented.



Each module must be developed incrementally according to the AIGIS engineering roadmap and its acceptance criteria.



Changes to major architectural decisions must be documented in new ADRs.


