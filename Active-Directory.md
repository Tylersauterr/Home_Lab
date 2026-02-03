# Active Directory Domain Services Lab

## Overview
This project documents the deployment of a Windows Server 2022 domain environment within the home lab. The goal was to gain hands-on experience with identity management, DNS, and core Windows enterprise services.

The environment is used to simulate real-world domain administration scenarios.

---

## Server Deployment
- Deployed Windows Server 2022 as a virtual machine on Proxmox.
- Assigned a static IP address within the server VLAN.
- Configured DNS to point to itself after role installation.
- Verified connectivity to VyOS gateway.

---

## Active Directory Setup
- Installed Active Directory Domain Services (AD DS).
- Promoted the server to a domain controller.
- Created a new forest and domain.
- Integrated DNS with Active Directory.

---

## Domain Configuration
- Created Organizational Units for users, computers, and servers.
- Added test users and security groups.
- Joined Windows client machines to the domain.
- Verified authentication and DNS resolution.

---

## Administrative Practices
- Applied basic Group Policy Objects (GPOs).
- Enforced password and account policies.
- Practiced role-based access using security groups.
- Tested login, lockout, and policy enforcement behavior.

---

## Lessons Learned
- DNS configuration is critical for AD stability.
- Static IPs are mandatory for domain controllers.
- Group Policy changes should be tested incrementally.

---

## Next Steps
- Add a secondary domain controller for redundancy.
- Integrate file services and permissions.
- Explore logging and event monitoring for AD activity.
