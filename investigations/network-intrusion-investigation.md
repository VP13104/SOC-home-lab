# Network Intrusion Investigation

## Incident Summary

A network reconnaissance scan was simulated from a Kali Linux attacker machine against a monitored Ubuntu endpoint. Suricata IDS detected suspicious scanning activity and generated alerts which were forwarded to Wazuh SIEM for centralized monitoring and investigation.

---

## Alert Details

| Field             | Value             |
|---                |   ---             |
| Alert Severity    | Medium            |
| Detection Source  | Suricata IDS |
| Source IP         | 192.168.30.5 |
| Destination IP    | 192.168.30.4 |
| Attack Type       | Network Reconnaissance |
| Protocol          | UDP/TCP |
| Detection Engine  | Suricata |

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
The observed reconnaissance behavior was manually mapped to the following MITRE ATT&CK techniques during investigation:

| Tactic | Technique | ID |
|---|---|---|
| Reconnaissance | Active Scanning | T1595 |
| Reconnaissance | Network Service Scanning | T1046 |

---

## Evidence Collection

### Nmap Scan Execution
[Kali](../images/kali-nmap-scan.png)

### Wazuh Dashboard Alerts
[Alerts](../images/suricata%20events.png)

### Alert Details
[Details](../images/suricata-events-details.png)

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

