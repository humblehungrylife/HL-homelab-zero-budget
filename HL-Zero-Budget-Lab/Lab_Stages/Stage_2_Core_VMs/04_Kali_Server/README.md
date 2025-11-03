# 🧩 Stage 2 — Core VM Setup: Kali Server (104)

Kali Server provides a **headless penetration-testing environment** inside the internal LAN.  
It is used for network scanning, remote access testing, and lab automation within the pfSense-protected network.

---

## ⚙️ 1. VM Configuration in Proxmox

| Setting | Value |
|:--|:--|
| VM ID | 104 |
| Name | Kali-Server |
| ISO | kali-linux-2024.x-installer-amd64.iso |
| Machine | Q35 |
| BIOS | OVMF (UEFI) |
| CPU | 4 Cores |
| Memory | 8 GB |
| Disk | 64 GB (on HDD_8TB) |
| Network Device 1 | **vnet0 → vmbr1 (LAN)** |
| Display | Default (VGA = std) |
| Audio | None (headless) |

> Ensure **Network Device → vmbr1 (LAN)** so the VM receives an IP in the 10.0.0.x range from pfSense.

---

## 💿 2. Installation Steps

1. Boot from the Kali Server ISO.  
2. Choose **Graphical Install** or **Install** (terminal mode is fine).  
3. Language → default.  
4. Hostname → `kali-server`.  
5. Domain → `local.lan`.  
6. Set root password or create a user.  
7. Network → accept **DHCP** (should assign 10.0.0.x from pfSense).  
8. Partition disk → use entire disk.  
9. Select **SSH server** and **standard system utilities** for installation.  
10. Finish install → Reboot.

---

## 🔌 3. Enable QEMU Guest Agent

After first login:

```bash
sudo apt update
sudo apt install -y qemu-guest-agent
sudo systemctl enable --now qemu-guest-agent
```

---

## 🌐 4. Verify LAN and Gateway

```bash
ip a
```
Confirm you have an IP like `10.0.0.20/24`.

Check route:

```bash
ip r
```
Expected: `default via 10.0.0.1`

---

## 🧠 5. Connectivity Tests

```bash
ping 10.0.0.1 -c 4
ping 8.8.8.8 -c 4
ping google.com -c 4
```

✅ Expected:  
- `10.0.0.1` → pfSense gateway reachable.  
- `8.8.8.8` → Internet access through pfSense.  
- `google.com` → DNS resolution confirmed.  

If `ping 8.8.8.8` fails, run:

```bash
pfctl -d
```

inside pfSense to temporarily disable firewall rules for initial testing.

---

## 🧰 6. Recommended Packages

Install base tools for server operations:

```bash
sudo apt install -y net-tools curl wget vim htop neofetch
sudo apt install -y openssh-server
```

For pentesting automation and remote access:

```bash
sudo apt install -y metasploit-framework nmap john hydra
```

---

## 💾 7. System Update & Snapshot

```bash
sudo apt update && sudo apt upgrade -y
```

Then in Proxmox:

**VM 104 → Snapshots → Take Snapshot**

Name:
```
Week2_Kali_Server_Stable
```

---

✅ **Status:** Stable  
- LAN IP (10.0.0.x) confirmed  
- Internet access verified (`ping 8.8.8.8`, `ping google.com`)  
- pfctl -d tested for first-time access  
- QEMU agent installed  
- Base toolset ready for remote ops  
- Snapshot taken for rollback  

---
