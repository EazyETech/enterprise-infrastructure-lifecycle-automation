# Change Record

| Field | Value |
|---|---|
| Change ID | CHG-YYYYMMDD-01 |
| Date and time | |
| Engineer | |
| Environment | Lab |
| Risk level | Low / Medium / High |

## 1. Planned change
What is being changed and why.

## 2. Systems affected
List each host and its role.

## 3. Pre-checks performed
- [ ] Baseline playbook run and report saved
- [ ] Disk space verified
- [ ] Required services confirmed running
- [ ] Snapshot or backup taken
- [ ] Rollback plan confirmed

## 4. Execution
Exact command(s) run, including any `-e` switches.

```
ansible-playbook playbooks/linux_maintenance.yml -e target_hosts=serverb
```

## 5. Results
What happened, including relevant output.

## 6. Failures (if any)
- What failed:
- How it was diagnosed:
- How it was recovered:

## 7. Post-change verification
- [ ] Required services running
- [ ] Disk usage acceptable
- [ ] Baseline playbook rerun and compared to the pre-change report
- [ ] Idempotency check: second run reports no unnecessary changes

## 8. Outcome
Success / Rolled back / Partial. Notes and follow-up actions.
