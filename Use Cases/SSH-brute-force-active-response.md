# Detecting and Blocking SSH Brute Force Attacks

## Objective
Detect repeated failed SSH login attempts and automatically drop the attacker's traffic using Wazuh Active Response.

---

## Overview

SSH brute force attacks attempt to gain unauthorized access by trying multiple username and password combinations. Wazuh detects repeated authentication failures and can trigger an Active Response script to temporarily drop/block the traffic from attacker's IP address.

In this lab, the Kali Linux VM launches a brute force attack against the Ubuntu agent, and Wazuh responds by adding a firewall rule to drop the attacker's traffic.

---

## Tools Used

- Wazuh SIEM
- Wazuh Active Response
- Ubuntu Agent
- Kali Linux
- Hydra
- UFW / iptables

---

## Configuration

1. Server VM
    - edit ossec.conf file <br>
    `sudo nano /var/ossec/etc/ossec.conf`
    - scroll down to "active-response" ensure this command is present.
    ```
    <command>
        <name>firewall-drop</name>
        <executable>firewall-drop</executable>
        <timeout_allowed>yes</timeout_allowed>
    </command>
    ```
    - next up we need to add the actual active response command that will be trigger when a rule is tiggered 
    ```
     <!-- active-response options here -->
     <active-response>

        <command>firewall-drop</command>
        <location>local</location>
        <rules_id>5763</rules_id>
        <timeout>120</timeout>

    </active-response>
    ```
    - restart wazuh-manager `sudo systemctl restart wazuh-manager`
    - Reference Images:
        - [Ossec.conf active response](../images/server%20active-response.png)
        - [Agent /bin active responses](../images/agent%20active-response_bin.png)
        - 


### Pointers:
- make sure the rule id matches the one in the wazuh dashboard
- 5763 matchs to ssh brute force rule 
- the timeout tag:<br>
<b>here's the main point the timeout tag specifies the amount of time the traffic will be blocked after whiich the traffic is unbloacked again </b>
- you either extend the time or remove it completely.

*NOTICE: YOU CAN VERIFY THE TYPE OF ACTIVE RESPONSES AVAILABLE IN YOUR AGENT BY VISITING /var/ossec/active-response/bin*

### Attack simulation
- use hydra to start a ssh brute force attack
```
hydra -l root -P rockyou.txt ip address ssh
```
- Head back to the dashboard -> agents -> ubuntu agent -> threat hunting -> events
- Refernce images:
    - [dashboard events](../images/ssh%20brute%20force%20events.png)
    