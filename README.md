# Newolf

A multi-tenant API hardened for cloud-native security and governance.

## Purpose
Demonstrate applied cloud security and DevSecOps practices through a real system — built while studying, not after.

## Status
**Early development** — Foundational architecture and threat modeling in progress.

## Planned Stack
- **API Core:** Python *(e.g., FastAPI)*
- **Data Layer:** *(e.g., PostgreSQL / DynamoDB)*
- **Containerization:** Docker
- **Infrastructure:** AWS via Terraform (IaC)
- **Pipelines:** CI/CD with integrated DevSecOps gates (SAST, SCA, container scanning)

## Project Structure
- `src/` — Application source code
- `infra/` — Terraform configurations and IaC scripts
- `docs/` — Project documentation (Charter, Threat Model, Architecture diagrams)
- `tests/` — Automated test suites

## Roadmap
- [ ] Core API with robust tenant isolation
- [ ] Authentication & authorization layer (RBAC/ABAC)
- [ ] Secrets management and encryption at rest
- [ ] Audit logging and centralized monitoring
- [ ] Container hardening and minimal base images
- [ ] Infrastructure as Code deployment (AWS)
- [ ] CI/CD pipeline integration with automated security gates