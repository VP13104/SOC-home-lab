# Network Intrusion Investigation

## Incident Summary

A network reconnaissance scan was simulated from a Kali Linux attacker machine against a monitored Ubuntu endpoint. Suricata IDS detected suspicious scanning activity and generated alerts which were forwarded to Wazuh SIEM for centralized monitoring and investigation.

---

## Alert Details

| Field | Value |
|---|---|
| Alert Severity | Medium |
| Detection Source | Suricata IDS |
| Source IP | Kali Attacker IP |
| Destination IP | Ubuntu Target |
| Attack Type | Network Reconnaissance |
| Protocol | TCP |
| Detection Engine | Suricata |

---

## Attack Simulation

### Tool Used
- Nmap

### Command Executed

```bash
nmap -sS -sV <TARGET-IP>
```

---

## Detection Workflow

1. Nmap initiated reconnaissance scan
2. Suricata detected scanning signatures
3. Alerts forwarded to Wazuh manager
4. Wazuh indexed and correlated events
5. Alerts visualized in dashboard

---

## Investigation Findings

The following suspicious behavior was identified:

- Multiple connection attempts across several ports
- Service enumeration activity
- Rapid SYN packet generation
- Reconnaissance behavior consistent with pre-attack scanning

Suricata successfully generated IDS alerts associated with port scanning and suspicious network probing activity.

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Reconnaissance | Active Scanning | T1595 |
| Reconnaissance | Network Service Scanning | T1046 |

---

## Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| Source IP | Attacker IP |
| Scan Type | SYN Scan |
| Tool | Nmap |
| Target Ports | Multiple |
| Protocol | TCP |

---

## Evidence Collection

### Nmap Scan Execution
(Add Screenshot)

### Suricata Alerts
(Add Screenshot)

### Wazuh Dashboard Alerts
(Add Screenshot)

### Alert Details
(Add Screenshot)

### Network Traffic Analysis
(Add Screenshot)

---

## Security Impact

Network reconnaissance is commonly used by attackers to:

- Identify open ports
- Discover exposed services
- Enumerate software versions
- Identify attack surfaces
- Prepare for exploitation attempts

Early detection of reconnaissance activity improves defensive visibility and enables proactive response measures.

---

## Recommendations

To improve network intrusion detection capabilities:

- Enable additional Suricata detection rules
- Implement firewall rate limiting
- Monitor repeated scan activity
- Correlate IDS alerts with endpoint telemetry
- Create custom detection rules for aggressive scanning patterns

