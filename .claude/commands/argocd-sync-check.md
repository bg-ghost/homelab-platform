---
description: Check ArgoCD sync status across all applications and surface any drift
allowed-tools: Bash(argocd:*), Bash(kubectl:*)
---

Check the sync and health status of all ArgoCD applications.

1. `argocd app list` — full application list with sync/health status
2. For any app that is OutOfSync or Degraded:
   - `argocd app diff <app-name>` — show what has drifted
   - `argocd app get <app-name>` — detailed status and events
3. Check ArgoCD itself: `kubectl get pods -n argocd`
4. Check for any suspended or failed ApplicationSets

Report:
- Count of Synced / OutOfSync / Degraded / Unknown apps
- Full diff for anything OutOfSync
- Recommended action: sync, investigate, or ignore (with reason)
- Any ArgoCD component health issues
