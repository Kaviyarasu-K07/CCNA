# CCNA Practical Lab — LAN Connectivity, RDP & File Sharing

A hands-on CCNA lab demonstrating LAN connectivity between two virtual Windows 7 machines: static IP configuration, `ping` testing, Remote Desktop (RDP), and network file sharing — performed in a QEMU virtual lab environment.

📄 **[Full Lab Report (PDF)](./CCNA_LAN_Lab_Report.pdf)**

---

## 🧪 Lab Overview

| | |
|---|---|
| **Lab Environment** | QEMU Virtual Machines (Windows 7) |
| **Systems Used** | System 1 (Win1) &nbsp;\|&nbsp; System 2 (Win2) |
| **IP Addressing** | `70.0.0.40 /8` (System 1) &nbsp;and&nbsp; `70.0.0.50 /8` (System 2) |
| **Topics Covered** | Static IP configuration, IPv4 ping test, computer name & workgroup setup, name-based ping test, Remote Desktop (RDP), network file sharing |

## 🎯 Objective

Connect two virtual Windows 7 machines over a simulated LAN, configure static IPv4 addressing, verify Layer-3 connectivity, configure host names for name-based access, and enable Remote Desktop and file sharing between the two hosts.

## 📋 What's Covered

1. **Static IP Address Configuration** — assigning static IPv4 addresses via `ncpa.cpl` and verifying with `ipconfig`
2. **Connectivity Test using IP Address** — `ping` between systems using IP addresses
3. **Computer Name & Workgroup Configuration** — renaming hosts via `sysdm.cpl` and joining a common `WORKGROUP`
4. **Connectivity Test using Computer Name** — `ping` between systems using host names
5. **Enabling Remote Desktop (RDP)** — configuring Remote Desktop access on System 1
6. **Establishing a Remote Desktop Connection** — connecting from System 1 to System2 using `mstsc`
7. **Configuring Network File Sharing** — creating and sharing a folder with configured Share Permissions
8. **Verifying File Sharing from the Remote System** — browsing the network from System 2 and accessing the shared folder

## 🛠️ Tools & Commands Used

- `ncpa.cpl` — Network Connections
- `ipconfig` — IP configuration verification
- `ping` — connectivity testing (by IP and by computer name)
- `sysdm.cpl` — System Properties (computer name, workgroup, Remote settings)
- `mstsc` — Remote Desktop Connection
- Windows folder sharing & Share Permissions

## 📁 Repository Contents

```
├── CCNA_LAN_Lab_Report.pdf   # Full lab report with step-by-step screenshots
└── README.md                 # This file
```

## ✅ Result

Both systems successfully:
- Pinged each other with 0% packet loss (by IP and by computer name)
- Established a working Remote Desktop session
- Shared and accessed a folder over the network

---

*Lab performed as part of CCNA practical training.*
