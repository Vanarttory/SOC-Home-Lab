# SOC-Home-Lab

What is is.

It is a Security Operations Center home lab built on a personal machine using Oracle VirtualBox. A SOC is an environment where security analysts monitor, detect and respond to cyber threats in real time. This lab simulates that environment at a small scale.

Why did i build it?

To gain hands-on experience
To practice detection of attacks in real time using industry standard tools.
To build a portfolio demonstrating SOC analyst skills.
To understand how attacker thinks and how defenders respond.

Lab Architecture

┌─────────────────────────────────────────────────────┐
│               Host Machine (Arch Linux)             │
│                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐  │
│  │ Ubuntu Server│  │Windows Target│  │   Kali    │  │
│  │192.168.56.10 │  │192.168.56.106│  │192.168.56 │  │
│  │  Wazuh SIEM  │  │Sysmon+Agent  │  │  .20      │  │
│  │   Manager    │  │              │  │ Attacker  │  │
│  └──────┬───────┘  └──────┬───────┘  └─────┬─────┘  │
│         │                 │                │        │
│         └─────────────────┴────────────────┘        │
│                     vboxnet0                        │
│                 192.168.56.0/24                     │
└─────────────────────────────────────────────────────┘

 Tools used:
 VirtualBox as the virtualization platform, the host machine is Arch linux
 Wazuh Manager for SIEM , collecting and analyzing security alerts, on Ubuntu Server
 Wazuh Agent , ships logs from Windows to Wazuh Manager, on Windows(Target)
 Sysmon , detailed Windows event logging also on the Windows
 Kali linux, the attack simulation on the Kali VM
 Nmap for Newtork reconnaissance on the Kali VM 
 
 Virtual Machines
 
1. Ubuntu Server (SIEM/Defender)

OS: Ubuntu Server 24.04 LTS
RAM: 3GB
Role: Hosts Wazuh Manager which receives and analyzes logs from all agents
IP: 192.168.56.10

2. Windows Target (Victim Machine)

OS: Windows 10 Enterprise LTSC
RAM: 2GB
Role: Simulates a corporate Windows workstation being monitored
IP: 192.168.56.106
Tools: Sysmon (event logging) + Wazuh Agent (log shipping)

3. Kali Linux (Attacker)

OS: Kali Linux 2026.1
RAM: 2GB
Role: Simulates an attacker performing reconnaissance and exploitation
IP: 192.168.56.20

Network Configuration

All VMs communicate over a host-only network (vboxnet0) on the 192.168.56.0/24 subnet. Each VM also has a NAT adapter for internet access.

Projects

   Project                                                Status
1. Lab Setup & Configuration                             Complete
2. Nmap Reconnaissance Detection                         In Progress
3. Brute Force Attack Detection                          Pending
4. Malware Detection with Sysmon                         Pending
5. Incident Response Simulation                          Pending
   
What i learned:
1. How to configure and manade virtual machines
2. How SIEM tools like Wazuh work
3. How sysmon captures detailed Windows events
4. How attackers perform nework recon
5. How defenders detect and respond to threats

   Victoria | Cybersecurity Student | Nairobi, Kenya.
 
