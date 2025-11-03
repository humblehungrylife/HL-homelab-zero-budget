# 🎮 GPU Passthrough — AMD 7900 XTX (Base Configuration)

This document records the verified **GPU passthrough baseline** for the HL Zero-Budget Homelab project.  
It covers enabling **IOMMU**, binding the GPU to **VFIO**, and preparing **hook scripts** without isolating the GPU from the host — allowing Proxmox GUI and dual‑boot Windows 11 to continue using the GPU.

---

## ⚙️ 1. Enable IOMMU in BIOS

Reboot and enter your BIOS (MSI MEG Z790 ACE):

**Settings → Advanced → Integrated Devices**
- ✅ *Enable* **Intel VT-d / AMD-Vi**
- ✅ *Enable* **Above 4G Decoding**
- ✅ *Enable* **Resizable BAR**
- 💡 Secure Boot can remain *Disabled* for passthrough testing.

Save & exit.

---

## 🧩 2. Enable IOMMU in Proxmox GRUB

Edit GRUB configuration:

```bash
nano /etc/default/grub
```

Locate the line:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
```

Modify it as:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
```

*(If using AMD CPU, use `amd_iommu=on iommu=pt`)*

Save and update:

```bash
update-grub
update-initramfs -u -k all
reboot
```

---

## 🔍 3. Verify IOMMU Activation

After reboot:

```bash
dmesg | grep -e IOMMU -e DMAR
```

Expected output includes:
```
DMAR: IOMMU enabled
AMD-Vi: Initialized for IOMMU
```

---

## 🆔 4. Identify GPU Devices

Run:

```bash
lspci -nn | grep -E "VGA|Audio"
```

Example output for **AMD Radeon 7900 XTX**:

```
0a:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Navi 31 [1002:744c]
0a:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Device [1002:ab30]
```

Copy the IDs (in brackets):  
**1002:744c,1002:ab30**

---

## 📦 5. Prepare VFIO (Without Host Blacklist)

You did **not** blacklist or detach the GPU from the Proxmox host  
to preserve full display output for your Windows 11 NVMe dual‑boot setup.

Instead, only prepare the VFIO binding file (for future passthrough use):

```bash
nano /etc/modprobe.d/vfio.conf
```

Add (for reference only — not yet active):
```
options vfio-pci ids=1002:744c,1002:ab30
```

> ⚠️ Do **not** blacklist `amdgpu` or `radeon` since the GPU is still used by Proxmox’s GUI and by Windows when dual‑booted.

You may safely skip `/etc/modprobe.d/blacklist.conf` creation.

---

## 🔍 6. Check IOMMU Groups

```bash
find /sys/kernel/iommu_groups/ -type l
```

Ensure your GPU and its audio function share a clean group (e.g., group 12).  
This confirms the system supports isolated passthrough even without blacklisting.

---

## 🪄 7. Optional Hook Script (Preparation Stage)

Create folder for VM‑specific passthrough hooks:

```bash
mkdir -p /etc/pve/qemu-server/hooks
```

Template script (example for VM 104 Kali):

```bash
nano /etc/pve/qemu-server/hooks/104.conf
```

Content:

```bash
#!/bin/bash
case "$1" in
  pre-start)
    echo "🔒 Detaching GPU from host before VM start"
    echo 1 > /sys/bus/pci/devices/0000:0a:00.0/remove
    echo 1 > /sys/bus/pci/devices/0000:0a:00.1/remove
    ;;
  post-stop)
    echo "🔓 Re‑attaching GPU to host after VM shutdown"
    echo "1" > /sys/bus/pci/rescan
    ;;
esac
```

Make it executable:

```bash
chmod +x /etc/pve/qemu-server/hooks/104.conf
```

---

## 🧰 8. VM Configuration Snippet

When editing `/etc/pve/qemu-server/104.conf`:

Add (example):

```
hostpci0: 0000:0a:00,pcie=1,x-vga=1
machine: q35
```

Then start the VM normally.

---

## ✅ 9. Verification Steps

In Proxmox Shell:

```bash
lspci -nnk -d 1002:744c
```

You should see:
```
Kernel driver in use: vfio-pci
```

If so, passthrough binding is successful.

---

## 🧩 10. Troubleshooting

<details>
<summary>VM starts but black screen or no output</summary>

- Try `x-vga=1` and `romfile=` only if required.  
- Confirm GPU is bound to `vfio-pci` and not `amdgpu`.  
- Ensure UEFI boot (OVMF) is selected for the VM.
</details>

<details>
<summary>Host boot fails after GRUB changes</summary>

- Revert GRUB parameters:  
  Edit `/etc/default/grub`, remove `intel_iommu=on iommu=pt`, run  
  `update-grub && reboot`
</details>

---

✅ **Status:** Baseline stable  
- IOMMU enabled and confirmed  
- VFIO prepared but not blacklisted  
- Host retains display output  
- Dual‑boot Windows GPU access preserved  
- Ready for full passthrough activation when needed

---
