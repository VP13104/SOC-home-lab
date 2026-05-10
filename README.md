# SOC-lab
# 📌 Overview
This repository documents the design, deployment, and testing of a Security Operations Center (SOC) lab environment Using Wazuh SIEM. The lab simulates real-world attack scenarios and demonstrates detection, monitoring, and analysis using industry-standard tools.

The primary goal of this project is to gain hands-on experience in:

* SIEM deployment and log analysis
* Intrusion Detection Systems (IDS)
* Attack simulation and detection workflows
# 💻 Wazuh
Wazuh is a free and open source security platform that unifies XDR and SIEM capabilities. It protects workloads across on-premises, virtualized, containerized, and cloud-based environments.<br>
The Wazuh Security Information and Event Management (SIEM) solution is a centralized platform for aggregating and analyzing telemetry in real time for threat detection and compliance. Wazuh collects event data from various sources like endpoints, network devices, cloud workloads, and applications for broader security coverage.
<img width="814" height="246" alt="Wazuh-logo-dark-backgroud" src="https://github.com/user-attachments/assets/39f703e9-6d92-4eb4-8bc5-fa38a8893fbe" />
# 🏗️ Architecture
<img width="800" height="600" alt="architecture" src="https://github.com/user-attachments/assets/75fd617c-24f0-45d5-b7e7-f6cd55fb83f0" />

### Lab Components
* Oracle VirtualBox
* Ubuntu Live server
* Kali Linux – Attacker machine for simulation
* Wazuh Manager – Centralized SIEM for log collection and analysis
* Wazuh Agents – Installed on monitored endpoints
* Suricata IDS – Network-based intrusion detection
### Network Design
* NAT Adapter → Internet access
* Host-Only Adapter → Internal attack simulation network

# 🔩 Setting Up lab
* Install oracle virtualbox
* create VM's : install Ubuntu using .iso file
* set network configuration
* install wazuh-manager
* onboard wazuh-agent

# 🗂️ Simulations
1. File Integrity Monitoring (FIM)
* Monitors critical system files and directories for unauthorized changes.
* Detects file creation, modification, deletion, and permission changes.
* Implemented using Wazuh File Integrity Monitoring.
2. Network Intrusion Detection with Suricata IDS
* Detects suspicious and malicious network activity.
* Suricata analyzes traffic and generates alerts in eve.json.
* Alerts are forwarded to Wazuh for correlation and investigation.
3. Vulnerability Detection
* Wazuh Vulnerability Detector identifies known CVEs affecting installed packages.
* Provides severity, CVSS scores, and remediation guidance.
4. Malicious Command Execution Detection with Auditd
* Auditd records executed commands and privileged actions.
* Wazuh analyzes audit logs to detect potentially malicious behavior such as:
* Reverse shell commands, Privilege escalation attempts, Unauthorized user management, Sensitive file access
5. SSH Brute Force Detection and Active Response
* Detects repeated failed SSH login attempts.
* Wazuh triggers an active response to block the attacker's IP address automatically.
* The block is removed after the configured timeout period.
6. Malicious File Detection with VirusTotal
* Wazuh integrates with the VirusTotal API.
* Newly created or modified files are hashed and checked against VirusTotal.
* Alerts are generated when files are identified as malicious or suspicious.
