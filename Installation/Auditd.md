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

sudo augenrules --load   (loads rules)
```
using auditctl 
```
auditctl -s   -> Status of auditd
```
| enable flag                           | Description                   |
| --------------------------------------|-------------------------------|
| 0                                     |auditd is disabled             |
|1                                      |auditd is enabled              |
|2                                      |auditd is enabled but the configuration is locked and can only be changed by rebooting the machine.|



## Important Auditd File Locations

| File/Directory | Description |
|---------------|-------------|
| `/etc/audit/auditd.conf` | Main configuration file for the Auditd service |
| `/etc/audit/rules.d/` | Directory containing persistent custom audit rules |
| `/etc/audit/rules.d/audit.rules` | Custom rules file used in this lab |
| `/etc/audit/audit.rules` | Compiled rules loaded by Auditd at runtime |
| `/var/log/audit/audit.log` | Primary audit log file containing recorded events |
| `/sbin/auditctl` | Command-line utility to view and manage active audit rules |
| `/sbin/ausearch` | Utility to search and filter audit logs |
| `/sbin/aureport` | Utility to generate summarized audit reports |
| `/sbin/augenrules` | Utility to compile and load rules from `/etc/audit/rules.d/` |
| `/lib/systemd/system/auditd.service` | Systemd service definition for Auditd |


# 🔗 Links
For more info, read the following documentations <br>
[Auditd](https://linux.die.net/man/8/auditd)<br>
[Red Hat Docs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/chap-system_auditing)
