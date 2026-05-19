# Network Security & Traffic Analysis Reference Guide

## 📌 Core Concepts
Network Security fundamentally revolves around two primary mechanisms:
* **Authentication:** Verifying the identity of a user, process, or device.
* **Authorisation:** Granting or denying specific access rights to authenticated identities.

To maintain continuity, reliability, and robust security management, operations are executed across three distinct **Base Control Levels**:

```
┌─────────────────────────────────────────────────────────────────┐
│                NETWORK SECURITY CONTROL LEVELS                  │
├─────────────────┬────────────────────────┬──────────────────────┤
│    PHYSICAL     │       TECHNICAL        │    ADMINISTRATIVE    │
├─────────────────┼────────────────────────┼──────────────────────┤
│ Prevents        │ Prevents unauthorized  │ Ensures consistency  │
│ unauthorized    │ access to network data │ via policies, access │
│ physical access │ via tunnels, encryption│ levels, and defined  │
│ to hardware,    │ layers, and digital    │ authentication       │
│ cabling, and    │ protocol controls.     │ processes.           │
│ facility locks. │                        │                      │
└─────────────────┴────────────────────────┴──────────────────────┘
```

---

## 🛠️ Security Approaches & Key Elements

Network security operations split into two primary operational approaches: **Access Control** (securing parameters and entry) and **Threat Control** (detecting and neutralizing active malice).

### 1. Access Control
The starting baseline of network security. Ensures compliance, structural isolation, and identity management before and during network attachment.

* **Firewall Protection:** Controls inbound and outbound traffic using predefined security rules; blocks application-layer threats and unauthorized traffic.
* **Network Access Control (NAC):** Validates device health and compliance configurations against explicit profiles before permitting network connection.
* **Identity and Access Management (IAM):** Manages asset identities and regulates user privileges across resources.
* **Load Balancing:** Distributes workloads across multiple computing resources to optimize data flow and prevent resource exhaustion.
* **Network Segmentation:** Divides networks into distinct ranges to isolate user access levels and safeguard high-value internal data assets.
* **Virtual Private Networks (VPN):** Establishes encrypted communication tunnels over public infrastructure for secure remote access.
* **Zero Trust Model:** Operates on a *"Never trust, always verify"* mindset. Minimizes default access permissions to the absolute threshold necessary for an assigned role.

### 2. Threat Control
Proactively monitors, detects, and intercepts anomalous or malicious activities using internal (trusted) and external traffic data probes.

* **IDS / IPS:** Inspects live traffic to generate alert triggers (**Intrusion Detection**) or force connection termination (**Intrusion Prevention**) upon detecting anomalies.
* **Data Loss Prevention (DLP):** Conducts deep content inspection and contextual analysis on active network lines to block unauthorized exfiltration of sensitive data.
* **Endpoint Protection:** Deploys multi-layered security controls (encryption, anti-malware, host-based DLP/IDS) directly onto client/server appliances.
* **Cloud Security:** Secures distributed online environments using dedicated counter-measures like native encryption and secure transit channels.
* **SIEM:** Aggregates logs and traffic metrics to execute centralized correlation and context analysis for vulnerability and threat detection.
* **SOAR:** Automates and orchestrates automated response playbooks between security tools, teams, and platforms within a single console.
* **Network Traffic Analysis (NTA) & NDR:** Analyzes direct traffic streams or full packet captures to isolate complex threat profiles.

---

## 📊 Operational Security Management Matrix

| Deployment | Configuration | Management | Monitoring | Maintenance |
| :--- | :--- | :--- | :--- | :--- |
| • Device installation<br>• Software deployment | • Initial configuration<br>• Automation scripts<br>• Feature implementation | • Initial access config<br>• Policy enforcement<br>• NAT & VPN rollout | • System monitoring<br>• User activity logs<br>• Active threat tracking<br>• Log/traffic capture | • Upgrades & patches<br>• Security updates<br>• Rule adjustments<br>• Licence tracking<br>• Config updates |

---

## 🤝 Managed Security Services (MSS)

When organizations lack localized technical expertise, budget, or staff headcount to manage dedicated security structures internally, operations are outsourced to **Managed Security Service Providers (MSSPs)**. MSS implementations offer cost-effective, easily integrated extensions for scaling operational security.

