# Network Intrusion Detection Lab - Wazuh & Suricata

##  Project Overview

This lab was designed to monitor network activity, generate security events, collect those events through Wazuh, and investigate the resulting alerts from a centralized dashboard.

The environment was built using virtual machines in VirtualBox, with Kali Linux hosting the Wazuh Manager and Ubuntu running the Wazuh Agent and Suricata.

---
## Project Objectives

The main objectives of this lab were to:

- Install and configure Suricata IDS.
- Integrate Suricata with Wazuh.
- Monitor network traffic and security events.
- Generate test network activity.
- Analyze detected events and alerts in Wazuh.
- Gain practical experience with network security monitoring and alert investigation.

---

##  Lab Architecture

The lab consists of a Kali Linux machine running the Wazuh Manager and an Ubuntu machine running the Wazuh Agent and Suricata IDS.

### Traffic and Detection Flow

```
                Network Traffic
                       │
              ┌────────────────┐
              │    Suricata    │
              │      IDS       │
              └───────┬────────┘
                      │              Security Events
              ┌────────────────┐
              │  Wazuh Agent   │
              │    Ubuntu      │
              └───────┬────────┘
                      │
              ┌────────────────┐
              │ Wazuh Manager  │
              │     Kali       │
              └───────┬────────┘
                      │
              ┌────────────────┐
              │ Wazuh Dashboard│
              │ Alert Analysis │
              └────────────────┘
```
---

##  Lab Environment 

| component     | system    |
|---------------|-----------|
| Wazuh Manager | kali linux|
| wazuh agent   | ubuntu    |
| ids           | suricata  |
| virtualization | virtualbox|

---

## STEP 1: lab  Setup

- Installed and configured suricata IDS on Ubuntu monitored host.
- configured suricata to monitor the appropriate network interface.
- Enabled and configured suricata detection rules for network traffic analysis.
- configured wazuh agent to collect suricata ``` eve.json``` events.
- validated network connectivity between attacker and victim.

---

## STEP 2: Attack Simulation

Controlled network activity was generated from the Kali Linux against the monitored Ubuntu host.

Activities performed 

- ICMP echo request  ``` ping -c 20 192.168.0.6 ```
- Nmap SYN scan    ``` nmap -sS 192.168.0.6 ```

Output: 
- suricata alerts in ``` eve.json ```
- ICMP traffic
- TCP SYN packets (the 3 way handshake is not complete)

---

## STEP 3: Alert Detection

Suricata monitored the generated traffic and produced security events.

The events were sent to the Suricata log ``` eve.json ``` file and collected by the Wazuh Agent.

The resulting events were then displayed in the Wazuh Dashboard for analysis

### Alert Analysis

The alert was analyzed using the following information:

- source IP: 192.168.0.4
- Destination IP: 192.168.0.6
- Timestamps: 2/sep/2026 [14:14:47]
- Suricata signature: ET SCAN NMAP -sS window 1024 
- Signature/Rule ID: 2018317
- Destination ports: 21, 22, 80.
- Protocol: TCP
- Alert Severity: Low

---

## Key Findings
- The source 192.168.0.4 targeted 192.168.0.6 on ports 21, 22, and 80.
- Suricata identified the traffic as an ET SCAN NMAP -sS scan.
- The SYN scan activity across multiple ports was consistent with reconnaissance and service discovery.
- Connection state analysis confirmed these were SYN only probes with no completed TCP handshake, consistent with a half open scan rather than an attempted connection.
- No authentication attempts, exploitation activity, or other suspicious follow up activity from 192.168.0.4 were observed, and no evidence of compromise was identified during the investigation window. 


---

## Skills Demonstrated
- Network traffic analysis
- Log Analysis & Event correlation
- suricata configuration and analysis
- security alert analysis

--- 

## Repository Structure
```
network-intrusion-detection-lab/
├── README.md
├── images/
│   ├── 01_setup.png
│   ├── 02_ping.png
|   ├── 03_Nmap_scan.png
│   ├── 04_suricata_ping_alerts.png
│   ├── 05_suricata_Nmap_alert.png
├── logs/
│   ├── nmap_scan_output.txt
│   └── ping_scan_output.txt
└── queries_and_rules.md
```


## Author

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Joshua_Mayowa-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/joshua-mayowa-773bb7375)

[![X](https://img.shields.io/badge/X-sudomayor-black?style=for-the-badge&logo=x)](https://x.com/sudomayor)