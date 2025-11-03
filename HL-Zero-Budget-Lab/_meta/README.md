# 🧠 META FILES TEMPLATE — HL Zero-Budget Homelab

This folder contains metadata files for versioning, contributors, and change tracking.

---

## 📄 project_info.yml
```yaml
project_name: HL Zero-Budget Cybersecurity & AI Homelab
author: HumbleHungryLife
version: 2.0.0
license: MIT
last_updated: 2025-11-02
description: >
  Self-contained virtualization, firewall, and cybersecurity lab using Proxmox VE and open-source VMs.
status: Active
stages_completed:
  - Stage_0: Proxmox Base Setup
  - Stage_1: pfSense Deployment
  - Stage_2: Core VM Cluster
next_stages:
  - Stage_3: Monitoring & SIEM Integration
  - Stage_4: Incident Response / AI Automation
```

---

## 📄 version_history.yml
```yaml
- version: 2.0.0
  date: 2025-11-02
  changes:
    - Completed Stage_2 Core VMs documentation
    - Added secure pfSense configuration
    - Added repository structure and license files
- version: 1.9.0
  date: 2025-10-21
  changes:
    - GPU passthrough verified (Ubuntu & Kali)
    - HDD_8TB added for backups and ISOs
```

---

## 📄 contributors.yml
```yaml
maintainers:
  - name: HumbleHungryLife
    role: Architect / Documentation Lead
    contact: contact@humblehungrylife.dev
collaborators: []
```

---

## 📄 changelog.md
```markdown
# 🧾 Changelog — HL Zero-Budget Homelab

## 2025-11-02
- Added secure pfSense rules and DHCP/DNS notes
- Finalized Stage_2 Core VMs documentation
- Generated MIT License and repo structure files

## 2025-10-21
- Stable GPU passthrough verified on Ubuntu + Kali
- HDD_8TB mapped as main VM storage and backup drive
```
