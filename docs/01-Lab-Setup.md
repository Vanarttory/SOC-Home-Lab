Project 1: Lab Setup and Configuration

Objective: To set up a virtualized SOC environment with three VMs that can communicate with each other over a private network.

Steps:

Install VirtualBox on Arch Linux Virtualbox was used as the hypervisor to run all three VMs on a single host machine.

Download ISOs Ubuntu Server 24.04.4 LTS Windows 10 Eterprise LTSC 2021 Kali Linux 2026.1

Create Virtual Machines Each Virtual Machine was created via commandline using VBoxManage: Ubuntu Server:3GB RAM, 20GB disk Windows Target: 2GB RAM, 50GB disk Kali Linux : 2GB RAM, 30GB disk

Configure Networking Each VM was given two network adapters: NAT for internet access Host-only(vboxnet0) for internal lab communication

Install Security Tools Wazuh Manager on Ubuntu Server Sysmon on Windows Target Wazuh Agent on Windows Target connected to Wazuh Manager

Verify Connectivity All three VMs can ping each other over the 192.168.56.0/24 network.

Challenges:

Wazuh Dashboard installation failure due to insufficient RAM. Resolved by installing the Wazuh Manger only.
Ubuntu disk filled up during installation. Resolved by clearing Wazuh logs and queue files.
Windows firewall blocking ICMP. Resolved by sdding firewall rules.
Result:

All VMs running and communicating
Wazuh Manager active on Ubuntu
Windows-Target showing as Active agent in WAzuh
Sysmon running and logging events on Windows
