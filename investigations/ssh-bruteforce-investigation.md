# SSH Brute Force Investigation

## Incident Summary

A brute force SSH attack was simulated from a Kali Linux attacker machine against an Ubuntu agent monitored by Wazuh SIEM.

---

## Alert Details

| Field     | Value                 |
|-----------|-----------------------|
| Severity | 10 (High) |
| Source  | 192.168.30.5 / 38620 |
| Target | Ubuntu Agent / 192.68.30.4 / root user  |
| Rule ID | 5763 |
| MITRE Technique | T1110 |
| Timestamp | May 9 2026 @23:21:02:497 |

---

## Attack Simulation

Tool Used:
- Hydra

Command:
```bash
hydra -l root -P rockyou.txt ssh://192.168.56.20
```

---

## Investigation Findings

Observed:
- Multiple failed SSH login attempts
- Repeated authentication failures from same IP
- High login frequency within short timeframe

Wazuh correlated:
- auth.log entries
- failed login events
- brute force behavior pattern

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force | T1110 |


---

## Response Actions

- Active response triggered
- Attacker IP blocked via iptables
- Block automatically removed after timeout

## Active Response

|Field |Value|
|---|---|
|Source | 192.168.30.5|
|Target |192.168.30.4|
|Active Response Rule_ID | 651|
|Rule Group |ossec, active _response|
|Rule Description | Host Blocked by firewall-drop active response |

---
## Escalation

The active response rule was configured with a timeout value, meaning the source IP remains blocked only for the duration specified in the rule configuration.<br>

After the timeout expires, the Wazuh manager triggers the corresponding unblock rule (Rule ID: 652), which removes the firewall block and restores traffic from the source IP.<br>

This introduces a potential security gap, as the attacker may continue the brute-force activity once the temporary block is lifted.<br>

Further improvements could include:
- Increasing the block duration
- Implementing repeated-offender tracking
- Adding permanent blocking for persistent attacks
- Integrating external threat intelligence or firewall automation

## Lessons Learned

This simulation demonstrated that temporary active response mechanisms reduce attack impact but may not fully prevent persistent brute-force attempts without additional correlation and escalation logic.


## Evidence

### Wazuh Alert
[Alert](../images/ssh%20brute%20force%20events.png)<br>
***The image if of the same simulation but with a different timestamp***

### Firewall Rule Verification
[iptable]()

---

## Conclusion

The simulated SSH brute force activity was successfully detected and mitigated using Wazuh active response automation.