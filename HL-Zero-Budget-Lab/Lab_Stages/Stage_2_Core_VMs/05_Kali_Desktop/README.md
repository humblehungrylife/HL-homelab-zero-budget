# 🖥️ Stage 2 — Core VM Setup: Kali Desktop (105)

Kali Desktop provides a **GUI pentesting environment** within the internal LAN (useful for interactive testing, browser-based tools, and GPU-accelerated tasks).  
This guide mirrors the Ubuntu Desktop GPU passthrough options and includes the startup/hand-off procedure for sharing the GPU safely between VMs.

---

## ⚙️ 1. VM Configuration in Proxmox

| Setting | Value |
|:--|:--|
| VM ID | 105 |
| Name | Kali-Desktop |
| ISO | kali-linux-2024.x-desktop-amd64.iso |
| Machine | Q35 |
| BIOS | OVMF (UEFI) |
| CPU | 6 Cores |
| Memory | 12 GB |
| Disk | 128 GB (on HDD_8TB) |
| Network Device 1 | **vnet0 → vmbr1 (LAN)** |
| Display | Default (**VGA = std**) |
| Audio Device | None or Bluetooth (optional) |

> Ensure **Network Device → vmbr1 (LAN)** so the VM receives an IP in the 10.0.0.x subnet from pfSense.

---

## 💿 2. Installation Steps

1. Boot from the Kali Desktop ISO.  
2. Choose **Graphical Install**.  
3. Language → default.  
4. Hostname → `kali-desktop`.  
5. Domain → `local.lan`.  
6. Create user and password.  
7. Network → DHCP (accept 10.0.0.x from pfSense).  
8. Partition disk → use entire disk.  
9. Select desktop environment and SSH if needed.  
10. Finish install → Reboot → login.

---

## 🔌 3. QEMU Agent & Guest Tools

After first login:

```bash
sudo apt update
sudo apt install -y qemu-guest-agent spice-vdagent
sudo systemctl enable --now qemu-guest-agent
```

---

## 🌐 4. Verify Network & Gateway

```bash
ip a
ip r
```

Confirm an IP like `10.0.0.30/24` and `default via 10.0.0.1`.

---

## 🧠 5. Connectivity Tests

Run the standard tests to ensure pfSense routing and DNS work:

```bash
ping 10.0.0.1 -c 4
ping 8.8.8.8 -c 4
ping google.com -c 4
```

If `ping 8.8.8.8` fails, temporarily run on pfSense host:

```bash
pfctl -d
```

to allow first-time access and troubleshoot.

---

## 🎮 6. GPU Passthrough (Same Options as Ubuntu Desktop)

If you plan to use the GPU with Kali Desktop, follow the same passthrough recommendations used for Ubuntu Desktop:

| PCI Device | Description | Action |
|:------------|:-------------|:--------|
| `0000:03:00` | AMD GPU (Display) | ✅ Keep defaults (attach) |
| `0000:03:01` | AMD GPU (Audio) | ❌ Uncheck ROM / primary GPU / extra flags |

- **Display Type:** keep **VGA = std**. Do **not** switch to `none`.  
- **Second device (audio):** leave ROM and extra flags unchecked to avoid conflicts.  
- **Audio:** use a Bluetooth dongle for reliable playback in the VM.

---

## 🔁 7. Safe GPU Handoff Procedure (Start Ubuntu First Unless Using Hooks)

To avoid GPU conflicts, follow this policy:

- If you are **not** using hook scripts, **always start the Ubuntu Desktop VM first**, use it, then stop it before starting Kali Desktop.  
- If you prefer automatic attach/detach, create a hook script (see GPU_Passthrough README) to detach the GPU from the host before VM start and rescan after stop.

Manual flow example (replace VM IDs to match your setup):

1. Stop Ubuntu Desktop (if running):
```bash
qm stop 103 && sleep 10
```

2. Start Kali Desktop (GPU target):
```bash
qm start 105
```

3. When finished, stop Kali Desktop:
```bash
qm stop 105 && sleep 10
```

4. Restart Ubuntu Desktop:
```bash
qm start 103
```

> If you implement hook scripts, they can automate detach/attach and allow starting VMs in either order. Without hooks, follow the start-ubuntu-first policy.

---

## 🔧 8. Troubleshooting GPU & Display

<details>
<summary>Black screen after starting VM</summary>

- Ensure the GPU is bound to `vfio-pci` on the host.  
- Use `qm stop <vmid> && sleep 10` to allow the GPU to reset between VMs.  
- Try `x-vga=1` in the `hostpci` line if using legacy GPU options.
</details>

<details>
<summary>No audio in Kali Desktop</summary>

- Confirm Bluetooth dongle is present and recognized.  
- Install `pulseaudio pavucontrol bluez` and restart bluetooth:  
```bash
sudo apt install -y pulseaudio pavucontrol bluez
sudo systemctl restart bluetooth
```
</details>

---

## 💾 9. Update & Snapshot

```bash
sudo apt update && sudo apt upgrade -y
```

Then create a Proxmox snapshot:

**VM 105 → Snapshots → Take Snapshot**

Name:
```
Week2_Kali_Desktop_GPU_Ready
```

---

✅ **Status:** Stable  
- Kali Desktop integrated on LAN (10.0.0.x)  
- Internet verified (`ping 8.8.8.8` & `ping google.com`)  
- GPU passthrough options mirror Ubuntu Desktop settings  
- Safe GPU handoff policy and hook-script alternative included  
- Snapshot for rollback created

---
