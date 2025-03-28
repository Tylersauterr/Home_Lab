# 🛠️ Tyler's Homelab Setup

This is a quick breakdown of how I set up my personal homelab using an old workstation, Ubuntu Server, and a router I wired into my apartment. I use this lab to practice cybersecurity, system administration, and to just mess around with tools I wouldn’t normally run on my main PC.

---

## 💻 What I'm Running

- **Main Machine:** Windows 10 desktop (used for SSH access and general management)
- **Server:** HP Z-series workstation running Ubuntu Server (no GUI, completely headless)
- **Network:** TP-Link AX5400 Pro router with my own private LAN (separate from apartment Wi-Fi)

---

## 🔧 How I Set It Up

### Wiped the Server & Installed Ubuntu
I took an old HP workstation that had Windows on it, wiped the drive, and installed the latest version of **Ubuntu Server**. I’m running it totally headless — no monitor, keyboard, or GUI.

---

### Set a Static IP Using Netplan

After installation, I gave the server a static IP so I could always SSH into it without having to check DHCP leases.

```bash
# Get interface name
ip a

# Edit the Netplan config
sudo nano /etc/netplan/50-cloud-init.yaml
```

**Example Config:**
```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      dhcp4: no
      addresses:
        - 192.168.0.150/24
      routes:
        - to: default
          via: 192.168.0.1
      nameservers:
        addresses:
          - 1.1.1.1
          - 8.8.8.8
```

Apply the config:
```bash
sudo netplan apply
```

---

### Enabled SSH Access

To manage the server remotely, I installed OpenSSH and made sure it starts on boot:
```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

I also opened it in the firewall just in case:
```bash
sudo ufw allow ssh
sudo ufw enable
```

---

### Set Up SSH Key Login (No Passwords)

On my Windows machine, I generated an SSH key:
```powershell
ssh-keygen
```

Then I copied the public key over to the server:
```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
# pasted my public key
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

After that, I could SSH in with:
```powershell
ssh tyler@192.168.0.150
```

And it logs me in instantly without a password.

---

## 🧰 Some Utilities I Installed

Just some basics I always like to have available:
```bash
sudo apt install htop git curl net-tools neofetch unzip tmux ufw fail2ban -y
```

---

## ✅ Current Status

- ✅ Ubuntu Server is up and running with a static IP
- ✅ Fully headless — I only access it over SSH
- ✅ SSH key-based login is set up
- ✅ Router is locked down and running a private network just for the lab

---

## 💡 What's Next?

I'll be using this server for a bunch of projects, including:
- Running WireGuard as a personal VPN
- Deploying logging & monitoring tools (rsyslog, ELK stack, etc.)
- Hosting vulnerable VMs for pentesting practice
- Doing local network recon and packet analysis
- Possibly turning it into a small SOC simulation

---

If you want to see updates or follow along with what I’m building in the lab, I’ll be documenting each project in this repo as I go.
