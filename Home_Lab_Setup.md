# Homelab Setup

This notes file documents the initial setup of a personal homelab built from an older workstation running Ubuntu Server. The lab is used for learning system administration, networking, DevOps, and security techniques. The goal is to keep the environment reproducible and privacy-safe for public sharing.

---

## System Summary

- Main workstation (management): Windows desktop for SSH and administration.  
- Server: Recycled workstation running Ubuntu Server (headless).  
- Network: Consumer-grade router providing a private lab LAN separate from the primary household network.  
- Purpose: learning, testing, and experimentation with services that are not run on the daily-driver machine.

---

## Initial Setup Steps

### 1. Wipe and install Ubuntu Server
- Installed the latest Ubuntu Server on the workstation.
- Set up the system as headless (no GUI).  
- Created a local administrative account for initial configuration.

### 2. Assign a static IP (Netplan)
Set a static address so the server is reachable consistently over SSH. Replace placeholders with values appropriate for your environment.

Example Netplan configuration (`/etc/netplan/50-cloud-init.yaml`):
```yaml
network:
  version: 2
  ethernets:
    <interface_name>:
      dhcp4: no
      addresses:
        - <static_ip>/<prefix_length>
      routes:
        - to: default
          via: <gateway_ip>
      nameservers:
        addresses:
          - <dns_primary>
          - <dns_secondary>
