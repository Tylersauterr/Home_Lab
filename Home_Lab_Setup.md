# Homelab Setup

This document outlines the initial setup of a personal homelab using an older workstation running Ubuntu Server. The lab is designed as a learning environment for practicing system administration, networking, DevOps, and security concepts. All examples are sanitized and safe for public sharing.

---

## System Summary

- Management workstation: Windows desktop used for SSH and administration  
- Server: Recycled workstation running Ubuntu Server (headless)  
- Network: Consumer-grade router providing a private lab LAN isolated from the primary home network  
- Purpose: Hands-on training and experimentation with infrastructure services

---

## Initial Setup Steps

### 1. Install Ubuntu Server
- Wiped the existing operating system and installed the latest version of Ubuntu Server.  
- Configured as a headless system (no GUI).  
- Created a local administrative account for setup.

### 2. Configure Static IP (Netplan)
To ensure reliable SSH access, the server uses a static IP address. Replace the placeholders with your own values.

Example configuration (`/etc/netplan/50-cloud-init.yaml`):
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
          - <primary_dns>
          - <secondary_dns>
```

Apply the configuration:
```bash
sudo netplan apply
```

> Note: Never share real addresses or gateway information publicly.

---

### 3. Enable SSH Access
Install and enable the OpenSSH server to allow remote management:
```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```

Allow SSH through the firewall (example using UFW):
```bash
sudo ufw allow ssh
sudo ufw enable
```

---

### 4. Configure Key-Based SSH Login
Generate an SSH key pair on the management system:
```bash
ssh-keygen -t ed25519
```

On the server:
```bash
mkdir -p ~/.ssh
# Paste public key into ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Connect to the server from your workstation:
```bash
ssh <username>@<server_ip>
```

Key-based authentication eliminates the need for password logins and improves security.

---

### 5. Install Common Utilities
Install a few essential command-line tools:
```bash
sudo apt install -y htop git curl net-tools neofetch unzip tmux ufw fail2ban
```

---

## Current Status
- Ubuntu Server running headless and accessible over SSH  
- Static IP assigned within the lab subnet  
- SSH key-based authentication configured  
- Firewall active with limited access rules  
- System ready for additional services and Docker containers

---

## Planned Projects
- WireGuard VPN for secure remote access  
- Docker and Portainer for container management  
- Monitoring stack (Prometheus, Grafana)  
- Centralized logging (Loki or ELK)  
- NAS or NFS-based storage for backups  
- Vulnerable lab VMs for penetration testing practice  

---

## Security & Privacy Guidelines
- Never post private IPs, MAC addresses, hostnames, or ports publicly  
- Keep keys, credentials, and configuration files outside of repositories  
- Restrict remote access to trusted devices only  
- Use VPNs and SSH keys for secure administration  
- Follow least-privilege principles when creating service accounts  

---

## Author
Tyler Sauter  
Cybersecurity Student | IT Support Technician  
Focus Areas: Infrastructure Engineering, Network Security, DevOps, and Virtualization
