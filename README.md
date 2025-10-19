# Tyler's Personal Home Lab

Welcome — this repo is my lab notebook. It documents the projects, configs, and experiments I run on a headless Ubuntu server in my spare time. I built this environment to learn systems, networking, and security by doing, not just reading.

---

## What you'll find here
- **Lab Setup Guide** — step-by-step notes for how the lab was built and configured  
- **SSH & Networking** — examples for static addressing, SSH key login, and firewall basics (sanitized for public sharing)  
- **Projects & Playbooks** — Ansible snippets, container stacks, and notes for repeatable deployments  
- **Security Exercises** — controlled exercises and detection labs (safely confined to the lab environment)

---

## Tech summary
- Ubuntu Server (headless) as the primary host  
- A consumer router providing a dedicated lab LAN (kept separate from the main household network)  
- Proxmox for virtualization (on a dedicated node)  
- Docker for lightweight services and containers  
- AdGuard Home for local DNS and ad/telemetry blocking  
- Prometheus and Grafana for metrics and dashboards  
- Portainer for quick container management

All configs referenced here are sanitized. No sensitive IPs, keys, or credentials are stored in this public repo.

---

## Why I built this
I’m a cybersecurity student and IT tech. This lab is my practical classroom — a place to:
- Break things safely and learn how to fix them  
- Run real tools (EDR, VPN, C2 frameworks) in an isolated environment  
- Practice monitoring, incident response, and systems management  
- Build artifacts I can show in interviews or use as study references

---

## Current goals and next steps
The lab is iterative. My working roadmap includes:
- WireGuard for secure remote access to the lab  
- A small NAS/NFS target for backups and Proxmox snapshots  
- Centralized logging and alerting (Loki/ELK/Graylog)  
- Additional detection labs and tooling for red/blue team practice  
- More automation (Ansible playbooks and documented runbooks)

---

## How I use this repo
- Everything here is meant to be reproducible and understandable.  
- I keep practical notes, commands, and short guides so I can rebuild or recover the lab quickly.  
- Sensitive configs and keys live outside the repo in a private store.

---

## About me
I’m Tyler — I like building infrastructure, understanding how systems interact, and learning how to defend them. This repo is the running log of that work.

If you find something useful or want to suggest an improvement, open an issue or drop a line.

---

## Safety and ethics
All offensive or red-team tooling described in this repo is used only in isolated lab environments where I have explicit authorization. Do not run offensive tools on networks or systems you do not own or have permission to test.

