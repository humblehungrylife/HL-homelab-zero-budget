# ⚙️ CONFIG FILES TEMPLATE — HL Zero-Budget Homelab

This folder defines environment and lab configuration defaults for consistent setups across machines.

---

## 📄 proxmox.env
```bash
# Proxmox environment defaults
PROXMOX_HOSTNAME=proxmox-node1
PROXMOX_VERSION=9.0
ISO_PATH=/mnt/pve/HDD_8TB/ISO
BACKUP_PATH=/mnt/pve/HDD_8TB/dump
SNAPSHOT_PREFIX=Week
```

---

## 📄 vm_defaults.yml
```yaml
base_cpu: 4
base_ram: 8192
base_disk: 64G
storage_pool: HDD_8TB
bridge_wan: vmbr0
bridge_lan: vmbr1

vm_ids:
  pfSense: 101
  Ubuntu_Server: 102
  Ubuntu_Desktop: 103
  Kali_Server: 104
  Kali_Desktop: 105
```

---

## 📄 network_map.yml
```yaml
wan:
  bridge: vmbr0
  description: Connected to physical router
  mode: dhcp
lan:
  bridge: vmbr1
  subnet: 10.0.0.0/24
  gateway: 10.0.0.1
  dhcp_range: 10.0.0.50-10.0.0.200
dns:
  primary: 8.8.8.8
  secondary: 1.1.1.1
```

---

## 📄 storage_map.yml
```yaml
HDD_8TB:
  mount_point: /mnt/pve/HDD_8TB
  type: ntfs3
  usage:
    - ISO storage
    - VM disks
    - Backups
  verified: true

NVMe_1TB:
  mount_point: /mnt/pve/nvme-boot
  type: ext4
  usage:
    - Proxmox OS
    - Temporary snapshots
```

---

## 📄 gitignore_reference.txt
```
# Sensitive files to ignore
*.conf
*.key
*.crt
*.pem
*.xml
*.vma
*.zst
*.img
backups/
logs/
secrets/
```
