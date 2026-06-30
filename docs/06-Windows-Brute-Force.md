Overview:
Demonstration of the detection and investigation of brute force authentication attempts against a Window's workstation.

Goals:

To generate failed logon events
Identify relevant Windows Security Event ids
Collect evidence
Produce an incident report
Tools used:
Windows 10
Event Viewer

Relevant IDS
4625 - Failed Logon
4624 - Successful Logon
4740 - Account Lockout

Steps:

Create a test account
Perform multiple failed login attempts
Review security logs
Collect evidence
Write an incident report
Result:
Successfully detect and analyze brute force activity using Windows Security logs.

Incident Report 001- Failed Logon Investigation

Executive Summary:

Multiple failed authentication attempts were detected against a Windows 10 workstation during a controlled lab experiment . The activity generated Windows Security Event ID 4625 entries, indicating unsuccessful logon attempts against a local user account.

Objective

To detect and investigate brute-force authentication attempts using native Windows Security logs and determine the nature of the failures.

Environments:

Windows 10 VM
Event Viewer : Windows Security Logs
Local user account: Mideva

Timeline: 10:56:26 AM to 11:32:01 AM

Evidence Collected:

Event ID 4625 - Failed Logon

Timestamp: 6/22/2026 11:00:32 AM
Account Name: Mideva
Workstation : DESKTOP-DKBM2RF
Source Netwrok Address : 127.0.0.1
Logon Type: 2 (Interactive Logon)
Failure Reason: Unknown user name or bad password
Status code : 0xC0000006D
Total Failed Attempts : 18
Analysis:
The Security logs show repeated failed authentication attempts against the local account "Mideva". The source address of 127.0.0.1 and Logon Type 2 indicate that the attempts originated directly from the workstation rather than from a remote host.
The failure reason indicates that invalid credentials were supplied during authentication.
Because the activity was generated during a controlled lab exercise, the event was expected and not malicious behavior.

MITRE ATTACK MAPPING : T1110 - Brute Force

Conclusion:
Windows Security Event ID 4625 successfully recorded and preserved evidence of failed authentication activity. The investigation demonstrates how failed logon attempts can be identified , analyzed and documented using native Windows logging capabilities.

Image Image
Logon Type Meaning
2 Interactive (keyboard login at the machine)
3 Network (SMB, shared folders, remote access)
4 Batch job
5 Service account
7 Unlock workstation
10 Remote Desktop (RDP)
11 Cached credentials

Incident Report 002: Successful Logon Investigation

An account was successfully logged on.

Subject:
Security ID: SYSTEM
Account Name: DESKTOP-DKBM2RF$
Account Domain: WORKGROUP
Logon ID: 0x3E7

Logon Information:
Logon Type: 2
Restricted Admin Mode: -
Virtual Account: Yes
Elevated Token: No

Impersonation Level: Impersonation

New Logon:
Security ID: Font Driver Host\UMFD-3
Account Name: UMFD-3
Account Domain: Font Driver Host
Logon ID: 0x12CAB76
Linked Logon ID: 0x0
Network Account Name: -
Network Account Domain: -
Logon GUID: {00000000-0000-0000-0000-000000000000}

Process Information:
Process ID: 0xe0c
Process Name: C:\Windows\System32\winlogon.exe

Network Information:
Workstation Name: -
Source Network Address: -
Source Port: -

Detailed Authentication Information:
Logon Process: Advapi
Authentication Package: Negotiate
Transited Services: -
Package Name (NTLM only): -
Key Length: 0 An account was successfully logged on.

Subject:
Security ID: SYSTEM
Account Name: DESKTOP-DKBM2RF$
Account Domain: WORKGROUP
Logon ID: 0x3E7

Logon Information:
Logon Type: 2
Restricted Admin Mode: -
Virtual Account: Yes
Elevated Token: Yes

Impersonation Level: Impersonation

