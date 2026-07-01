Overview:
This project documents the deployment of a Wazuh Security Information and Event Management (SIEM) platform using Docker. The objective was to build a centralized log management system capable of collecting, analyzing, and monitoring security events from Windows endpoints within a home SOC lab.
The deployment forms part of my SOC Analyst portfolio and demonstrates the process of installing, troubleshooting, and preparing an enterprise-grade SIEM solution for future detection engineering and incident response projects.

Objectives:

Deploy Wazuh using Docker
Configure the required Linux kernel parameters
Build a centralized log management platform
Connect Windows endpoints for monitoring
Prepare the lab for future attack simulations
Document the installation and troubleshooting process
Wazuh Installation Guide:

Step 1 - Install Docker-
Docker was installed on the Arch Linux host to provide containerized deployment of the Wazuh platform.

Step 2 - Enable Docker-
sudo systemctl enable docker
sudo systemctl start docker
Step 3 - Verify Docker-
sudo systemctl status docker

Step 4 - Configure vm.max_map_count-
The Wazuh Indexer (OpenSearch) requires a higher virtual memory map limit.
sudo sysctl -w vm.max_map_count=262144
To make the change persistent:-
echo "vm.max_map_count=262144" | sudo tee /etc/sysctl.d/99-wazuh.conf

Step 5 - Clone the Repository-
Initially, the incorrect GitHub URL was used. After troubleshooting, the correct repository was cloned.
git clone --branch v4.13.0 --single-branch https://github.com/wazuh/wazuh-docker.git

Step 6 - Navigate to the Deployment-
cd wazuh-docker/single-node

Step 7 - Generate Certificates-
docker compose -f generate-indexer-certs.yml run --rm generator

Step 8 - Start the Stack-
docker compose up -d

Step 9 - Verify Containers-
docker ps
Expected containers:

Wazuh Manager
Wazuh Dashboard
Wazuh Indexer
Image Image Step 10 - Access the Dashboard- Open: https://localhost Default credentials: Username:admin Password:SecretPassword

I added Windows as an agent and that was very easy, but then i encountered some challenges while adding Ubuntu Server as an agent <img width="1366" height="736" alt="Screenshot From 2026-06-30 14-54-52" src="https://github.com/user-attachments/assets/aff36f5a-f4e0-4883-9874-be0e7efc6d28" />

Troubleshooting
I have spent the past almost 6 hours trying to find what the problem was trying to register my Ubuntu sever as an agent.

   The Ubuntu Server remained in an unconnected state even though:
 
   -The service was running
   -The manager was reachable
   -The port was open
  
 The root causes were:
                
  -An incorrect VirtualBox -host-Only adapter IP configuration.
   -Duplicate registration on Wazuh manager
  -SStale authentication(client) key on the Ubuntu agent
  
   The resolution:
      -I reconfigures the Host-Only adapter from a guest to a host IP 
      -Verified connectivity using ping and nc
      -Removed the duplicate agent from the manager
      -Deleted the old client keys file from Ubuntu agent
      -Created a new agent on the manager
      -Installed the newly generated authentication key on the Ubuntu Vm
      -Restarted the Wazuh agent
          
  Verification:
    -systemctl status wazuh-agent showed the service running
    -nc -vz 192.168.#.# confirmed connectivity
    -Wazuh dashboard dislplayed:
                                  Status: Active
                                  IP: 192.168.#.#
                                  Operating System : Ubuntu 24.04.4 LTS
  <img width="1366" height="736" alt="Screenshot From 2026-07-01 11-32-05" src="https://github.com/user-attachments/assets/aab8d8e2-3342-4473-8bd5-0ca22504e485" />

During the Kali agent deployment, enrollment failed because the installed agent version (v4.14.5-rc1) was newer than the Wazuh manager (v4.13.0). Wazuh requires the manager version to be equal to or newer than the agent version, so the agent was downgraded to match the manager.
<img width="1366" height="736" alt="Screenshot From 2026-07-01 14-25-31" src="https://github.com/user-attachments/assets/99feba44-c09c-4277-b59c-5b49007c3835" />
<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/bc9b9fc1-de8c-4c0c-8c0f-055771c5faac" />

