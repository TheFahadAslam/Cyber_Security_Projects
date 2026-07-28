# Cyber_Security_Projects
# Project-1: Isolated Lab for RDP Brute-Force Attack Simulation, Wazuh SIEM Triage, and Custom Rule Engineering
# 🛡️ SOC Portfolio Project: RDP Brute-Force Detection & SIEM Rule Engineering

## 📌 Executive Summary
This project demonstrates an end-to-end Cybersecurity Operations Center (SOC) workflow—ranging from network provisioning and attack simulation to real-time log analysis and custom detection engineering. 

An active Remote Desktop Protocol (RDP) password-spraying attack was executed from an isolated **Kali Linux** attacker node against a **Windows 10** victim endpoint. The resulting authentication telemetry was ingested into a **Wazuh SIEM** collector engine, analyzed for pattern anomalies (Event ID `4625`), and mapped to a custom high-severity detection rule under the **MITRE ATT&CK Framework**.

---

## 📐 Network Topology & Architecture
All virtual machines were configured within a custom isolated private switch (`VMnet2` / Host-Only) to isolate attack traffic and prevent external leaks.

| Node Role | OS / Platform | IP Address | Subnet Mask | Function |
| :--- | :--- | :--- | :--- | :--- |
| **Attacker** | Kali Linux | `192.168.10.10` | `255.255.255.0` | Reconnaissance & Brute-Force Execution |
| **Victim** | Windows 10 Enterprise | `192.168.10.20` | `255.255.255.0` | Endpoint Target & Security Log Generation |
| **SIEM Engine** | Ubuntu Server / Wazuh | `192.168.10.50` | `255.255.255.0` | Log Aggregation, Parsing, and Escalation |

---

## 🛠️ Tools & Technologies Used
* **SIEM Platform:** Wazuh 4.x (Indexer, Server, Dashboard)
* **Attack Tools:** Nmap (Service Enumeration), Hydra (RDP Brute Force)
* **Target Operating System:** Windows 10 with Wazuh Agent & Audit Logging
* **Framework Mapping:** MITRE ATT&CK Matrix (Technique T1110)
* **Custom Rule Engineering:** XML Logic in `local_rules.xml`

---

## 🚀 Phase-by-Phase Walkthrough

### Phase 1: Environment Provisioning & Reconnaissance
1. **Network Binding Verification:** Set up static networking on the Kali Linux attacker node (`192.168.10.10/24`) and confirmed lateral connectivity across the sandbox using ICMP ping tests to both the target (`192.168.10.20`) and SIEM server (`192.168.10.50`).
2. **Port & Service Enumeration:** Executed an Nmap scan targeting TCP Port 3389 to verify active RDP bindings:
   ```bash
nmap -p 3389 192.168.10.20
Result: Confirmed status 3389/tcp open ms-wbt-server.

Figure 1.1: Nmap scan output confirming open TCP port 3389 on target host 192.168.10.20.

----------------------------------------

### Phase 2: Weaponization & Brute-Force Execution
Custom Dictionary Generation: Created a targeted wordlist containing failed credential candidates alongside valid target credentials.

Automated Password Spraying: Executed Hydra against the target RDP endpoint with single-threaded verbose output (-t 1 -vV):

Bash
hydra -l fahad -P ~/Desktop/passwords.txt rdp://192.168.10.20 -vV -t 1
Result: Hydra successfully extracted valid login credentials for user fahad after cycling through failed attempts.

Figure 2.1: Hydra brute-force execution output displaying credential extraction success.

---------------------------------------------------
### Phase 3: Telemetry Analysis & SIEM Triage
Log Ingestion: Filtered incoming telemetry within the Wazuh SIEM Discover module using the query:

Plaintext
data.win.system.eventID: "4625"
Field Parsing: Extracted critical log key-value pairs to triage the security event:

Target User: data.win.eventdata.targetUserName -> fahad

Source IP: data.win.eventdata.ipAddress -> 192.168.10.10

Event Summary: Systemic spike in Windows Security Log Event ID 4625 (An account failed to log on).

Figure 3.1: Centralized security event timeline showing high-volume logon failure alerts during the attack window.

---------------------------------------------------

### Phase 4: Custom Detection Engineering & Escalation
To automatically detect and escalate recurring brute-force patterns from a single host, a custom detection rule was injected into /var/ossec/etc/rules/local_rules.xml:

XML
<group name="local,">
  <rule id="100002" level="10" frequency="5" timeframe="30">
    <if_matched_sid>60122</if_matched_sid>
    <same_source_ip />
    <description>Active RDP Brute Force Attack Detected from Host Source</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>

Rule Logic Explanation:
* id="100002": Custom user-defined rule range.
* level="10": Severity escalation from standard informational log to high-priority alert.
* frequency="5" timeframe="30": Triggers when 5 failed logons occur from the same source within 30 seconds.
* <if_matched_sid>60122</if_matched_sid>: Evaluates against baseline Windows Event ID 4625 matches.
* <mitre><id>T1110</id></mitre>: Maps the incident directly to MITRE ATT&CK T1110 (Brute Force).

--------------------------------------------------------------

### 🎯 Key SOC Analyst Takeaways
* Log Volume Management: Discovered the importance of field filtering (ipAddress, targetUserName) when dealing with massive raw event streams.

* Threat Mapping: Successfully linked network-level attack signatures to standardized threat intelligence frameworks (MITRE ATT&CK).

* Threshold Tuning: Learned how fine-tuning rule frequencies prevents false positives while ensuring rapid response times to brute-force anomalies.

