---
name: kyverno-policies
description: Use when writing Kyverno ClusterPolicies or Policies for admission
  control, mutation, or generation. Applies security and compliance conventions
  for this project.
allowed-tools: Read, Grep, Glob, Bash(kubectl:*), Bash(kyverno:*)
---

# Kyverno Policy Patterns

## Policy Categories in This Project
1. **Validate** — block non-compliant resources at admission
2. **Mutate** — add defaults (labels, securityContext) automatically
3. **Generate** — create companion resources (NetworkPolicy, RoleBinding) on namespace creation

## Policy Naming
- `validate-<what>` — e.g., `validate-resource-limits`
- `mutate-<what>` — e.g., `mutate-add-default-labels`
- `generate-<what>` — e.g., `generate-default-networkpolicy`

## Standard Validate Policy Template
```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: validate-resource-limits
  annotations:
    policies.kyverno.io/title: Require Resource Limits
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      All containers must have CPU and memory limits defined.
spec:
  validationFailureAction: Enforce   # or Audit during rollout
  background: true
  rules:
    - name: check-container-limits
      match:
        any:
          - resources:
              kinds: [Pod]
      validate:
        message: "CPU and memory limits are required on all containers."
        pattern:
          spec:
            containers:
              - resources:
                  limits:
                    cpu: "?*"
                    memory: "?*"
```

## Rollout Strategy
1. Start with `validationFailureAction: Audit` — generates PolicyReport without blocking
2. Review: `kubectl get policyreport -A`
3. Fix violations in existing workloads
4. Switch to `Enforce` once violations are at zero

## Core Policies This Project Enforces
- `validate-resource-limits` — CPU/memory limits on all containers
- `validate-no-root` — `runAsNonRoot: true` required
- `validate-registry` — images must come from `harbor.homelab.local`
- `validate-network-policy` — every namespace must have a NetworkPolicy
- `validate-readonly-fs` — `readOnlyRootFilesystem: true` required
- `generate-default-networkpolicy` — deny-all NetworkPolicy on new namespaces
- `mutate-add-labels` — inject standard labels if missing
