# 🧱 HL Zero-Budget Cybersecurity & AI Homelab — Repository Structure Guide

This structure defines how all documentation, configs, and troubleshooting files are organized for the Homelab project.

---

## 📁 Repository Layout

```
homelab-zero-budget/
│
├── LICENSE
│   └── MIT License (no modifications)
│
├── README.md
│   └── Main overview and roadmap of the HL Zero-Budget Homelab
│
├── 00_Documentation/
│   ├── Project_Overview.md
│   ├── Hardware_Inventory.md
│   ├── Network_Topology.md
│   ├── Backup_Strategy.md
│   ├── Risk_Mitigation.md
│   └── Monetization_Plan.md
│
├── 01_Proxmox_Setup/
│   ├── GPU_Passthrough/
│   │   └── README.md
│   ├── Network_Configs/
│   │   └── README.md
│   ├── Screenshots_Topology/
│   │   └── README.md
│   └── Storage_Mount/
│       └── README.md
│
├── 02_Stage_2_Core_VMs/
│   ├── 01_pfSense/
│   │   └── README.md
│   ├── 02_Ubuntu_Server/
│   │   └── README.md
│   ├── 03_Ubuntu_Desktop/
│   │   └── README.md
│   ├── 04_Kali_Server/
│   │   └── README.md
│   ├── 05_Kali_Desktop/
│   │   └── README.md
│   ├── Troubleshooting_Index.md
│   └── README.md
│
├── 03_Snapshots_And_Recovery/
│   ├── Snapshot_List.md
│   ├── Recovery_Procedure.md
│   └── Backup_Verification.md
│
├── 04_Future_Expansions/
│   ├── Windows_Server_2022_ADDS.md
│   ├── Wazuh_SIEM_Stack.md
│   ├── Splunk_Lab.md
│   ├── Velociraptor_IR.md
│   └── AI_Lab_Integration.md
│
├── Screenshots/
│   ├── BIOS/
│   ├── Proxmox_Dashboard/
│   ├── pfSense_Interface/
│   └── VMs/
│
└── notes_archive/
    ├── commands_reference.txt
    ├── backup_log.txt
    └── version_history.md
```

---

## 🧠 Naming Standards

| Type | Format Example | Notes |
|:--|:--|:--|
| Directory | Stage_2_Core_VMs | Use underscores, prefix with 0x_ for order |
| File | README.md | Each subfolder should have one |
| Logs | *_log.txt | Text-only logs |
| Snapshots | WeekX_VMName_Stable | Match actual Proxmox snapshot names |

---

## 🧩 VM Dependency Order

1️⃣ pfSense → must boot first (firewall + DHCP)  
2️⃣ Ubuntu Server → confirms LAN connectivity  
3️⃣ Ubuntu Desktop → GPU passthrough validated  
4️⃣ Kali Server → headless node for testing  
5️⃣ Kali Desktop → GPU + sound passthrough  
   - Use `qm stop 103 && sleep 10` before starting to avoid GPU conflicts.

---

## 🧱 Snapshot & Backup Flow

```bash
# Create snapshot
qm snapshot 101 "Week1_pfSense_Stable_SecureRules"
qm snapshot 102 "Week2_Ubuntu_Server_Stable"
qm snapshot 103 "Week2_Ubuntu_Desktop_GPU_Ready"
qm snapshot 104 "Week2_Kali_Server_Stable"
qm snapshot 105 "Week2_Kali_Desktop_GPU_Ready"

# Verify backup integrity
vma verify /mnt/pve/HDD_8TB/dump/Week2_Ubuntu_Desktop_GPU_Ready.vma.zst
```

---

## 🪄 GitHub Publishing Checklist

- ✅ Verify all IPs are private (10.x / 192.168.x)
- ✅ Exclude sensitive files via `.gitignore`
- ✅ Include LICENSE (MIT)
- ✅ Maintain Screenshots_Topology for visuals
- ✅ Use consistent emoji headers
- ✅ Run `git status` before every commit

---

© 2025 HumbleHungryLife — MIT License
