# 🧱 Stage 2 — Core VM Setup: Ubuntu Server (102)

Ubuntu Server acts as the **first LAN client** inside the Zero-Budget Homelab.  
It confirms that pfSense routing, DHCP, and DNS resolution are working correctly for all future VMs.

---

## ⚙️ 1. VM Configuration in Proxmox

| Setting | Value |
|:--|:--|
| VM ID | 102 |
| Name | Ubuntu-Server |
| ISO | ubuntu-24.04-live-server-amd64.iso |
| Machine | Q35 |
| BIOS | OVMF (UEFI) |
| CPU | 4 Cores |
| Memory | 8 GB |
| Disk | 64 GB (on HDD_8TB) |
| Network Device 1 | **vnet0 → vmbr1 (LAN)** |
| Display | Default (VGA) |

> 🔗 Ensure the network device is attached to **vmbr1 (LAN)** so the server receives an IP from pfSense (10.0.0.x).

---

## 💿 2. Installation Steps

1. Boot the ISO and choose **Install Ubuntu Server**.  
2. Language → Keyboard → default (press Enter).  
3. Network Configuration → **DHCP (auto)** – accept 10.0.0.x assigned by pfSense.  
4. Proxy → (blank) → press Enter.  
5. Mirror → default archive.ubuntu.com.  
6. Storage → Use entire disk → Done.  
7. Profile → set your username & password.  
8. SSH → ✅ Enable “Install OpenSSH Server.”  
9. Wait for installation → Reboot → login as your user.

---

## 🔌 3. Install QEMU Agent (for Proxmox Integration)

```bash
sudo apt update
sudo apt install -y qemu-guest-agent
sudo systemctl enable --now qemu-guest-agent
```

---

## 🌐 4. Verify LAN IP and Gateway

```bash
ip a
```

Expected output shows an IP like `10.0.0.10/24` on interface `ens18`.

Check gateway:

```bash
ip r
```

You should see `default via 10.0.0.1`.

---

## 🧠 5. Connectivity Tests

Run the following from the Ubuntu shell to confirm pfSense routing and Internet access:

```bash
ping 10.0.0.1 -c 4
ping 8.8.8.8 -c 4
ping google.com -c 4
```

✅ Expected:
- `10.0.0.1` responds → LAN gateway reachable.  
- `8.8.8.8` responds → Internet routing works through pfSense.  
- `google.com` resolves → DNS functional.

If `ping 8.8.8.8` fails, verify pfSense LAN rules or temporarily use:

```bash
pfctl -d
```

inside the pfSense console.

---

## 💾 6. System Update & Snapshot

After successful pings:

```bash
sudo apt update && sudo apt upgrade -y
```

Then create a snapshot in Proxmox:

**Proxmox → VM 102 → Snapshots → Take Snapshot**

Name:
```
Week2_Ubuntu_Server_Stable
```

---

✅ **Status:** Stable  
- LAN IP assigned (10.0.0.x)  
- pfSense gateway reachable  
- Internet connectivity verified (`ping 8.8.8.8`, `ping google.com`)  
- QEMU agent installed  
- Snapshot taken for rollback  

---
