# 🖼️ Screenshots & Network Topology

This section provides a visual reference of the **Proxmox + pfSense + VM topology**  
for the HL Zero-Budget Homelab project.  
It combines screenshots (BIOS, Proxmox dashboard, pfSense interfaces) and ASCII diagrams  
to visualize the architecture you configured in your stable Week 1 setup.

---

## 🧩 1. Folder Structure

Recommended screenshot organization inside this folder:

```
Screenshots_Topology/
│
├── BIOS_Settings/
│   ├── IOMMU_Enable.jpg
│   ├── Resizable_BAR.jpg
│   └── VTd_Vi_Enable.jpg
│
├── Proxmox_Interface/
│   ├── vmbr0_WAN_Config.jpg
│   ├── vmbr1_LAN_Config.jpg
│   └── Storage_HDD_8TB.jpg
│
├── pfSense_Interface/
│   ├── WAN_Status.jpg
│   ├── LAN_Status.jpg
│   ├── DHCP_Assignments.jpg
│   └── Dashboard_Overview.jpg
│
└── VM_Views/
    ├── Ubuntu_VM_102.jpg
    ├── Kali_VM_104.jpg
    └── LAN_Connectivity_Test.jpg
```

> 📸 Save all screenshots in `.jpg` or `.png` format for consistent rendering on GitHub.

---

## 🧱 2. Logical Topology Diagram

```
                        🌐 Internet
                             │
                     ┌────────────────┐
                     │ Home Router     │
                     │ 192.168.1.254   │
                     └────────────────┘
                             │
                     (WAN → vmbr0 → vnet0)
                             │
               ┌─────────────────────────────────┐
               │ pfSense VM (ID 101)             │
               │ WAN: 192.168.1.70 (DHCP)        │
               │ LAN: 10.0.0.1 (Static)          │
               └─────────────────────────────────┘
                             │
                     (LAN → vmbr1 → vnet1)
                             │
       ┌─────────────────────────────────────────────────┐
       │ Internal Network — 10.0.0.0/24                  │
       │                                                 │
       │  • Ubuntu Server VM (ID 102) → 10.0.0.10        │
       │  • Kali Linux Desktop (ID 104) → 10.0.0.20      │
       │  • Future Core VMs (AD, Wazuh, etc.)            │
       └─────────────────────────────────────────────────┘
```

> This represents the verified Week 1 network baseline —  
> pfSense acts as the gateway for all LAN VMs via `vmbr1` (vnet1),  
> while `vmbr0` connects Proxmox and pfSense WAN to your home router.

---

## ⚙️ 3. Screenshot Checklist

✅ BIOS → IOMMU, VT-d, Above 4G, Resizable BAR  
✅ Proxmox → Network Bridges (vmbr0, vmbr1)  
✅ Proxmox → HDD_8TB under Storage  
✅ pfSense → WAN/LAN interface status  
✅ pfSense → Gateway + DNS test successful  
✅ Ubuntu/Kali → LAN IP assigned (10.0.0.x)  
✅ Ping + DNS verification through pfSense  

---

## 🧭 4. Verification Reference

| Test | From | To | Expected Result |
|:------|:------|:--|:----------------|
| WAN ping | pfSense shell | 8.8.8.8 | ✅ Successful |
| LAN ping | Ubuntu VM | 10.0.0.1 | ✅ Gateway reachable |
| DNS | Ubuntu VM | google.com | ✅ Resolved |
| WebGUI | Host browser | 10.0.0.1 | ✅ pfSense login page |

---

## 🪄 5. Optional Visualization (for GitHub)

You can include a Markdown-based diagram generated via Mermaid syntax:

```mermaid
graph TD
    A[Internet] --> B(Home Router 192.168.1.254)
    B --> C[vmbr0 (WAN) → pfSense WAN vnet0]
    C --> D[pfSense VM (101)]
    D --> E[vmbr1 (LAN) → vnet1]
    E --> F[Ubuntu VM (102)]
    E --> G[Kali VM (104)]
```

---

✅ **Status:** Documented & Verified  
All screenshots and network paths validated during final testing.  
Use this section as a visual anchor for your GitHub readers and troubleshooting reference.

---
