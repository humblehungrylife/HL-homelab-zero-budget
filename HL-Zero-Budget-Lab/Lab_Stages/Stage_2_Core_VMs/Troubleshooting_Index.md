# 🛠️ Troubleshooting Index — Stage 2 Core VMs & Proxmox GPU Passthrough

This troubleshooting index collects the **final working hook script**, common fixes, and diagnostic steps used in the HL Zero‑Budget Homelab.  
Keep this file next to your `Stage_2_Core_VMs/` folder for quick reference.

---

## 🔧 1. Final working VM hook script (GPU detach / reattach)

**Location:** `/etc/pve/qemu-server/hooks/<VMID>.sh`  
**Permissions required:** `root:root`, executable only.

**Create the file:**
```bash
sudo nano /etc/pve/qemu-server/hooks/104.sh
```

**Contents (final tested):**
```bash
#!/bin/bash
# Simple attach/detach hook for GPU passthrough (tested)
# Usage: placed in /etc/pve/qemu-server/hooks/<VMID>.sh
# Note: Change 0000:0a:00.0 and 0000:0a:00.1 to your GPU device IDs

case "$1" in
  pre-start)
    # detach GPU devices from host before VM starts
    echo "pre-start: detaching GPU devices from host"
    echo 1 > /sys/bus/pci/devices/0000:0a:00.0/remove || true
    echo 1 > /sys/bus/pci/devices/0000:0a:00.1/remove || true
    sleep 2
    ;;
  post-stop)
    # re-scan PCI bus to reattach devices to host after VM stops
    echo "post-stop: rescanning PCI bus"
    echo 1 > /sys/bus/pci/rescan || true
    sleep 2
    ;;
  *)
    # no-op for other hook events
    ;;
esac
```

**Make it executable and owned by root:**
```bash
sudo chown root:root /etc/pve/qemu-server/hooks/104.sh
sudo chmod 0755 /etc/pve/qemu-server/hooks/104.sh
```

> ✅ *Important*: **Do not** modify the script to run other system commands; keep it minimal (detach → sleep → rescan). This script was the final working version used in the lab.

---

## 🩺 2. Basic diagnostic commands

Run these to collect info when something breaks:

```bash
# Kernel & IOMMU
dmesg | egrep -i "iommu|dmar|vfio|vfio-pci|amd_iommu|intel_iommu"

# PCI & device IDs
lspci -nnk | egrep -i "vga|audio|pci"

# IOMMU groups
for d in /sys/kernel/iommu_groups/*/devices/*; do echo $d; lspci -nns ${d##*/}; done

# Check driver binding for a device (replace the vendor id)
lspci -nnk -d 1002:744c

# Proxmox and system logs
journalctl -u pveproxy --no-pager --since "1 hour ago"
journalctl -u pve-cluster --no-pager --since "1 hour ago"
tail -n 200 /var/log/syslog
```

---

## ⚠️ 3. Common issues & fixes

### Host boots but GPU not available to VMs
- **Symptom:** VM fails to start with black screen or host keeps the GPU driver.
- **Fix:**
  - Ensure `options vfio-pci ids=<vendor:device>,<vendor:device>` exists in `/etc/modprobe.d/vfio.conf`.
  - Do **not** blacklist `amdgpu` unless you want the host to lose the GPU display.
  - Check `lspci -nnk` shows `vfio-pci` as the driver for the GPU when passthrough intended.

### Black screen / no video in VM after passthrough
- **Fix:**
  - Use `qm stop <vmid> && sleep 10` on the VM that last used the GPU before starting the new VM.
  - Confirm VM uses `machine: q35` and `hostpci0: 0000:0a:00,pcie=1,x-vga=1`.
  - Try adding `x-vga=1` if using legacy drivers.
  - Ensure OVMF (UEFI) firmware is selected for the VM.

### Host becomes unresponsive after blacklisting GPU drivers
- **Fix:**
  - Boot into recovery or use alternate console and remove GPU blacklists from `/etc/modprobe.d/blacklist.conf`.
  - Rebuild initramfs: `update-initramfs -u -k all` and reboot.

### GPU fails to reattach after VM stops
- **Fix:**
  - Run manual rescan: `echo 1 > /sys/bus/pci/rescan`
  - If that doesn't work, check `dmesg` for errors and try a host reboot.

### vfio-pci not binding
- **Fix:**
  - Confirm `ids=` uses correct lspci IDs (format `vvvv:dddd`).
  - Run `update-initramfs -u` if you changed modprobe configs and reboot if necessary.
  - Verify `/etc/modprobe.d/vfio.conf` is readable and correct.

### Networking: pfSense WebGUI unreachable / LAN VMs have no internet
- **Fix:**
  - Remember to run `pfctl -d` in the Proxmox shell only when first connecting to the pfSense console (temporary disable to allow initial WebGUI access).
  - Verify bridge assignment: `vnet0 → vmbr0` and `vnet1 → vmbr1`.
  - From a LAN VM run: `ip r`, `ping 10.0.0.1`, `ping 8.8.8.8`.
  - Check pfSense dashboard `Status → Interfaces` for interface UP and assigned IPs.

### Storage: VM disk not available / backups failing
- **Fix:**
  - Verify `/mnt/pve/HDD_8TB` mount with `df -h`.
  - Check Proxmox storage config in Datacenter → Storage.
  - Ensure permissions: `chmod 755 /mnt/pve/HDD_8TB` and `chown root:root /mnt/pve/HDD_8TB`.
  - Clean old snapshots and backups if space low.

---

## 🧾 4. Logs & where to look

- `/var/log/syslog` — general system events  
- `journalctl -u pveproxy` — Proxmox web UI logs  
- `journalctl -u pvedaemon` — QEMU related actions  
- `dmesg` — kernel messages (IOMMU / VFIO / PCI errors)  
- `/var/log/pve/tasks/` — task logs for VM start/stop & errors

---

## 🧰 5. Safe rollback & snapshot strategy

- Take snapshots before making network, firewall, or GPU changes:
  - `Week1_pfSense_Stable_No_pfctl`
  - `Week2_Ubuntu_Server_Stable`
  - `Week2_Ubuntu_Desktop_GPU_Ready`
  - `Week2_Kali_Server_Stable`
  - `Week2_Kali_Desktop_GPU_Ready`
- If a change breaks the host, boot from a rescue environment or attach storage to another host to recover files.

---

## 🧩 6. Small checklist before changing GPU settings

1. Take host snapshot/checkpoint and relevant VM snapshots.  
2. Stop any VM currently using the GPU: `qm stop <vmid> && sleep 10`.  
3. Confirm `vfio.conf` is ready (but do not blacklist host drivers unless intended).  
4. Apply host changes (GRUB, initramfs) only if you accept potential host display loss.  
5. Test VM start → if broken, revert snapshot.

---

## 📝 7. Helpful one-liners

```bash
# Check what driver a PCI device uses (replace 0000:0a:00.0)
lspci -s 0000:0a:00.0 -k

# Stop a VM and wait 10s (safe GPU reset)
qm stop 103 && sleep 10

# Rescan PCI bus
echo 1 | sudo tee /sys/bus/pci/rescan

# View last 200 syslog lines
tail -n 200 /var/log/syslog
```

---

## 🔒 8. Security reminders

- Do **not** store public IPs, admin passwords, or private keys in plaintext under the repo.  
- Keep backups and snapshots on `HDD_8TB` encrypted if storing sensitive VMs.

---

If you'd like, I can also write this index to `/mnt/data/Troubleshooting_Index.txt` and provide a download link — say the word and I'll save it now.
