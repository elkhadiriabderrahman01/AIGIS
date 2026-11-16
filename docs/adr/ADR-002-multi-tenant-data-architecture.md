# ADR-002: Multi-Tenant Data Architecture



- **Status:** Accepted

- **Date:** 2026-11-16

- **Decision Owners:** AIGIS Project Team



## 1. Context



AIGIS is an MSSP SOC platform serving multiple customer organizations (tenants).



Each tenant must have access only to its own users, alerts, cases, integrations, reports, and security data.



The architecture must prevent unauthorized cross-tenant access while remaining scalable and maintainable.



## 2. Decision



AIGIS will initially use a shared PostgreSQL database with logical tenant separation.



Every tenant-owned database record must contain a `tenant\_id` identifying its owning organization.



PostgreSQL Row-Level Security (RLS) will provide an additional database-level isolation layer.



The backend will enforce tenant authorization on every protected operation.



## 3. Tenant Model



A tenant represents one customer organization using AIGIS.



Each tenant has:

- A unique tenant identifier.

- Organization information.

- Tenant administrators.

- Authorized users and roles.

- Security integrations.

- Alerts and cases.

- Tenant-specific configurations.



The Super Admin can manage tenant organizations at the platform level.



Tenant administrators can manage users and resources within their own organization.



## 4. Database Architecture



The initial database strategy is:



- One shared PostgreSQL database.

- Shared tables for tenant-owned entities.

- A `tenant\_id` column on every tenant-owned record.

- Foreign-key relationships that preserve tenant ownership.

- Database indexes supporting tenant-scoped queries.

- PostgreSQL RLS for additional isolation.



Platform-wide records that do not belong to a specific tenant must be explicitly identified.



## 5. Tenant Context and Authorization



The backend must establish tenant context using authenticated identity and trusted server-side authorization.



The backend must never trust a tenant identifier supplied by a client without validating the user's authorization.



Every request accessing tenant-owned data must verify:

1\. The user is authenticated.

2\. The user's role permits the requested operation.

3\. The user belongs to the requested tenant.

4\. The requested resource belongs to that tenant.



Unauthorized cross-tenant access must be rejected.



## 6. PostgreSQL Row-Level Security



PostgreSQL RLS will be used to enforce tenant-level database access restrictions.



Implementation requirements:



- Enable RLS on tenant-owned tables.

- Define policies for tenant-scoped access.

- Set tenant context securely for each database transaction.

- Prevent tenant context from leaking between pooled database connections.

- Use a restricted application database role.

- Ensure the application role cannot bypass RLS.

- Test isolation using multiple tenants.



RLS is an additional protection layer, not a replacement for backend authorization.



## 7. Other Data and Service Isolation



Tenant separation must also apply to:



- Redis cache keys.

- Background jobs.

- File and report storage.

- Security integrations and credentials.

- Wazuh data access and tenant-specific monitoring.

- AI prompts and investigation context.

- Logs and audit records.



No service may accidentally combine sensitive information from different tenants.



## 8. Tenant Lifecycle



AIGIS must eventually support:



- Tenant creation.

- Tenant configuration.

- User assignment.

- Tenant activation and suspension.

- Tenant-specific integration configuration.

- Secure tenant data export.

- Tenant deletion according to retention and legal requirements.



Tenant deletion must not bypass audit or retention obligations.



## 9. Security Requirements



The implementation must satisfy these requirements:



- TEN-01: Every tenant-owned record has a tenant identifier.

- TEN-02: Users can access only authorized tenant resources.

- TEN-03: Backend APIs enforce tenant authorization.

- TEN-04: PostgreSQL RLS protects tenant-owned records.

- TEN-05: Cross-tenant access attempts are rejected and logged.

- TEN-06: Reports are restricted to the owning tenant.

- TEN-07: Integrations and credentials are isolated by tenant.

- TEN-08: Tenant lifecycle operations are controlled and audited.



## 10. Testing Requirements



Before production use, tests must demonstrate:



1\. Tenant A cannot read Tenant B's alerts.

2\. Tenant A cannot modify Tenant B's cases.

3\. Tenant administrators cannot manage users from another tenant.

4\. Manipulating a client-supplied tenant identifier does not bypass authorization.

5\. RLS prevents unauthorized database access.

6\. Background jobs preserve tenant context.

7\. Redis keys and stored files do not expose another tenant's data.

8\. AI investigation context cannot leak between tenants.



## 11. Consequences



### Positive consequences



- Clear tenant ownership.

- Stronger data isolation.

- Centralized database management.

- Easier initial deployment.

- A foundation for future MSSP scalability.



### Trade-offs and challenges



- Shared database tables require strict tenant filtering.

- RLS introduces additional database configuration.

- Connection pooling requires careful tenant-context management.

- Background processing must preserve tenant context.

- Wazuh and external integrations require additional isolation controls.



## 12. Future Scalability



The initial shared-database approach may evolve.



Depending on customer scale and isolation requirements, AIGIS may introduce:



- Dedicated databases for selected tenants.

- Dedicated telemetry indices.

- Separate deployment environments.

- Tenant-specific encryption keys.



These changes must be evaluated through future architecture decisions.



## 13. Implementation Scope



This ADR defines the target architecture and security requirements.



It does not indicate that tenant isolation has already been implemented.



Implementation must follow the AIGIS roadmap and pass the required security tests before handling real customer data.




