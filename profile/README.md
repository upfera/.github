# Upfera Platform — GitHub Organization Overview

This GitHub organization contains the full infrastructure and platform engineering stack for the Upfera system. It is designed as a **realm-centric, multi-layer platform architecture** that separates concerns between platform capabilities, execution mechanics, and system state.

---

## 🧠 Core Concept: The Realm

A Realm is the atomic deployment boundary of the platform. It defines everything required to operate a full system, including infrastructure, configuration context, cluster topology, and application state.

Think of a Realm as equivalent to an AWS Account or a GCP Project, but extending beyond a single cluster boundary into a full system scope spanning infrastructure, clusters, and workloads.

A Realm is purely declarative – it defines *what should exist*, not *how it is provisioned or deployed*.

---

## 🏛️ Architecture Model

The system is built around a **Realm-Centric Platform Model**:

* The platform defines reusable capabilities.
* Pipelines execute provisioning and delivery workflows.
* Realms define the desired system state.
* Kubernetes clusters reconcile and materialize that state.

```mermaid
%%{init: {"flowchart": {"diagramPadding": 50}}}%%
graph TD
  subgraph REALM_LAYER["🌍 Realm Layer"]
    subgraph REALM_REPO["📦️ <b>upfera-realm</b>"]
      INFRA["📂 <b>infrastructure/</b><br/><small>Cloud & On-Prem<br/>(Terraform)</small>"]
      INV["📂 <b>inventories/</b><br/><small>Hosts & Vars<br/>(Ansible)</small>"]
      CLUSTER_OVERLAY["📂 <b>cluster/</b><br/><small>GitOps<br/>(ArgoCD)</small>"]
    end

  end

  subgraph PLATFORM_LAYER["🏗️ Platform Layer"]
    PB["📦 <b>platform-bootstrap</b><br/><small>Ansible Collection</small>"]
    PP["📦 <b>platform-pipelines</b><br/><small>Dagger Modules</small>"]
    CB["📦 <b>cluster-base</b><br/><small>Kustomize</small>"]
  end

  subgraph RUNTIME_LAYER["⚡ Runtime Layer"]
    SUBSTRATE["🖥️ <b>System components</b>"]
    K8S["☸️ <b>Kubernetes Cluster</b>"]
    
  end

  INV ~~~ PB
  CLUSTER_OVERLAY ~~~ CB

  CLUSTER_OVERLAY -. composes from .-> CB
  INV -. provides context .-> PP
  INFRA -. provides context .-> PP
  PB -. defines provisioning<br/>workflows .-> PP

  PP -- seeds & configures --> SUBSTRATE
  PP -- installs & deploys --> K8S

  CLUSTER_OVERLAY -. reconciles desired state .-> K8S

  classDef component fill:#FAFAFA,stroke:#BDBDBD,stroke-width:1px,color:#212121,rx:8px,ry:8px;
  classDef repo fill:#FFFFFF,stroke:#9E9E9E,stroke-width:1.5px,color:#212121,rx:12px,ry:12px;
  classDef pad fill:transparent,stroke:transparent,color:transparent;

  class INFRA,INV,CLUSTER_OVERLAY,SUBSTRATE,K8S component;
  class REALM_REPO,PB,PP,CB repo;
  class REALM_PAD,PLATFORM_PAD,RUNTIME_PAD pad;

  style REALM_LAYER fill:#F5F5F5,stroke:#9E9E9E,stroke-width:2px,stroke-dasharray: 5 5,color:#424242;
  style PLATFORM_LAYER fill:#F5F5F5,stroke:#9E9E9E,stroke-width:2px,stroke-dasharray: 5 5,color:#424242;
  style RUNTIME_LAYER fill:#F5F5F5,stroke:#9E9E9E,stroke-width:2px,stroke-dasharray: 5 5,color:#424242;
```

## 📦 Architectural Repositories & Layers

The repositories in this system map directly to the architectural layers defined above.

### 🌍 Realm Layer (System State)

**\<name>-realm/** (e.g., upfera-realm) — Declarative source of truth for system state. This is the authoritative definition of what should exist.

- **infrastructure/** → Declarative cloud and on-prem infrastructure definitions managed via Terraform or OpenTofu. Represents provider-level resources such as networks, IAM, compute, storage, and load balancers. Defines what infrastructure should exist, not how it is provisioned at runtime.

- **inventories/** → Ansible inventory definitions including host groups and configuration variables. Provides topology and execution context for provisioning workflows (e.g., regions, roles, cluster membership).

- **cluster-state/** → Declarative Kubernetes cluster state managed via GitOps (e.g., ArgoCD). Continuously reconciled with the live cluster runtime.

  - **bootstrap/** → Cluster bootstrap definitions responsible for establishing GitOps control over the cluster (e.g., ArgoCD installation, base controllers, CRDs).

  - **core/** → Baseline cluster infrastructure components and platform foundations (e.g., networking, ingress, observability, storage). Shared across all workloads.

  - **services/** → Platform-managed services and shared tooling configuration (e.g., monitoring, logging, security tooling). Consumed by workloads and platform components.

  - **apps/** → Application workloads deployed into the cluster. Contains business services defined via Kubernetes manifests and Kustomize overlays.

---

### 🏗️ Platform Layer (Capabilities)

Core reusable building blocks and execution mechanics.

- **platform-pipelines/** → Centralized Dagger modules used across the platform for CI/CD and workflow execution.
- **platform-bootstrap/** → Ansible playbooks for OS provisioning, host configuration, and Kubernetes cluster bootstrap.
- **cluster-base/** → Base Kubernetes cluster manifests defining foundational runtime components (CNI, Ingress, cert-manager, storage) and shared platform services (observability, logging, monitoring, telemetry).

---

### ⚡ Runtime Layer

The runtime layer represents the live execution environment where the desired state defined in Realms is materialized and continuously reconciled.

It consists of Kubernetes clusters and all running workloads, system components, and platform services operating in real time.

- Kubernetes clusters execute and continuously reconcile the desired state defined in `cluster-state/`.
- Workloads (applications and platform services) are deployed and managed within the cluster runtime.
- Platform controllers and GitOps systems ensure convergence between desired and actual state.
- The runtime layer is ephemeral and continuously evolving, driven by declarative state definitions.

---

## 🤖 AI Agents

The platform leverages AI agents to automate development and operations:

- **Junie** → Autonomous programmer for codebase management.
- **OpsAgent** → Automated infrastructure scaling and incident response.
- **SecurityAgent** → Vulnerability monitoring and automated patching.

For more details, see [AGENTS.md](../AGENTS.md).

---

## 🤝 Contribution & Community

We use standardized templates and configurations to maintain the platform.

- **Issue Templates** → Structured reporting for bugs and features.
- **Pull Request Template** → Standardized review process.
- **Security Policy** → Guidelines for reporting vulnerabilities.

These are managed in the `.github/` directory.