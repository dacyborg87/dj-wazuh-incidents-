![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue)
![SIEM](https://img.shields.io/badge/SIEM-Operations-purple)
![SOC](https://img.shields.io/badge/SOC-Analyst-green)
![Linux](https://img.shields.io/badge/Linux-Administration-yellow)
![MITRE](https://img.shields.io/badge/MITRE-ATT%26CK-red)
![Git](https://img.shields.io/badge/Git-Version%20Control-orange)

# DJ Wazuh Incidents & Labs (Home SOC Portfolio)

This repo contains **SOC-style incident writeups** and **hands-on Wazuh labs** built in my home security lab.  
Everything is documented with repeatable steps, evidence, and conclusions—like you’d do in a real SOC.

## Contents

- [Incident 001 – Wazuh Agent Tampering & Privileged Access](INCIDENT_001.md)
- [Incident 004 – SIEM Platform Validation & UI Outage Recovery](incidents/INCIDENT_004/report.md)
- [Lab 002 – Rootcheck md5sum false positive](incidents/INCIDENT_002/report.md)
- [Lab 003 – Windows agent + Sysmon API](incidents/INCIDENT_003/report.md)
- [Lab 005 – Brute Force Detection (Wazuh)](lab-005-brute-force-detection-wazuh/)
- [LAB 006: PowerShell Suspicious Activity Detection](./LAB-006-powershell-suspicious-activity)

## Core Skills Demonstrated

- SIEM monitoring and alert triage
- Wazuh dashboard investigation
- Timeline correlation and event analysis
- Windows endpoint telemetry validation
- Sysmon integration and log ingestion
- Rootcheck and false positive analysis
- PowerShell-related detection review
- Linux command-line investigation
- Evidence-based documentation
- Git and GitHub workflow for security reporting
  
## Lab Skills Matrix

| Lab | Focus Area | Skills Demonstrated |
|-----|------------|---------------------|
| LAB 002 | Rootcheck False Positive Investigation | Rootcheck analysis, false positive validation, file integrity monitoring, Linux path analysis, SIEM tuning |
| LAB 003 | Windows Agent + Sysmon API | Windows agent deployment, Sysmon integration, log ingestion, API validation, endpoint telemetry |
| LAB 006 | PowerShell Suspicious Activity Detection | PowerShell alert analysis, timeline correlation, baseline vs spike comparison, Windows event review, SOC-style investigation writeup |

## Featured Work

### ✅ LAB 006 — PowerShell Suspicious Activity Detection
Goal: Simulate suspicious PowerShell behavior on a Windows endpoint and analyze how Wazuh captures pre-activity, alert spikes, and failed execution traces.

Highlights:
- Compared baseline alerts to post-execution alert spikes
- Correlated Windows system errors, shell-related alerts, and timing of suspicious activity
- Documented how even failed PowerShell execution attempts still leave detectable evidence
- Built a SOC-style writeup using screenshots, event analysis, and conclusions

➡️ Start here: [LAB 006 – PowerShell Suspicious Activity Detection](./LAB-006-powershell-suspicious-activity)

### ✅ LAB 003 — Windows Agent + Sysmon API Validation
Goal: Deploy a Windows endpoint into Wazuh, validate Sysmon telemetry, and confirm log ingestion through the API.

Highlights:
- Installed and validated the Windows Wazuh agent
- Integrated Sysmon for deeper endpoint telemetry
- Confirmed event visibility through the Wazuh API
- Documented endpoint onboarding and verification workflow

➡️ View lab: [Lab 003 – Windows agent + Sysmon API](./incidents/LAB-003)

### ✅ LAB 002 — Rootcheck md5sum False Positive Investigation
Goal: Investigate a Wazuh Rootcheck alert claiming `/usr/bin/md5sum` was trojaned and determine whether it was malicious or a false positive.

Highlights:
- Parsed the alert and validated the affected file path
- Confirmed symlink behavior and identified the true cause
- Ruled out compromise through evidence-based validation
- Documented tuning value for reducing future false positives

➡️ View lab: [Lab 002 – Rootcheck md5sum false positive](./incidents/LAB-002)

**Highlights:**
- Parsed and validated Wazuh alert context (rule, decoder, fields)
- Verified file path + ownership and confirmed Rust coreutils symlink behavior
- Identified the root cause and documented evidence-based conclusions
- Captured tuning notes to reduce future false positives

















## Tools used

- Wazuh (Manager, Dashboard, Indexer)
- Rootcheck / Syscheck (FIM)
- Linux CLI (grep, sed, file, stat, sha256sum)
- Git + GitHub (documentation + version control)

## Notes

- All investigations and labs in this repository are performed in a personal home lab environment.
- Each writeup is built to reflect SOC-style documentation with repeatable steps, evidence, screenshots, and conclusions.
- The goal of this repo is to show practical detection, investigation, and analysis skills as they continue to improve over time.

## Planned Labs

- Dashboard query creation and custom alert hunting workflows
- Log pipeline tuning and alert noise reduction
- Additional PowerShell detection validation scenarios
- Windows persistence and suspicious process investigation labs
- Expanded incident-style writeups using Wazuh + Sysmon telemetry

## Author
DJ (dacyborg87)
