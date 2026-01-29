# LAB 003 — Windows Agent + Sysmon + API Troubleshooting (Wazuh)

## Goal
- Enroll a Windows 11 endpoint into Wazuh
- Validate baseline Windows Security logs
- Attempt Sysmon install and document Windows security blocks
- Verify Wazuh API and dashboard health

## Environment
- Wazuh Manager: wazuh-1
- Wazuh Dashboard: https://192.168.64.7
- Windows Agent: WIN-5DSHP37VL26 (Agent ID 002)
- macOS Agent: DJ-Mac (Agent ID 001)

---

## Phase 1 — Validate Windows Security Logs (Baseline)

Command:
```powershell
Get-WinEvent -LogName Security -MaxEvents 5

### Evidence

Observed recent Windows Security events confirming audit logging is active:

- Event ID 4624 — Successful logon
- Event ID 4672 — Special privileges assigned to new logon

These events were generated during normal VM usage and administrative sessions.

### Interpretation

This confirms:

- Windows Security auditing is enabled
- Authentication activity is being logged locally
- Wazuh agent is capable of collecting Windows Security telemetry

This establishes a clean baseline before introducing Sysmon.

### Screenshot

- Screenshot: Windows Security log output showing Event IDs 4624 and 4672
- Source: Apple Notes → dj-wazuh-incidents → LAB-003
