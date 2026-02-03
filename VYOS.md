# VyOS Routing, VLANs, and Network Segmentation

## Overview
This project covers the deployment of VyOS as the primary router and firewall for the home lab. VyOS is used to handle VLAN segmentation, routing, firewall rules, and gateway services for all lab networks.

The design mirrors common enterprise network layouts with separate management, server, and client segments.

---

## Network Design
VLANs implemented:
- Management VLAN (infrastructure and hypervisors)
- Server VLAN (domain controllers, services, containers)
- Client VLAN (domain-joined test machines)

Each VLAN uses its own subnet with controlled inter-VLAN routing.

---

## VyOS Deployment
- Deployed VyOS as a virtual machine within Proxmox.
- Assigned WAN and LAN interfaces.
- Configured persistent configuration storage.

---

## VLAN Configuration
- Created VLAN interfaces on the LAN trunk.
- Assigned static gateway IPs for each VLAN.
- Verified DHCP relay and static routing behavior.
- Ensured proper tagging at the hypervisor and switch level.

---

## Firewall Rules
- Restricted access between VLANs by default.
- Allowed management VLAN access to all infrastructure.
- Limited server and client VLAN traffic to required services only.
- Explicitly logged denied traffic for visibility.

---

## Routing & Gateway Services
- Configured VyOS as the default gateway for all VLANs.
- Set upstream routing through the primary household router.
- Maintained lab isolation while allowing internet access.

---

## Lessons Learned
- VLAN planning matters more than initial setup speed.
- Firewall rules should be documented as they grow.
- Testing inter-VLAN traffic early prevents troubleshooting later.

---

## Next Steps
- Add VPN access for remote administration.
- Implement IDS/IPS capabilities.
- Harden firewall rules based on service needs.
