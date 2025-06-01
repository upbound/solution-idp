# Internal Developer Platform (IDP) Starter Kit

A ready-to-use architecture designed to turn Upbound Control Planes into a
streamlined, self-service platform for your engineering teams. Clone and tailor
it to your needs to roll out an Internal Developer Platform (IDP) that enables
developers to quickly spin up infrastructure that's production-ready,
policy-compliant, and governed by Upbound's control mechanisms and proven
patterns.

---

## Overview

This repository kickstarts a complete GitOps workflow for the **solution-idp**
platform using Upbound. It provisions control planes, deploys ArgoCD and
Backstage, and sets up syncing for environment manifests located under the
`state/` directory.

Key features powered by the **Upbound `up` CLI**:

* Create and configure Control Planes (CTPs) within a designated organization
  and space
* Configure Crossplane providers and dependencies
* Deploy ArgoCD
* Install Backstage on an EKS cluster (AWS)
* Auto-sync manifests from structured state directories like `compute`, `db`,
  `frontend`, etc.

Available demo setups:

* `solution-idp-non-prod`
* `solution-idp-prod`

---

## Prerequisites

Before you start, make sure you have:

* A Unix-like system (macOS/Linux/WSL) with Docker and `kubectl`
* [Upbound CLI (`up`)](https://docs.upbound.io/cli/)
* Access to an Upbound Space and Organization
* AWS credentials stored at `/Users/$USER/.aws/aws.json` (customizable in
  Taskfile)

---

## Project Layout

```
.
├── _output/                   # Local Upbound CLI binary
├── state/                     # GitOps source of truth for environments
│   ├── solution-idp-non-prod/
│   └── solution-idp-prod/
├── Taskfile.yaml              # Main task runner config
└── README.md
```

Inside each environment directory:

* `configurations.yaml`: List of Crossplane configuration packages for bootstrap
* `environments.yaml`: Logical environment definitions
* Subdirectories like `frontend`, `db`: Kubernetes manifests
* `Taskfile.yaml`: Env-specific automation logic

---

## Quick Start

Once running, ArgoCD will begin syncing from:

```
state/solution-idp-non-prod
├── compute
├── db
├── frontend
├── network
```

Each subfolder represents a separate Upbound Control Plane / Group and may include:

* Crossplane configurations
* Providers and Functions
* Upbound Controllers
* XRs and Claims
* ProviderConfigs
