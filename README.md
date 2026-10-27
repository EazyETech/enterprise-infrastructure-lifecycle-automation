# Enterprise Infrastructure Lifecycle Automation Lab

Enterprise style infrastructure automation with Ansible, Terraform, Linux, and Windows, built and tested in a personal VMware vSphere lab.

The goal is an automated workflow that checks server health, manages configuration, supports controlled maintenance, and verifies everything afterward.

```
Terraform  ->  Infrastructure  ->  Ansible  ->  Validation  ->  Documentation
```

Terraform provisions infrastructure. Ansible configures and operates it.

## Lab environment

| Role | Hosts |
|---|---|
| Control node | `et-masternode` (RHEL, Ansible core) |
| Linux managed nodes | `servera`, `serverb`, `serverc`, `serverd` |
| Windows managed node | planned |
| Hypervisor | VMware vSphere |

## Project status

This repository is a work in progress. The table below is updated only with what has actually been run and verified.

| Area | Status |
|---|---|
| Linux baseline playbook | Written. Verification in progress |
| Linux controlled maintenance | Written. Not yet tested |
| Windows baseline and maintenance | Written. Needs a Windows lab and WinRM |
| Terraform (vSphere) | Planning scaffold only |
| Idempotency testing | Not started |
| Runbooks and change records | Templates in place |

## Repository layout

```
ansible/       playbooks, inventory, group variables, collection requirements
terraform/     provider scaffold and plan for the vSphere lab
docs/          change record template and maintenance runbooks
```

## Quick start (Linux)

```bash
cd ansible
ansible-galaxy collection install -r requirements.yml
cp inventory/hosts.yml.example inventory/hosts.yml
ansible all -m ping
ansible-playbook playbooks/linux_baseline.yml
```

## Design principles

- **Safe by default.** Maintenance playbooks only check unless you set `authorize_updates=true`. Reboots require `allow_reboot=true`.
- **One host at a time.** Maintenance uses `serial: 1` so a bad change never reaches the whole fleet.
- **Verify before and after.** Pre-checks gate the change, post-checks confirm health.
- **Idempotent.** Rerunning against a compliant system should report no unnecessary changes.
- **No secrets in Git.** Credentials live in Ansible Vault or environment variables. See `.gitignore`.

## Documentation

- [Linux maintenance runbook](docs/runbooks/linux-maintenance.md)
- [Windows maintenance runbook](docs/runbooks/windows-maintenance.md)
- [Change record template](docs/change-record-template.md)
