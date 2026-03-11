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

# Promotion Workflow

1. Application image is built in the **DEV pipeline**
2. Image is tagged with a version
3. Image tag is updated for **STAGING deployment**
4. After validation, the image is promoted to **PROD**

---

# Why Image Promotion?

Benefits:

* Prevents environment drift
* Guarantees tested artifacts
* Improves release stability
* Enables traceable deployments

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

# Technologies

* GitHub Actions
* Docker
* Kubernetes
* ArgoCD
* Amazon EKS

---

# Release Control

Promotion requires updating image tags which triggers the GitOps deployment.

This provides controlled releases and safer production deployments.

---

