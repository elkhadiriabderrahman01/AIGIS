# AIGIS

AIGIS is a modular, multi-tenant Security Operations Center (SOC) platform.

## Status

Phase 0 - Foundations. Nothing here is production-ready.

## Planned capabilities (not yet implemented)

- Telemetry collection and detection (Wazuh-based)
- Multi-tenant alert ingestion and normalization
- Case management and investigation
- Threat intelligence enrichment
- Automated response with human approval
- AI-assisted triage

## Repository layout

- backend/        API, core logic, authentication, tenants, services
- frontend/       web interface
- detection/      rules, correlation, detection tests
- ingestion/      collectors, parsers, normalization
- automation/     playbooks and integrations
- ai/             AI-assisted analysis
- infrastructure/ Docker Compose and environment setup
- tests/          cross-module tests
- docs/           architecture, API, security, deployment, ADRs
- .github/        CI workflows
