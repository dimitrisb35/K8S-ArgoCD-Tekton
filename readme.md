# Tekton + ArgoCD 

## Overview

This guide demonstrates how to combine Tekton and ArgoCD to create a complete GitOps CI/CD pipeline for Kubernetes applications.

---

## Benefits of Combining Tekton + ArgoCD

### Separation of Concerns
- **Tekton**: Handles builds and tests (ephemeral artifacts)
- **ArgoCD**: Manages deployment and long-term state

### End-to-End Automation
Complete automation from code commit through testing, building, and deployment without manual intervention.

### Scalability & Cloud-Native Architecture
- Tekton tasks run as pods, scaling dynamically with cluster resources
- ArgoCD scales by managing multiple applications and clusters declaratively

---

## Architecture Comparison

### Tekton
- **Type**: Push-based CI/CD model
- **Components**: Tasks + Pipelines
- **Purpose**: Build, test, and push container images to registries

### ArgoCD
- **Type**: Pull-based GitOps model
- **Components**: Git repository + Kubernetes cluster synchronization
- **Purpose**: Declarative deployment management

### Combined Workflow

1. Tekton builds and tests your application
2. Tekton pushes the container image to a registry
3. Configuration files are stored in a Git repository
4. ArgoCD monitors the Git repository
5. ArgoCD automatically syncs changes to the Kubernetes cluster

---


## Installation

### Install Tekton

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

<img width="846" height="326" alt="image" src="https://github.com/user-attachments/assets/94b87d49-373d-4b0d-a29d-0fdac335589a" />

**Verify installation:**

<img width="568" height="92" alt="image" src="https://github.com/user-attachments/assets/5141fd8e-7f6d-494d-a15c-5fcb62dab781" />

### Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

<img width="885" height="237" alt="image" src="https://github.com/user-attachments/assets/64f8c5a2-f6ac-4d44-bb31-cda692644bdf" />

**Verify installation:**

<img width="601" height="163" alt="image" src="https://github.com/user-attachments/assets/baf26dc1-6d1a-4cac-b148-b059c04b137b" />

## Configuration

### Expose ArgoCD Server

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

### Retrieve Admin Password

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

**Default credentials:**
- Username: `admin`
- Password: (output from command above)

### Access ArgoCD UI

<img width="1881" height="380" alt="image" src="https://github.com/user-attachments/assets/d9501b7f-93d7-4032-a76b-3f16f14be112" />

### Install Tekton Dashboard

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml
```

<img width="838" height="220" alt="image" src="https://github.com/user-attachments/assets/3f895f8f-b1e5-4230-b24e-4cb8522e5d9f" />

**Access Tekton Dashboard:**

The Tekton dashboard runs on port 9097 by default.

<img width="1903" height="742" alt="image" src="https://github.com/user-attachments/assets/7d473345-7374-41c9-9d1d-5be986bfa79d" />

---

## Dashboard Comparison

### Separate UIs by Design

There is no single unified UI that natively combines both ArgoCD and Tekton. They solve different problems and maintain separate dashboards:

#### ArgoCD UI
**Focus**: GitOps deployment management
- Application state visualization
- Git synchronization status
- Drift detection between Git and cluster state
- Deployment history and rollback capabilities

#### Tekton Dashboard
**Focus**: CI/CD pipeline execution
- Pipeline and task definitions
- Pipeline run status and history
- Execution logs and debugging
- Real-time build and test feedback

---
## Workflow Summary

```
Code Commit → Tekton Pipeline → Build & Test → Push Image → Update Git
                                                                ↓
                                                         ArgoCD Sync
                                                                ↓
                                                    Deploy to Kubernetes
```

This separation of concerns ensures that:
- Build processes remain independent of deployment state
- Deployment configuration is version-controlled and auditable
- Both systems can scale and operate independently
- Teams can manage CI and CD concerns separately

---
