# Newolf — Threat Model

**Phase:** 00 — Research & Environment Charter  
**Owner:** Luan Thales  
**Status:** In progress  

---

## Overview

This document captures the threat model I built for Newolf during Phase 00.
The goal was to identify what needs to be protected, where the system is weak,
and how an attacker could exploit those weaknesses before I write a single line
of production code. I used two frameworks: STRIDE to categorize threat types,
and PASTA to connect those threats to real attack scenarios and mitigations.

---

## 1. STRIDE

I applied STRIDE to map the six threat categories against Newolf's core design
as a multi-tenant secrets management API.

| Category | How it applies to Newolf |
|---|---|
| Spoofing | An attacker uses a stolen credential or token to impersonate a legitimate user or tenant |
| Tampering | The API accepts a valid token but fails to verify ownership — attacker modifies or replaces another tenant's API key |
| Repudiation | A sensitive action is performed with no audit log — no way to prove who made the change or deletion |
| Info Disclosure | Missing ownership check on a resource endpoint (IDOR) allows one tenant to read another tenant's secrets |
| Denial of Service | An attacker floods the API with requests, exhausting CPU, memory or connections and taking the service down for everyone |
| Elevation of Privilege | The system trusts a role field sent by the client — attacker edits it to gain admin access |

---

## 2. PASTA

### Stage 1 — Objectives

Three things I need to protect in Newolf, and why they matter:

1. **Client secrets and credentials** — if a secret leaks, the client loses
control of their own systems. That is a direct business failure, not just
a technical one.

2. **Tenant isolation** — if one tenant can see another's data, every client
is affected at once. Trust breaks for all of them simultaneously.

3. **Service availability** — if Newolf goes down, clients cannot authenticate
their apps, run automations or access their secrets. That means broken
operations, financial loss, SLA violations and reputation damage.

### Stage 2 — Scope

For this analysis I focused on three core components:

1. **Newolf API** — the main entry point. Every request passes through here,
so authentication, authorization and input validation all live in this layer.

2. **Secrets storage** — where the most sensitive client data lives. Getting
this wrong means confidentiality, integrity and availability all fail at once.

3. **Identity and access control** — responsible for authenticating users,
issuing and validating tokens, and enforcing what each user or tenant is
allowed to do.

### Stage 3 — Decomposition

This is the flow of a single request to store a secret in Newolf:

1. Client sends an HTTP request with the secret in the request body
2. API receives the request
3. System verifies the user is authenticated
4. System verifies the user has permission to perform this action
5. Input is validated
6. Secret is encrypted
7. Secret is associated with the correct tenant
8. Secret is saved to secure storage
9. Action is recorded in the audit log
10. API returns a confirmation to the client

### Stage 4 — Threat Analysis

Covered by STRIDE in Section 1 of this document.

### Stage 5 — Vulnerabilities

These are the specific weaknesses I identified in Newolf at this stage:

1. Insufficient identity and permission validation
2. Inadequate tenant isolation at the API layer
3. Secrets stored without proper encryption or access control
4. Missing audit logging for sensitive actions
5. No rate limiting or protection against API abuse
6. Weak server-side enforcement of administrative permissions

### Stage 6 — Attacks

**Scenario 1 — Cross-tenant data leak (IDOR)**  
After authenticating normally, I change the secret ID in the request URL.
The API checks that I am logged in, but never checks if that secret belongs
to my tenant. The server returns a secret from a different company.  
Impact: credential leak, exposed API keys, broken tenant isolation.

**Scenario 2 — Administrative takeover**  
I authenticate as a regular user and send a direct request to an admin
endpoint. The interface hides it from me, but the API never validates my
permission server-side. The server accepts the request and promotes my account
to admin.  
Impact: full privilege escalation, access to all tenant data, potential
complete platform compromise.

### Stage 7 — Mitigations

**Scenario 1:**
- Always validate that the requested secret belongs to the authenticated tenant
before returning anything
- Enforce authorization on every API request, not just in the interface layer
- Use unpredictable identifiers where possible — this raises the bar but does
not replace proper authorization
- Log every denied access attempt for detection and investigation

**Scenario 2:**
- Validate all permissions server-side on every request, regardless of what
the interface shows
- Never trust role or permission fields sent by the client
- Apply least privilege — every user gets only what they need, nothing more
- Log all administrative actions and alert on unusual patterns

## 3. Attack Trees

### Goal: Access another tenant's secret

Acessar segredo de outro tenant (OR)
├── Caminho 1: Spoofing (OR)
│ ├── Usar credencial roubada de usuário legítimo
│ └── Usar token de autenticação comprometido
└── Caminho 2: Information Disclosure - IDOR (OR)
├── Alterar o identificador (ID) do segredo na requisição
└── Explorar ausência de verificação de ownership na API

Both paths lead to the same outcome: unauthorized access to another
tenant's secrets. Closing one path is not enough both must be addressed
independently.