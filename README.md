# SOC-Home-Lab

What is is.

It is a Security Operations Center home lab built on a personal machine using Oracle VirtualBox.

A SOC is an environment where security analysts monitor, detect and respond to cyber threats in real time.

This lab simulates that environment at a small scale.

This repository documents my SOC analyst home lab built on Arch Linux using VirtualBox and Docker.

The objective is to simulate realistic attack scenarios, collect telemetry with Wazuh and Sysmon, investigate alerts, and document findings as incident reports.

Why did i build it?

To gain hands-on experience

To practice detection of attacks in real time using industry standard tools.

To build a portfolio demonstrating SOC analyst skills.

To understand how attacker thinks and how defenders respond.

Lab Architecture

                Internet
                    |
             Arch Linux Host
                    |
        ┌───────────|────────────┐
        │                        |
 Docker (Wazuh)             VirtualBox
        │                        |
        │         ┌──────────────|──────────────┐
        │         |              |              |
     Manager   Windows      Ubuntu Server      Kali

 Tools used:

 
 VirtualBox as the virtualization platform,
 
 the host machine is Arch linux
 
 Wazuh Manager for SIEM , collecting and analyzing security alerts, on dokcer
 
 Wazuh Agent - ships logs from Windows to Wazuh Manager, on Windows(Target)
 
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

   Skills Obtained :

   
 Windows Event Logging

 Sysmon

 Wazuh SIEM

 Docker

 Linux Administration

 VirtualBox Networking

 Incident Response

 Threat Detection

 MITRE ATT&CK Mapping

 SSH Analysis

 Windows Authentication

 Log Analysis

 Network Enumeration

 Metasploit

 Nmap

 
 Lessons Learnt :

 Deploying Wazuh in Docker requires understanding Docker networking.
 
Agent versions must match the manager version. 

Disk space planning is important when running several virtual machines.

VirtualBox Host-Only networking is critical for reliable communication.

Debugging often takes longer than deployment.

Documentation is just as important as implementation.

Areas yet to learn:

Active Directory

Sysmon custom rules

Sigma rules

Splunk comparison

Suricata IDS

pfSense firewall

Active Response

Malware analysis VM
 
