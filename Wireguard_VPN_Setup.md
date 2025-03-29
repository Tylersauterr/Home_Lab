# WireGuard VPN Setup for Home Lab

This project documents the process of setting up a **WireGuard VPN server** on my headless Ubuntu home lab system. The goal was to securely access my private network and services from outside my local environment. The VPN tunnel allows me to SSH into my lab and eventually route traffic through it. This was one of the more technical projects I’ve completed from scratch and a great learning experience.

---

## 🖥️ System Overview

**Server:** HP Workstation running Ubuntu Server (no GUI)  
**Client:** Windows 10 machine (main daily driver)  
**Router:** TP-Link AX5400 Pro  
**VPN Software:** [WireGuard](https://www.wireguard.com/)  
**DNS Server:** Unbound (for private recursive DNS resolution)

---

## ✅ Setup Steps

### 1. Install WireGuard on the Server
```bash
sudo apt update && sudo apt install wireguard -y
```

### 2. Generate Keys
```bash
wg genkey | tee server_private.key | wg pubkey > server_public.key
wg genkey | tee client_private.key | wg pubkey > client_public.key
```

### 3. Configure `wg0.conf`
File: `/etc/wireguard/wg0.conf`
```ini
[Interface]
Address = 10.0.0.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = <server_private_key>

[Peer]
PublicKey = <client_public_key>
AllowedIPs = 10.0.0.2/32
```

### 4. Enable IP Forwarding
```bash
sudo nano /etc/sysctl.conf
# Ensure this line is uncommented:
net.ipv4.ip_forward=1
sudo sysctl -p
```

### 5. Add MASQUERADE Rule
```bash
sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
```

### 6. Allow Necessary Ports with UFW
```bash
sudo ufw allow 22/tcp
sudo ufw allow 51820/udp
sudo ufw allow 53
sudo ufw allow 53/udp
sudo ufw enable
```

### 7. Port Forwarding on Router
In your router admin panel:
- External/Internal Port: `51820`
- Protocol: `UDP`
- IP: Your Ubuntu server's LAN IP (e.g. `192.168.0.150`)

### 8. Setup Windows Client
```ini
[Interface]
PrivateKey = <client_private_key>
Address = 10.0.0.2/24
DNS = 10.0.0.1

[Peer]
PublicKey = <server_public_key>
Endpoint = <your_public_ip>:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

---

## 🔍 Issues Faced & Troubleshooting

### ❌ DNS Lookups Failed
- **Problem:** I could ping IPs but couldn’t resolve domains (e.g., google.com).
- **Fix:** Installed and configured Unbound for DNS resolution. Verified with `dig` and opened port 53 via UFW.

### ❌ Couldn’t Access the Internet
- **Problem:** VPN tunnel was up, but no browser access.
- **Fix:** IP forwarding was set up, but the UFW config was blocking traffic. Adjusted UFW rules to allow DNS and NAT forwarding.

### ❌ Unbound Wouldn’t Start
- **Problem:** Unbound conflicted with systemd-resolved on port 53.
- **Fix:** Disabled systemd-resolved to free up port 53:
```bash
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
```

---

## 📈 Outcome
- I can securely tunnel into my home network from anywhere.
- DNS is resolved locally through Unbound.
- Client has internet access over VPN.

---

## 📌 Key Takeaways
- Learned how to work with iptables NAT and UFW simultaneously.
- Understood WireGuard’s public key infrastructure.
- Gained confidence in DNS, NAT, and port forwarding concepts.

---

## 🔐 Next Steps
- Use VPN for remote SSH and internal service access.
- Create monitoring and logging dashboard.
- Extend to mobile devices with WireGuard app.

---

If you're reading this and trying to build your own setup, keep pushing through the frustrating parts—it clicks eventually.
