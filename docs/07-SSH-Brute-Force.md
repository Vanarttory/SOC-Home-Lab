Objective:

Simulate a brute force attack against an SSH server using Hydra, then detect and analyze the attack using Wazuh SIEM.

What is a Brute Force Attack?

It is when an attacker systematically tries every possible password combination till they find the correct one. SSH brute force is one of the most common attacks on the internet-facing linux servers, automated bot scans the entire internet constantly looking for open SSH ports.

Tools Used:

Tool Purpose Machine
Hydra Password brute forcing tools Kali Linux
Wazuh Manager SIEM, detecting amd alerting on failed logins Ubuntu Server
SSH Target service being attacked Ubuntu Server

Steps:

Reconnaissance - Confirming the SSH is running on Ubuntu by running nmap -p 22 192.168.#.#
Create password list- echo "passwords list" > /tmp/passwords.txt
Launch brute force with hydra -hydra -l victoria -P /tmp/passwords.txt ssh://192.168.#.# -t 4 -V
-l victoria - indicates username to attack
-P /tmp/passwords.txt - password list to try
ssh: //192.168.#.# - target service and IP
-t 4 - 4 parallel threads
-V - verbose, shows each attempt
Monitor wazuh alerts - sudo tail -f /var/ossec/logs/alerts/alerts.log
Analyze the detection - Rule 5710- SSH authentication failed and Rule 5712 - SSHD btute force attack detected.
Note:
-The source IP
-Number of attempts made
-Check if any attemot succeeded
-Block the source IP
-Check other systems for the same IP source
-Write incident report
Key Lessons learnt:
SSH brute force is a common real world attack
SIEM tools like Wazuh detcetd patterns monst humans wouldn't
A single failed login is noise but 100 failed ones is a signal
Defenders should implement fail2ban or rate limiting to block brute force automatically

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/cfa43845-50f3-4887-b06b-4c4d8e20e0af" />
<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/ce3d26d2-8e04-4153-b091-b1554960f714" />
<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/410f7e3b-3868-44e9-a91f-718aedd02054" />



As seen there have been some trouble in the Wazuh Agent:disk space and dependency crashes. How I fixed it by switching to Wazuh Docker as documented in [03- Wazuh-Deployment](03- Wazuh-Deployment.md).
Having learnt about Virtualization last semester and having to see it in the real world is quite astonishing, Understanding the difference between a Virtual environment and a container, a Virtual environment like Oracle that can run full virtual machines like Windows but containers like Docker can only run individual applications and it cannot simulate the virtual hardware, only shares the hosts underlying operating system kernel.
