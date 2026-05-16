# 💻Suricata IDS Installation 

## Objective
Install and configure Suricata IDS on the Ubuntu agent to monitor network traffic and forward intrusion alerts to the Wazuh Manager for centralized analysis.

## What is Suricata?
Suricata is a high performance Network IDS, IPS and Network Security Monitoring engine. It is open source and owned by a community-run non-profit foundation, the Open Information Security Foundation (OISF). Suricata is developed by the OISF.

### Two ways to download suricata 
1. Install from Ubuntu <br>
    - the version maintained by the ubuntu distribution
    - Very stable. 
    - May be several versions behind the latest.
    - No additional repositories are added to your system.
    - Labs where you do not need the newest version 

2. Install from OISF
    - the version maintained by Open Information security Foundation the organization behind suricata
    - provides newest version
    - Gives faster access to new features, performance improvements and rule-engine updates
    - Adds 3rd party package source to your system.
    <br><br>

### Ubuntu
```code
sudo apt update
sudo apt install suricata -y
suricata --build-info
sudo systemctl status suricata
sudo systemctl enable suricata
sudo systemctl start suricata
```

### OISF
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata jq 

suricata --build-info
sudo systemctl status suricata
sudo systemctl enable suricata
sudo systemctl start suricata
```

## Configure suricata 

* after the installation of suricata is done 
* edit the config file to ensure it monitors the interface that is required 
* `sudo nano /etc/suricata/suricata.yaml`
* Under address-groups -> HOME NET -> write the ip address of the agent vm. 
* Ensure logging is enabled. 
* Under af-packet -> specify the interface.
* image reference:
    - [suricata address-groups](../images/agent%20suricata%20yaml.png)
    - [suricata logging](../images/agent%20suricata%20yaml2.png)
    - [suricata af-packet](../images/agent%20suricata%20yaml3.png)


## Important Suricata Files and Directories

| File/Directory | Description |
|---------------|-------------|
| `/etc/suricata/suricata.yaml` | Main Suricata configuration file |
| `/var/log/suricata/eve.json` | Primary JSON log file containing alerts, DNS, HTTP, TLS, and flow events |
| `/var/log/suricata/fast.log` | Human-readable alert log |
| `/var/log/suricata/stats.log` | Performance and runtime statistics |
| `/var/log/suricata/suricata.log` | Suricata daemon log file |
| `/var/lib/suricata/rules/suricata.rules` | Downloaded detection rules used by Suricata |
| `/etc/suricata/rules/` | Directory for custom rule files |
| `/var/lib/suricata/` | State data, rule files, and supporting assets |
| `/usr/bin/suricata` | Suricata executable |
| `/usr/bin/suricata-update` | Utility to download and update rules |
| `/lib/systemd/system/suricata.service` | Systemd service definition |


# 🔗 Links
For more info, visit suricata documentation site<br>
[suricata Docs](https://docs.suricata.io/en/latest/what-is-suricata.html)
