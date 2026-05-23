# File Integrity Monitoring (FIM) with Wazuh

## Objective
Configure Wazuh File Integrity Monitoring (FIM) to detect unauthorized file creation, modification, deletion, and permission changes on critical system files and directories.

---

## What is File Integrity Monitoring?
File Integrity Monitoring (FIM) helps in auditing sensitive files and meeting regulatory compliance requirements. Wazuh has an inbuilt FIM module that monitors file system changes to detect the creation, modification, and deletion of files.

The FIM module runs periodic scans on specific paths and monitors specific directories for changes in real time. You can set which paths to monitor in the configuration of the Wazuh agents and manager.

FIM stores the files checksums and other attributes in a local FIM database. Upon a scan, the Wazuh agent reports any changes the FIM module finds in the monitored paths to the Wazuh server. The FIM module looks for file modifications by comparing the checksums of a file to its stored checksums and attribute values. It generates an alert if it finds discrepancies.

Wazuh implements FIM through the module, which monitors:
- File content changes
- Permission changes
- Ownership changes
- Creation and deletion events
- Cryptographic hash changes (MD5, SHA1, SHA256)

`NOTE: we can use auditd to the same with some changes in features provided. But for this use we are going ahead with wazuh FIM module`

<img width="800" height="400" src="../images/FIM arch.png">

## Comparison: Wazuh FIM vs Auditd

| Feature                                        | Wazuh FIM | Auditd  |
| ---------------------------------------------- | --------- | ------- |
| Detect file create/modify/delete               | Yes       | Yes     |
| Calculate file hashes (MD5/SHA1/SHA256)        | Yes       | No      |
| Store baseline and compare changes             | Yes       | No      |
| Detect permission and ownership changes        | Yes       | Yes     |
| Native Wazuh dashboard integration             | Yes       | Partial |
| Designed specifically for integrity monitoring | Yes       | No      |
| Compliance use cases (PCI DSS, CIS)            | Excellent | Good    |

---

## Configuration 
1. Server VM
    - edit ossec.conf file to enable all logs 
    `sudo nano /var/ossec/etc/ossec.conf`
    - under "ossec_config" 
    - make sure logall is enabled <br>
    ```
    <logall>all</logall>
    <logall_json>yes</logall_json>
    ```
    [Server Ossec file](../images/ex1_(server%20nano%20ossec).png)

    - restart wazuh services<br> 
    `sudo systemctl wazuh-manager`

2. Agent VM (Ubuntu)
    - edit ossec.conf file to specify the file or directory to watch over 
    - `sudo nano /var/ossec/etc/ossec.conf`
    - under "File Integerity monitoring"
    - ensure syscheck is enabled <br>
    ```
    <syscheck>
        <disabled>no</disabled>
    ```
    - specify directory<br>
    ```
    <directories check_all="yes" report_changes="yes" realtime="yes">/root</directories>
    ```
    - use the same code to specify any directory or file you want to monitor.
    [Agent ossec file](../images/ex1_(agent%20nano%20ossec.conf).png)

    - restart wazuh agent<br>
    `sudo systemctl wazuh-agent`

3. Simulation 
    - Open the wazuh dashboard `http://<server VM ip>/`
    - login and head to the security events of the particular agent<br> 
    `Dashboard -> agents -> ubuntu agent -> security events`
    - Alredy some alerts maybe visible 
    - To Verify the FIM is activily working 
    <br><br>
    - Head back to the agent 
    - As the root directory was specified for monitoring 
    - create a random file and save it 
    <br><br>
    - Now in the dashboard you will see the alerts about file creating in the root directory 

4. Rules<br>
    - the rules triggered here are 554,550,553
    - visit 0015-ossec_rules.xml in the dashboard to know about the ruleset 
    - [Dashboard alet](../images/ex1_(dashboard%20alert).png)
    - [Rule 554](../images/ex1_(dashboard%20rule%20554).png)

5. Advancements
    - Now mainly the FIM checks for add, delete, modification and checksums etc. But what if a exploit has been added to the directory 
    - For such use case we integrate the FIM with virustotal to get alerts if the file is a know exploit 
    - this will be covered in the next use case [FIM integration with virustotal](../use-cases/virustotal-malicious-file-detection.md)
