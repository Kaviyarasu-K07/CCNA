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

----------------------------------------------------------------------LAB 2--ROUTER CONFIGURATION AND TELNET ACESS------------------------------------------------------------------------

# CCNA Lab – Basic Router Configuration & Telnet Access

**Topic:** Router Configuration and Introduction to Telnet
**Tool used:** Cisco Packet Tracer / EVE-NG (Suren Network lab template)

---

## 📌 Objective

To configure a Cisco router's Ethernet interface, verify connectivity from a
Windows PC, and set up local/console authentication as a foundation for
Telnet remote access.

---

## 🖧 Topology

```
 [ROUTER]  e0/0 -------------------- e0  [Windows PC]
 50.0.0.1/8                          50.0.0.10/8
        \____________ SSH - TELNET ____________/
```

| Device  | Interface | IP Address | Subnet Mask   |
|---------|-----------|------------|---------------|
| Router  | Ethernet0/0 | 50.0.0.1 | 255.0.0.0 (/8) |
| Win PC  | LAN Adapter | 50.0.0.10 | 255.0.0.0 (/8) |

---

## ⚙️ Step 1 – Configure the Router Interface

```
ROUTER#enable
ROUTER#config term
ROUTER(config)#interface ethernet0/0
ROUTER(config-if)#ip address 50.0.0.1 255.0.0.0
ROUTER(config-if)#no shutdown
ROUTER(config-if)#exit
ROUTER(config)#exit
```

Console log confirms the interface came up:

```
*SYS-5-CONFIG_I: Configured from console by console
```

---

## 💻 Step 2 – Configure the Windows PC IP Address

1. Open Command Prompt → run `ncpa.cpl` to open Network Connections.
2. Open **Local Area Connection Properties → Internet Protocol Version 4 (TCP/IPv4) → Properties**.
3. Set a static IP:
   - IP address: `50.0.0.10`
   - Subnet mask: `255.0.0.0`
   - Default gateway: *(left blank – same subnet as router)*

Verify with:

```
C:\Users\user>ipconfig

Ethernet adapter Local Area Connection:
   IPv4 Address. . . . . . . . . . . : 50.0.0.10
   Subnet Mask . . . . . . . . . . . : 255.0.0.0
```

---

## 🔍 Step 3 – Verify Connectivity (Ping)

```
C:\Users\user>ping 50.0.0.1

Reply from 50.0.0.1: bytes=32 time=2ms TTL=255
Reply from 50.0.0.1: bytes=32 time=1ms TTL=255
Reply from 50.0.0.1: bytes=32 time=1ms TTL=255
Reply from 50.0.0.1: bytes=32 time<1ms TTL=255

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

✅ Router and PC can reach each other over the 50.0.0.0/8 network.

Router-side confirmation:

```
ROUTER#show ip int br
Interface       IP-Address   OK? Method Status   Protocol
Ethernet0/0     50.0.0.1     YES manual up       up
Ethernet0/1     unassigned   YES TFTP   down      down
Ethernet0/2     unassigned   YES TFTP   down      down
Ethernet0/3     unassigned   YES TFTP   down      down
```

---

## 🔐 Step 4 – Local Username & Console Login

```
ROUTER#config term
ROUTER(config)#username Kavi password 12345
ROUTER(config)#line console 0
ROUTER(config-line)#login local
ROUTER(config-line)#exit
ROUTER(config)#exit
```

Console login prompt now requires the local username/password:

```
User Access Verification
Username: Kavi
Password:
ROUTER>
```

---

## 🔑 Step 5 – Enable Secret Password

```
ROUTER(config)#enable secret Kaviyarasu
```

Test:

```
ROUTER>enable
Password:
ROUTER#
```

> **Note:** An attempt to run `login` on `line console 0` without first
> setting a line password produced the expected error:
> `% Login disabled on line 0, until 'password' is set`
> — this is normal Cisco IOS behavior: `login` (without `local`) requires a
> `password` to be configured on that line first.

---

## 🌐 Step 6 – Prepare VTY Lines for Telnet

```
ROUTER(config)#line vty 0 4
ROUTER(config-line)#transport input telnet
ROUTER(config-line)#password 7890
```

This enables Telnet access on VTY lines 0–4 with password `7890`,
setting up the router to accept remote Telnet sessions from the PC in the
next stage of the lab.

---

## 📝 Key Learnings

- `no shutdown` is required to bring an interface administratively up.
- Static IP configuration must match on both ends of a link for connectivity (same subnet/mask).
- `ping` and `show ip int br` are the primary tools to verify Layer 3 reachability.
- `username <name> password <pwd>` + `line console 0` + `login local` enables local database authentication.
- `enable secret` sets an **encrypted** privileged-mode password (preferred over `enable password`).
- `line vty 0 4` + `transport input telnet` + `password` is the minimum setup to allow Telnet remote access.

---

## 📷 Reference Screenshots

Screenshots from the original lab session are included in this repo under `screenshots/` (topology diagram, router CLI, and Windows network configuration steps).

---

*Lab documented as part of CCNA practice – Router Configuration & Intro to Telnet.*

*Lab performed as part of CCNA practical training.*
