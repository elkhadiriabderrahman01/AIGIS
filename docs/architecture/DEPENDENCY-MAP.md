# AIGIS - Requirements Dependency Map v1



## 1. Purpose

Define the dependencies between AIGIS components and establish the recommended development order.



## 2. Component Dependencies



| Component | Depends On |

|---|---|

| Authentication | Database, user model |

| RBAC | Authentication, user model |

| Multi-Tenancy | Database, authentication, RBAC |

| Wazuh Integration | Telemetry infrastructure |

| Event Ingestion | Wazuh integration, event normalization |

| Detection Engineering | Telemetry, event ingestion |

| Alert Management | Authentication, RBAC, multi-tenancy, ingestion |

| Case Management | Alert management, database |

| Threat Intelligence | Alert management, external APIs |

| SOAR | Alert management, case management, approval system |

| AI Assistance | Alert data, case context, external LLM |

| Dashboard | Backend APIs, authentication |

| Reporting | Alerts, cases, tenant isolation |



## 3. Development Order



### Phase 1 - Foundations

- Validate product requirements.

- Define RBAC.

- Define tenant isolation.

- Establish architecture decisions.



### Phase 2 - Telemetry Core

- Deploy Wazuh.

- Connect monitored endpoints.

- Verify event collection.

- Validate telemetry reliability.



### Phase 3 - Platform Backbone

- Build FastAPI backend.

- Configure PostgreSQL.

- Implement authentication.

- Implement RBAC.

- Implement tenant isolation.

- Build alert ingestion APIs.



### Phase 4 - Detection Engineering

- Develop detection rules.

- Map detections to MITRE ATT&CK.

- Test detection scenarios.



### Phase 5 - Case Management and UI

- Implement alert management.

- Implement case management.

- Develop React dashboard.



### Phase 6 - Enrichment

- Integrate threat intelligence APIs.

- Implement caching and rate limits.



### Phase 7 - Automation

- Develop internal playbook engine.

- Implement approval workflow.

- Record automation actions.



### Phase 8 - AI Assistance

- Integrate external LLM.

- Implement alert triage assistance.

- Generate investigation summaries.

- Evaluate AI accuracy and safety.



### Phase 9 - Multi-Tenant Hardening

- Test tenant isolation.

- Validate permissions.

- Perform cross-tenant security tests.



### Phase 10 - Production Readiness

- Test backups and restoration.

- Configure monitoring.

- Perform security testing.

- Evaluate deployment readiness.



## 4. Parallel Development



The following activities can progress in parallel when their prerequisites are satisfied:



- Detection rule development and backend development.

- Threat intelligence integration and case management.

- Frontend development and backend API development.

- AI evaluation and threat intelligence integration.



## 5. Critical Dependencies



- Authentication must precede protected API access.

- Tenant isolation must be implemented before handling real customer data.

- Telemetry must be verified before detection validation.

- Alert management must precede case management.

- Approval controls must exist before automated response actions.

- AI must respect tenant boundaries and must not execute destructive actions autonomously.



## 6. Completion Criteria



A component is ready for the next development stage only when:

- Its dependencies are satisfied.

- Its implementation is tested.

- Its acceptance criteria are verified.

- Evidence is documented.



## 7. Architecture Principle



Build AIGIS incrementally using a modular architecture. Avoid implementing dependent components before their foundational requirements are validated.


