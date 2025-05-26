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

## Getting Started with Taskfile

This project leverages [`Task`](https://taskfile.dev) for automating setup
steps. To get going:

### 1. Clone this repository

```bash
git clone https://github.com/upbound/solution-idp.git
cd solution-idp
```

### 2. Install Task CLI

```bash
brew install go-task/tap/go-task
```

### 3. Bootstrap your environment

```bash
task bootstrap-all
```

This process will:

* Ensure the `up` CLI is installed and ready
* Initialize the root group and control plane
* Set up bootstrap Upbound Control Plane along with its components
* Create secrets and provider configurations
* Deploy ArgoCD

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

## Configuration

Each environment folder (e.g., `state/solution-idp-non-prod/`) contains its own `Taskfile.yaml`, which defines settings specific to that environment.

You can adjust the following variables inside the environment-specific `Taskfile.yaml`:

* `AWS_CREDENTIALS_PATH` – Path to the AWS credentials file
  *(e.g., `/Users/haarchri/.aws/aws.json`)*
* `UPBOUND_ORG` – Upbound organization name *(e.g., `idpcompany`)*
* `UPBOUND_ORG_TEAM` – Team within the organization used for provisioning *(e.g., `CI`)*
* `SPACE` – Upbound Space where control planes will be deployed *(e.g., `space-the-final-frontier`)*
* `ROOT_GROUP_NAME` – The logical group name for this IDP environment *(e.g., `solution-idp-non-prod`)*
* `ROOT_CTP_NAME` – The name of the root Control Plane used to bootstrap *(e.g., `bootstrap`)*
* `GIT_REPO` – Git repository URL for sourcing manifests *(e.g., `https://github.com/upbound/solution-idp.git`)*
* `GIT_REVISION` – Git branch or revision to pull manifests from *(e.g., `main`)*

These values can differ per environment, allowing for tailored configurations between environments like `non-prod` and `prod`.

---

## Common Issues

* **Missing `up` binary?** Run `task check-up` to install
* **Wrong org/space?** Verify using `up profile current`

---

## Further Resources

* [Upbound Documentation](https://docs.upbound.io/)
* [Crossplane.io](https://crossplane.io/)
* [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)
* [Taskfile.dev](https://taskfile.dev/)

---

Enjoy building your IDP with Upbound Control Planes 🚀
