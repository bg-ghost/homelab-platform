---
name: helm-charts
description: Use when creating or modifying Helm charts in the helm-charts/ directory.
  Applies chart conventions, values structure, and security defaults for this project.
allowed-tools: Read, Grep, Glob, Bash(helm:*)
---

# Helm Chart Conventions

## Chart Location
All custom Helm charts live in `helm-charts/<chart-name>/`.
Third-party charts are referenced by URL in ArgoCD Applications — never vendored.

## Required Templates
Every chart must include:
- `deployment.yaml` (or `daemonset.yaml` / `statefulset.yaml`)
- `service.yaml`
- `servicemonitor.yaml` — Prometheus scraping (even if disabled by default)
- `networkpolicy.yaml` — deny-all ingress, explicit egress
- `_helpers.tpl` — standard label helpers
- `NOTES.txt` — post-install usage notes

## values.yaml Required Fields
```yaml
replicaCount: 1

image:
  repository: harbor.homelab.local/library/my-app
  pullPolicy: IfNotPresent
  tag: ""  # Defaults to .Chart.AppVersion

podSecurityContext:
  runAsNonRoot: true
  runAsUser: 1000
  fsGroup: 1000

securityContext:
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: [ALL]

resources:
  requests:
    cpu: 50m
    memory: 64Mi
  limits:
    cpu: 200m
    memory: 256Mi

serviceMonitor:
  enabled: true
  interval: 30s
  path: /metrics

networkPolicy:
  enabled: true
  ingress: []  # Define explicitly per chart
```

## Label Helpers (_helpers.tpl)
Always use standard `app.kubernetes.io/*` labels:
```
{{- define "mychart.labels" -}}
app.kubernetes.io/name: {{ include "mychart.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
app.kubernetes.io/version: {{ .Chart.AppVersion | quote }}
app.kubernetes.io/managed-by: {{ .Release.Service }}
{{- end }}
```

## Testing
- `helm lint helm-charts/<chart>/` before commit
- `helm template helm-charts/<chart>/ | kubectl apply --dry-run=client -f -`
- Write at least one chart test in `templates/tests/`
