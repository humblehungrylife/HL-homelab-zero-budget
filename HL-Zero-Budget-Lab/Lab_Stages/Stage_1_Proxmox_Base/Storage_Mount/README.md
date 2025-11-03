# 🧱 Storage Mount — HDD_8TB Configuration (VMs + Backups)

This document captures the final, verified **storage configuration** used in the HL Zero-Budget Homelab project.  
It reflects the **hybrid setup** where the 8 TB HDD serves as both **VM disk storage** and **backup repository**.

---

## ⚙️ 1. Verify Disk Detection

Open Proxmox shell or SSH into host:

```bash
lsblk -f
```

Expected output (example):

```
sdb
└─sdb1  UUID=XXXX-XXXX  ntfs3  /mnt/pve/HDD_8TB
```

Confirm your 8 TB drive appears and has a UUID assigned.

---

## 🪣 2. Create Mount Point

```bash
mkdir -p /mnt/pve/HDD_8TB
```

> Use this standardized mount path for consistency across backups and VMs.

---

## 🧩 3. Add Storage in Proxmox Web UI

**Datacenter → Storage → Add → Directory**

| Field | Value |
|:------|:------|
| ID | `HDD_8TB` |
| Directory | `/mnt/pve/HDD_8TB` |
| Content | Disk image, ISO image, VZDump backup file |
| Nodes | Select your Proxmox node |
| Enable | ✅ Checked |

Click **Add** → then verify under the left-side “Storage” list.

---

## 🧰 4. Verify Mount and Usage

```bash
df -h | grep HDD_8TB
```

Example output:

```
/dev/sdb1    7.3T   1.1T   6.2T  15% /mnt/pve/HDD_8TB
```

You should also see `HDD_8TB` appear in:
- **VM creation → Target Storage**
- **Backup → Storage dropdown**

---

## 💾 5. (Optional) Set as Backup Target

**Datacenter → Backup → Add**

| Setting | Value |
|:---------|:------|
| Storage | `HDD_8TB` |
| Schedule | `daily` / `weekly` as preferred |
| Compression | `ZSTD` |
| Mode | `Snapshot` |

> Matches your working baseline from *Week 2 – Proxmox Automated Backup Script*.

---

## 🧩 6. Troubleshooting

<details>
<summary>Drive not visible in “Add Directory” list</summary>

- Ensure it’s partitioned and formatted:
  ```bash
  mkfs.ntfs -f /dev/sdb1
  ```
- Recreate mount point:
  ```bash
  mkdir -p /mnt/pve/HDD_8TB
  mount -t ntfs3 /dev/sdb1 /mnt/pve/HDD_8TB
  ```
</details>

<details>
<summary>Backups fail or “No space left on device”</summary>

- Confirm there’s no quota restriction:  
  `pct config <VMID> | grep quota`
- Free unused snapshots:  
  `qm listsnapshot <VMID>` → `qm delsnapshot <VMID> <name>`
</details>

<details>
<summary>Permissions issue on mounted drive</summary>

```bash
chmod 755 /mnt/pve/HDD_8TB
chown root:root /mnt/pve/HDD_8TB
```
</details>

---

✅ **Status:** Stable  
This storage mount has been tested with:
- pfSense (VM 101)  
- Ubuntu Server (VM 102)  
- Kali Desktop (VM 104)  
and verified for both ISO and snapshot backups.

---
