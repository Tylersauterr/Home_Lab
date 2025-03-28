# 🔄 Automated Updates with Ansible + Cron

As part of my personal homelab project, I automated system updates on my Ubuntu server using Ansible and cron. This setup checks how long the server has been running, and only performs updates if it's been online long enough to ensure everything has properly booted.

---

## ⚙️ Goal
Automate system updates without running them immediately after boot — and ensure updates only happen when the server has been up and stable for a set amount of time.

---

## 🛠️ What I Built
- A custom Ansible playbook to update packages
- A smart Bash script that checks system uptime before running updates
- A cron job that runs this script every 2 hours
- A logging system to keep track of update activity

---

## 📁 File Structure
```
~/ansible/
├── inventory.ini
├── smart_update.sh
├── smart_update.log
└── playbooks/
    └── update.yml
```

---

## 📦 Ansible Playbook: `update.yml`
```yaml
---
- name: Update and upgrade packages
  hosts: homelab
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Upgrade all packages
      apt:
        upgrade: dist

    - name: Autoremove unused packages
      apt:
        autoremove: yes
```

---

## 🔐 Ansible Inventory: `inventory.ini`
```ini
[homelab]
localhost ansible_connection=local
```

---

## 🧠 Smart Bash Script: `smart_update.sh`
```bash
#!/bin/bash

# Smart Ansible Update Runner
# Only runs if uptime is greater than 3 hours

MIN_UPTIME_HOURS=3
LOG_FILE="$HOME/ansible/smart_update.log"
PLAYBOOK_PATH="$HOME/ansible/playbooks/update.yml"
INVENTORY_PATH="$HOME/ansible/inventory.ini"

# Get uptime in hours
UPTIME_HOURS=$(awk '{print int($1 / 3600)}' /proc/uptime)

echo "[`date`] Uptime is $UPTIME_HOURS hours" >> "$LOG_FILE"

if [ "$UPTIME_HOURS" -ge "$MIN_UPTIME_HOURS" ]; then
    echo "[`date`] Running Ansible update playbook..." >> "$LOG_FILE"
    ansible-playbook -i "$INVENTORY_PATH" "$PLAYBOOK_PATH" >> "$LOG_FILE" 2>&1
    echo "[`date`] Update complete." >> "$LOG_FILE"
else
    echo "[`date`] Skipping update: uptime <$MIN_UPTIME_HOURS hours" >> "$LOG_FILE"
fi
```

Make sure the script is executable:
```bash
chmod +x ~/ansible/smart_update.sh
```

---

## 📆 Cron Setup
Run the script every 2 hours:
```bash
crontab -e
```
Add:
```cron
0 */2 * * * ~/ansible/smart_update.sh
```

---

## ✅ Result
Now my system:
- Checks uptime every 2 hours
- Runs updates only if it's been up for 3+ hours
- Logs everything to `smart_update.log`
- Stays clean, updated, and self-maintaining — even when I forget

---

This setup is part of my broader homelab project to automate and self-manage my server like a true sysadmin. More projects will be added as I keep building!

