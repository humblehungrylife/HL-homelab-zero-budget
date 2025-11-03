# 🧱 Stage 2 — Core VM Setup: pfSense (VM 101)

This document details the **pfSense firewall setup** for the HL Zero‑Budget Homelab project.  
pfSense acts as the secure network gateway and firewall between the WAN (home router) and LAN (internal VMs).

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
- **vmbr0 → WAN (connected to router)**  
- **vmbr1 → LAN (internal network)**

---

## 🧩 2. Installation Steps

1. Boot the pfSense ISO.  
2. Choose **Install pfSense (Quick/Default)**.  
3. Select default keymap and install to your virtual disk.  
4. When installation completes, pfSense reboots automatically.

---

## 🌐 3. Assign Interfaces (Console Setup)

Once pfSense reboots into the console menu, type:

```
2
```
to **Assign Interfaces**.

Follow these steps:

1. **Assign LAN first**, then WAN.  
2. When prompted:  
   - **LAN:** `vtnet1`  
   - **WAN:** `vtnet0`  
3. For all other prompts, just press **Enter** to skip (no VLANs).  
4. Confirm configuration when asked.

pfSense will then assign:
- **LAN (vtnet1)** → Static 10.0.0.1/24  
- **WAN (vtnet0)** → DHCP (from your router)

---

## 🔒 4. Access the pfSense Web Interface

From your Proxmox shell, temporarily disable the pfSense firewall to allow LAN connectivity:

```bash
pfctl -d
```

> This command is needed **only once** during initial setup to reach the WebGUI.  
> pfSense automatically re-enables its firewall after configuration.

Then access the WebGUI from your host browser or LAN VM:

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

Complete the Setup Wizard:

- Hostname: `pfsense`  
- Domain: `local.lan`  
- Primary DNS: `8.8.8.8`  
- WAN: DHCP  
- LAN: 10.0.0.1/24  
- Set your own admin password

Let pfSense apply and reload.

---

## 🧱 6. Verify LAN & Internet Connectivity

From pfSense shell:

```bash
ping 8.8.8.8
```

From Ubuntu Server (VM 102, next setup):

```bash
ping 10.0.0.1
ping 8.8.8.8
```

✅ Expected results:
- pfSense WAN gets DHCP IP from your router  
- LAN gateway reachable (10.0.0.1)  
- LAN → Internet connection working

---

## 🔧 7. Safe Firewall & Access Rules

After confirming Internet access, apply these **minimal safe rules**:

### LAN Rules (allow traffic to WAN and pfSense GUI)
Navigate to **Firewall → Rules → LAN** and ensure the following rule exists:

| Action | Interface | Source | Destination | Protocol | Description |
|:--|:--|:--|:--|:--|:--|
| ✅ Pass | LAN | LAN net | any | any | Allow LAN → any |

> This rule allows internal LAN VMs to access the Internet and the pfSense GUI.  
> The GUI remains *accessible only from LAN* — not from WAN.

### WAN Access (block management by default)
- Go to **System → Advanced → Admin Access**  
- Confirm that “WebGUI accessible from WAN” is **unchecked**  
- Keep management restricted to LAN only

### DHCP & DNS
Enable LAN DHCP for VM automation:
- **Services → DHCP Server → LAN**
  - Range example: `10.0.0.50 – 10.0.0.200`
- **Services → DNS Resolver → Enable DNS Resolver** (keep defaults)

### ICMP (Ping)
- **Firewall → Rules → LAN → Add**
  - Action: Pass  
  - Protocol: ICMP  
  - Source: LAN net  
  - Destination: any  
  - Description: “Allow ICMP (Ping) LAN → Any”

> These safe rules maintain LAN access, internal DNS, and diagnostics, while keeping WAN management locked out.

---

## 🔁 8. Snapshot

Once verified, take a Proxmox snapshot:

**Proxmox → VM 101 → Snapshots → Take Snapshot**

Name:
```
Week1_pfSense_Stable_SecureRules
```

---

✅ **Final Status:**  
- pfSense fully installed and configured  
- LAN configured first (vtnet1 → LAN, vtnet0 → WAN)  
- WebGUI restricted to LAN only  
- DHCP, DNS, and ICMP active on LAN  
- WAN management blocked (secure)  
- Internet access verified  
- Snapshot created for rollback  

---
