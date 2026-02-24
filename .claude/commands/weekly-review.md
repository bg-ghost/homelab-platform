---
description: Review progress against 18-month platform engineering career plan
allowed-tools: Read, Bash(git log:*), Bash(kubectl:*), Bash(terraform:*), Bash(argocd:*)
---

Review my progress this week against the platform engineering career plan.

Check:
1. What Git commits were made this week: `git log --oneline --since="1 week ago"`
2. Current state of Kubernetes cluster: `kubectl get nodes` and `kubectl get pods -A`
3. ArgoCD application sync status: `argocd app list`
4. Any new Go code: `git diff --stat HEAD~10 -- '*.go'`
5. Terraform changes: `git diff --stat HEAD~10 -- '*.tf'`

Then provide:
- Summary of what was accomplished this week
- What's next based on the monthly milestones in the plan
- Any blockers or areas that need attention
- Specific tasks for the coming week
