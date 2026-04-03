# Image Promotion Pipeline (DEV → STAGING → PROD)

## Overview

This repository manages **container image promotion between environments**.

Instead of rebuilding images in every environment, a tested image is **promoted across environments**.

Promotion flow:

```
DEV → STAGING → PROD
```

This guarantees that the **same tested artifact reaches production**.

---

# Purpose of This Repository

This repository exists to separate artifact release progression from:

* infrastructure provisioning
* runtime workload deployment
* cluster operations

Its responsibility is to control which container image version is allowed to move between environments.

This reflects a more realistic release engineering / deployment promotion workflow.

# Promotion Workflow

1. Application image is built in the **DEV pipeline**
2. Image is tagged with a version
3. Image tag is updated for **STAGING deployment**
4. After validation, the image is promoted to **PROD**

---

# Why Image Promotion?

## Benefits:

* Prevents environment drift
* Guarantees tested artifacts
* Improves release stability
* Enables traceable deployments
* supports safer production releases
* simplifies rollback to known-good versions

---

# Repository

Promotion repository:

https://github.com/Uprightbalance/backend-frontend--DEV_TAG-IMAGE-promote-to-staging-prod-env.git

---

# CI/CD Flow

```
Code Commit
   │
   ▼
Build Docker Image
   │
   ▼
Push Image to Registry
   │
   ▼
Deploy to DEV
   │
   ▼
Promote Image
   │
   ▼
Deploy to STAGING
   │
   ▼
Promote Image
   │
   ▼
Deploy to PROD
```

---

# How It Works

This repository manages promotion by updating the image tags used by downstream Kubernetes deployment manifests.

When a new image version is approved for the next environment:

1. The image tag is updated
2. The corresponding GitOps manifest reflects the promoted tag
3. ArgoCD detects the change
4. The environment is updated declaratively

This creates a clean separation between:

* image creation
* image approval
* environment deployment

# CI/CD Role in the Platform

This repository is part of a broader three-repository platform design:

## 1. Platform / Infrastructure Repository

Responsible for:

* AWS infrastructure
* EKS
* IAM
* networking
* RDS

## 2. GitOps Repository

Responsible for:

* Kubernetes manifests
* ArgoCD applications
* monitoring
* logging
* tracing
* backups

## 3. This Promotion Repository

Responsible for:

* image tag progression
* release control
* artifact promotion between environments

### Why this separation matters

This makes the delivery system more realistic and maintainable by separating:

* platform provisioning
* runtime deployment state
* artifact release progression

# Technologies Used

* GitHub Actions
* Docker
* Kubernetes
* ArgoCD
* Amazon EKS

---

# Release Control

Promotion requires explicitly updating image tags that correspond to the next environment.

This provides a more controlled deployment model than automatically pushing every build directly into production.

## Release Benefits
* safer environment progression
* easier validation gates
* improved deployment traceability
* clearer audit history for releases

Design Decisions & Trade-offs

This repository intentionally follows a promotion-based release strategy instead of a rebuild-per-environment strategy.

# Why this design was chosen

This approach was selected to ensure:

* the same tested image moves across environments
* deployments are reproducible
* releases are easier to audit and debug

## Benefits
* stronger release confidence
* reduced environment drift
* safer production rollout
* easier rollback to known-good versions

## Trade-offs
* requires disciplined image versioning
* promotion workflow is slightly more structured than direct auto-deploy
* release control introduces additional approval steps

These trade-offs are acceptable because they improve release safety and deployment consistency.

---

# Example Promotion Use Case

## Scenario

A backend image is built and deployed to DEV as:

backend:v1.0.7

After successful validation:

* STAGING is updated to use backend:v1.0.7
* later, PROD is updated to use backend:v1.0.7

At no point is a new image rebuilt for those environments.

## Why this matters

This ensures that production receives the exact same tested artifact that was already validated earlier in the pipeline.

# Operational Value

This repository demonstrates an important DevOps principle:

CI should build artifacts, but CD should control how those artifacts are promoted safely.

That distinction is what separates a basic pipeline from a more mature deployment workflow.

# Failure Scenarios This Design Helps Prevent

This promotion model helps reduce risk in scenarios such as:

* environment drift caused by separate builds
* untested image changes reaching production
* accidental deployment of the wrong image version
* unclear release lineage across environments

# Key Takeaway

This repository demonstrates how to manage artifact progression across environments using a build once, promote many model.

It improves deployment maturity by ensuring that:

* the same tested image moves across environments
* releases remain traceable
* production receives validated artifacts
* rollback becomes easier and safer

```md
## Related Repositories

This project is split into **three repositories** to separate platform provisioning, runtime deployment state, and artifact promotion.

- **Platform Repository**  
  Provisions the AWS infrastructure foundation using Terraform, including VPC, EKS, IAM, and RDS.
https://github.com/Uprightbalance/production-aws-cloud-native-app.git

- **GitOps Repository**  
  Manages Kubernetes workloads and operational components declaratively through ArgoCD, including monitoring, logging, tracing, and backup tooling.
https://github.com/Uprightbalance/gitops-repo.git

- **Image Promotion Repository**  
  Controls image tag promotion across **DEV → STAGING → PROD**, ensuring the same tested artifact is promoted safely across environments.
https://github.com/Uprightbalance/backend-frontend--DEV_TAG-IMAGE-promote-to-staging-prod-env.git

### Why this structure was used

This separation improves:

- maintainability
- deployment traceability
- release safety
- operational clarity
- alignment with real-world DevOps / Platform Engineering practices
```
