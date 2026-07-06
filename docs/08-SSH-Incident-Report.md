
Executive Summary:



On 6th JUly 2026,a brute force authentication attack was detected against the SSH service running on

Victoria's Ubuntu Server 192.168.56.10, originating from Vanart's Kali Linux 192.168.56.20

within the isolated environment. The attack was identified in real time by Wazuh SIEM manager which generated alerts basd on repeated failed 

authentication attempts maching a custopn detection rule. A successful login was eventually observed following 8 attempts, indicating a successful compromise. 

This report documents the attack methodology, detection logic, investigative process and recommendations.


Objective:

 
 
 This exercise was to simulate real world SSH brute force attack using industry standard tools like Hydra.
 
 Validate Wazuh's ability to detect and alert on repeated authentication failures.
 
 Practise log correlation,alert triage and incident documentation as performed in a live SOC environment.

 Map observed attacker behaviour to the MITRE ATT&CK framework.



 
 Lab Environment:


 
Attacker machine - Kali Linux (192.168.56.20)

Targert Machine - Ubuntu Server (192.168.56.10)

SIEM - Wazuh Manager

Network - VirtualBox Host-Only adapter ( 192.168.56.0/24)

Attack tool - Hydra




Attack Method:



Hydra was used to perform the password list brute force attack aginst the SSH service on Ubuntu.

The attack generated 8 authentication attempts over 30 seconds.



Evidence:



<img width="1366" height="736" alt="Screenshot From 2026-07-06 11-19-31" src="https://github.com/user-attachments/assets/d40c98e8-9eb2-4891-b41a-136966477009" />

<img width="1366" height="736" alt="Screenshot From 2026-07-06 11-20-34" src="https://github.com/user-attachments/assets/76b465cc-1bff-4644-b418-e3452b6c8630" />

<img width="1366" height="736" alt="Screenshot From 2026-07-06 11-55-40" src="https://github.com/user-attachments/assets/8bd5378f-e33f-4ca8-8b40-970288181a55" />

<img width="1366" height="736" alt="Screenshot From 2026-07-06 11-55-26" src="https://github.com/user-attachments/assets/ccbb4f31-a628-4fc8-89c2-619f391bfaec" />


Investigation:


















MITRE ATT&CK Mapping:



MITRE ID: T1110

MITRE Tactic: Credential access

MITRE Technique : Brute Force

MITRE ID: T1078

MITRE Tactic: Defense Evasion, Persistence, Priviledge Escalation, Initial Access

MIRRE Technique: Valid Account



Logic explanation:











Recommendations:

1. Ensure lockout policies to automatically block the source IP after a repeated failures.

2. Enable active response in Wazuh to automatically firewall-block source IPs after threshold breaches.

3. Extend monitoring to correlate SSH brute force attempts with subsequent process execution or file access.


Conclusion:

This exercise validated that Wazuh SIEM deployment successfully detects ssh brute force activity using custom correlation rule with alerts generated on near real time and correctly mapped to the MITRE ATT&ck framework.

The exercise reinformed the log-decoder-rule-alert pipeline and demonstrated practical SOC triage steps.

Future iterations of this lab should incorporate automated reponse actions and expand detection coverage to related attack chains.

 
