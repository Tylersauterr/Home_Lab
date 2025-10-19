# Homelab Monitoring and DNS Infrastructure

This document details the setup of a local DNS filtering and monitoring stack deployed in a homelab environment. The configuration uses Proxmox for virtualization, AdGuard Home for DNS resolution and filtering, Docker for containerized services, and Prometheus + Grafana for observability.

All systems are deployed in an isolated lab network, separate from the main home network, for the purpose of learning infrastructure, networking, and monitoring principles.

---

## Overview

### Objectives
- Build a self-contained network environment for infrastructure experimentation  
- Host a local DNS resolver and ad-blocking service  
- Implement monitoring and observability across hosts  
- Practice virtualization, containerization, and service management in a controlled lab  

### Components
| System | Function |
|---------|-----------|
| Proxmox Node | Virtualization host running core containers |
| AdGuard Home | DNS filtering, local name resolution, and query logging |
| Ubuntu Server | Docker host running Portainer, Prometheus, and Grafana |
| Prometheus | Metrics collection and time-series database |
| Grafana | Metrics visualization and dashboard platform |
| Switch & Router | Local networking backbone (private subnet) |

---

## Network Architecture

```
[ISP Modem]
    │
[Main Router]
    │
[Lab Router] (Isolated Subnet)
    │
[Managed Switch]
 ├────────────┬────────────┬────────────┐
 │            │            │            │
 │            │            │            │
[Proxmox Host] [Ubuntu Docker Host] [Clients via AdGuard DNS]
```

---

## Step 1 — Proxmox Setup

1. Installed Proxmox on a dedicated laptop as a 24/7 node.  
2. Configured static IP addressing and web UI access.  
3. Verified web and SSH access within the lab network.  
4. Disabled power and sleep features to keep the node continuously active.  

---

## Step 2 — AdGuard Home Deployment

1. Created a **Proxmox container** using a Debian base template.  
2. Assigned a static IP for reliability.  
3. Installed AdGuard Home using:
   ```bash
   curl -sSL https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | bash
   ```
4. Configured the router’s **primary DNS** to point to AdGuard Home.  
5. Verified DNS filtering and local resolution for lab devices.  

AdGuard Home now provides:
- DNS-level blocking for ads, telemetry, and malicious domains  
- Query logging by device  
- Custom filtering lists  
- Local DNS resolution (e.g., proxmox.lan, docker.lan)

---

## Step 3 — Docker Host Setup

On an older desktop acting as a server:
1. Installed Docker and verified the daemon:
   ```bash
   sudo apt update
   sudo apt install -y docker.io
   sudo systemctl enable --now docker
   ```
2. Installed Portainer for GUI-based container management:
   ```bash
   docker run -d -p 9443:9443 --name portainer \
     --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v portainer_data:/data \
     portainer/portainer-ce:latest
   ```
3. Verified access to the Portainer web UI.

---

## Step 4 — Prometheus Deployment

Prometheus collects metrics from local exporters and makes them available for Grafana dashboards.

Example container deployment:
```bash
docker run -d \
  --name=prometheus \
  --restart=always \
  -p 9090:9090 \
  -v /opt/prometheus:/etc/prometheus \
  prom/prometheus
```

Prometheus configuration file (`prometheus.yml`):
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['host.docker.internal:9100']
```

---

## Step 5 — Grafana Deployment

Grafana visualizes the metrics collected by Prometheus.

Example deployment:
```bash
docker run -d \
  --name=grafana \
  --restart=always \
  -p 3000:3000 \
  grafana/grafana
```

Then:
1. Accessed Grafana’s web UI.  
2. Added Prometheus as a data source (`http://<prometheus-container-ip>:9090`).  
3. Imported the **Node Exporter Full Dashboard (ID: 1860)** for host metrics.  

---

## Step 6 — Validation and Monitoring

Grafana now displays:
- CPU, memory, and disk utilization  
- Network traffic and system uptime  
- Docker container performance metrics  

Prometheus actively scrapes:
- Node Exporter data for system metrics  
- Docker container performance where configured  

AdGuard logs DNS activity by device, allowing full visibility into local DNS requests and blocking actions.

---

## Current Status

| Component | Status |
|------------|--------|
| Proxmox Host | Operational |
| AdGuard Home | Running and filtering traffic |
| Docker Engine | Installed and functional |
| Portainer | Accessible and managing containers |
| Prometheus | Collecting metrics |
| Grafana | Dashboard operational |
| Network Segmentation | Fully isolated lab subnet |

---

## Key Outcomes

- Full observability stack implemented (Prometheus + Grafana)  
- Local DNS and ad-blocking service established via AdGuard Home  
- Isolated, stable Proxmox-based virtualization environment  
- Centralized container management through Portainer  
- Achieved baseline infrastructure for further blue-team or DevOps experimentation  

---

## Next Steps

- Add cAdvisor or Node Exporter (Docker) for container-level metrics  
- Integrate Proxmox monitoring into Prometheus  
- Set up NFS or OpenMediaVault for backup and storage  
- Configure WireGuard for remote access into the lab  
- Implement alerting in Grafana via email or webhook  

---

## Notes

All systems are on private subnets and use internal IP addressing. No real IPs or ports are exposed publicly.  
This documentation is for educational use and should not be deployed to production without additional hardening.

---

## Author
Tyler Sauter  
Cybersecurity Student | IT Support Technician  
Focus Areas: Infrastructure Engineering, Virtualization, Network Security, and DevOps  
