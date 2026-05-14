# 💻 Auditd Installation 

## Objective 
Install and configure Auditd on the Ubuntu agent to monitor command execution, privileged actions, and security-relevant system events. Audit logs are collected by the Wazuh agent and forwarded to the Wazuh Manager for centralized analysis.

## What is Auditd?
auditd or Linux Audit Daemon is a user-space component of the Linux Auditing System, responsible for collecting and writing audit log file records to the disk.

### Installation
```
sudo apt update && sudo apt upgrade -y
sudo apt install auditd audispd-plugins -y
```
enable and verify status 
```
sudo systemctl enable auditd
sudo systemctl start auditd
sudo systemctl status auditd
```

| Path                                 | Description                   |
| ------------------------------------ | ----------------------------- |
| `/etc/audit/auditd.conf`          | Main auditd configuration file |
| `/etc/audit/rules.d/`          | Custom audit rules logs            |
| `/var/log/audit/audit.log`       | Audit event log utility      |

