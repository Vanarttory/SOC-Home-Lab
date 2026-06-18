Project 1: Lab Setup and Configuration

Objective:
To set up a virtualized SOC environment with three VMs that can communicate with each other over a private network.

Steps:

1. Install VirtualBox on Arch Linux
Virtualbox was used as the hypervisor to run all three VMs on a single host machine.

2. Download ISOs
Ubuntu Server 24.04.4 LTS
Windows 10 Eterprise LTSC 2021
Kali Linux 2026.1

3. Create Virtual Machines
Each Virtual Machine was created via commandline using VBoxManage:
Ubuntu Server:3GB RAM, 20GB disk
Windows Target: 2GB RAM, 50GB disk
Kali Linux : 2GB RAM, 30GB disk

4. Configure Networking
Each VM was given two network adapters:
NAT for internet access
Host-only(vboxnet0) for internal lab communication

5. Install Security Tools
Wazuh Manager on Ubuntu Server
Sysmon on Windows Target 
Wazuh Agent on Windows Target connected to Wazuh Manager

6. Verify Connectivity
All three VMs can ping each other over the 192.168.56.0/24 network.

Challenges:
1. Wazuh Dashboard installation failure due to insufficient RAM. Resolved by installing the Wazuh Manger only.
2. Ubuntu disk filled up during installation. Resolved by clearing Wazuh logs and queue files.
3. Windows firewall blocking ICMP. Resolved by sdding firewall rules.

Result:
1. All VMs running and communicating
2. Wazuh Manager active on Ubuntu
3. Windows-Target showing as Active agent in WAzuh
4. Sysmon running and logging events on Windows
