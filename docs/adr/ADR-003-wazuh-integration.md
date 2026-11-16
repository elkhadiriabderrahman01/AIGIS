# ADR-003: Security Telemetry and Wazuh Integration



- **Status:** Accepted

- **Date:** 2026-11-16

- **Decision Owners:** AIGIS Project Team



## 1. Context



AIGIS requires a reliable security telemetry and detection foundation to monitor customer environments and provide actionable security alerts.



Building a SIEM or endpoint detection engine from scratch would significantly increase development complexity and time.



AIGIS will therefore integrate with an established security monitoring solution while focusing on the MSSP platform capabilities.



## 2. Decision



AIGIS will use **Wazuh** as its initial security monitoring and detection engine.



Wazuh will provide:



- Endpoint monitoring through Wazuh agents.

- Security event collection.

- Log analysis.

- Detection rules and alert generation.

- File integrity monitoring.

- Security configuration assessment.

- Centralized security event management.



AIGIS will provide the platform layer for multi-tenancy, alert management, investigations, case management, enrichment, automation, AI assistance, and reporting.



AIGIS will not rebuild Wazuh's core SIEM or endpoint monitoring capabilities.



## 3. Architecture and Data Flow



The initial telemetry flow will be:



1\. Monitored endpoints generate security events.

2\. Wazuh agents collect endpoint telemetry.

3\. Wazuh Manager receives and processes events.

4\. Wazuh rules generate security alerts.

5\. Wazuh Indexer stores indexed security data.

6\. AIGIS retrieves relevant alerts through supported interfaces.

7\. AIGIS normalizes and associates the events with the correct tenant.

8\. Alerts become available for investigation and case management.



## 4. Integration Strategy



AIGIS will implement a dedicated Wazuh integration adapter.



The adapter will be responsible for:



- Connecting to supported Wazuh interfaces.

- Retrieving security alerts.

- Validating incoming data.

- Normalizing event fields.

- Handling authentication and credentials securely.

- Managing connection failures.

- Preventing duplicate event processing.

- Recording integration errors.



The integration must use supported Wazuh APIs or documented interfaces.



AIGIS must not directly modify Wazuh internal data or bypass its supported security controls.



## 5. Event Normalization



AIGIS will define a normalized event structure independent of the original security monitoring provider.



The initial normalized alert model should support:



- Unique event identifier.

- Tenant identifier.

- Source endpoint.

- Source IP address.

- Destination IP address, when available.

- Event timestamp.

- Event category.

- Severity.

- Detection rule identifier.

- Detection description.

- MITRE ATT\&CK references, when available.

- Original event payload.

- Processing status.



The original Wazuh event must remain available for investigation and traceability.



## 6. Multi-Tenant Considerations



Wazuh integration must respect AIGIS tenant isolation requirements.



The initial development environment may use one Wazuh deployment.



However, AIGIS must not assume that a shared Wazuh deployment automatically provides customer-level isolation.



Requirements:



- Associate monitored agents and their events with the correct tenant.

- Maintain trusted tenant-to-agent mappings.

- Validate tenant ownership before accepting or exposing telemetry.

- Prevent cross-tenant alert access.

- Protect integration credentials.

- Evaluate separate Wazuh deployments or stronger isolation for production customers when required.



Tenant ownership must be determined using trusted server-side configuration, not an unverified tenant identifier inside an incoming event.



## 7. Reliability and Event Processing



The integration must support reliable event processing.



Requirements:



- Handle temporary Wazuh API or service unavailability.

- Retry failed retrieval operations safely.

- Prevent duplicate alert creation.

- Track processing status.

- Log integration failures.

- Support recovery after service restarts.

- Avoid silently losing events.



Event processing should be idempotent wherever possible.



## 8. Security Requirements



The Wazuh integration must follow these security principles:



- Use authenticated connections.

- Protect API credentials and secrets.

- Apply least-privilege access.

- Validate incoming event data.

- Restrict access to Wazuh interfaces.

- Use secure communication channels.

- Audit integration configuration changes.

- Prevent unauthorized access to raw telemetry.

- Enforce tenant isolation in AIGIS.



## 9. Development and Testing Environment



The initial development environment will use Docker Compose.



The telemetry laboratory must include:



- Wazuh deployment.

- At least one Linux monitored endpoint.

- At least one Windows monitored endpoint.

- AIGIS integration component.

- Test security events.

- Validation of alert retrieval and processing.



The environment must be reproducible and documented.



## 10. Testing Requirements



Before considering the integration reliable, tests must demonstrate:



1\. Wazuh successfully receives telemetry from monitored endpoints.

2\. Wazuh generates alerts for selected detection scenarios.

3\. AIGIS successfully retrieves Wazuh alerts.

4\. AIGIS correctly normalizes incoming events.

5\. Duplicate events do not create duplicate alerts.

6\. Temporary integration failures can recover.

7\. Events remain associated with the correct tenant.

8\. Unauthorized access to telemetry is rejected.

9\. Integration credentials are not exposed in logs.

10\. Processing remains reliable after service restarts.



## 11. Consequences



### Positive consequences



- Reuse of an established security monitoring engine.

- Faster development of AIGIS platform capabilities.

- Support for endpoint monitoring.

- Access to existing detection rules.

- Reduced need to build SIEM functionality from scratch.



### Trade-offs and challenges



- AIGIS depends on Wazuh availability and compatibility.

- Integration requires careful event normalization.

- Multi-tenant telemetry isolation requires additional design.

- Large event volumes introduce ingestion and storage challenges.

- Wazuh upgrades may require adapter maintenance.



## 12. Future Considerations



The integration architecture should remain modular enough to support additional telemetry providers in the future.



Potential integrations may include other SIEM, EDR, NDR, and security monitoring solutions.



Such integrations must follow the same normalized event model and tenant isolation requirements.



## 13. Implementation Scope



This ADR establishes the architectural decision to use Wazuh as the initial telemetry and detection engine.



It does not indicate that the Wazuh integration has already been implemented.



Implementation and validation will follow the AIGIS engineering roadmap.


