# Terraform

Status: planning scaffold, not yet a deployable configuration.

Terraform provisions the infrastructure. Ansible then configures and maintains it.

## Target environment

VMware vSphere lab. The provider is `hashicorp/vsphere`.

## Planned layout

| File | Purpose |
|---|---|
| `versions.tf` | Terraform and provider version constraints (start from `versions.tf.example`) |
| `variables.tf` | vCenter address, datacenter, cluster, datastore, network, template name |
| `main.tf` | Data sources for the template and one or more cloned lab VMs |
| `outputs.tf` | VM names and IP addresses, to feed the Ansible inventory |

## Workflow to practice

1. Define a small disposable VM cloned from an existing template.
2. Use variables and outputs instead of hard coded values.
3. Run `terraform fmt` and `terraform validate`.
4. Review the change with `terraform plan`.
5. Provision with `terraform apply`.
6. Inspect state with `terraform state list` and understand why state is sensitive.
7. Clean up with `terraform destroy`.

## Safety rules

- Never commit `*.tfstate`, `*.tfvars`, or credentials. The root `.gitignore` already excludes them.
- Only create resources you are willing to destroy.
- Always read the `terraform plan` output before applying.
