# Azure Landing Zone Blueprint

> **Enterprise Azure Landing Zone architecture for governance, networking, security, and identity at scale.**

[![Status](https://img.shields.io/badge/status-reference--architecture-blue)](https://github.com/gethepc/azure-landing-zone-blueprint)
[![Stack](https://img.shields.io/badge/stack-Azure%20%7C%20Terraform%20%7C%20Bicep%20%7C%20Policy-orange)](https://github.com/gethepc/azure-landing-zone-blueprint)

---

## Overview

This blueprint documents an **enterprise-grade Azure Landing Zone** architecture designed to onboard workloads safely, securely, and consistently across multiple business units and environments.

Designed and implemented at a **global utility and IoT enterprise** as part of a hybrid cloud programme.

---

## Architecture

```
┌──────────────────────────────────────────────────────┐
│               Management Group Hierarchy              │
│  Root MG → Platform → Corp/Online/Sandbox Workloads  │
├──────────────────────────────────────────────────────┤
│                  Platform Subscriptions               │
│  ┌────────────┐  ┌────────────┐  ┌────────────────┐  │
│  │ Management │  │Connectivity│  │    Identity    │  │
│  │ (Monitor,  │  │(Hub VNet,  │  │  (AD DS, PIM,  │  │
│  │  Defender) │  │ ExpRoute,  │  │   Entra ID)    │  │
│  └────────────┘  │  Firewall) │  └────────────────┘  │
│                  └────────────┘                       │
├──────────────────────────────────────────────────────┤
│                  Landing Zone Subscriptions           │
│  Corp Workloads │ Online Workloads │ Sandbox          │
└──────────────────────────────────────────────────────┘
```

---

## Key Design Pillars

### 1. Management Group Hierarchy

| Level | Scope |
|---|---|
| Root MG | Global policies, RBAC baseline |
| Platform | Core infrastructure services |
| Corp | Internal enterprise workloads |
| Online | Customer-facing workloads |
| Sandbox | Dev/test with cost guardrails |

### 2. Networking (Hub-Spoke)

- Hub VNet with Azure Firewall and route tables
- ExpressRoute / VPN Gateway for hybrid connectivity
- Private DNS zones for PaaS service resolution
- NSG baseline policies applied via Azure Policy
- DDoS Standard on public-facing spokes

### 3. Identity & Access

- Entra ID (Azure AD) as identity provider
- PIM for privileged role activation
- Break-glass accounts with monitoring
- RBAC at MG level — least privilege by default
- Conditional Access for admin portals

### 4. Security & Compliance

- Microsoft Defender for Cloud enabled on all subscriptions
- Azure Policy initiatives for CIS / ISO27001 compliance
- Log Analytics workspace centralised in Management subscription
- Diagnostics routed to central workspace
- Alert rules for privileged operations

### 5. Governance & Cost

- Azure Cost Management budgets per subscription
- Tagging policy — mandatory tags enforced via Policy deny effect
- Naming convention standards defined and enforced
- Resource locks on platform subscriptions

---

## IaC Strategy

| Tool | Usage |
|---|---|
| Terraform | Platform subscriptions, networking, core services |
| Bicep | Workload-level deployments |
| Azure Policy | Guardrails and compliance enforcement |
| GitHub Actions | CI/CD for IaC pipelines |

---

## Repository Structure

```
azure-landing-zone-blueprint/
├── docs/
│   ├── management-group-design.md
│   ├── networking-design.md
│   ├── identity-design.md
│   ├── security-baseline.md
│   └── governance-model.md
├── assets/
│   └── landing-zone-architecture.png
├── examples/
│   ├── policy-assignments/
│   └── naming-conventions.md
└── README.md
```

---

## Author

**Prashant Chauhan** — Designed · Engineered · Validated · Delivered  
GitHub: [@gethepc](https://github.com/gethepc)  
LinkedIn: [prashant-chauhan-2b5a0b11](https://www.linkedin.com/in/prashant-chauhan-2b5a0b11/)

---

*Reference architecture. Proprietary implementation details are not included.*
