---
description: Run and summarize a Terraform plan for the homelab environment
allowed-tools: Bash(terraform:*), Read
---

Run a Terraform plan and summarize the changes:

1. Change to the appropriate environment directory based on $ARGUMENTS (default: homelab)
2. Run: `terraform plan -var-file=environments/$ARGUMENTS/terraform.tfvars -out=tfplan`
3. Summarize:
   - Resources to be created, modified, and destroyed
   - Any resources that will cause downtime (replacements)
   - Security-relevant changes (IAM, security groups, network ACLs)
   - Estimated cost impact if identifiable

Flag any plan that destroys more than 3 resources and ask for confirmation before suggesting `terraform apply`.
