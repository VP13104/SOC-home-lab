# VM installation

1. Install Hypervisior 
([Oracle VirtualBox](https://www.oracle.com/in/virtualization/technologies/vm/downloads/virtualbox-downloads.html))

2. Download .iso file for ubuntu live server
([Ubuntu](https://ubuntu.com/download/server#how-to-install-tab-lts))

3. Set up ubuntu vms 
* In this lab i have used two network adapters 
* NAT Network - Open internet connectivity 
* Host-only - for vm communications 
("You can use any configuration, whichever suits you best")

# 💻wazuh_manager Installation
## Objective 
Deploy The wazuh manager and Dashboard on Ubuntu server virtual machine to serve as the central SIEM platform 
([wazuh documentation](https://documentation.wazuh.com/current/index.html))

curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a

