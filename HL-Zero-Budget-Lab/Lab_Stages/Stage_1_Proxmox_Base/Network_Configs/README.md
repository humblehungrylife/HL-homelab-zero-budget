# 🌐 Network Configuration — vmbr0 (WAN) & vmbr1 (LAN)

This document records the **final verified Proxmox network bridge setup**  
for the HL Zero-Budget Homelab project.  
It reflects the configuration used for the stable pfSense VM (101) and subsequent core VMs.

---

## ⚙️ 1. Overview

| Bridge | Connected To | Purpose | VM Interface | Notes |
|:--------|:--------------|:---------|:--------------|:------|
| **vmbr0** | Physical NIC → Home Router (192.168.1.x) | **WAN** | `vnet0` | Internet-facing bridge |
| **vmbr1** | Internal virtual bridge (no physical port) | **LAN** | `vnet1` | Internal network for VMs |

---

## 🧩 2. Verify Physical NIC

In the host shell:

```bash
ip link show
```

Example output:
```
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
```

In this build, **eno1** is the physical interface connected to your home router.

---

## 🧱 3. Configure Proxmox Bridges

Edit `/etc/network/interfaces`:

```bash
nano /etc/network/interfaces
```

Final configuration:

```
auto lo
iface lo inet loopback

iface eno1 inet manual

auto vmbr0
iface vmbr0 inet dhcp
    bridge-ports eno1
    bridge-stp off
    bridge-fd 0
    comment: WAN Bridge → Router → pfSense WAN (vnet0)

auto vmbr1
iface vmbr1 inet static
    address 10.0.0.1/24
    bridge-ports none
    bridge-stp off
    bridge-fd 0
    comment: LAN Bridge → pfSense LAN (vnet1) → Internal VMs
```

Apply changes:

```bash
systemctl restart networking
```

> ⚠️ Ensure **vmbr0** obtains a DHCP lease from your router (for Internet access).

---

## 🔌 4. pfSense VM Network Setup

**VM ID 101** — pfSense Firewall

| Adapter | Bridge | Function | IP Address |
|:--------|:--------|:----------|:------------|
| **vnet0** | vmbr0 | WAN | DHCP → 192.168.1.70 |
| **vnet1** | vmbr1 | LAN | Static → 10.0.0.1 |

Access pfSense via LAN IP → https://10.0.0.1

---

## ⚠️ 5. About the IP Addresses Used

All IPs used in this documentation (192.168.x.x and 10.x.x.x) are **private, non-routable addresses**  
defined under [RFC1918](https://datatracker.ietf.org/doc/html/rfc1918).  
They are completely **safe to publish** because they only exist within your **local home or lab network**.

If desired, you can mask them for GitHub visibility:

| Actual | Example (Masked) |
|:--------|:----------------|
| 192.168.1.70 | 192.168.1.XX |
| 10.0.0.1 | 10.0.0.1 (Standard LAN Gateway) |

> These IPs cannot be used to reach your real system from the Internet.

---

## 🔍 6. Test Connectivity

From pfSense shell or GUI:
```bash
ping 8.8.8.8
ping google.com
```

From a LAN VM (e.g., Ubuntu 102):
```bash
ping 10.0.0.1
ping 8.8.8.8
```

✅ Expected results:
- LAN VM → Internet works  
- DNS resolves correctly  
- pfctl manual disable not required  

---

## 🧭 7. Topology Diagram

```
                     Internet
                         │
                  ┌───────────────┐
                  │ Home Router   │
                  │ 192.168.1.254 │
                  └───────────────┘
                         │
                 (vmbr0 → vnet0 WAN)
                         │
                ┌─────────────────────┐
                │ pfSense VM (101)   │
                │ WAN: 192.168.1.70  │
                │ LAN: 10.0.0.1      │
                └─────────────────────┘
                         │
                 (vmbr1 → vnet1 LAN)
                         │
          ┌─────────────────────────────┐
          │ Internal VMs (102, 104, etc.) │
          │ 10.0.0.x/24 via pfSense LAN  │
          └─────────────────────────────┘
```

---

## 🧩 8. Troubleshooting

<details>
<summary>VM has no Internet connectivity</summary>

- Confirm correct bridge assignment (vnet0 → vmbr0, vnet1 → vmbr1)  
- Restart networking: `systemctl restart networking`  
- In pfSense: `Status → Interfaces` → ensure both WAN and LAN show ✓ UP
</details>

<details>
<summary>pfSense WebGUI not loading from LAN VM</summary>

- Ensure LAN VM uses DHCP or gateway `10.0.0.1`  
- Verify firewall rule: `LAN → any → allow` exists in pfSense
</details>

---

✅ **Status:** Stable  
- vmbr0 (WAN) ↔ Router verified  
- vmbr1 (LAN) ↔ Internal VMs working  
- pfSense WebGUI accessible at 10.0.0.1  
- Internet accessible from LAN VMs  
- No pfctl override needed  

---
