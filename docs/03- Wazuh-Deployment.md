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
Image Image Step 10 - Access the Dashboard- Open: https://localhost Default credentials: Username: Password:
Troubleshooting

Issue 1
-Incorrect Git Repository
Resolution : git clone https://github.com/wazuh/wazuh-docker.git

Issue 2
-Invalid ZIP Download
Resolution :Remove the invalid file and cloned the repository correctly.

Issue 3
-Missing Files
Resolution :Check out the stable v4.13.0 release which included the required deployment files.

Issue 4

Elasticsearch Kernel Requirement
Resolution: sudo sysctl -w vm.max_map_count=262144
Lessons Learned: -

Always verify repository URLs before cloning.
Read official documentation carefully.
Validate Docker installation before deploying containers.
Check kernel requirements before starting OpenSearch.
Troubleshooting is an important part of SOC engineering and should be documented.
