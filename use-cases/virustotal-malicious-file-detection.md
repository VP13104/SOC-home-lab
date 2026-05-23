# Detecting Malicious Files with VirusTotal Integration

## Objective
Integrate Wazuh with VirusTotal to detect malicious or suspicious files created on the Ubuntu agent.


---

## Overview

When Wazuh File Integrity Monitoring detects a new or modified file, the file hash can be automatically submitted to VirusTotal. If the hash is known to be malicious or suspicious, Wazuh generates an alert with the detection results.

In this lab, a test file is created on the Ubuntu agent. Wazuh calculates the file hash, queries VirusTotal, and displays the verdict in the dashboard.

---

## Configuration

1. Server VM
    - Go to dashboard <br>
        rules -> manage rules -> search for local_rules.xml
    - add two new rules that will be trigged by FIM rules of create and modified file<br>
    ```
    <rule id="100002" level="7">
    <if_sid>550</if_sid>
    <field name="file">/root</field>
    <description>file modified in /root directory</description>
    </rule>
    ```
    &
    ```
    <rule id="100003" level="7">
    <if_sid>554</if_sid>
    <field name="file">/root</field>
    <description>file added to  /root directory</description>
    </rule>
    ```
    
    - Refernce Image: [local rules](../images/local%20rules.png)

    - Next edit ossec.conf
    - sudo nano /var/ossec/etc/ossec.conf
    - scroll down to integration <br>
    ```
    <integration>
        <name>virustotal</name>
        <api_key>"you get this key after register and create an account with virustotal"</api_key>
        <rule_id>100002,100003</rule_id>
        <alert_format>json</alert_format>
    </integration>
    ```
    - restart services<br>
    ```
    sudo systemctl restart wazuh-manager
    ```

### Attack Simulation 
- install a malware file on the agent machines root directory<br>
```
curl -Lo /root/eicar.com https://secure.eicar.org/eicacr.com && sudo ls -lah /root/eicar.com
```
- check dashboard 
- Refernce Image: [malware file events](../images/virustotal%20events.png)

