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
  
