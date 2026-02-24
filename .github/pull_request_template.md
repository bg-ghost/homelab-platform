## Summary

<!--
What does this PR do? Write 1-3 sentences describing the change.
Focus on WHAT changed and WHY, not HOW.
-->

## Type of Change

- [ ] `feat` — New feature or capability
- [ ] `fix` — Bug fix or misconfiguration correction
- [ ] `security` — Security fix, hardening, or policy
- [ ] `refactor` — Code restructure, no behavior change
- [ ] `chore` — Dependencies, tooling, maintenance
- [ ] `docs` — Documentation only
- [ ] `ci` — CI/CD pipeline changes
- [ ] `breaking` — Breaking change (requires migration)

## Scope

<!-- Which part of the platform does this affect? -->
- [ ] Terraform / Infrastructure
- [ ] Kubernetes manifests
- [ ] ArgoCD / GitOps
- [ ] Go tooling
- [ ] Observability (Prometheus/Grafana/Loki)
- [ ] Security (Kyverno/Vault/Trivy)
- [ ] CI/CD pipelines
- [ ] Documentation

## Changes Made

<!-- Bullet list of concrete changes. Be specific. -->

-
-
-

## Testing

<!-- How was this tested? What would a reviewer run to verify? -->

# Example test commands
terraform plan -var-file=environments/homelab/terraform.tfvars
kubectl apply --dry-run=client -f kubernetes/
go test ./...

## Checklist

- [ ] Code follows project conventions (see CLAUDE.md)
- [ ] `terraform fmt` passes (if Terraform changed)
- [ ] `go vet ./...` and `golangci-lint run` pass (if Go changed)
- [ ] YAML is valid (if manifests changed)
- [ ] No secrets committed (pre-commit secret detection passed)
- [ ] Documentation updated if needed
- [ ] ADR added if this is an architecture decision

## Related Issues / Context

<!--
Link related issues or provide additional context.
Use: "Closes #N" to auto-close an issue.
Use: "Relates-to #N" for reference.
-->

## Screenshots / Evidence

<!-- For UI changes, policy changes, or non-obvious infrastructure changes,
show before/after or a plan output -->

<details>
<summary>Terraform Plan Output (if applicable)</summary>

paste plan output here
<details>
<summary>Test Output (if applicable)</summary>
paste test output here
</details>
