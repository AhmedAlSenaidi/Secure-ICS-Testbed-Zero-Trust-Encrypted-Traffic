# Secure ICS Testbed: Zero Trust & Encrypted Traffic

**Final Year Project – Spring 2025**  
Sultan Qaboos University – Department of Computer Science  
Group 4: Ahmed Said Al-Senaidi, Al-Salt Fahad Al-Naabi, Ahmed Naser Al-Khanbashi, Mohamed Yousef Al-Harrasi  
Supervisor: Dr. Shdha AlAmri  
Examiner: Prof. Abderezak Touzene  

---

## 📘 Project Overview

This project presents the design and implementation of a secure, virtualized **Industrial Control System (ICS) cybersecurity testbed** that mimics a real-world industrial environment. Built using **open-source tools** and following the **Zero Trust Architecture (ZTA)** model, this testbed aims to demonstrate the feasibility of robust cybersecurity in ICS networks even with limited resources.

It integrates **network segmentation, intrusion detection, secure access control, and encrypted communication** to provide a layered security approach compliant with industry standards such as **NIST SP 800-82**, **NIST SP 800-207 (ZTA)**, and **IEC 62443**.

---

## 🎯 Objectives

- Design a modular ICS testbed simulating SCADA systems and programmable logic controllers (PLCs) using **GRFICSv2**
- Implement **micro-segmented networks** based on the **Purdue Model**
- Apply **Zero Trust Architecture**: enforce least-privilege access using a **Jumpserver**
- Secure Modbus/TCP communication with **TLS encryption** via **stunnel**
- Monitor network and system activity using **Wazuh SIEM** and **Suricata IDS**
- Simulate cyberattacks using **Metasploit Framework**
- Validate the effectiveness of defense mechanisms through security testing

---

## 🧱 Testbed Architecture

### ✏️ Purdue Model-based Segmentation

- **Level 4 (IT Zone)**: Admin Workstations, Wazuh SIEM
- **Level 3.5 (DMZ Zone)**: Jump Server, Suricata IDS
- **Level 0-3 (OT Zone)**: SCADA Server, PLCs, Engineering Workstation, Virtual Plant (GRFICSv2)

### 🔒 Zero Trust Enforcement

- Default-deny policy via **pfSense firewall**
- **All access** to OT must go through the **Jumpserver**
- MFA and logging for remote access
- Encrypted inter-zone communication

### 🔌 Network Layout

```
[ IT Zone (192.168.1.x) ]
    → Wazuh SIEM
    → Admin Workstations

[ DMZ Zone (192.168.2.x) ]
    → Jump Server
    → Suricata IDS

[ OT Zone (192.168.3.x) ]
    → SCADA Server
    → PLCs
    → Engineering WS (GRFICSv2)
```

---

## 🛠️ Tools & Technologies

| Tool         | Purpose                                     |
|--------------|----------------------------------------------|
| GRFICSv2     | ICS simulation: SCADA, PLC, virtual sensors |
| pfSense      | Firewall & VLAN segmentation                |
| Wazuh SIEM   | Log management, event correlation           |
| Suricata IDS | Real-time traffic and anomaly detection     |
| stunnel      | TLS encryption for Modbus TCP               |
| VirtualBox   | Host virtual machines for all components    |
| Metasploit   | Ethical hacking for attack simulation       |

---

## 🧠 Security Controls

### 🛡️ Network Segmentation
- Enforced via **pfSense** rules
- DMZ acts as a secure buffer between IT and OT zones
- No direct IT-to-OT access allowed

### 🔑 Access Control
- All user access routed through **Jump Server**
- **MFA**, session logging, and command restrictions applied

### 📊 Real-Time Monitoring
- **Wazuh** aggregates logs from all VMs
- **Suricata** detects and reports malicious or suspicious traffic

### 🔐 Encryption
- Modbus/TCP communication encrypted using **TLS via stunnel**
- Encryption overhead and effectiveness verified via **Wireshark**

---

## 🔢 Functional Test Scenarios

| Scenario | Description |
|----------|-------------|
| 1 | Direct IT-to-OT Access Blocked (Zero Trust) |
| 2 | Jumpserver Gateway Validated |
| 3 | Unencrypted Modbus Traffic Captured |
| 4 | Command Injection via Metasploit |
| 5 | SCADA Emergency Shutdown Simulation |
| 6 | Encrypted Modbus Session Active |
| 7 | Post-Encryption Attack Attempt Blocked |
| 8 | Wazuh & Suricata Detection Confirmed |

---

## 🔹 Implementation Overview

- Virtual machines created in **VirtualBox**
- Firewall policies applied using **pfSense**
- Security events logged via **Wazuh**, IDS events generated via **Suricata**
- All system interactions tunneled through **Jump Server** with strong authentication
- **Attack simulations** performed using Metasploit to test resilience

---

## 🔍 Evaluation Metrics

| Metric | Outcome |
|--------|---------|
| IDS Accuracy | High detection rate with Suricata rules |
| Encryption Latency | < 10% increase in Modbus latency |
| Firewall Enforcement | No lateral traffic observed between zones |
| Logging | All events captured and visible in Wazuh dashboard |

---

## 📁 Repository Structure

```
FYP2025-Testbed/
├── README.md
├── .gitignore
├── docs/
│   └── Final_Report.pdf
├── config/
│   └── pfSense_rules.conf
│   └── stunnel_config.conf
├── diagrams/
│   └── Purdue_Model_Diagram.png
│   └── Data_Flow_Diagram.png
├── screenshots/
│   └── Jumpserver_Access.png
│   └── Wazuh_Alerts.png
├── simulations/
│   └── metasploit_attack_script.rc
├── logs/
│   └── wazuh_logs_sample.json
```

---

## 👨‍💻 Contributors

- **Ahmed Said Al-Senaidi**: VirtualBox configuration, Wazuh setup
- **Al-Salt Fahad Al-Naabi**: pfSense firewall, rule enforcement
- **Ahmed Naser Al-Khanbashi**: SCADA system setup, GRFICSv2 deployment
- **Mohamed Yousef Al-Harrasi**: Attack simulation, documentation, final testing

---

## 📄 References

- [NIST SP 800-82 Rev 2 (ICS Security)](https://csrc.nist.gov/publications/detail/sp/800-82/rev-2/final)
- [NIST SP 800-207 (Zero Trust Architecture)](https://csrc.nist.gov/publications/detail/sp/800-207/final)
- [ISA/IEC 62443 Standard](https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443)
- [MITRE ATT&CK for ICS](https://attack.mitre.org/matrices/ics/)
- [GRFICSv2](https://github.com/nsacyber/GRFICS)

---

## 🔒 License

This repository is for academic and educational purposes only. Do not deploy or use in a live ICS production environment.

---

## 🔋 .gitignore

```
# Ignore system files
*.DS_Store
Thumbs.db

# Ignore logs
logs/*
!logs/.gitkeep

# Ignore virtual machine images
*.vdi
*.vmdk
*.vbox
*.iso
*.ova
*.ovf

# Ignore backups and temp files
*.bak
*.tmp

# Ignore credentials
*.pem
*.key
*.crt
secrets.env

# Ignore compiled files
*.pyc
__pycache__/
```

