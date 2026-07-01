Objective:

Use Sysmon to monitor and analyse key windows sysem events to detect suspicious activity.



What is Sysmon?

System Monitor (sysmon) is a Windows system service that logs detailed system activity to the Windows Event Log.
Unlike default windows logging, sysmon captures granular details invaluable for threat detection.



Events Monitored:



EventID              Name                                         What it detects
1                Process Creation                       Any new process started — catches malware, scripts, suspicious executables
3               Network Connection                      Outbound connections — catches malware phoning home, C2 traffi
6/7              Driver/Image                           Loaded DLL and driver loading — catches DLL injection, rootkits
8                CreateRemoteThread                     One process injecting into another — classic malware technique
10                ProcessAccess                         One process accessing another's memory — credential dumping (Mimikatz)
22                 DNS Query                             All DNS lookups — catches beaconing, C2 domains

Why these events matter:


Event 1 (Process Creation) is the most important. Almost every attack involves running a process. 

Sysmon captures the full command line, parent process and hash of the executable. It answers the questions "What run, when and who launched it"

Event 3 (Netwrok Connection)- it answers "What process made this connection and where did it go?".

It is critical for detecting reverse shells and Comannd and control beacons.

Event 8 (Create Remote Thread). It is a major red flag. Legitimate software rarely injects into other processes.

This is how Metasploit's Meterpreter works.

Event 10 (Process Access). If a process accesses lsass.exe memory there is almost certainly credential dumping.

Event 22 (DNS query). Malware often uses DNS for C2 communication. Seeing unusual domains here is a key detection opportunity.

How to view sysmon events:

On Windows-Target, open Eveent Viewer


Navigate to Applications -> Service logs -> Microsoft -> Windows -> Sysmon -> Operational

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/a95f010f-29de-46da-9b3c-8af3943359e0" />



Event ID 1 captured the creation of notepad.exe.

A SOC analyst would use this to track what processes are being launched, by whom and from where.

Malware often disguises itself as legitimate processes checking the file path and hash helps verify authenticity. 

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/9e7ed587-ad4e-4d35-911a-c239c85a6145" />


 Event ID 22 captured a DNS query for example.com made by PowerShell (PID 6396). 
 
In a real SOC environment, analysts monitor DNS queries to detect malware beaconing to Command & Control (C2) servers. 

Unusual or random domain names at regular intervals are a major red flag. 

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/6f927284-95d7-4ea7-8639-a14abbb6b321" />


Event ID 3 captured PowerShell making a network connection immediately after the DNS query (Event 22).

This sequence ,DNS lookup followed immediately by a network connection is classic malware behavior.

In a real incident, this would trigger an immediate investigation into what PowerShell was connecting to and why.

Events 6/7 (Driver/Image Load) and Event 10 (ProcessAccess) were configured in Sysmon but did not trigger during basic testing. These events typically appear during more advanced activities such as DLL injection or credential dumping which will be demonstrated in [05-Nmap-and-Metasploit.md](05-Nmap-and-Metsplot.md)
