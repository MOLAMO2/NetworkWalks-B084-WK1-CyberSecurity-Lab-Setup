# 
![PM1 Cybersecurity Lab banner](assets/banner.svg)

# 🛡️ NetworkWalks-B084-WK1-CyberSecurity-Lab-Setup

📅 **Week 1** • 📂 **PM1 Cybersecurity Lab** • 🏷️ **Batch B084** • 🎓 **NetworkWalks Academy • CyberSecurity Training**

![](https://img.shields.io/badge/Kali%20Linux-2026.3-557C94?style=for-the-badge&logo=kalilinux&logoColor=white) ![](https://img.shields.io/badge/VirtualBox-7.2.22-183A61?style=for-the-badge&logo=virtualbox&logoColor=white) ![](https://img.shields.io/badge/IPv4-Static-2f6feb?style=for-the-badge) ![](https://img.shields.io/badge/Tests-Passed-brightgreen?style=for-the-badge)

---

# · Overview and Setup

## 🚀 Overview

The goal of Week 1 is to build a virtual cybersecurity lab using Oracle VirtualBox and Kali Linux to support ethical hacking, penetration testing, network configuration, and daily cybersecurity practice.

The lab provides a controlled environment where security tools can be explored, network connectivity tested, and practical exercises performed safely without affecting production systems.

Week 1 builds the foundation of the lab: **Kali Linux 2026.3** on **Oracle VirtualBox 7.2.22**, a private **NAT Network**, a **static IPv4 address**, verified connectivity and a **snapshot** to restore from.

## 🌐 Lab Topology

```text
💻 Windows 11 Host
        │
        ▼
📦 VirtualBox 7.2.22
        │
        ▼
🌐 NAT Network (10.0.0.0/24)
        │
        ▼
🐉 Kali Linux 2026.3
   IP: X.X.X.X
        │
        ▼
🚪 Gateway / DNS
   10.0.0.1
        │
        ▼
☁️ Internet
```

## 🎯 Purpose of the Lab

A safe, isolated and repeatable environment for learning cybersecurity by doing, without putting real systems at risk.

|  | Purpose | How the lab delivers it |
| --- | --- | --- |
| 🛡️ | **Safe practice** | Everything runs in a private NAT Network on virtual machines |
| 🌐 | **Networking fundamentals** | Static IPv4, routing, gateway and DNS configured and verified by hand |
| 🐉 | **Hands-on Kali Linux** | A real security workstation for Linux administration and tooling |
| 🔁 | **Repeatable baseline** | Snapshots let you roll back after any experiment |
| 🚀 | **Foundation for later work** | Ready for reconnaissance, vulnerability assessment and ethical hacking exercises |

## 🧩 Lab more details 

|  |  |
| --- | --- |
| 💻 **Host** | Windows 11 · VirtualBox 7.2.22 |
| 🐉 **Guest** | Kali Linux 2026.3 (`kali-linux-2026.3-virtualbox-amd64`) |
| 🌐 **Network** | NAT Network `NatNetwork` · `10.0.0.0/24` · DHCP server enabled |
| 📍 **Kali IP** | `X.X.X.X/24` on `eth0` · method **Manual** · profile `LabTest` |
| 🚪 **Gateway / DNS** | `10.0.0.1` |

## 📋 Requirements

|  | Requirement | Used for |
| --- | --- | --- |
| ✅ | **Windows 11 host** | Runs VirtualBox and the lab VM |
| ✅ | **7-Zip / WinRAR** | Extracting the Kali VM archive |
| ✅ | **Oracle VirtualBox 7.2.22** | Hypervisor for the lab |
| ✅ | **VirtualBox Extension Pack** | Extra VirtualBox features; must match the VirtualBox version |
| ✅ | **Kali Linux 2026.3** VirtualBox image | The security workstation VM |

> \[Be Alert\] Hardware virtualization (Intel VT-x / AMD-V) must be enabled in BIOS/UEFI to run a 64-bit guest.

## 🪜 Setup

**1 · Add the VM.** Extract the Kali archive with 7-Zip or WinRAR, then use **Machine → Add** in VirtualBox and boot it.

<img width="1895" height="997" alt="kali-desktop " src="https://github.com/user-attachments/assets/f1da37d3-ff41-4e0e-b7eb-c9fc736be369" />
#
<img width="1915" height="1185" alt="kali-desktop" src="https://github.com/user-attachments/assets/3690a7bf-7fbd-408f-9028-75d03131df5d" />


• Kali Linux 2026.3 running in VirtualBox

**2 · Create the NAT Network.** **File → Tools → Network Manager → NAT Networks → Create**, name `NatNetwork`, prefix `10.0.0.0/24`. Then attach the VM: **Settings → Network → Adapter 1 → NAT Network → `NatNetwork`**.
<img width="1901" height="1187" alt="virtualbox-nat-network" src="https://github.com/user-attachments/assets/5b2bdbdd-497a-4dd3-bf62-907069b2c318" />
#
<img width="1917" height="1198" alt="image" src="https://github.com/user-attachments/assets/baa9738c-5fc7-4d4f-a59d-6a6aa038db45" />

• NatNetwork · 10.0.0.0/24 · DHCP enabled

**3 · Set the static IP.** In Kali, edit the Ethernet connection (`LabTest`) → **IPv4 Settings** → Method **Manual**: address `10.0.0.2`, netmask `24`, gateway `10.0.0.1`, DNS `10.0.0.1`.

<img width="1911" height="1166" alt="ipv4-manual-settings" src="https://github.com/user-attachments/assets/b2938ecb-25c5-4fff-81b1-4846ddb7fe9f" />

• NetworkManager manual IPv4 profile

> \[!NOTE\] The NAT Network keeps its DHCP server on, but Kali bypasses it with the manual address. Pick static addresses deliberately to avoid clashes.

---

# · Verification

## 📡 Test Connectivity

Test from the inside out, so a failure shows which layer is broken:

```bash
ip -br addr && ip route        # address and default route (expect: default via 10.0.0.1 proto static)
ping -c 4 10.0.0.1             # gateway
ping -c 4 8.8.8.8              # internet by IP
nslookup google.com            # DNS through 10.0.0.1
curl -I https://google.com     # HTTPS
```

## ✅ Results

| Test | Result | Detail |
| --- | --- | --- |
| 🚪 Gateway | ✅ | 4/4 packets, \~1.5 ms |
| 🌍 Internet | ✅ | `8.8.8.8` and `1.1.1.1`, \~5–7 ms |
| 🔎 DNS | ✅ | Served by `10.0.0.1`, IPv4 and IPv6 answers |
| 🔒 HTTPS | ✅ | `HTTP/2 301` → `https://www.google.com/` |

#
**0% packet loss** on every test. A timestamped report, `network_test_20261027_162420.log`, was saved to the Desktop (Tue Oct 27 2026, 04:24 PM EDT).
![Connectivity tests and log]<img width="1917" height="1147" alt="connectivity-tests-and-log" src="https://github.com/user-attachments/assets/4c291580-9e84-4e6b-a254-c1589b5efab7" />

• Live tests (left) and the generated report (right)

---

# · Troubleshooting, Snapshot and Wrap-up

## 🧯 Troubleshooting

| Mistake | What happened | Lesson |
| --- | --- | --- |
| `"10.0.0.1 "` (space inside quotes) | `Name or service not known` | The IP became an invalid hostname |
| `google,com` and missing `-c` | `Name or service not known` | Typos break name resolution |
| `googlr.com` | Replied normally | Misspelled domains can exist, which is how typosquatting works |

![Troubleshooting]<img width="1041" height="1197" alt="ip-route-and-troubleshooting" src="https://github.com/user-attachments/assets/25010f8f-b4cf-4f66-be13-18a3e069c447" />

• Errors, fixes and the final route check

## 📸 Snapshot

Snapshot `Kali-2026-baseline-network configured` preserves the working lab. The tree also holds `Fresh Installation` and `New Patch update`.

![Snapshots]<img width="1656" height="1197" alt="virtualbox-snapshots" src="https://github.com/user-attachments/assets/fe2c3f13-3b0a-4b7d-a7e5-411c431c3d6d" />

VirtualBox Snapshots tab

## 🔐 Safe Use

> \[!WARNING\] Authorised, ethical learning only. Test only systems you own or have written permission to assess, keep the lab isolated, snapshot before big changes and never commit credentials.

## ⏭️ Reference 

- [ ] Sponsored by: NetworkWalks Academy
- [ ] Mentor and Instructor: Waqas Karim (CCIE)
- [ ] Email: waqasulkarim@gmail.com

---

⚡ **Learning • Practicing • Securing** ⚡\
🎓 NetworkWalks CyberSecurity Training · Batch B084 · **Author:** Molamo
