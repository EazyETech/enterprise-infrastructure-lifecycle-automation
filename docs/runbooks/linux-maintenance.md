# Runbook: Linux Maintenance

## Purpose
Patch RHEL family servers in a controlled way, one host at a time, with verification before and after.

## Prerequisites
- SSH key access works: `ansible linux -m ping`
- Passwordless sudo for the automation user
- A snapshot of the VM in vSphere (for anything beyond a check only run)

## Procedure

1. **Baseline**
   ```
   cd ansible
   ansible-playbook playbooks/linux_baseline.yml
   ```
   Keep the JSON reports in `ansible/reports/` for comparison.

2. **Check only (safe default)**
   ```
   ansible-playbook playbooks/linux_maintenance.yml -e target_hosts=serverb
   ```
   Confirm the update count and that pre-checks pass.

3. **Authorize updates on one host**
   ```
   ansible-playbook playbooks/linux_maintenance.yml -e target_hosts=serverb -e authorize_updates=true
   ```

4. **Authorize a reboot if one is required**
   ```
   ansible-playbook playbooks/linux_maintenance.yml -e target_hosts=serverb -e authorize_updates=true -e allow_reboot=true
   ```

5. **Verify**
   Rerun the baseline playbook and compare uptime, kernel, services, and disk usage to the earlier report.

6. **Roll out to the rest of the fleet** only after the first host is healthy.

## Troubleshooting

| Symptom | Likely cause | Action |
|---|---|---|
| `UNREACHABLE` with permission denied | SSH key missing or home directory not mounted | Check `ls -ld /home/student` on the host and that the autofs and NFS mount works |
| Pre-check fails on disk space | Root filesystem too full | Free space, then rerun |
| Required service not running | Service failed or disabled | `systemctl status <service>` and `journalctl -u <service>` |
| `dnf needs-restarting` not found | Missing plugin | `dnf install dnf-utils` |
| Host does not return after reboot | Boot problem | Open the VM console in vSphere, or revert to the snapshot |

## Recovery
Revert to the pre-change vSphere snapshot, then document what failed in a change record.
