# WireGuard VPN Setup for Home Lab

This project documents the process of setting up a WireGuard VPN server on a headless Ubuntu system in my home lab. The goal was to create a secure, encrypted tunnel for remote access to internal lab resources and services while maintaining network isolation from the main home network. The project provided hands-on experience with VPN configuration, key management, and network routing.

---

## System Overview

**Server:** Linux host (headless Ubuntu Server)  
**Client:** Windows workstation  
**Router:** Consumer-grade router supporting custom port forwarding  
**VPN Software:** [WireGuard](https://www.wireguard.com/)  
**DNS Service:** Local recursive resolver (Unbound)

---

## Setup Summary

### 1. Install WireGuard
```bash
sudo apt update && sudo apt install wireguard -y
```

### 2. Generate Key Pairs
```bash
wg genkey | tee server_private.key | wg pubkey > server_public.key
wg genkey | tee client_private.key | wg pubkey > client_public.key
```

### 3. Configure the Server Interface
File: `/etc/wireguard/wg0.conf`
```ini
[Interface]
Address = <private_subnet_address>
SaveConfig = true
ListenPort = <vpn_port>
PrivateKey = <server_private_key>

[Peer]
PublicKey = <client_public_key>
AllowedIPs = <client_ip_or_subnet>
```

### 4. Enable IP Forwarding
```bash
sudo nano /etc/sysctl.conf
# Uncomment:
net.ipv4.ip_forward=1
sudo sysctl -p
```

### 5. Set Up NAT for Outbound Traffic
```bash
sudo iptables -t nat -A POSTROUTING -o <network_interface> -j MASQUERADE
```

### 6. Adjust Firewall Rules
Example (UFW):
```bash
sudo ufw allow <ssh_port>/tcp
sudo ufw allow <vpn_port>/udp
sudo ufw enable
```

### 7. Configure Port Forwarding on the Router
Forward UDP traffic from your external interface to the VPN port on the server’s internal IP address.

### 8. Create Client Configuration
```ini
[Interface]
PrivateKey = <client_private_key>
Address = <client_vpn_ip>
DNS = <internal_dns_ip>

[Peer]
PublicKey = <server_public_key>
Endpoint = <public_ip_or_ddns>:<vpn_port>
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

---

## Issues and Fixes

**DNS Lookups Failed**  
- Cause: The system could ping IPs but not resolve domains.  
- Fix: Configured Unbound for local DNS recursion and allowed traffic on the DNS port.

**No Internet Access Through VPN**  
- Cause: Forwarding rules blocked outbound traffic.  
- Fix: Enabled NAT via iptables and confirmed UFW forwarding rules.

**Unbound Conflict**  
- Cause: systemd-resolved occupied port 53.  
- Fix: Disabled systemd-resolved to free the port.
```bash
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
```

---

## Outcome
- Secure remote access to the home lab network.  
- Local DNS resolution through a private resolver.  
- Successful routing of client traffic through the VPN tunnel.

---

## Key Takeaways
- Learned how to configure and route traffic through WireGuard.  
- Gained practical experience with key-based authentication.  
- Improved understanding of NAT, UFW, and DNS recursion.  

---

## Next Steps
- Extend VPN access to additional devices (mobile and laptop).  
- Add VPN usage monitoring and logging.  
- Integrate with existing Prometheus/Grafana monitoring stack.
