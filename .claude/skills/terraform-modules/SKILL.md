---
name: terraform-modules
description: Use when writing or modifying Terraform code — modules, environments,
  or provider configurations. Applies HCL conventions for this project.
allowed-tools: Read, Grep, Glob, Bash(terraform:*)
---

# Terraform Module Patterns

## Directory Layout
```
terraform/
├── modules/
│   ├── kvm-vm/          # Reusable KVM VM module
│   ├── kvm-network/     # Reusable KVM network module
│   └── aws-eks/         # EKS cluster module
└── environments/
    ├── homelab/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── outputs.tf
    │   └── terraform.tfvars
    └── aws/
        ├── main.tf
        └── terraform.tfvars
```

## Module Conventions
- Every input variable must have a `description`
- Every output must have a `description`
- No hardcoded values — everything parameterized
- `terraform fmt` must pass before commit
- `terraform validate` must pass before commit
- Use `terraform-docs` to auto-generate README tables

## Variable Naming
- `name_prefix` for resource naming
- `tags` map for all taggable resources
- Boolean flags: `enable_*` (e.g., `enable_backup`)

## Remote State
```hcl
terraform {
  backend "s3" {
    bucket         = "homelab-tfstate"
    key            = "homelab/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

## KVM VM Module Usage Pattern
```hcl
module "k8s_control_plane" {
  source     = "../../modules/kvm-vm"
  count      = 3
  name       = "k8s-cp-${count.index}"
  vcpu       = 4
  memory_mb  = 8192
  disk_gb    = 50
  network    = "k8s-net"
  cloud_init = file("${path.module}/cloud-init/cp.yaml")
}
```

## Security Conventions
- Never store secrets in .tfvars — use environment variables or Vault provider
- Tag all resources with `environment`, `managed-by = "terraform"`, `owner`
- IAM: always least privilege — no `*` actions unless unavoidable and documented
