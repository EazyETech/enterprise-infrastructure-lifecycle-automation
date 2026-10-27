# Runbook: Windows Maintenance

## Purpose
Check a Windows Server for security and critical updates through Ansible and WinRM, and verify required services afterward.

## Prerequisites
- A Windows Server VM in the lab
- WinRM enabled over HTTPS (port 5986) with a lab certificate
- The `ansible.windows` collection installed: `ansible-galaxy collection install -r requirements.yml`
- The Windows password stored in an Ansible Vault file as `vault_windows_password`, never in plain text

## Procedure

1. **Verify connectivity**
   ```
   cd ansible
   ansible windows -m ansible.windows.win_ping --ask-vault-pass
   ```

2. **Baseline**
   ```
   ansible-playbook playbooks/windows_baseline.yml --ask-vault-pass
   ```

3. **Search for updates (default, installs nothing)**
   ```
   ansible-playbook playbooks/windows_maintenance.yml --ask-vault-pass
   ```

4. **Install updates in the lab only after step 3 looks correct**
   ```
   ansible-playbook playbooks/windows_maintenance.yml --ask-vault-pass -e authorize_updates=true
   ```
   Add `-e allow_reboot=true` if a reboot is acceptable.

5. **Verify**
   Rerun the baseline playbook and confirm services are running.

## Troubleshooting

| Symptom | Likely cause | Action |
|---|---|---|
| Connection timeout | Firewall or WinRM listener missing | Check the listener with `winrm enumerate winrm/config/listener` on the server |
| Authentication failure | Wrong account or password | Check the vaulted password and the transport setting |
| Certificate error | Self signed certificate | Lab only: `ansible_winrm_server_cert_validation: ignore` |
| Update search hangs | Windows Update service busy | Check the `wuauserv` service and retry |

## Recovery
Revert to the pre-change vSphere snapshot and record the failure in a change record.
