# 🛠️ Home Lab Setup
 
This section documents how I built and configured my personal home lab environment from scratch. The goal of this setup is to enable hands-on learning in system administration, networking, and cybersecurity.

---

## 💻 System Overview

- **Main Machine**: Windows 10 PC (used for SSH access)
- **Server Machine**: HP Workstation running Ubuntu Server (headless setup)
- **Network**: Private LAN using a TP-Link AX5400 Pro router

---

## ⚙️ Server Setup

- Wiped old Windows OS and installed **Ubuntu Server (no GUI)**
- Connected the server directly to the router via Ethernet cable

---

## 🌐 Static IP Configuration (Netplan)

### Step 1: Identify network interface
```bash
ip a
```

### Step 2: Edit Netplan config
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

### Configuration Used:
```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      dhcp4: no
      addresses:
        - address.used.here/24
      routes:
        - to: default
          via: gateway.used.here
      nameservers:
        addresses:
          - 1.1.1.1
          - 8.8.8.8
```

### Step 3: Apply Netplan
```bash
sudo netplan apply
```

---

## 🔐 SSH Server Setup

### Install OpenSSH
```bash
sudo apt update && sudo apt install openssh-server -y
```

### Enable and Start SSH
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

### Open SSH Port on Firewall
```bash
sudo ufw allow ssh
sudo ufw enable
```

---

## 🔑 SSH Key-Based Login

### On Windows Machine:
Generate key pair:
```powershell
ssh-keygen
```

### On Ubuntu Server:
Manually copy the public key:
```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
# Paste key here

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## 🧰 Installed Utilities
To assist with system monitoring, diagnostics, and management:
```bash
sudo apt install htop git curl net-tools neofetch unzip tmux ufw fail2ban -y
```

---

This setup provides the foundation for all future sysadmin and cybersecurity projects in my homelab. For ongoing project documentation, check out the `/projects` or root README for more.
