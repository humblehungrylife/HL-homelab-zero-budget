# 🧠 HL Zero-Budget Cybersecurity & AI Lab  
*A self-hosted homelab for cybersecurity, AI experimentation, and automation.*

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/@humblehungrylife-blue?logo=github&style=flat)](https://github.com/humblehungrylife)

---

## ⚡ Quick Links

| Section | Description |
|:--|:--|
| 🧰 [Hardware & Boot Config](Weekly_Progress/Week_0_Prep/README.md) | BIOS setup, TPM, Secure Boot, GRUB chainload |
| 🔧 [Proxmox Setup](Weekly_Progress/Week_1_Proxmox_Base/README.md) | Type-1 Proxmox VE installation + base configuration |
| 🌐 [Networking & Firewalls](Weekly_Progress/Week_3_Firewall_Segmentation.md) | VLANs, pfSense LAN/WAN segmentation, IDS/IPS setup |
| 🧩 [AI & LLMs](Weekly_Progress/Week_11_LLM_AI/README.md) | Mini-LLM & quantum automation integrations |
| ⚙️ [Automation](Weekly_Progress/Week_14_Automation_IaC.md) | Terraform + Ansible workflows |
| 🗂️ [Diagrams & Docs](docs/overview.md) | Hardware, topology, and backup architecture |
| 💰 [Monetization Plan](Resources/Monetization_Roadmap.md) | Turning your lab into content & value |

---

## ⏱️ Weekly Progress Timeline    
*Click a week below to open its section.*

### 🧩 Week 0 — Prep  
**Path:** `Weekly_Progress/Week_0_Prep/`  
BIOS tuning, ISO creation, hardware checks  

- [Assets](Weekly_Progress/Week_0_Prep/_assets/)  
- [Meta Logs](Weekly_Progress/Week_0_Prep/_meta/)  
- [GRUB Dual Boot Guide](Weekly_Progress/Week_0_Prep/README.md)  

---

### ⚙️ Week 1 — Proxmox Base  
**Path:** `Weekly_Progress/Week_1_Proxmox_Base/`  
Core Proxmox VE installation, base configuration, and storage mounts  

- [Documentation](Weekly_Progress/Week_1_Proxmox_Base/docs/)  
- [GPU Passthrough Guide](Weekly_Progress/Week_1_Proxmox_Base/docs/Proxmox_GPU_Passthrough.md)  
- [Hook Scripts](Weekly_Progress/Week_1_Proxmox_Base/config/grub_hook_scripts/)  

---

### 🧠 Week 2 — Core VMs  
**Path:** `Weekly_Progress/Week_2_Core_VMs/`  
pfSense, Ubuntu, Kali, and GPU passthrough setup  

- [pfSense](Weekly_Progress/Week_2_Core_VMs/pfSense/)  
- [Ubuntu Server](Weekly_Progress/Week_2_Core_VMs/Ubuntu_Server/)  
- [Ubuntu Desktop](Weekly_Progress/Week_2_Core_VMs/Ubuntu_Desktop/)  
- [Kali Server](Weekly_Progress/Week_2_Core_VMs/Kali_Server/)  
- [Kali Desktop](Weekly_Progress/Week_2_Core_VMs/Kali_Desktop/)  
- [Troubleshooting Index](Weekly_Progress/Week_2_Core_VMs/Troubleshooting_Index.md)  

---

### 🌐 Week 3 — Firewall Segmentation  
**Path:** `Weekly_Progress/Week_3_Firewall_Segmentation.md`  
VLANs, pfSense rules, and IDS/IPS tuning  

---

### 🧰 Week 6 — SIEM  
**Path:** `Weekly_Progress/Week_6_SIEM.md`  
Security Information & Event Management setup  

- [Examples](examples/)  

---

### 🚨 Week 7 — Incident Response  
**Path:** `Weekly_Progress/Week_7_IR.md`  
Playbooks, forensics, and response workflows  

- [Ansible Playbooks](examples/Ansible_Playbooks/)  
- [Jupyter Notebooks](examples/Jupyter/)  

---

### 🔐 Week 8 — Cryptography  
**Path:** `Weekly_Progress/Week_8_Cryptography.md`  
Certificates, PKI, and encryption lab  

- [Scripts](examples/Scripts/)  

---

### 🤖 Week 11 — AI & LLMs  
**Path:** `Weekly_Progress/Week_11_LLM_AI/README.md`  
Mini-LLM & quantum automation integrations  

- [AI Lab](examples/AI_Lab/)  
- [Monetization](Resources/Monetization_Roadmap.md)  

---

### 🧩 Week 14 — Automation & IaC  
**Path:** `Weekly_Progress/Week_14_Automation_IaC.md`  
Terraform and Ansible workflow integrations  

- [Terraform](examples/Terraform/)  
- [Portfolio](examples/Portfolio/)  

---

## 🧱 Repository Overview

| Folder | Purpose |
|:--|:--|
| `_config/` | Security + setup scripts |
| `_meta/` | Logs + snapshots + session notes |
| `docs/` | Hardware + topology + backups |
| `Weekly_Progress/` | Core week-by-week documentation |
| `examples/` | Reusable playbooks, Jupyter notebooks, scripts, and IaC templates |
| `Resources/` | Hardware layout, storage plan, risk mitigation, monetization roadmap |

---

## 🧩 About the Project  
The **HL Zero-Budget Lab** simulates a modern SOC + AI research environment entirely on personal hardware.  
Each phase documents the build process — from BIOS prep to GPU passthrough — while maintaining a **hybrid public/private model** for security and monetization.  

---

## 🧠 Hybrid Repository Policy

| Branch | Purpose |
|:--|:--|
| **`main`** | Public — docs, sanitized configs (MIT-licensed) |
| **`lab-private`** | Private — sensitive configs & automation (proprietary) |

> Certain configurations, datasets, and proprietary materials are **not** covered under the MIT License and remain confidential.  

---

## ⚖️ License  
This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file.  

---

<div align="center">

### 🤝 Connect  
[💻 GitHub Profile](https://github.com/humblehungrylife)  |  🔗 LinkedIn *(coming soon)*  |  ✉️ Email *(after business contact ready)*  

</div>

--- 
