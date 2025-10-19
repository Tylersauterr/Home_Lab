# Automated Updates with Ansible and Cron

This project automates package updates on a headless Ubuntu server using Ansible and a cron job. The configuration ensures that updates only run after the system has been online long enough to guarantee stable uptime, avoiding unnecessary update runs right after a reboot.

---

## Goal
Automate system updates in a reliable, low-maintenance way, while preventing update runs immediately after boot. Updates are triggered on a schedule only when the system has been up for a defined period.

---

## Overview
- Custom Ansible playbook to perform updates
- Bash script that checks system uptime before running updates
- Cron job to trigger the script every 12 hours
- Logging to record when updates are skipped or executed

---

## File Structure
```
~/ansible/
├── inventory.ini
├── smart_update.sh
├── smart_update.log
└── playbooks/
    └── update.yml
```

---

## Ansible Playbook — `update.yml`
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

## Ansible Inventory — `inventory.ini`
```ini
[homelab]
localhost ansible_connection=local
```

---

## Bash Script — `smart_update.sh`
```bash
#!/bin/bash

# Smart Ansible Update Runner
# Only runs if uptime is greater than 12 hours

MIN_UPTIME_HOURS=12
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

Make the script executable:
```bash
chmod +x ~/ansible/smart_update.sh
```

---

## Cron Configuration
To run the update script every 12 hours, open the cron configuration:
```bash
crontab -e
```
Add the following entry:
```cron
0 */12 * * * ~/ansible/smart_update.sh
```

---

## Result
The automation now:
- Checks system uptime every 12 hours  
- Runs updates only if the system has been online for 12+ hours  
- Logs every decision and result to `smart_update.log`  
- Keeps the system updated without manual maintenance or risk of conflicts during early boot  

