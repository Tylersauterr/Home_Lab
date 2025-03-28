# 🧠 C2 Detection Lab with Sliver & LimaCharlie

This project simulates a Command & Control (C2) detection scenario in a homelab environment. The goal was to create a controlled lab for testing and detecting malicious activity using the Sliver C2 framework and LimaCharlie’s EDR tooling.

---

## 💻 Lab Environment

|      Component       | Description |
|----------|-----------|
| **Attacker Machine** | Ubuntu Live Server VM running Sliver C2 framework |
| **Victim Machine**   | Another VM or host with LimaCharlie agent installed |
| **Monitoring**       | LimaCharlie dashboard and YARA rules for detection |
| **Hypervisor**       | VMware Workstation on Windows host |

---

## 🔧 Setup Steps

### 1. **Configured Ubuntu Live Server as Attacker Box**
- Installed Sliver C2:
  ```bash
  curl https://sliver.sh/install | sudo bash
  ```
- Generated payloads
- Launched listener for command/control

### 2. **Installed LimaCharlie Agent on Victim Machine**
- Registered endpoint to LimaCharlie console
- Verified check-in and visibility

### 3. **Deployed C2 Payload**
- Transferred Sliver payload to victim machine
- Executed payload to simulate compromise
- Verified C2 communication and remote shell via Sliver

### 4. **Monitored Activity with LimaCharlie**
- Enabled YARA-based detection rules
- Observed process creation, network activity, and file drops
- Detected C2 beaconing and shell activity via logs and alerts

---

## 🎯 Outcome
- Successfully simulated a red-team style C2 engagement
- Verified detection of unauthorized access and C2 beacon using LimaCharlie
- Practiced real-world response strategy in a safe environment

---

## 📚 Skills Practiced
- Command & Control (C2) frameworks
- Endpoint Detection & Response (EDR)
- Malware simulation and behavioral detection
- Linux server management
- Network isolation and safe malware testing

---

## ⚠️ Notes
- All testing was performed in a **closed, isolated environment**
- No live networks or unauthorized systems were involved
- This lab was built purely for educational and research purposes

---

## 🔗 Tools Used
- [Sliver C2](https://github.com/BishopFox/sliver)
- [LimaCharlie EDR](https://www.limacharlie.io/)
- Ubuntu Server
- VMware Workstation

