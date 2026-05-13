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

