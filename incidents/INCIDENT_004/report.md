# INCIDENT_004 — SIEM Platform Validation & UI Outage Recovery

## Summary
Wazuh SIEM dashboard was initially inaccessible from the analyst workstation. Backend Wazuh Manager services were confirmed operational, but the web UI was not reachable using default access methods.

## Environment
- Platform: Wazuh SIEM (Manager, Dashboard, Indexer)
- VM OS: Ubuntu Linux (VM)
- Host OS: macOS (Mac)
- Hypervisor: UTM
- Access Method: HTTPS over port 443
- VM IP (observed): 192.168.64.7

## Detection
The analyst observed that the Wazuh Manager was running and processing data, but the Wazuh Dashboard UI was not loading in the browser using assumed/default endpoints.

## Investigation
Steps performed to isolate the issue:
- Verified Wazuh Manager service status (active/running)
- Verified dashboard process and enumerated listening sockets/ports
- Confirmed indexer (OpenSearch) backend was running and bound locally
- Identified correct management-plane access method via VM IP over HTTPS

Key technical observations:
- Dashboard listener: 0.0.0.0:443 (reachable externally)
- Indexer listener: 127.0.0.1:9200 (local-only, expected/secure)

## Root Cause
The dashboard was operational, but it was accessed using an incorrect endpoint/assumption (e.g., localhost or wrong port). Correct access required using the VM’s IP address over HTTPS on port 443.

## Resolution
Accessed Wazuh Dashboard successfully via:
https://192.168.64.7

Confirmed alert ingestion and SIEM functionality using the Discover view with active Wazuh alerts.

## Validation Evidence
- Screenshot of Wazuh Discover page showing alert ingestion and filtering:
  - File: artifacts/screenshots/wazuh_dashboard_discover.png

## Lessons Learned
- SIEM platforms are multi-service stacks: Manager, Dashboard, and Indexer have different roles and ports.
- Always enumerate listening ports and bindings before assuming a service is down.
- VM networking and access path (host → VM) is a common source of “UI down” symptoms.
- Local-only bindings for indexer services are often intentional security design.

## Analyst Notes
This incident documents a realistic SOC troubleshooting workflow: validate backend health, inspect ports/bindings, confirm network path, and verify UI functionality with live telemetry.
