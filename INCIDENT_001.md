# Incident 001 – Wazuh Agent Tampering & Privileged Access (Lab Validation)

**Author:** Dominic “DJ” Jones  
**Date:** 2025-12-23  
**Environment:** Home SOC Lab (Wazuh + Ubuntu + macOS)

---

## 1. Executive Summary

During routine troubleshooting and configuration of my Wazuh deployment in my home SOC lab, the SIEM generated multiple alerts indicating:

- AppArmor blocking extra capabilities for the CUPS printing service on the Wazuh manager.
- Repeated privileged `sudo` sessions by user `dacyborg` on the Wazuh manager (`wazuh-1`).
- Multiple stop/start events for the Wazuh agent on my macOS endpoint (`DJ-Mac`), mapped to MITRE ATT&CK technique **T1562.001 – Disable or Modify Tools**.

After analysis, all activity was determined to be **legitimate administrative maintenance** that I performed while tuning and restarting the agent. However, this incident **validates that the Wazuh lab correctly detects**:

- Defense-evasion–like behavior (tool tampering).
- Privileged escalation via `sudo` to root.
- Valid account usage patterns tied to powerful actions.

This report documents the alerts, AI-assisted threat classification, and my own SOC-style analysis.

---

## 2. Environment

- **SIEM / HIDS:** Wazuh v4.12
- **Manager:** `wazuh-1` (Ubuntu)
- **Agents:**
  - `wazuh-1` (Agent ID `000`, local agent on the manager)
  - `DJ-Mac` (Agent ID `001`, macOS, IP `192.168.1.231`)
- **Admin user:** `dacyborg` (UID 1000 on `wazuh-1`)

**Key log source:**

- `/var/ossec/logs/alerts/alerts.json` on the Wazuh manager.

---

## 3. Timeline of Key Events (Condensed)

> Times approximate based on `alerts.json` timestamps (CST).

- **~00:48**
  - AppArmor DENIED events on `wazuh-1`:
    - `apparmor="DENIED"` for `/usr/sbin/cupsd`
    - Attempted `capname="net_admin"` and read access to `/etc/paperspecs`
  - These are normalized under rule **52002 – Apparmor DENIED**.

- **~00:56 – 01:04**
  - Multiple PAM and sudo alerts involving user `dacyborg`:
    - `PAM: Login session opened.` (rule `5501`, MITRE `T1078 – Valid Accounts`)
    - `PAM: Login session closed.` (rule `5502`)
    - `Successful sudo to ROOT executed.` (rule `5402`, MITRE `T1548.003 – Sudo and Sudo Caching`)
  - Commands executed as root include:
    - `tail -f /var/ossec/logs/alerts/alerts.json`
    - `nano /Library/Ossec/etc/ossec.conf`

- **~01:09, 01:18, 01:42**
  - On agent `DJ-Mac` (ID `001`, IP `192.168.1.231`):
    - `Wazuh agent stopped.` (rule `506`, MITRE `T1562.001 – Disable or Modify Tools`)
    - `Wazuh agent started.` (rule `503`)
  - Each stop/start pair corresponds to me restarting the agent on macOS while fixing `ossec.conf`.

- **~01:43**
  - Additional sudo session on `wazuh-1`:
    - `dacyborg` uses sudo to run:
      - `tail -n 40 /var/ossec/logs/alerts/alerts.json`
    - Followed by PAM session close alerts.

---

## 4. Alerts and MITRE Mapping

### 4.1 AppArmor DENIED (CUPS)

- **Rule:** `52002 – Apparmor DENIED`  
- **Agent:** `wazuh-1` (ID `000`)  
- **Process:** `/usr/sbin/cupsd`  
- **Actions:**
  - Denied capability: `capname="net_admin"`
  - Denied file read: `/etc/paperspecs`

**Interpretation:**

- This indicates AppArmor is enforcing a restrictive profile for CUPS.
- No follow-on suspicious behavior observed.
- Treated as **low-risk hardening noise**, but useful to confirm that mandatory access control is functional.

---

### 4.2 Sudo Sessions and Privilege Escalation

- **Rules:**
  - `5501 – PAM: Login session opened.`
  - `5502 – PAM: Login session closed.`
  - `5402 – Successful sudo to ROOT executed.`  
- **MITRE Techniques:**
  - `T1078 – Valid Accounts`
  - `T1548.003 – Sudo and Sudo Caching`
- **User:** `dacyborg` → `root`  
- **Commands seen:**
  - `tail -f /var/ossec/logs/alerts/alerts.json`
  - `nano /Library/Ossec/etc/ossec.conf`
  - `tail -n 40 /var/ossec/logs/alerts/alerts.json`

**Interpretation:**

- In a real environment, repeated root-level sudo sessions are **highly relevant** for privilege escalation monitoring.
- In this scenario, the activity matches my known administrative maintenance:
  - Inspecting alerts.
  - Editing the agent configuration.
