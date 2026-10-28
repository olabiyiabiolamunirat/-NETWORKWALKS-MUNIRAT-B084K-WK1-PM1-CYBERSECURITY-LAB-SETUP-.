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

## 🚀 Project Overview

This documentation covers the creation of a fully isolated cybersecurity practice lab built with Oracle VM VirtualBox and Kali Linux.

The lab was designed to be a safe, self-contained space where I can practice core cybersecurity skills such as network discovery, vulnerability assessment, and penetration testing without any risk to real-world systems. It is completely separated from my main operating system.

## 🧭 Objectives

My main goals in this project were to:
- Successfully install and configure VirtualBox and the Kali Linux virtual machine
- Build a fully isolated cybersecurity lab environment
- Create and configure a custom NAT Network for the lab to enable VM-to-VM communication
- Properly configure IP address, default gateway, and DNS for reliable connectivity
- Assign a static IP to Kali and verify all network settings
- Create a baseline snapshot for easy restoration during future experiments
- Clearly document the entire process for future labs 
---

## Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for:
- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice

> Warning ⚠️ This laboratory must only be used for systems that belong to you or with explicit permission to test.

---

## Lab Architecture

**Design:** Host (Windows) -> VirtualBox NAT Network (NatNetwork 10.0.0.0/24) -> Kali Linux (10.0.0.2) + Future Target VMs

This lab is an isolated environment inside VirtualBox. All lab traffic stays inside 10.0.0.0/24, safe from my real network. Additional target machines can be added to same virtual network in future.

**Configuration:**
- Network: NatNetwork / intnet - 10.0.0.0/24
- Kali Linux: 10.0.0.2/24 (Attacker)
- Gateway: 10.0.0.1
- DNS: 8.8.8.8

![Lab Architecture](architecture.png)
---

## ⚙️ Lab Configuration

| Component | Configuration |
|:--- |:--- |
| Host OS | Windows 10/11 |
| Hypervisor | VirtualBox 7.2 |
| Security OS | Kali Linux 2026.2 |
| Kali RAM | 2048 MB |
| Virtual Network | NAT Network |
| Network Address | 10.0.0.0/24 |
| Kali IP Address | 10.0.0.2/24 |
| Default Gateway | 10.0.0.1 |
| DNS Server | 8.8.8.8 |
| Future VM Range | 10.0.0.3–10.0.0.99 |

---

## 🪜 Lab Setup Procedure

### Step 1. Install 7-Zip
7-Zip was installed to extract the Kali Linux VM package (.7z archive).

### Step 2. Install VirtualBox
VirtualBox was installed as the hypervisor from virtualbox.org

### Step 3. Create the NAT Network
A dedicated NAT Network was created:
- Network Name: NatNetwork
- IPv4 Prefix: 10.0.0.0/24
- DHCP: Enabled
- IPv6: Disabled

This allows multiple VMs to communicate with each other while having outbound connectivity.

### Step 4. Import Kali Linux
Kali Linux was imported and configured:
- Attached to: NAT Network
- Network Name: NatNetwork
- RAM: 2048 MB

### Step 5. Configure the Kali Linux Network

The Kali Linux network configuration was checked and configured with a consistent IPv4 address.

**Configuration:**
- IP Address: 10.0.0.2
- Subnet Mask: 255.255.255.0
- Gateway: 10.0.0.1
- DNS: 8.8.8.8

### Step 5. Configure Kali Network (Static IP)
Consistent IP makes documentation and future labs easier:
```bash
nmcli connection modify "Wired connection 1" ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns "8.8.8.8" ipv4.method manual
nmcli connection down "Wired connection 1"
nmcli connection up "Wired connection 1"
ip a

Step 6. Create Clean Snapshot
Snapshot name: `Clean Kali - Network Setup`
This is the baseline to restore if future exercises break the VM.

---

🔎 Lab Verification

Test	Command	Expected Result
Check IP	`ip a`	10.0.0.2/24 displayed
Test gateway	`ping 10.0.0.1`	Successful replies
Test Internet	`ping 8.8.8.8`	Successful replies
Test DNS	`nslookup google.com`	Domain resolves
Verify Nmap	`nmap --version`	Version displayed

---

🐞 Problems Encountered & Solutions

*Problem 1: No Internet / DNS Fails*
Solution:

sudo nano /etc/resolv.conf
# Add:
nameserver 8.8.8.8
sudo systemctl restart NetworkManager

*Problem 2: VMs Cannot Communicate*
Solution: Ensure both VMs use exact same NAT Network name `NatNetwork` and use TCP scan instead of ping:

nmap -Pn 10.0.0.0/24

*Problem 3: Static IP Breaks Connectivity*
Solution: Use `nmcli` method above instead of manual file editing.

---

💡 What I Learned

What I Learned
1. NAT vs NAT Network: NAT Network lets multiple VMs talk to each other (10.0.0.0/24) while staying isolated - perfect for lab.
2. Virtual Networking: How Virtua Box adapters connect VMs.
3. Static IP: How to configure IP, gateway, DNS in Kali with nmcli.
4. Snapshots: Clean snapshot = recovery
point before risky experiments.
5. Documentation: How to document professionally on GitHub.
---

🔐 Security & Usage
This lab is strictly for educational purposes in an isolated environment.

---

🔗 Tools & Resources
- VirtualBox: https://www.virtualbox.org/wiki/Downloads
- Kali Linux: https://www.kali.org/get-kali
- 7-Zip: https://7-zip.org/download.html

---

👤 Author
*Olabiyi Abiola Munirat*
Cybersecurity Student | B084

LinkedIn: https://www.linkedin.com/in/olabiyi-abiola-05a584429
GitHub: https://github.com/Olabiyiabiolamunirat

---

📌 Project Information
Program Name: Cybersecurity at Networkwalks | Week: 01 | Project: Cybersecurity & Pentesting Lab
