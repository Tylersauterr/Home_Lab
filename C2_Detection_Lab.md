# C2 Detection Lab — Sliver & LimaCharlie

This document summarizes a controlled Command & Control (C2) detection lab built in an isolated homelab environment. The objective was to simulate an adversary using a C2 framework and to validate detection and response using an EDR platform. All testing was performed in an air-gapped or otherwise isolated network; do not run offensive tooling on production or public networks.

---

## Environment Overview

| Component | Role |
|-----------|------|
| Attacker VM | Ubuntu Server VM running the Sliver C2 framework (operator) |
| Victim VM | Target VM or host with an EDR agent (LimaCharlie) installed |
| Monitoring / EDR | LimaCharlie console, configured detection rules (including YARA) |
| Hypervisor | Local hypervisor (e.g., VMware Workstation) hosting isolated VMs |

---

## Safety & Legal Notice

- All tests must be executed in an isolated environment with no connectivity to production networks or the internet (except controlled lab updates if necessary).  
- Only perform offensive simulations on systems and networks for which you have explicit authorization.  
- Retain strict access controls and audit logs for the lab. Delete any artifacts and revert snapshots after testing if required by policy.

---

## High-Level Steps

### 1. Prepare isolated lab environment
- Create a dedicated virtual network isolated from any production or home networks (NAT or host-only).  
- Snapshot clean VM images before any offensive activity so they can be restored.

### 2. Configure the attacker VM (Sliver operator)
- Provision an Ubuntu Server VM and keep it offline or on the isolated lab network.  
- Install Sliver using the project’s official installation instructions:
```bash
# Example (follow official docs for the latest method)
curl https://sliver.sh/install | sudo bash
```
- Generate payloads and configure a Sliver listener to accept operator sessions.

### 3. Configure the victim VM (EDR / agent)
- Provision the victim VM (Windows or Linux) inside the same isolated network.  
- Install and register the EDR agent (LimaCharlie) to the lab account or test tenant.  
- Verify the agent checks in and sends telemetry to the EDR console.

### 4. Deploy and execute the payload
- Transfer the generated Sliver payload to the victim VM using lab-only transfer methods (shared folder, isolated HTTP server, or tooling that does not leave the lab).  
- Execute the payload on the victim to simulate compromise.  
- Confirm the Sliver operator can establish communication and spawn a remote shell.

### 5. Observe and validate detections in the EDR
- Enable and tune detection rules (YARA signatures, behavioral rules) in the EDR console.  
- Observe telemetry such as process creation, network connections, file writes, and persistence attempts.  
- Validate that alerts are generated and correlate them to the attack activity (beacons, command execution, lateral movement attempts if simulated).

### 6. Containment and cleanup
- Terminate active sessions and restore VMs from snapshots when testing is complete.  
- Collect and securely store any allowed forensic artifacts if required for analysis.  
- Revert the environment to a clean baseline.

---

## Outcomes & Validation

- The lab successfully simulated a C2-style engagement using Sliver and produced observable telemetry in the EDR platform.  
- Detections triggered on expected indicators (process behavior, network beaconing, and YARA matches).  
- The exercise validated detection rules and response workflows in a controlled environment.

---

## Skills Practiced

- Deployment and operation of a C2 framework in a lab setting  
- Endpoint telemetry analysis and EDR policy tuning  
- Safe malware simulation and containment procedures  
- Forensic artifact collection and incident validation  
- Secure lab network design and snapshot-based cleanup

---

## Tools & References

- Sliver C2 — https://github.com/BishopFox/sliver  
- LimaCharlie EDR — https://www.limacharlie.io/  
- Ubuntu Server — https://ubuntu.com/  
- VMware Workstation / other hypervisor (local VM hosting)

---

## Additional Notes

- Use snapshots regularly and document the exact snapshot IDs used for rollback.  
- If repeating experiments, rotate YARA rules and payload characteristics to avoid biased results from cached artifacts.  
- Maintain a private repository of scripts and detection rules; do not publish offensive tooling or lab-specific identifiers publicly.

