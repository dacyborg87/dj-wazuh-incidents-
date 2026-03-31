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

- SIEM monitoring and alert triage (Wazuh)
- Timeline-based investigation and event correlation
- Windows endpoint telemetry analysis
- Sysmon integration and log validation
- PowerShell activity detection and review
- Rootcheck and false positive investigation
- Linux command-line analysis and troubleshooting
- Security event interpretation and pattern recognition
- SOC-style documentation and reporting
- Git and GitHub workflow for technical projects
  
## Lab Skills Matrix

| Lab | Focus Area | Skills Demonstrated |
|-----|------------|---------------------|
| LAB 002 | Rootcheck Investigation | File integrity monitoring, false positive validation, Linux analysis, alert interpretation |
| LAB 003 | Windows + Sysmon | Endpoint onboarding, Sysmon logging, API validation, telemetry analysis |
| LAB 006 | PowerShell Detection | Alert correlation, baseline vs spike analysis, PowerShell monitoring, SOC investigation workflow |

## Featured Work

### ✅ LAB 006 — PowerShell Suspicious Activity Detection
Simulated suspicious PowerShell behavior and analyzed how Wazuh captures baseline activity, alert spikes, and failed execution traces.

- Compared pre-activity alerts to post-execution spikes
- Correlated system errors, shell activity, and timing of events
- Demonstrated how failed PowerShell execution still generates detectable logs
- Built a SOC-style investigation with timeline-based analysis

➡️ [View Lab 006](./LAB-006-powershell-suspicious-activity)



### ✅ LAB 003 — Windows Agent + Sysmon Integration
Deployed a Windows endpoint into Wazuh and validated Sysmon telemetry and API ingestion.

- Installed and validated Wazuh Windows agent
- Integrated Sysmon for enhanced logging visibility
- Verified event ingestion through Wazuh API
- Documented endpoint onboarding and validation workflow

➡️ [View Lab 003](./incidents/LAB-003)



### ✅ LAB 002 — Rootcheck False Positive Investigation
Investigated a Wazuh Rootcheck alert involving `/usr/bin/md5sum` and determined it was a false positive.

- Analyzed file integrity alert behavior
- Validated system file paths and symlink structure
- Confirmed no compromise through investigation
- Demonstrated false positive analysis and validation

➡️ [View Lab 002](./incidents/LAB-002)

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

- All labs are performed in a personal home SOC environment using Wazuh.
- Each lab focuses on detection, investigation, and real-world security concepts.
- Writeups are structured to reflect SOC-style documentation with clear analysis and conclusions.
- This repository is continuously evolving as new labs and skills are developed.

## Planned Labs

- Custom Wazuh dashboard queries and alert hunting
- Log tuning and noise reduction techniques
- Advanced PowerShell detection scenarios
- Windows persistence and process investigation
- Multi-stage incident simulation and response workflow

## Author
DJ (dacyborg87)
