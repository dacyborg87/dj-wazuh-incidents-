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


## Incident Skills Matrix

| Incident | Core Skills Demonstrated |
|-----------|----------------------------|
| Incident 001 | Wazuh Agent Security, Privilege Escalation Analysis, Host-Based IDS, Log Analysis, Endpoint Hardening |
| Incident 004 | SIEM Troubleshooting, Service Validation, Port Enumeration, VM Networking, Dashboard/Indexer Architecture, Root Cause Analysis |

## Lab Skills Matrix

| Lab | Core Skills Demonstrated |
|-----|----------------------------|
| Lab 002 | Rootcheck Analysis, False Positive Validation, File Integrity Monitoring, Linux Path Analysis, SIEM Tuning |
| Lab 003 | Windows Agent Deployment, Sysmon Integration, Log Ingestion, API Validation, Endpoint Telemetry |


⭐ **Featured lab:** Lab 002 documents a real-world Rootcheck false positive caused by Rust coreutils symlinks and demonstrates structured SIEM triage and validation.

## What you’ll find here

### Incidents
- Incident reports based on alerts from **Wazuh / Rootcheck / Syscheck / SIEM** sources.
- Each incident focuses on: **alert context → investigation → evidence → conclusion → remediation**.

### Labs
- Repeatable labs that validate detections, tune false positives, and document the workflow end-to-end.





## Repository structure

├── INCIDENT_001.md
├── incidents/
│   ├── INCIDENT_002/
│   │   ├── report.md
│   │   └── artifacts/
│   │       └── alert_hits.txt
│   ├── INCIDENT_003/
│   │   ├── report.md
│   │   └── artifacts/
│   │       └── screenshots/
└── README.md


---

## Featured work

### ✅ Lab 002 — Rootcheck “Trojaned File” Alert (md5sum)
**Goal:** Investigate a Wazuh Rootcheck alert claiming `/usr/bin/md5sum` was trojaned and determine if it’s a true compromise or false positive.

**Highlights:**
- Captured the **structured SIEM JSON alert**
- Verified the file path and proved `/usr/bin/md5sum` was a **symlink**
- Identified the root cause: **Rust (cargo) coreutils path mismatch**
- Documented evidence + outcome and prepared tuning strategy

➡️ Start here: [Lab 002 – Rootcheck md5sum false-positive investigation](incidents/INCIDENT_002/report.md)

---

## Tools used
- Wazuh Manager / Dashboard
- Rootcheck / Syscheck
- Linux CLI (grep, sed, file, stat, sha256sum)

---

## Notes
- All investigations are performed in a **learning lab environment**.
- Screenshots are included where they strengthen evidence and readability.

## Planned labs
- Windows agent + Sysmon telemetry in Wazuh
- Log pipeline tuning and dashboard queries
- Detection rule validation and false-positive reduction

## Author
DJ (dacyborg87)