- Classified as **legitimate but sensitive** activity that should always be monitored and correlated with change management.

---

### 4.3 Wazuh Agent Stop/Start on DJ-Mac

- **Rules:**
  - `506 – Wazuh agent stopped.`
  - `503 – Wazuh agent started.`
- **Agent:** `DJ-Mac` (ID `001`, IP `192.168.1.231`)
- **MITRE Technique:**
  - `T1562.001 – Disable or Modify Tools` (Defense Evasion)

**Interpretation:**

- Repeated agent stop/start events are classic indicators of **tool tampering**:
  - An attacker might try to blind monitoring by disabling the agent.
- In this case, each stop/start pair matches my **intentional restarts** while editing `ossec.conf` and troubleshooting connectivity/FIM.
- Even though the root cause here is benign, this proves the lab successfully detects behavior that would be very suspicious in production.

---

## 5. AI Threat Classification (Project Wensday)

I used my AI assistant (“Project Wensday”) to classify the alerts in this batch. The classification logic grouped these events into three main clusters:

1. **AppArmor Enforcement (Low Risk)**
   - Tied to CUPS only.
   - No lateral indicators or follow-on activity.
   - Classified as hardening noise with low security impact.

2. **Privileged Access via Sudo (Medium Sensitivity, Low Risk in Context)**
   - Valid account (`dacyborg`) using sudo to gain root.
   - Commands are consistent with Wazuh administration.
   - Tagged as:
     - **T1078 – Valid Accounts**
     - **T1548.003 – Sudo and Sudo Caching**
   - Would be considered **medium risk** if unexplained, but is **low risk** with a known admin and documented maintenance.

3. **Agent Tampering Signals (Medium–High Potential, Low Risk in Context)**
   - Repeated Wazuh agent stop/start events reported from `DJ-Mac`.
   - Tagged as:
     - **T1562.001 – Disable or Modify Tools (Defense Evasion)**
   - In a real compromise, this would be a major red flag.
   - In this lab, it directly aligns with scripted restarts during testing.

**Overall AI Verdict for This Incident:**

- **Primary Theme:** Legitimate maintenance that mimics potential Defense Evasion and Privilege Escalation behaviors.
- **Overall Severity in Context:** **Low**
- **Potential Severity if Unexplained / in Production:** **Medium–High**

---

## 6. SOC-Style Analysis

From a SOC analyst’s perspective:

1. The SIEM is correctly:
   - Capturing privileged access.
   - Mapping to relevant MITRE techniques.
   - Detecting when security tooling (the Wazuh agent) is stopped/started.

2. This set of alerts alone cannot distinguish between:
   - A malicious user disabling agents and abusing sudo.
   - A legitimate admin (me) performing maintenance.

3. Context (who, when, why) is critical:
   - User `dacyborg` is a known administrator.
   - The activity matches the timeframe when I was:
     - Editing `ossec.conf`
     - Restarting the Wazuh agent
     - Tailing `alerts.json` to verify functionality.

---

## 7. Recommended Response & Hardening Actions

If this pattern appeared in a **real production environment**, I would recommend:

1. **Verify the Actor**
   - Confirm that `dacyborg` (or equivalent account) is an authorized admin.
   - Validate that they were scheduled to perform Wazuh or endpoint maintenance.

2. **Correlate with Change Management**
   - Check for a change ticket or internal request that explains:
     - Wazuh configuration edits.
     - Agent restarts on specific hosts.

3. **Alert Tuning**
   - Keep alerts for `agent stopped` and `agent started` enabled.
   - Add logic to:
     - Suppress noise during scheduled maintenance windows.
     - Escalate if agents are stopped outside those windows or remain down for too long.

4. **Logging and Audit Trail**
   - Store this incident write-up and related alerts as:
     - Evidence that monitoring is functioning.
     - A reference pattern for future, possibly malicious, tool tampering.

---

## 8. Lessons Learned & Portfolio Value

This incident validates that my home SOC lab using Wazuh can:

- Detect Defense Evasion behaviors like **tool disabling** (T1562.001).
- Track privileged access paths via `sudo` (T1548.003).
- Log and correlate valid account usage (T1078).

Even though this was just me working on my own environment, the same detection patterns would be extremely important in an enterprise SOC.

**How I will present this publicly (example):**

> *Built a home SOC using Wazuh with Ubuntu and macOS agents.  
> Simulated and analyzed Wazuh agent stop/start events mapped to MITRE T1562.001, along with privileged sudo activity mapped to T1548.003 and T1078.  
> Produced a formal incident report documenting detection, analysis, and recommended response—similar to what a Tier 1–2 SOC analyst would be expected to deliver in a real environment.*

This is **Incident 001** in my growing library of hands-on SOC and detection engineering case studies.
