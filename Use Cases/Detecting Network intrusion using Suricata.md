# Detecting Network Intrusions Using Suricata IDS

## Objective
Detect reconnaissance and intrusion attempts originating from the Kali Linux attack machine using Suricata IDS and analyze the resulting alerts in the Wazuh Dashboard.

---

## Overview
In this use case, the Ubuntu agent is configured with Suricata IDS to monitor network traffic on machine interface. When malicious or suspicious traffic is detected, Suricata generates alerts in `eve.json`. These alerts are collected by the Wazuh agent and forwarded to the Wazuh Manager for centralized analysis.

---

## Configuration 

First of Follow the steps in [suricata.md](../Installation/Suricata.md) and install suricata. After installation and ip configuration there is still one step left that is installing rules. You can use either default suricata rules or other rulesets developed by third parties.

### Using Suricata-update
1. Default rules:-
```
sudo suricata-update
```
- this will download emerging threats open ruleset into `/var/lib/suricata/rules/` 

2. Using other rulesets
```
sudo suricata-update update-sources
sudo suricata-update list-sources
```
Reference image:- [suricata rules list](../images/suricata%20rules%20list.png)<br>
Each of the rulesets has a name that has a 'vendor' prefix, followed by a set name. be sure to check the licence(commercial, MIT..)<br>
<b>#To enable emerging threat open or osif </b>
```
sudo suricata-update enable-source et/open
sudo suricata-update
```

### Using Third party sites
```
cd /tmp
curl -LO https://rules.emergingthreats.net/open/suricata-7.0.3/emerging-all.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 640 /etc/suricata/rules/*.rules
```

### Next step
 #After installing the rules.
1. Agent VM
    - edit suricata configuration file 
    - `sudo nano /etc/suricata/suricata.yaml`
    - under 
    ```
    default-rule-path: <path to rules file>
    rule-files:
            - <rules file>
    ```
    - Reference Image: 
        - [Suricata Emerging rules](../images/suricata%20emerging%20rules.png)
        - [Suricata config file](../images/suricata%20rules%20config.png)<br>
    
    Notice: For this Lab i installed Emerging threats rules directly from the website and moved it to /etc/suricata/rules/ & used it as default rule path<br>
    - Now we need to send the suricata logs to wazuh dashboard
    - `sudo nano /var/ossec/etc/ossec.conf`
    - scroll down to localfile 
    ```
    <localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
    </localfile>
    ```
    - restart wazuh-agent `sudo systemctl restart wazuh-agent`

2. Simulation
    - Use kali machine to start an nmap scan on the agent 
    - Head back to the dashboard -> agents -> Threat hunting -> events
    - Reference Images:
        - [dashboard events](../images/suricata%20events.png)
3. Advancements
    - In this exercise the wazuh dashboards shows the scans as alerts, But there is no active reponse towards the security event
    - For such use case we enable the active reponse of wazuh to block or ignore the such malicious traffics 
    - This will be covered in [Detecting and blocking SSH brute force attacks](../Use%20Cases/SSH-brute-force-active-response.md)
    
    