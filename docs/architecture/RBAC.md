# AIGIS - Role-Based Access Control (RBAC) v1



## 1. Purpose

Define permissions for users accessing the AIGIS platform.



## 2. User Roles



### 2.1 Super Admin

- Manage customer tenants.

- Manage platform-wide configuration.

- Manage users across the platform.

- Access tenant data only through explicitly authorized and audited access.



### 2.2 Tenant Admin

- Manage users within their tenant.

- Configure tenant integrations.

- View and manage tenant alerts and cases.

- Approve response actions.

- View tenant audit logs and reports.



### 2.3 SOC Analyst

- View tenant dashboards and alerts.

- Investigate and update alerts.

- Create and manage investigation cases.

- View tenant reports.

- Cannot manage users or platform configuration.

- Cannot approve response actions.



### 2.4 SOC Viewer

- View dashboards.

- View alerts and cases.

- View reports.

- Cannot modify alerts, cases, users, or configurations.



## 3. Permission Principles

- Apply least privilege.

- Enforce permissions on the backend.

- Restrict access according to tenant boundaries.

- Deny access by default.

- Record privileged operations in audit logs.



## 4. Security Requirements

- Users must not access resources outside their authorized tenant.

- Unauthorized API requests must return HTTP 403.

- Tenant identifiers supplied by clients must never be trusted without server-side validation.

- Privileged access must be explicitly authorized and audited.



## 5. Acceptance Criteria

- Tenant Admin cannot access another tenant's users.

- SOC Analyst cannot modify platform-wide configuration.

- SOC Viewer cannot modify alerts or cases.

- Unauthorized requests return HTTP 403.

- Cross-tenant access is blocked.

- Privileged operations are recorded in audit logs.


