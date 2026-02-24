---
description: Run a security audit against a namespace or the whole cluster
allowed-tools: Bash(kubectl:*), Bash(kubescape:*), Bash(trivy:*), Bash(kube-bench:*)
---

Run a security audit against $ARGUMENTS (namespace name, or "cluster" for full audit).

Checks to run:
1. Kyverno policy compliance: `kubectl get policyreport -n $ARGUMENTS -o yaml`
2. Kubescape CIS scan: `kubescape scan framework cis-v1.23 --namespace $ARGUMENTS`
3. Images in namespace: list all images, then run `trivy image <image>` for any flagged HIGH or CRITICAL
4. RBAC review: `kubectl get rolebindings,clusterrolebindings -n $ARGUMENTS -o yaml` — flag any wildcard permissions or cluster-admin bindings
5. Network policies: `kubectl get networkpolicies -n $ARGUMENTS` — flag namespaces with no policies

Produce a summary with:
- PASS / WARN / FAIL per check
- Critical findings at the top
- Remediation commands for each failure
