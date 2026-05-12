# VM installation

1. Install Hypervisior 
([Oracle VirtualBox](https://www.oracle.com/in/virtualization/technologies/vm/downloads/virtualbox-downloads.html))

2. Download .iso file for ubuntu live server
([Ubuntu](https://ubuntu.com/download/server#how-to-install-tab-lts))

3. Set up ubuntu vms 
* In this lab i have used two network adapters 
* NAT Network - Open internet connectivity 
* Host-only - for vm communications<br>
("You can use any configuration, whichever suits you best")

# 💻wazuh_manager Installation
## Objective 
Deploy The wazuh manager and Dashboard on Ubuntu server virtual machine to serve as the central SIEM platform 
([wazuh documentation](https://documentation.wazuh.com/current/index.html))

1. Download and run the Wazuh installation assistant.<br>
```curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a```

Once the assistant finishes the installation, the output shows the access credentials and a message that confirms that the installation was successful.<br>

2. Access the Wazuh web interface with https://<WAZUH_DASHBOARD_IP_ADDRESS> and your credentials:<br>

Username: admin<br>
Password: <ADMIN_PASSWORD>

3. <b>Onboarding Agnets</b> <br>
The Wazuh agent is multi-platform and runs on the endpoints that you want to monitor. It communicates with the Wazuh server, sending data in near real-time through an encrypted and authenticated channel.<br>

* Log in to the Wazuh Dashboard.

* Navigate to:

```text
Agents Management → Deploy New Agent 
```
* Select the following options:<br>
Operating System: Linux<br>
Wazuh Server Address: <WAZUH_MANAGER_IP><br>
Agent Name: ubuntu-agent<br>
Copy the generated installation command and run it on the Ubuntu agent VM<br>

* Enable and start the agent service <br>
```sudo systemctl enable wazuh-agent```<br>
```sudo systemctl start wazuh-agent```<br>

* Verify the agent status:<br>
```sudo systemctl status wazuh-agent```<br>

* Return to the Wazuh Dashboard and confirm that the agent appears with a status of Active.<br>

| Path                                 | Description                   |
| ------------------------------------ | ----------------------------- |
| `/var/ossec/etc/ossec.conf`          | Main Wazuh configuration file |
| `/var/ossec/logs/ossec.log`          | Wazuh Manager logs            |
| `/var/ossec/bin/agent_control`       | Agent management utility      |
| `/var/ossec/logs/alerts/alerts.json` | Generated alerts              |

