---
description: Debug a Kubernetes resource — pod, deployment, service, or node
allowed-tools: Bash(kubectl:*), Bash(stern:*), Bash(k9s:*)
---

Debug the Kubernetes resource specified in $ARGUMENTS (e.g., "pod/my-pod -n my-namespace").

Run through this diagnostic sequence:
1. `kubectl describe $ARGUMENTS` — events, conditions, resource limits
2. `kubectl get $ARGUMENTS -o yaml` — full spec and status
3. If a pod: `kubectl logs $ARGUMENTS --previous 2>/dev/null` — previous container logs
4. If a pod: `kubectl logs $ARGUMENTS` — current logs (last 100 lines)
5. Check related resources (ReplicaSet, Deployment, Service, Endpoints)
6. Check node conditions if resource pressure is suspected: `kubectl describe node <node>`

Summarize:
- Root cause (if identifiable)
- Immediate remediation steps
- Longer-term fix to prevent recurrence
