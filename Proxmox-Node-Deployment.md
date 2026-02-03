# Proxmox Node Deployment & Cluster Setup

## Overview
This project documents the process of building and configuring multiple Proxmox VE nodes for a self-hosted enterprise-style home lab. The goal was to create a stable virtualization platform capable of running server and network infrastructure workloads.

The environment is designed to support Windows Server, Linux servers, and network appliances while maintaining proper isolation and management practices.

---

## Hardware Overview
- Multiple repurposed desktop and laptop systems
- Intel i7-class CPUs
- 32–64 GB RAM per node
- Local SSD storage for VM disks
- Dedicated NICs where available

---

## Proxmox Installation
1. Installed Proxmox VE directly onto bare metal using the official ISO.
2. Configured static IP addresses on the management interface.
3. Disabled unnecessary services on the host OS to reduce attack surface.
4. Updated all nodes to the latest stable Proxmox packages.

---

## Node Configuration
- Created Linux bridges for VM networking.
- Assigned management interfaces to a dedicated management VLAN.
- Verified inter-node connectivity and SSH access.
- Configured NTP for time synchronization across all nodes.

---

## Cluster Setup
- Initialized a Proxmox cluster from the primary node.
- Joined additional nodes using secure cluster join commands.
- Verified quorum and cluster health.
- Enabled centralized management via the Proxmox web interface.

---

## Lessons Learned
- Static IP planning is critical before clustering.
- Consistent naming conventions prevent long-term confusion.
- Separating management traffic early avoids rework later.

---

## Next Steps
- Expand shared storage options.
- Implement backup and snapshot strategies.
- Integrate monitoring and alerting.