New Logon:
Security ID: Window Manager\DWM-3
Account Name: DWM-3
Account Domain: Window Manager
Logon ID: 0x12CAE91
Linked Logon ID: 0x12CAEAB
Network Account Name: -
Network Account Domain: -
Logon GUID: {00000000-0000-0000-0000-000000000000}

Process Information:
Process ID: 0xe0c
Process Name: C:\Windows\System32\winlogon.exe

Network Information:
Workstation Name: -
Source Network Address: -
Source Port: -

Detailed Authentication Information:
Logon Process: Advapi
Authentication Package: Negotiate
Transited Services: -
Package Name (NTLM only): -
Key Length: 0
An account was successfully logged on.

Subject:
Security ID: SYSTEM
Account Name: DESKTOP-DKBM2RF$
Account Domain: WORKGROUP
Logon ID: 0x3E7

Logon Information:
Logon Type: 2
Restricted Admin Mode: -
Virtual Account: Yes
Elevated Token: No

Impersonation Level: Impersonation

New Logon:
Security ID: Window Manager\DWM-3
Account Name: DWM-3
Account Domain: Window Manager
Logon ID: 0x12CAEAB
Linked Logon ID: 0x12CAE91
Network Account Name: -
Network Account Domain: -
Logon GUID: {00000000-0000-0000-0000-000000000000}

Process Information:
Process ID: 0xe0c
Process Name: C:\Windows\System32\winlogon.exe

Network Information:
Workstation Name: -
Source Network Address: -
Source Port: -

Detailed Authentication Information:
Logon Process: Advapi
Authentication Package: Negotiate
Transited Services: -
Package Name (NTLM only): -
Key Length: 0

These successful logons differing is a clear indication of how background operating system logs activities, the successful ones: Services, Scheduled tasks, DMW (Desktop Windows Manager), UMFD (User Mode Font Driver), System, LocalService, NetworkService.
Multiple Event ID 4624 entries were observed for Windows system accounts
(DWM and UMFD). These events are consistent with normal operating system
behavior and are not indicative of malicious activity.

Executive Summary:
A series of successful logins were detected on Windows 10 worktstation during a controlled lab exercise. The activity generated Windows Security Event ID 4624 entries, indicting successful logins.

Objective:
To investigate successful logon activity using native Windows Security logs and determine the nature of successful authentications.

Timeline: 12:24:53 PM to 12:38:35 PM

Evidence Collected:

Event ID: 4624 - Successful Logon
Workstation : DESKTOP-DKBM2RF
Source Network Address: -
Security ID: SYSTEM
Account Name: DESKTOP-DKBM2RF$
Account Domain: WORKGROUP
Logon ID: 0x3E7
Logon Type: 2

Incident 003: Account Lockout Detection and Investigation
Most Windows have this disabled by default so we will enable this to ensure maximum security:

Steps:

Check Current Lockout Policy: net accounts - lookout for: lockout threshold ,lockout duration and lockout observation window
Configure Lockout policy: net accounts /lockoutthreshold:5 , net accounts /lockoutduration:15 , net lockoutwindow:15
Meaning: 5 bad passwords, account is locked for 15 minutes
Create a test account
Trigger lockout
Investigate
Image Image Image
Evidence Collected:
Event ID 4740 - Account Lockout
Timestamp : 1:53:36 PM
Locked Account : Mideva
Security ID : DESKTOP-DKBM2RF\Mideva
Caller Computer: DESKTOP-DKMB2RF
Logon ID : 0x3EF
Event description: A user was locked out after exceeding the configured failed threshold.
Analysis: The Windows Security Log recorded Event ID 4740 after repeated failed logon attempts against the local account "Mideva". The event confirms the account lockout policy was successfully triggered and enforced.

MITRE ATTACK : T1110 - Brute Force
Conclusion:
Windows successfully enforced the configured account lockout policy and generated Event ID 4740. This investigation demonstrates how account lockout events can be identified , correlated with failed logon activity and documented as part of brute force detection workflow.

Image
Victoria | CyberSecurity | Nairobi, Kenya
