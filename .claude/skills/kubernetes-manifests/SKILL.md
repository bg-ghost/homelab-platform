---
name: kubernetes-manifests
description: Use when writing or reviewing Kubernetes YAML manifests — Deployments,
  Services, ConfigMaps, Kustomize overlays, or ArgoCD Application resources.
allowed-tools: Read, Grep, Glob, Bash(kubectl:*), Bash(kustomize:*)
---

# Kubernetes Manifest Conventions

## Hard Rules (enforced by Kyverno)
Every manifest that defines a Pod (directly or via Deployment/DaemonSet/etc.) must have:
- `resources.requests` and `resources.limits` set (CPU and memory)
- `securityContext.runAsNonRoot: true`
- `securityContext.readOnlyRootFilesystem: true`
- `securityContext.allowPrivilegeEscalation: false`
- Image from Harbor registry only: `harbor.homelab.local/<image>`

Every namespace must have at least one NetworkPolicy.

## ArgoCD Application Template
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/you/homelab-platform
    targetRevision: HEAD
    path: kubernetes/my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: my-app
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

## Kustomize Structure
```
kubernetes/my-app/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
└── overlays/
    ├── homelab/
    │   └── kustomization.yaml
    └── aws/
        └── kustomization.yaml
```

## Resource Sizing Defaults
| Workload Type | CPU Request | CPU Limit | Mem Request | Mem Limit |
|---|---|---|---|---|
| Small service | 50m | 200m | 64Mi | 256Mi |
| Medium service | 100m | 500m | 128Mi | 512Mi |
| Large service | 250m | 1000m | 256Mi | 1Gi |

## Naming Conventions
- Namespaces: kebab-case, descriptive (`monitoring`, `platform-security`, `argocd`)
- Labels: always include `app.kubernetes.io/name`, `app.kubernetes.io/component`, `app.kubernetes.io/managed-by`
