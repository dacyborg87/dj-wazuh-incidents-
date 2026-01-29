# Lab 002 – Rootcheck “Trojaned File” Alert (md5sum)

## Objective
Investigate a Wazuh rootcheck alert reporting a trojaned system binary and determine whether the alert represents a true compromise or a false positive.

## Environment
- Host: wazuh-1
- OS: Linux
- Wazuh Manager: Local
- Detection Type: Rootcheck (Host-based integrity monitoring)

## Alert Summary
- Rule ID: 510
- Level: 7
- File: /usr/bin/md5sum
- Alert Type: Host-based anomaly detection (rootcheck)

## Evidence Collected
- alert_event.json
- alert_hits_raw.txt
- verification.txt

## Investigation Steps
1. Reviewed alert details from Wazuh SIEM
2. Verified file type and resolved symlink
3. Reviewed file metadata and timestamps
4. Calculated file hash
5. Analyzed rootcheck scan behavior

## Findings

- The file `/usr/bin/md5sum` is not a standalone binary.
- The file is a symbolic link pointing to `/lib/cargo/bin/coreutils/md5sum`.
- The resolved binary belongs to Rust-based coreutils installed via cargo.
- File ownership, permissions, and timestamps show no evidence of unauthorized modification.
- The detected signature is generic and does not correspond to a known malicious hash.
- Rootcheck flagged the file due to a non-standard binary path and signature mismatch.

## Conclusion

The Wazuh rootcheck alert indicating a trojaned version of `/usr/bin/md5sum` was determined to be a false positive. The alert was triggered due to the system using Rust-based coreutils installed via cargo, which differs from the standard GNU coreutils expected by rootcheck. No indicators of compromise were identified.

## Remediation

To reduce false positives while maintaining system integrity monitoring, the affected file paths were explicitly excluded from rootcheck monitoring. This tuning ensures continued alert fidelity without suppressing unrelated integrity checks.

Ignored paths:
- `/usr/bin/md5sum`
- `/lib/cargo/bin/coreutils/md5sum`

