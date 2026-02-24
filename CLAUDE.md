# Homelab Platform Engineering Project

## Overview
This is a platform engineering homelab for learning and portfolio development.
Target role: Senior/Staff Platform Engineer at remote-first infrastructure companies.
The entire project should reflect production-grade patterns and conventions.

## Architecture
- Hypervisor: KVM on RHEL/AlmaLinux 10
- Kubernetes: kubeadm cluster (3 control plane, 3 worker nodes)
- GitOps: ArgoCD with app-of-apps pattern
- CNI: Cilium
- Observability: Prometheus + Grafana + Loki (kube-prometheus-stack)
- Secrets: HashiCorp Vault + External Secrets Operator
- Policy: Kyverno
- Registry: Harbor with Trivy scanning
- IDP: Backstage

## Tech Stack Conventions
- Infrastructure as Code: Terraform with HCL
- Configuration Management: Ansible (minimal — base OS only)
- Programming Language: Go (all custom tooling)
- Scripting: Bash for glue only, prefer Go for anything substantial
- Container Images: Distroless or Alpine base, multi-stage builds
- Secrets: Never in Git. Use Vault or SOPS.

## Go Conventions
- Use Go modules
- Run `golangci-lint run` before committing
- Use `cobra` for CLI tools
- Use `client-go` for Kubernetes interaction
- Use `prometheus/client_golang` for exporters
- Tests required for all packages
- `go fmt` and `go vet` must pass

## Terraform Conventions
- Modules in `terraform/modules/`
- Environments in `terraform/environments/`
- Always use remote state
- `terraform fmt` must pass
- Variables must have descriptions
- Use `terraform-docs` for module documentation

## Kubernetes Conventions
- All manifests managed by ArgoCD (no manual kubectl apply)
- Helm charts for third-party software
- Kustomize for internal applications
- Network policies required for every namespace
- Resource limits required on all containers
- Pod security standards enforced

## Important Commands
- `terraform plan -var-file=environments/homelab/terraform.tfvars`
- `kubectl --context homelab`
- `argocd app list`
- `golangci-lint run ./...`
- `go test ./...`

## Key Directories
- `terraform/` — All infrastructure definitions
- `kubernetes/` — ArgoCD applications and Helm values
- `go-tools/` — Custom Go tooling (CLI, exporters, operators)
- `ansible/` — Minimal: base OS hardening and K8s node prep
- `policies/` — Kyverno policies
- `docs/` — Architecture decisions (ADRs) and runbooks

## Career Context
This project serves as portfolio evidence for platform engineering roles.
All code should be written as if it will be reviewed by a hiring manager at
Datadog, Cloudflare, or GitLab. Documentation, testing, and clean Git
history matter.
