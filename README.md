# k8s-tenant-crossplane

A production-ready Crossplane composition that provisions secure,
multi-tenant Kubernetes namespaces.

This repository provides a platform abstraction for Kubernetes tenants
using:

-   Crossplane XRD (CompositeResourceDefinition)
-   Crossplane Composition
-   Provider Kubernetes
-   Secure-by-default NetworkPolicies

------------------------------------------------------------------------

## 🎯 What This Platform Does

When a `Tenant` is created, Crossplane automatically provisions:

-   A dedicated Namespace
-   A ResourceQuota
-   A LimitRange
-   Default deny ingress policy
-   Default deny egress policy
-   DNS egress allowance (kube-system)
-   Same-namespace traffic allowance

The platform is declarative, GitOps-friendly, and production-oriented.

------------------------------------------------------------------------

## 📦 Repository Structure

    k8s-tenant-crossplane/
    ├── platform/
    │   ├── xrd/
    │   │   └── xtenant.yaml
    │   └── compositions/
    │       └── xtenant-kubernetes.yaml
    ├── claims/
    │   └── example.yaml
    ├── providers/
    │   └── provider-kubernetes.yaml
    └── README.md

------------------------------------------------------------------------

## 🧱 Architecture Overview

User applies a `Tenant` claim:

Tenant (Claim) ↓ XTenant (Composite Resource) ↓ Composition ↓ Managed
Kubernetes Objects

Crossplane handles reconciliation and lifecycle management.

------------------------------------------------------------------------

## 🔐 Secure Networking Model

Each tenant namespace is isolated by default.

### Enforced Policies

✔ Default deny ingress\
✔ Default deny egress\
✔ Allow DNS (UDP/TCP 53 to kube-system)\
✔ Allow same-namespace communication

This ensures:

-   No external communication unless explicitly allowed
-   No cross-tenant traffic
-   Controlled egress behavior

------------------------------------------------------------------------

## 🚀 Installation

### 1️⃣ Install Crossplane

    helm repo add crossplane-stable https://charts.crossplane.io/stable
    helm install crossplane crossplane-stable/crossplane   --namespace crossplane-system   --create-namespace

------------------------------------------------------------------------

### 2️⃣ Install Provider Kubernetes

    kubectl apply -f providers/provider-kubernetes.yaml

------------------------------------------------------------------------

### 3️⃣ Apply XRD & Composition

    kubectl apply -f platform/xrd/
    kubectl apply -f platform/compositions/

------------------------------------------------------------------------

### 4️⃣ Create a Tenant

    kubectl apply -f claims/example.yaml

------------------------------------------------------------------------

## 🧪 Example Tenant

``` yaml
apiVersion: platform.example.com/v1alpha1
kind: XTenant
metadata:
  name: team-devops
spec:
  namespace: team-devops
  quota:
    cpu: "2"
    memory: "4Gi"
    pods: 10
  limits:
    defaultCpu: "200m"
    defaultMemory: "256Mi"
    maxCpu: "1"
    maxMemory: "1Gi"
```

------------------------------------------------------------------------

## 📊 Why Crossplane?

This approach provides:

-   Infrastructure abstraction
-   Strong multi-tenancy boundaries
-   GitOps-native workflows
-   Clear separation between platform team and application teams

------------------------------------------------------------------------

## 🔮 Future Improvements

Potential enhancements:

-   Optional ingress policy profiles
-   Optional external egress allowlists
-   Label-based environment tiers
-   Crossplane Composition Functions
-   Multi-cluster targeting

------------------------------------------------------------------------

## 📜 License

Apache 2.0
