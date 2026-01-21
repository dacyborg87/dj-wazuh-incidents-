# DJ Wazuh Incidents & Labs (Home SOC Portfolio)

This repo contains **SOC-style incident writeups** and **hands-on Wazuh labs** built in my home security lab.  
Everything is documented with repeatable steps, evidence, and conclusions—like you’d do in a real SOC.

## Contents
- [Incident 001](INCIDENT_001.md)
- [Lab 002 – Rootcheck md5sum false positive](lab-002-rootcheck-md5sum/LAB-002.md)
⭐ **Featured lab:** Lab 002 documents a real-world Rootcheck false positive caused by Rust coreutils symlinks and demonstrates structured SIEM triage and validation.

## What you’ll find here

### Incidents
- Incident reports based on alerts from **Wazuh / Rootcheck / Syscheck / SIEM** sources.
- Each incident focuses on: **alert context → investigation → evidence → conclusion → remediation**.

### Labs
- Repeatable labs that validate detections, tune false positives, and document the workflow end-to-end.

---

## Repository structure


├── INCIDENT_001.md
├── lab-002-rootcheck-md5sum/
│   ├── LAB-002.md
│   ├── alert_hits.txt
│   └── screenshots/
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

➡️ Start here: [Lab 002 – Rootcheck md5sum false-positive investigation](lab-002-rootcheck-md5sum/LAB-002.md)

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