### Core MSS Technical Domains:
1.  **Network Penetration Testing:** Active assessment where teams simulate realistic attacker methodologies to breach security defenses.
2.  **Vulnerability Assessment:** Systematic discovery, enumeration, and analysis of exposed security flaws within the operating environment.
3.  **Incident Response:** An organized framework designed to identify, contain, mitigate, and systematically eradicate active security breaches.
4.  **Behavioral Analysis:** Baselining standard system and user activity metrics over time to identify anomalies, structural threats, and novel attack vectors.

---

## 🔍 Network Traffic Analysis (NTA)

Traffic Analysis is the process of intercepting, recording, monitoring, and interpreting network data stream patterns. It covers two vital operational vectors:
* **Operational Health:** Evaluating uptime, validating resource availability, and measuring performance metrics.
* **Security Posture:** Identifying anomalies and pinning down active malicious patterns on the network wire.

### Traffic Analysis Sub-Disciplines & Ecosystem Tools
* **Network Sniffing & Packet Analysis** (Platform: `Wireshark`)
* **Network Monitoring** (Platform: `Zeek`)
* **Intrusion Detection & Prevention** (Platform: `Snort`)
* **Network Forensics** (Platform: `NetworkMiner`)
* **Threat Hunting** (Platform: `Brim`)

### Technical Methodological Split

```
                         NETWORK TRAFFIC ANALYSIS
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼
   FLOW ANALYSIS                                        PACKET ANALYSIS
 ─────────────────                                    ───────────────────
 • Source: Device summary metrics.                     • Source: Raw network data stream.
 • Focus: High-level statistical summaries.            • Focus: Deep Packet Inspection (DPI).
 • Pro: Low compute, easy collection/analysis.         • Pro: Full forensic context & visibility.
 • Con: Missing deep packet details.                  • Con: Time & advanced skill intensive.
```

### Strategic Benefits
* **Full Network Visibility:** Absolute clarity over live network state.
* **Comprehensive Baselining:** Precise asset mapping and baseline behavior modeling for proactive delta tracking.
* **Rapid Anomaly/Threat Mitigation:** High-fidelity alerting and verification capabilities to intercept sophisticated adversaries.

### 🖋️ Current Relevance
Despite widespread encryption and native cloud migration strategies, traffic analysis remains irreplaceable. Attackers continuously alter their tactics to bypass explicit signatures, but **network data remains an unalterable source of truth**. Even when heavily encrypted, metadata anomalies, traffic volume shifts, and connection frequencies inevitably leave distinct patterns—making traffic analysis a fundamental core skill for modern Security Analysts.
---
## 🚀 Tasks Writeups
### Task 2
Q1: Which Security Control Level covers contain creating security policies?<br>
Ans: **Administrative**

Q2: Which Access Control element works with data metrics to manage data flow?<br>
Ans: **Load Balancing**

Q3: Which technology helps correlate different tool outputs and data sources?<br>
Ans: **SOAR**

### Task 3
Q1: What is the flag?<br>
Analysis the traffic and we can conclude where are the malicious traffic come from.<br>
<p align="center">
  <img width="400" height="400" alt="image296" src="https://github.com/user-attachments/assets/1d953bf7-8551-436d-afe8-9824429f740b" />
  <img width="400" height="400" alt="image99" src="https://github.com/user-attachments/assets/8ac28eda-cf42-4e28-a7e3-82ff1f5f83cb" />
</p><br>

Add filter for the malicious IPs. 10.10.99.99 and 10.10.99.62.<br>
Ans: **THM(PACKET_MASTER}** <br>

Q2: What is the flag?<br>
It's basicly the same as the previous question except now we try to filter the port either. Analysis the traffic.<br>
<p align="center">
  <img width="400" height="400" alt="image227" src="https://github.com/user-attachments/assets/03a8ba46-92fd-470e-813a-9c426a2dcc70" />
  <img width="400" height="400" alt="image292" src="https://github.com/user-attachments/assets/95000109-fa98-4c66-82a1-4418825edc7e" />
</p><br>

There's a few IPs and Ports, and those are 10.10.10.99.99:2222, 10.10.99.62:4444, and 10.10.99.74:7777.<br>
Ans: **THM{DETECTION_MASTER}** <br>
