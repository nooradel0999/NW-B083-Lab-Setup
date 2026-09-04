# NW-B083-Lab-Setup
# 🔒 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

![Cybersecurity](https://img.shields.io/badge/Skill-Cybersecurity-red)
![VirtualBox](https://img.shields.io/badge/Ver-VirtualBox%20v7.2-blue)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-v2026.2-blueviolet)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-green)
![Penetration Testing](https://img.shields.io/badge/Skill-Penetration%20Testing-orange)
![Virtualization](https://img.shields.io/badge/Skill-Virtualization-blue)
![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black)

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux as part of the **NetworkWalks Cybersecurity Internship Program (Batch B083, Week 1)**.

The purpose of the lab is to create a **controlled, isolated environment** where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly without affecting production networks or violating any laws.

The lab is configured on a **private virtual network (NAT Network)** so that additional machines can be added later and used as targets for authorized security testing.

---

## 🖥️ My Laptop Specifications

| Component | Specification |
|-----------|---------------|
| **Host OS** | Windows 10 Pro (Build 19045) |
| **Processor** | Intel Core i5-1035G1 @ 1.00GHz (4 Cores, 8 Threads) |
| **RAM** | 16 GB DDR4 |
| **Storage** | 512 GB NVMe SSD |
| **Virtualization** | Intel VT-x Enabled in BIOS |
| **Network** | Wi-Fi 5 (802.11ac) |

> **Note:** Recommended specs by NetworkWalks are 8GB+ RAM and 256GB+ SSD. My machine exceeds these requirements, allowing smooth operation of multiple VMs simultaneously.

---

## 🌐 Network Architecture

All virtual machines are connected to a **custom NAT Network** configured in VirtualBox with the following parameters:

- **Network Name:** NatNetwork
- **IPv4 Prefix:** 10.0.0.0/24
- **IP Range:** 10.0.0.2 – 10.0.0.99
- **Gateway:** 10.0.0.1
- **DHCP Server:** Enabled
- **DNS:** 8.8.8.8 (Google) / 10.0.0.1 (Fallback)

### IP Address Allocation Table

| Virtual Machine | IP Address | Subnet Mask | Gateway | Status |
|-----------------|------------|-------------|---------|--------|
| **Kali Linux** (Attacker) | 10.0.0.2 | /24 (255.255.255.0) | 10.0.0.1 | Required |
| **Windows 11** (Target) | 10.0.0.11 | /24 (255.255.255.0) | 10.0.0.1 | Required |
| **Windows 10** (Target) | 10.0.0.10 | /24 (255.255.255.0) | 10.0.0.1 | Required |
| **Windows 7** (Target) | 10.0.0.7 | /24 (255.255.255.0) | 10.0.0.1 | Optional |
| **Server 2016** (Target) | 10.0.0.16 | /24 (255.255.255.0) | 10.0.0.1 | Optional |
| **Android 9.x** (Target) | 10.0.0.9 | /24 (255.255.255.0) | 10.0.0.1 | Required |

![Network Topology](lab_topology_diagram.png)
![IP Allocation](ip_allocation_table.png)

---

## 🛠️ Implementation Steps

### Phase 1: Foundation Setup

#### Step 1: Install 7-Zip
- Downloaded from [https://7-zip.org/download.html](https://7-zip.org/download.html)
- Installed 7-Zip v24.09 for extracting compressed VM images

#### Step 2: Install Oracle VirtualBox
- Downloaded from [https://virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
- Installed VirtualBox v7.2.0 with Extension Pack
- Verified Virtualization (VT-x) was enabled in BIOS before installation

#### Step 3: Configure NAT Network
1. Opened VirtualBox → File → Tools → Network Manager
2. Navigated to **NAT Networks** tab
3. Created new NAT Network named **"NatNetwork"**
4. Set IPv4 Prefix to **10.0.0.0/24**
5. Enabled DHCP Server
6. Left IPv6 disabled

![NAT Network Settings](implementation_phases.png)

#### Step 4: Download & Import Kali Linux VM
- Downloaded Kali Linux VirtualBox image (64-bit) from [https://kali.org/get-kali](https://kali.org/get-kali)
- File: `kali-linux-2026.2-virtualbox-amd64.ova`
- Imported via File → Import Appliance
- Allocated 4 GB RAM and 2 CPU cores to Kali VM

#### Step 5: Configure Kali Linux IP Address
1. Started Kali Linux VM
2. Clicked Network icon (top-right) → **Edit Connections...**
3. Selected **Wired Connection 1** → IPv4 Settings tab
4. Changed Method from "Automatic (DHCP)" to **Manual**
5. Added address:
   - **Address:** 10.0.0.2
   - **Netmask:** 24
   - **Gateway:** 10.0.0.1
6. Set DNS servers to: **8.8.8.8**
7. Saved and restarted network connection

![Kali IP Config](kali_ip_config_guide.png)

**Verification:**
```bash
$ ip addr show eth0
$ ping 10.0.0.1
$ ping 8.8.8.8
```

#### Step 6: Take Baseline Snapshot
- In VirtualBox Manager: Machine → Take Snapshot
- Named: **"Baseline - Fresh Install"**
- Description: Clean Kali Linux before any modifications

---

### Phase 2: Target Deployment

#### Step 7: Install Windows 11 VM
- Created new VM with Windows 11 ISO
- Allocated 4 GB RAM, 2 CPU cores, 60 GB disk
- Installed Windows 11 Pro (Evaluation)
- Configured static IP: **10.0.0.11/24**
- Disabled Windows Firewall temporarily for ping testing
- Took snapshot: **"Win11 - Baseline"**

#### Step 8: Install Windows 10 VM
- Created new VM with Windows 10 ISO
- Allocated 4 GB RAM, 2 CPU cores, 50 GB disk
- Installed Windows 10 Pro (Evaluation)
- Configured static IP: **10.0.0.10/24**
- Took snapshot: **"Win10 - Baseline"**

#### Step 9: Install Windows 7 VM (Optional)
- Created new VM with Windows 7 SP1 ISO
- Allocated 2 GB RAM, 1 CPU core, 40 GB disk
- Configured static IP: **10.0.0.7/24**
- Took snapshot: **"Win7 - Baseline"**

#### Step 10: Install Windows Server 2016 VM (Optional)
- Created new VM with Server 2016 ISO
- Allocated 4 GB RAM, 2 CPU cores, 60 GB disk
- Configured static IP: **10.0.0.16/24**
- Took snapshot: **"Server2016 - Baseline"**

#### Step 11: Install Android 9.x VM
- Downloaded Android-x86 9.0 R2 ISO
- Created new VM with:
  - **Graphics Controller:** VBoxVGA (IMPORTANT - not VMSVGA)
  - **3D Acceleration:** Disabled
  - 2 GB RAM, 1 CPU core, 16 GB disk
- Installed Android 9.0
- Configured Wi-Fi (VirtWifi) with static IP:
  - **IP Address:** 10.0.0.9
  - **Gateway:** 10.0.0.1
  - **Network Prefix:** 24
  - **DNS 1:** 8.8.8.8
- Took snapshot: **"Android9 - Baseline"**

#### Step 12: Inter-VM Ping Testing
From Kali Linux terminal, verified connectivity to all targets:

```bash
# Test Windows 11
ping -c 4 10.0.0.11

# Test Windows 10
ping -c 4 10.0.0.10

# Test Windows 7
ping -c 4 10.0.0.7

# Test Server 2016
ping -c 4 10.0.0.16

# Test Android
ping -c 4 10.0.0.9
```

**Results:** All pings returned successfully with 0% packet loss.

![Ping Test Matrix](ping_test_matrix.png)

---

## 🔧 Troubleshooting & Solutions

### Problem 1: Kali Linux No Internet Connectivity
**Symptom:** After setting static IP 10.0.0.2, Kali could ping gateway (10.0.0.1) but not external sites like google.com.

**Root Cause:** Kali Linux 2026.1+ has a DAD (Duplicate Address Detection) timeout issue with VirtualBox NAT Networks.

**Solution:**
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```
After applying this fix, internet connectivity was restored immediately.

### Problem 2: Android VM Black Screen / Boot Loop
**Symptom:** Android VM would show black screen or get stuck at boot logo.

**Root Cause:** Default graphics controller (VMSVGA) is incompatible with Android-x86 9.0 in VirtualBox.

**Solution:**
1. Powered off Android VM
2. Settings → Display → Graphics Controller
3. Changed from **VMSVGA** to **VBoxVGA**
4. Unchecked **Enable 3D Acceleration**
5. Started VM successfully

### Problem 3: Windows Firewall Blocking Ping Requests
**Symptom:** Kali could not ping Windows 11/10 VMs despite correct IP configuration.

**Root Cause:** Windows Defender Firewall blocks ICMP echo requests (ping) by default.

**Solution:**
1. On Windows VM: Open Windows Defender Firewall
2. Advanced Settings → Inbound Rules
3. Enabled rules: "File and Printer Sharing (Echo Request - ICMPv4-In)"
4. Alternatively, temporarily disabled firewall for private networks during testing

---

## 📸 Screenshots

All configuration screenshots are organized in the `/screenshots` directory:

| Screenshot | Description |
|------------|-------------|
| `01_virtualbox_nat_network.png` | VirtualBox NAT Network configuration (10.0.0.0/24) |
| `02_kali_network_settings.png` | Kali Linux IPv4 manual configuration |
| `03_kali_terminal_ipaddr.png` | Terminal output of `ip addr` command |
| `04_kali_ping_google.png` | Successful ping to 8.8.8.8 from Kali |
| `05_win11_network_settings.png` | Windows 11 static IP configuration |
| `06_win10_network_settings.png` | Windows 10 static IP configuration |
| `07_android_wifi_settings.png` | Android VirtWifi static IP configuration |
| `08_ping_all_targets.png` | Kali terminal showing successful pings to all VMs |
| `09_virtualbox_snapshots.png` | VirtualBox snapshot manager showing all baselines |

*(Note: Replace with your actual screenshots when uploading to GitHub)*

---

## ⚖️ Legal Disclaimer

> **This lab environment is for EDUCATIONAL and AUTHORIZED TESTING PURPOSES ONLY.**
>
> All testing is performed on virtual machines that I own and control within my isolated lab environment. No testing is conducted on external networks, systems, or devices without explicit written permission.
>
> Unauthorized access to computer systems is illegal under the Computer Fraud and Abuse Act (CFAA) and similar laws worldwide. The knowledge gained from this program is intended solely for defensive security purposes and authorized penetration testing.

---

## 🙏 Acknowledgments

- **NetworkWalks Academy** for providing this structured cybersecurity internship program
- **Waqas Karim (CCIE)** for the excellent guidance and video tutorials
- **Offensive Security** for maintaining Kali Linux
- **Oracle** for VirtualBox

---

## 🔗 Connect With Me

- **NetworkWalks:** [https://networkwalks.com](https://networkwalks.com)

---

*Project completed as part of NetworkWalks Cybersecurity Internship - Batch B083 - Week 1*
*Date: September 2026*
