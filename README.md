# -NETWORKWALKS-MUNIRAT-B084K-WK1-PM1-CYBERSECURITY-LAB-SETUP-.
Cybersecurity LAB SETUP 
<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

Building an isolated virtual lab for penetration testing and ethical hacking practice

</div>

<p align="center">
 <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
 <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
 <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
 <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
 <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
 <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
 <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
 <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
 <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
 <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
 <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
</p>

---

# Week 01 - My Cybersecurity Lab Setup

**Name: Olabiyi Abiola Munirat**
**Program: Cybersecurity B084 - Networkwalks**
**LinkedIn: https://www.linkedin.com/in/olabiyi-abiola-05a584429**
**GitHub: https://github.com/Olabiyiabiolamunirat**

---

### 🔐 What This Lab Is About
This repo documents how I built my first isolated hacking lab using VirtualBox and Kali Linux.

The goal is to have a safe, private space on my laptop where I can practice ethical hacking skills without affecting real networks.

### 📌 Project Overview
This documentation covers the creation of a fully isolated cybersecurity practice lab built with Oracle VM VirtualBox and Kali Linux.

The lab was designed to be a safe, self-contained space where I can practice core cybersecurity skills such as network discovery, vulnerability assessment, and penetration testing without any risk to real-world systems. It is completely separated from my main operating system.

### 🧭 My Objectives
My main goals in this project were to:
- Successfully install and configure VirtualBox and the Kali Linux virtual machine
- Build a fully isolated cybersecurity lab environment
- Create and configure a custom NAT Network for the lab to enable VM-to-VM communication
- Properly configure IP address, default gateway, and DNS for reliable connectivity
- Assign a static IP to Kali and verify all network settings
- Create a baseline snapshot for easy restoration during future experiments
- Clearly document the entire process for future labs

### 🛡️ Why I Built This Lab
This lab will be used for:
- Learning network reconnaissance
- Practicing port scanning with Nmap
- Vulnerability assessment
- Understanding packets
- Safe exploitation practice

> ⚠️ Ethics: I will only test on VMs that I own. No unauthorized testing.

### 🏗️ Lab Architecture
**My Setup: Windows 11 (Host) -> VirtualBox NAT Network (NatNetwork) -> Kali Linux (10.0.0.2) + Future Targets (10.0.0.3-99)**

I used NAT Network because it lets all my lab VMs talk to each other, which normal NAT does not allow.

### ⚙️ My Lab Configuration
- Host OS: Windows 10/11
- Hypervisor: VirtualBox 7.2
- Security OS: Kali Linux 2026.2
- Kali RAM: 2048 MB
- Network Name: NatNetwork
- Network: 10.0.0.0/24
- My Kali IP: 10.0.0.2/24
- Gateway: 10.0.0.1
- DNS: 8.8.8.8

### 🪜 How I Set It Up

**Step 1: Install Tools**
Installed 7-Zip to extract Kali and installed VirtualBox.

**Step 2: Create Private Network**
In VirtualBox I created:
Name: NatNetwork, IPv4: 10.0.0.0/24, DHCP: On, IPv6: Off

**Step 3: Import Kali**
Imported Kali and set: Settings > Network > Attached to: NAT Network > Name: NatNetwork, RAM 2048 MB.

**Step 4: Set Static IP**
I used nmcli to avoid breaking the network:

nmcli connection modify "Wired connection 1" ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns "8.8.8.8" ipv4.method manual
nmcli connection down "Wired connection 1"
nmcli connection up "Wired connection 1"
ip a

**Step 5: Take Snapshot**
Created snapshot named `Clean-Kali-Base` - my clean restore point.

🔎 How I Verified It
- `ip a` - saw 10.0.0.2
- `ping 10.0.0.1` - gateway ok
- `ping 8.8.8.8` - internet ok
- `nslookup google.com` - DNS working
- `nmap -Pn 10.0.0.0/24` - can discover other VMs (ping is blocked on some targets)

🐞 Challenges I Faced

**1. DNS Failed**
Could ping 8.8.8.8 but not http://google.com. Fix:

sudo nano /etc/resolv.conf
nameserver 8.8.8.8
nameserver 1.1.1.1
sudo systemctl restart NetworkManager

**2. VMs Not Talking**
I had NAT on one and NAT Network on another. Fix: Changed both to same `NatNetwork`.

**3. Static IP Broke Connection**
Manual file editing conflicted with NetworkManager. Fix: Used `nmcli` method.

💡 What I Learned
- NAT vs NAT Network - NAT Network is best for multi-VM labs
- How VirtualBox adapters work
- How to set static IP correctly in modern Kali
- Why snapshots are important
- Documentation is key in cybersecurity

🔗 Resources
- VirtualBox: https://www.virtualbox.org/wiki/Downloads
- Kali: https://www.kali.org/get-kali/
- 7-Zip: https://7-zip.org/

📌 Info
Program: Cybersecurity at Networkwalks | Week 01 | Project: Cybersecurity & Pentesting Lab Setup

👤 Author
Olabiyi Abiola Munirat
Cybersecurity Student | B084
