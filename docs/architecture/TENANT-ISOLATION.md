# AIGIS - Tenant Isolation Requirements v1



## 1. Purpose

Ensure that each customer organization (tenant) can only access its own data and resources.



## 2. Tenant Model

- Each customer organization represents one tenant.

- Every tenant has a unique identifier (tenant_id).

- Users belong to an authorized tenant.

- A user must not access another tenant's resources without explicit authorization.



## 3. Data Isolation

- Use a shared PostgreSQL database initially.

- Every tenant-owned database record must contain a tenant_id.

- Implement PostgreSQL Row-Level Security (RLS).

- Enforce tenant isolation on the backend.

- Prevent cross-tenant access through API requests.



## 4. Resource Isolation

Tenant isolation must apply to:

- User accounts and permissions.

- Security alerts and events.

- Investigation cases.

- Reports and audit logs.

- Integration configurations and credentials.

- Background jobs and cached data.

- AI investigation context.



## 5. Security Requirements

- Never trust tenant_id supplied directly by the client.

- Derive tenant context from authenticated user permissions.

- Apply authorization checks to every tenant-scoped request.

- Prevent database connection pools from leaking tenant context.

- Log unauthorized cross-tenant access attempts.

- Ensure AI requests never include another tenant's private data.



## 6. Tenant Administration

- Super Admin manages tenant creation and lifecycle.

- Tenant Admin manages users within their own tenant.

- Tenant users cannot modify another tenant's configuration.

- Tenant-specific integrations must use isolated credentials.



## 7. Acceptance Criteria

- Tenant A cannot access Tenant B's alerts.

- Tenant A cannot access Tenant B's users or cases.

- Changing tenant_id in an API request cannot bypass authorization.

- PostgreSQL RLS blocks unauthorized tenant data access.

- Cached data and background jobs respect tenant boundaries.

- Cross-tenant access attempts are rejected and logged.

- Tenant reports contain only authorized tenant data.



## 8. Architecture Principle

Tenant isolation must be enforced at multiple layers:

- API authorization.

- Application services.

- PostgreSQL Row-Level Security.

- Cache and background job boundaries.

- Integrations and AI context.



Tenant isolation is a security requirement, not an optional feature.


