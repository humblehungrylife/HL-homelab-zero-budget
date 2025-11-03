# 🖥️ Stage 2 — Core VM Setup: Ubuntu Desktop (103)

Ubuntu Desktop provides a **GUI environment** within the internal LAN network, used for productivity, browser testing, and multimedia verification.

---

## ⚙️ 1. VM Configuration in Proxmox

| Setting | Value |
|:--|:--|
| VM ID | 103 |
| Name | Ubuntu-Desktop |
| ISO | ubuntu-24.04-desktop-amd64.iso |
| Machine | Q35 |
| BIOS | OVMF (UEFI) |
| CPU | 6 Cores |
| Memory | 12 GB |
| Disk | 128 GB (on HDD_8TB) |
| Network Device 1 | **vnet0 → vmbr1 (LAN)** |
| Display | Default (SPICE or VGA) |
| Audio Device | Intel HDA (ICH9) |

> Ensure **Network Device → vmbr1 (LAN)** for pfSense connectivity (10.0.0.x subnet).

---

## 💿 2. Installation Steps

1. Boot ISO → Select **Try or Install Ubuntu**.  
2. Choose **Install Ubuntu**.  
3. Language → default.  
4. Installation Type → **Normal installation**.  
5. Updates & other software → check both options (download updates, install 3rd party).  
6. Select entire disk → continue.  
7. Create user → set your credentials.  
8. Allow system to finish installation and reboot.  
9. Login to desktop.

---

## 🧰 3. Install Guest Tools & QEMU Agent

After first boot:

```bash
sudo apt update
sudo apt install -y qemu-guest-agent spice-vdagent
sudo systemctl enable --now qemu-guest-agent
```

This enables clipboard sharing, display scaling, and network integration.

---

## 🔉 4. Audio & Bluetooth (Optional)

If using a USB Bluetooth dongle or Proxmox audio passthrough:

```bash
sudo apt install -y pulseaudio pavucontrol bluez
sudo systemctl restart bluetooth
```

If sound doesn’t work immediately:
- Reboot once after installation.  
- Test using YouTube or a local media file.  
- You can change the audio output in **Settings → Sound → Output → Intel HDA**.

---

## 🌐 5. Connectivity Verification

Run from terminal:

```bash
ping 10.0.0.1 -c 4
ping 8.8.8.8 -c 4
ping google.com -c 4
```

✅ Expected:
- `10.0.0.1` → LAN Gateway reachable  
- `8.8.8.8` → Internet through pfSense  
- `google.com` → DNS resolution confirmed  

If not reachable, run `pfctl -d` inside pfSense temporarily.

---

## 🖥️ 6. Visual Checks

- Verify display scaling and resolution (1920×1080 or 4K depending on GPU passthrough).  
- Confirm network indicator shows connected.  
- Test browser Internet access.  
- Confirm audio playback.

---

## 💾 7. Update & Snapshot

```bash
sudo apt update && sudo apt upgrade -y
```

Then in Proxmox:

**VM 103 → Snapshots → Take Snapshot**

Name:
```
Week2_Ubuntu_Desktop_Stable
```

---

✅ **Status:** Stable  
- Connected to LAN (10.0.0.x)  
- Internet verified (ping 8.8.8.8 & google.com)  
- Audio + Bluetooth functional after restart  
- QEMU agent installed  
- Snapshot taken for rollback  

---
