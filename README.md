# Newolf

[![Status](https://img.shields.io/badge/Status-In%20Development-orange)](https://github.com/zar4t/newolf)
[![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Docker](https://img.shields.io/badge/Docker-planned-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](https://opensource.org/licenses/MIT)

> A multi-tenant API hardened for cloud-native security and governance — designed, attacked, detected and defended as a continuous engineering practice.

## Table of Contents

- [Project Description](#project-description)
- [Security Model](#security-model)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Roadmap](#roadmap)
- [Documentation](#documentation)
- [License](#license)

## Project Description

**Newolf** is a cloud-native, multi-tenant platform that stores and manages
secrets and credentials for client organizations.

The project demonstrates applied cloud security and DevSecOps practices
through a real system — built while studying, not after. Every component is
designed with a threat model first, then implemented, attacked in a controlled
lab, and defended.

The project demonstrates:
- Threat modeling before implementation (STRIDE, PASTA, Attack Trees)
- Multi-tenant isolation and least-privilege access control
- Secrets management with encryption at rest
- Audit logging and detection engineering
- Infrastructure as Code with security scanning
- CI/CD pipelines with integrated security gates

## Security Model

Security is designed before code, not added after. The current threat model
covers six STRIDE categories and two full attack scenarios, documented in
[`docs/THREAT-MODEL.md`](docs/THREAT-MODEL.md).

| Principle | How it applies |
|---|---|
| **Least privilege** | Every identity gets only what it needs |
| **Tenant isolation** | No tenant can reach another tenant's data |
| **Defense in depth** | Multiple independent layers, no single point of failure |
| **Auditability** | Every sensitive action is logged and attributable |
| **Zero trust** | Authorization enforced server-side on every request |

## Tech Stack

| Category | Technology |
|----------|------------|
| **API Core** | Python 3.12 (FastAPI) |
| **Data Layer** | PostgreSQL |
| **Containerization** | Docker |
| **Infrastructure** | Terraform, AWS |
| **CI/CD** | GitHub Actions with security gates |
| **Security Scanning** | Trivy, Checkov, SAST/SCA |
| **Observability** | OpenTelemetry, Prometheus, Grafana |

## Project Structure

newolf/
├── src/ # Application source code
│ ├── api/ # API endpoints and routing
│ ├── auth/ # Authentication and authorization
│ ├── tenants/ # Multi-tenant isolation logic
│ ├── secrets/ # Secret storage and encryption
│ ├── audit/ # Audit logging
│ └── models/ # Data models
├── tests/ # Automated test suites
│ ├── unit/ # Unit tests
│ ├── integration/ # Integration tests
│ └── security/ # Security-focused test cases
├── infra/ # Infrastructure as Code
│ ├── terraform/ # AWS provisioning
│ └── docker/ # Container configuration
├── docs/ # Project documentation
│ ├── PROJECT-CHARTER.md # Project scope and goals
│ ├── THREAT-MODEL.md # STRIDE, PASTA and Attack Trees
│ ├── PURPLE-TEAM-CHARTER.md # Purple team mission and rules
│ ├── CONVENTIONS.md # Naming and formatting standards
│ └── architecture/ # Architecture diagrams and decisions
├── labs/ # Purple team experiments and findings
├── .github/workflows/ # CI/CD pipelines
├── .gitignore # Git ignore rules
└── LICENSE # MIT License

## Roadmap

- [ ] Core API with robust tenant isolation
- [ ] Authentication & authorization layer (RBAC/ABAC)
- [ ] Secrets management and encryption at rest
- [ ] Audit logging and centralized monitoring
- [ ] Container hardening and minimal base images
- [ ] Infrastructure as Code deployment (AWS)
- [ ] CI/CD pipeline integration with automated security gates

## Documentation

- [Project Charter](docs/PROJECT-CHARTER.md) — scope, goals and ownership
- [Threat Model](docs/THREAT-MODEL.md) — STRIDE, PASTA and attack trees
- [Purple Team Charter](docs/PURPLE-TEAM-CHARTER.md) — mission, scope and lab rules
- [Conventions](docs/CONVENTIONS.md) — naming and formatting standards

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

## Author

**Luan Thales** — [GitHub](https://github.com/zar4t)
