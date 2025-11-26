# 🧠 HL Zero-Budget Cybersecurity & AI Lab  A+
*A self-hosted homelab for cybersecurity, AI experimentation, and automation.*

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/@humblehungrylife-blue?logo=github&style=flat)](https://github.com/humblehungrylife)

---

## ⚡ Quick Links

| Section | Description |
|:--|:--|
| 🧰 [Stage 0 — Prep](Lab_Stages/Stage_0_Prep/README.md) | BIOS setup, TPM, Secure Boot, GRUB chainload |
| 🔧 [Stage 1 — Proxmox Base](Lab_Stages/Stage_1_Proxmox_Base/README.md) | Type-1 Proxmox VE installation + base configuration |
| 🌐 [Stage 3 — Firewall Segmentation](Lab_Stages/Stage_3_Firewall_Segmentation/README.md) | VLANs, pfSense LAN/WAN segmentation, IDS/IPS setup |
| 💾 [Stage 4 — Backup](Lab_Stages/Stage_4_Backup/README.md) | Automated Proxmox backups, snapshot & retention strategy |
| 🧰 [Stage 5 — SIEM](Lab_Stages/Stage_5_SIEM/README.md) | Security Information & Event Management setup |
| 🔐 [Stage 6 — Cryptography](Lab_Stages/Stage_6_Cryptography/README.md) | PKI, certificates, and encryption workflows |
| 🤖 [Stage 7 — LLM & AI Integration](Lab_Stages/Stage_7_LLM_AI/README.md) | Mini-LLM & quantum automation integrations |
| 🗂️ [Documentation & Diagrams](docs/README.md) | Hardware, topology, and architecture index |

---

## ⏱️ Stage Progress Timeline  
*Click a stage below to open its section.*

### 🧩 Stage 0 — Prep  
**Path:** `Lab_Stages/Stage_0_Prep/`  
BIOS tuning, ISO creation, hardware checks  

- [Documentation](Lab_Stages/Stage_0_Prep/README.md)  

---

### ⚙️ Stage 1 — Proxmox Base  
**Path:** `Lab_Stages/Stage_1_Proxmox_Base/`  
Core Proxmox VE installation, storage setup, and network bridges  

- [GPU Passthrough Guide](Lab_Stages/Stage_1_Proxmox_Base/GPU_Passthrough/README.md)  
- [Storage Mount Configuration](Lab_Stages/Stage_1_Proxmox_Base/Storage_Mount/README.md)  
- [Network Configuration](Lab_Stages/Stage_1_Proxmox_Base/Network_Configs/README.md)  
- [Screenshots & Topology](Lab_Stages/Stage_1_Proxmox_Base/Screenshots_Topology/README.md)  

---

### 🧠 Stage 2 — Core VMs  
**Path:** `Lab_Stages/Stage_2_Core_VMs/`  
pfSense, Ubuntu, and Kali VM creation and GPU passthrough  

- [pfSense](Lab_Stages/Stage_2_Core_VMs/pfSense/README.md)  
- [Ubuntu Server](Lab_Stages/Stage_2_Core_VMs/Ubuntu_Server/README.md)  
- [Ubuntu Desktop](Lab_Stages/Stage_2_Core_VMs/Ubuntu_Desktop/README.md)  
- [Kali Server](Lab_Stages/Stage_2_Core_VMs/Kali_Server/README.md)  
- [Kali Desktop](Lab_Stages/Stage_2_Core_VMs/Kali_Desktop/README.md)  
- [Troubleshooting Index](Lab_Stages/Stage_2_Core_VMs/Z_Troubleshooting_Index.md)  

---

### 🌐 [Stage 3 — Firewall Segmentation](Lab_Stages/Stage_3_Firewall_Segmentation/README.md)  
**Path:** `Lab_Stages/Stage_3_Firewall_Segmentation/README.md`  
VLANs, pfSense rule design, and IDS/IPS configuration  

- [VLAN Setup](Lab_Stages/Stage_3_Firewall_Segmentation/README.md#vlan-setup)  
- [pfSense Rules](Lab_Stages/Stage_3_Firewall_Segmentation/README.md#pfsense-rules)  
- [IDS/IPS Configuration](Lab_Stages/Stage_3_Firewall_Segmentation/README.md#idsips-tuning)  

---

### 💾 [Stage 4 — Backup](Lab_Stages/Stage_4_Backup/README.md)  
**Path:** `Lab_Stages/Stage_4_Backup/README.md`  
Snapshot automation and backup retention strategy  

- [Proxmox Backup Script](Lab_Stages/Stage_4_Backup/README.md#proxmox-backup-script)  
- [Snapshot Automation](Lab_Stages/Stage_4_Backup/README.md#snapshot-automation)  
- [Retention Policy](Lab_Stages/Stage_4_Backup/README.md#retention-policy)  

---

### 🧰 [Stage 5 — SIEM](Lab_Stages/Stage_5_SIEM/README.md)  
**Path:** `Lab_Stages/Stage_5_SIEM/README.md`  
Security monitoring and log aggregation stack (Wazuh, Splunk, etc.)  

- [Wazuh Setup](Lab_Stages/Stage_5_SIEM/README.md#wazuh-setup)  
- [Splunk Integration](Lab_Stages/Stage_5_SIEM/README.md#splunk-integration)  
- [Syslog & Agents](Lab_Stages/Stage_5_SIEM/README.md#syslog-and-agent-configuration)  

---

### 🔐 [Stage 6 — Cryptography](Lab_Stages/Stage_6_Cryptography/README.md)  
**Path:** `Lab_Stages/Stage_6_Cryptography/README.md`  
PKI, SSL/TLS certificate management, and encryption lab  

- [PKI Architecture](Lab_Stages/Stage_6_Cryptography/README.md#pki-architecture)  
- [SSL/TLS Certificates](Lab_Stages/Stage_6_Cryptography/README.md#ssltls-certificates)  
- [Encryption Labs](Lab_Stages/Stage_6_Cryptography/README.md#encryption-labs)  

---

### 🤖 [Stage 7 — LLM & AI Integration](Lab_Stages/Stage_7_LLM_AI/README.md)  
**Path:** `Lab_Stages/Stage_7_LLM_AI/README.md`  
LLM deployments, fine-tuning, and quantum automation integration  

- [Local Model Deployment](Lab_Stages/Stage_7_LLM_AI/README.md#local-model-deployment)  
- [Fine-Tuning & Agents](Lab_Stages/Stage_7_LLM_AI/README.md#fine-tuning-and-agents)  
- [Quantum Integration](Lab_Stages/Stage_7_LLM_AI/README.md#quantum-integration)  
- [Automation](Lab_Stages/Stage_7_LLM_AI/README.md#automation-and-monitoring)  

## 🧱 Repository Overview

| Folder | Purpose |
|:--|:--|
| `Lab_Stages/` | Core stage-by-stage documentation of the homelab |
| `_config/` | Security, automation, and infrastructure-as-code scripts |
| `_meta/` | Logs, snapshots, and private notes |
| `docs/` | Documentation index, hardware specs, and architecture diagrams |

---

## 🧩 About the Project  
The **HL Zero-Budget Lab** simulates a modern SOC + AI research environment entirely on personal hardware.  
Each stage documents the build process — from BIOS prep to GPU passthrough — within a **hybrid public/private model** balancing transparency, learning, and security.

---

## 🧠 Hybrid Repository Policy

| Branch | Purpose |
|:--|:--|
| **`main`** | Public — documentation and sanitized configs (MIT-licensed) |
| **`lab-private`** | Private — sensitive configs, automation scripts, and datasets |

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
