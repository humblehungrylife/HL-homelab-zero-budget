# 🧱 Stage 2 — Core VM Setup: pfSense (VM 101)

This document details the **pfSense firewall setup** for the HL Zero-Budget Homelab project.  
pfSense acts as the network gateway and firewall between the WAN (home router) and LAN (internal VMs).

---

## ⚙️ 1. pfSense VM Configuration in Proxmox

| Setting | Value |
|:---------|:------|
| VM ID | 101 |
| Name | pfSense |
| ISO | pfSense-CE-2.7.x |
| Machine | Q35 |
| BIOS | OVMF (UEFI) |
| CPU | 2 Cores |
| Memory | 4 GB |
| Disk | 32 GB (on HDD_8TB) |
| Network Device 1 | **vnet0 → vmbr0 (WAN)** |
| Network Device 2 | **vnet1 → vmbr1 (LAN)** |
| Display | Default (VGA) |

✅ Confirm bridge mapping before booting:  
- **vmbr0 → WAN (Home Router)**  
- **vmbr1 → LAN (Internal Network)**

---

## 🧩 2. pfSense Installation Steps

1. Boot the pfSense ISO.
2. Choose **Install pfSense (Quick/Default)**.
3. Select default keymap and install to your 32 GB virtual disk.
4. Once installation completes, pfSense will reboot automatically.

---

## 🌐 3. Assign Interfaces (Console Setup)

Once pfSense reboots into the console menu, type:

```
2
```
to **Assign Interfaces**.

Follow these exact steps:

1. **Assign LAN first**, then WAN.  
2. When prompted:
   - **Enter interface name for LAN:** → `vtnet1`
   - **Enter interface name for WAN:** → `vtnet0`
3. For all other prompts, just press **Enter** to skip (no VLANs or additional interfaces needed).
4. Confirm configuration when asked.

pfSense will then assign:
- **LAN (vtnet1)** → Static 10.0.0.1/24  
- **WAN (vtnet0)** → DHCP (from your home router, e.g., 192.168.1.70)

---

## 🔒 4. Access the pfSense Web Interface

From your Proxmox shell, temporarily disable the pfSense firewall to allow LAN connectivity:

```bash
pfctl -d
```

> This command is essential for **first-time access** to the WebGUI.  
> Once inside and setup is complete, pfSense automatically re-enables its firewall.

Now access the WebGUI from your host browser or LAN VM:

```
https://10.0.0.1
```

Default credentials:
```
Username: admin
Password: pfsense
```

---

## 💡 5. Initial WebGUI Setup

1. Complete the Setup Wizard:
   - Hostname: `pfsense`
   - Domain: `local.lan`
   - Primary DNS: `8.8.8.8`
   - WAN: DHCP
   - LAN: 10.0.0.1/24
   - Admin password: (set your own)

2. Allow pfSense to reload and apply all settings.

---

## 🧱 6. Verify Internet & LAN Connectivity

From pfSense shell:

```bash
ping 8.8.8.8
```

From Ubuntu Server (once installed next):

```bash
ping 10.0.0.1
ping 8.8.8.8
```

✅ Expected results:
- pfSense WAN obtains IP from router (192.168.1.x)
- pfSense LAN gateway reachable (10.0.0.1)
- LAN → Internet connectivity confirmed

---

## 🔁 7. Create a Snapshot

Once pfSense WebGUI and network are verified:

**Proxmox → VM 101 → Snapshots → Take Snapshot**

Name it:
```
Week1_pfSense_Stable_No_pfctl
```

> This ensures you can always roll back before testing new rules or firewall changes.

---

## ⚙️ 8. Important Next Step

Install **Ubuntu Server (VM 102)** next — it is required to provide LAN Internet access for other VMs.  
Ubuntu Server will serve as your first LAN client and connectivity test point.

---

✅ **Status:** Stable  
- pfSense fully installed and configured  
- pfctl -d verified for initial WebGUI access  
- LAN configured first during interface setup (vtnet1 → LAN, vtnet0 → WAN)  
- WebGUI reachable at https://10.0.0.1  
- Ready for Ubuntu Server deployment (next stage)

---
