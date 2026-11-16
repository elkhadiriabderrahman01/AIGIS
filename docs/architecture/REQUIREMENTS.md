# AIGIS - Product Requirements v1

## 1. Product Overview
AIGIS is an all-in-one MSSP SOC platform designed to help security teams monitor, investigate, and respond to security threats across multiple customer organizations.

## 2. Deployment
- Cloud deployment
- On-premises deployment

## 3. Core Requirements

### 3.1 Authentication
- Secure user login and logout.
- Password protection.
- Session management.

### 3.2 User Roles
- Super Admin
- Tenant Admin
- SOC Analyst
- SOC Viewer

### 3.3 Multi-Tenancy
- Support multiple customer organizations.
- Isolate customer data.
- Enforce tenant access permissions.
- Use PostgreSQL Row-Level Security.

### 3.4 Security Monitoring
- Integrate Wazuh.
- Collect and normalize security events.
- Process security alerts.

### 3.5 Alert Management
- Display and filter alerts.
- Assign alerts.
- Update alert status.

### 3.6 Case Management
- Create investigation cases.
- Link alerts to cases.
- Track investigation progress.

### 3.7 Threat Intelligence
- Integrate external threat intelligence APIs.
- Enrich indicators of compromise.

### 3.8 Automation
- Support internal playbooks.
- Require human approval before response actions.

### 3.9 AI Integration
- Use an external LLM.
- Assist with alert triage and investigation.
- Generate summaries and response recommendations.
- No autonomous destructive actions.

### 3.10 Audit and Reporting
- Record important user actions.
- Generate tenant-specific security reports.

## 4. Technical Requirements
- Backend: Python and FastAPI.
- Frontend: React and TypeScript.
- Database: PostgreSQL.
- Cache and background tasks: Redis.
- Security monitoring: Wazuh.
- Infrastructure: Docker.

## 5. Data Strategy
Use a hybrid storage architecture.

## 6. Security Requirements
- Enforce role-based access control.
- Isolate tenant data.
- Protect credentials and sensitive information.
- Log privileged operations.
- Validate permissions on the backend.

## 7. Initial Release
The first release must be tested using a real security monitoring environment.

## 8. Architecture Principle
Start with a modular monolith. Avoid premature microservices.
