Objective:

Investigate a suspicious email reported by a user to determine whether it is malicious, identify IOCs , map attacker techniques to MITRE ATT&CK and produce an incident report with remedition recommendations.

  Scenario:

  A finance employee reports receiving an email claiming that their Microsoft 365 password will expire unless they cick a link.
  
  The employee has not clicked anything yet because they found it suspicious. 
  
  They are unsure whether it is legitimate and forwards it to the SOC for investigation.

  Steps:

  
  1. User reports suspicious email
  
  2. Collect the email (.eml)
  
  3. Analyze the headers

  4. Check SPF (Pass), DKIM (Pass) and DMARC (Pass)

  5. Inspect sender information

  6. Extract URLS

  7. Check the URL (Virus Total, URLScan, URL2PNG (captures a screenshot) and WHOIS lookup)

  8. Analyze attachment (file extensions, filenames, hashes, VirusTotal reputation) 

  9. Extract Indicators Of Compromise

  10. MiTRE ATT&CK Mapping

  11. Incident report

This is the email (user view) 

From: Microsoft Account Team <security@micr0soft-support.com>

To: sarah@victoriacorp.local

Subject: Immediate Action Required - Your Microsoft 365 Password Expires Today

Date:
Monday, July 6, 2026
08:44

Dear User,

Your Microsoft 365 password will expire TODAY.

Failure to verify your account within 60 minutes will result in permanent suspension of your mailbox.

Please verify immediately using the secure portal below.

Verify Account

Thank you,

Microsoft Security Team


This is the email header (extract)

Return-Path: <security@micr0soft-support.com>

From: Microsoft Account Team <security@micr0soft-support.com>

Reply-To: support@secure-login365.net

Received: from mail.secure-login365.net
(185.203.118.44)

Authentication-Results:

spf=fail

dkim=none

dmarc=fail

Message-ID:
<4829109348@secure-login365.net>


This is the embedded URL: https://secure-login365.net/microsoft/verify.php

The following are my observations:

The Sender email is micr0soft instead of microsoft indicating typosquatting as an indicator of compromise

SPF, DKIM and DMARC failed. 

Use of urgency as a social engineering tool: 

" Failure to verify within 60 minutes" and " Permanent suspension"

The message is addressed to " Dear User" not a username or a name that legitimate organizations use when communicating personalized messages.

  
<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/bc6d188c-801c-4ba8-bf7a-467e4fbc1131" />

IOCs:

Indicator type                                                       Indicator

Sender Email                                      support@micr0soft-support.com

Sender Domain                                     micr0soft-support.com

Authentication                                    SPF failed

Authentication                                    DKIM Failed

Authentication                                    DMARC Failed

URL                                               https://micr0soft-support.com/login

Subject                                           Urgent: Verify Your Microsoft Account

MITRE ATT&CK Mapping:

T1566.002 - Phishing: SpearPhishing Link - The email persuades the user to click a malicious link

T1583.001 - Acquire Infrastructure : Domains - The attacker regitred a microsoft looking domain

Incident Report:

Incident Type: Phishing Email Investigation

Severity : High

Status : Confirmed Phishing Attempt

Description:

A suspicious email impersonating Microsoft was submitted for analysis after requesting immediate account verification.

The sender domain used typosquatting to resemble Microsoft's legitimate domain.

Email authentication mechanisms ( SPF, DKIM, DMARC ) all failed, strongly indicating sender spoofing.

Altough the embedded URL was not flagged by VirusTotal at the time of analysis and URLScan was unable to retrive the destination,

the combination of technical indicators and social engineering characteristics provided sufficient evidence to classify the message as phishing.

Root Cause: The attacker attempted to deceive the receipient via sender impersonation and a spoofed Microsoft-themed domain.

Impact:

No user interaction occured

No credentials were submitted

No malware execution was observed


Recommendations:

Block the sender domain

Block the embedded URL

Educate users on typosquatting

Continue monitoring similr phishing attempts

Enable automated email filtering based on SPF/DKIM/DMARC failures















