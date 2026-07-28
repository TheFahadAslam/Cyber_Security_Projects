# 🛡️ Cybersecurity & SOC Engineering Portfolio

Welcome to my cybersecurity project hub! This repository contains hands-on labs, attack simulations, SIEM triage workflows, and custom detection engineering projects built in isolated environment labs.

---

## 📂 Project Directory

| Project # | Title & Overview | Focus Area | Status | Documentation |
| :---: | :--- | :--- | :---: | :---: |
| **01** | **SOC Lab: RDP Brute-Force Detection & SIEM Engineering**<br>Simulated password spraying via Hydra against Windows 10, triaged logs in Wazuh SIEM, and authored custom detection rules mapped to MITRE ATT&CK T1110. | `SIEM` `Wazuh` `Threat Detection` | 🟢 Complete | [View Documentation](./HomeLab_SOC_with_Wazuh%20%231) |
| **02** | **Network Hardening & Firewall Configuration** *(Upcoming)*<br>Configuring segmentation, stateful inspection rules, and egress filtering across virtual networks. | `Network Security` `Firewalls` | 🟡 In Progress | *Coming Soon* |

---

## 🛠️ Global Core Tech Stack & Tools

* **SIEM & Log Management:** Wazuh, Elastic Stack (ELK), Splunk
* **Offensive Security / Attack Simulation:** Kali Linux, Nmap, Hydra, Metasploit
* **Defensive & Analysis:** Windows Event Viewer, Wireshark, Sysmon, Custom XML Rules
* **Virtualization & Networking:** VMware Workstation, VirtualBox, Isolated Virtual Networks (`VMnet`)
* **Frameworks & Methodologies:** MITRE ATT&CK, NIST SP 800-61 (Incident Handling)

---

## 📜 How This Repository Is Structured

Each project resides in its own isolated directory containing step-by-step setup guides, log telemetry analysis, custom detection code, and screenshot evidence:

```text
Cyber_Security_Projects/
│
├── README.md                          <-- (You are here) Master Portfolio Index
│
├── HomeLab_SOC_with_Wazuh #1/          <-- Project 1 Folder
│   ├── README.md                      <-- Detailed Technical Walkthrough
│   └── images/                        <-- Proof of Work Screenshots
│
└── Project-02-Upcoming-Lab/           <-- Future Project Folder
    └── README.md
