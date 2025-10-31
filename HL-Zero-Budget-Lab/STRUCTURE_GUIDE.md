# 📖 Repository Structure Guide

## 🎯 Quick Start

Your repository is now ready with all placeholder files in place! No more 404 errors.

### How to Use This Structure

1. **Navigate to any linked section** from the main README.md
2. **Find the placeholder file** - it will have a clear "PLACEHOLDER" notice at the top
3. **Delete the placeholder content** and replace it with your actual documentation
4. **Keep the same filename** to maintain all links

## 🏗️ Complete Directory Structure

```
HL-Zero-Budget-Lab/
├── README.md                           # Main repository README (✅ Ready)
├── LICENSE                             # Your MIT License file (Add this)
│
├── _config/                            # Security + setup scripts
│   └── README.md                       # (Add when ready)
│
├── _meta/                              # Logs + snapshots + session notes
│   └── README.md                       # (Add when ready)
│
├── docs/                               # Hardware + topology + backups
│   └── README.md                       # (📝 Placeholder ready)
│
└── Weekly_Progress/
    │
    ├── Week_0_Prep/
    │   ├── README.md                   # (📝 Placeholder ready)
    │   ├── _assets/                    # Store images, files here
    │   └── _meta/                      # Store logs here
    │
    ├── Week_1_Proxmox_Base/
    │   ├── Week_1_Proxmox_Base.md      # (📝 Placeholder ready)
    │   └── docs/
    │       └── README.md               # (📝 Placeholder ready)
    │
    ├── Week_2_Core_VMs/
    │   ├── Troubleshooting_Index.md    # (📝 Placeholder ready)
    │   ├── pfSense/
    │   │   └── README.md               # (📝 Placeholder ready)
    │   ├── Ubuntu_Server/
    │   │   └── README.md               # (📝 Placeholder ready)
    │   ├── Ubuntu_Desktop/
    │   │   └── README.md               # (📝 Placeholder ready)
    │   ├── Kali_Server/
    │   │   └── README.md               # (📝 Placeholder ready)
    │   └── Kali_Desktop/
    │       └── README.md               # (📝 Placeholder ready)
    │
    ├── Week_3_Firewall_Segmentation.md # (📝 Placeholder ready)
    │
    ├── Week_6_SIEM/
    │   ├── README.md                   # (📝 Placeholder ready)
    │   └── examples/                   # Add example configs here
    │
    ├── Week_7_IR.md                    # (📝 Placeholder ready)
    │
    ├── Week_8_Cryptography.md          # (📝 Placeholder ready)
    │
    ├── Week_11_LLM_AI/
    │   ├── README.md                   # (📝 Placeholder ready)
    │   └── monetization/
    │       └── README.md               # (📝 Placeholder ready)
    │
    └── Week_14_Automation_IaC.md       # (📝 Placeholder ready)
```

## 🔄 Updating Placeholder Files

### Step-by-Step Process

1. **Open the placeholder file** you want to update
2. **Delete all content** from that file
3. **Add your real content** using markdown formatting
4. **Save the file** with the SAME filename
5. **Commit and push** to GitHub

### Example

**Before (Placeholder):**
```markdown
# 🧩 Week 0 — Prep

> **📝 PLACEHOLDER**: Replace this file with your actual Week 0 documentation.

## Overview
BIOS tuning, ISO creation, hardware checks
...
```

**After (Your Content):**
```markdown
# 🧩 Week 0 — Prep

## Hardware Preparation

I started by checking my server's BIOS settings...

### BIOS Configuration Steps
1. Enable VT-x/AMD-V
2. Configure boot order
...
```

## 📝 Writing Guidelines

### Markdown Best Practices

- Use headers (`#`, `##`, `###`) for organization
- Use code blocks for commands: \`\`\`bash
- Add images: `![Description](path/to/image.png)`
- Use tables for structured data
- Add links: `[Text](URL)`

### File Naming Conventions

- **Keep existing filenames** - all links depend on them
- Use underscores for spaces: `Week_1_Proxmox_Base.md`
- Always use `.md` extension
- Be consistent with capitalization

## 🎨 Adding Assets

### Images and Files

1. **Week 0**: Store in `Weekly_Progress/Week_0_Prep/_assets/`
2. **Week 6 Examples**: Store in `Weekly_Progress/Week_6_SIEM/examples/`
3. **General docs**: Store in `docs/`

### Linking Assets in Markdown

```markdown
![Network Diagram](docs/network-topology.png)
```

## 🚀 Future-Proofing Tips

1. **Don't change filenames** - this breaks links
2. **Keep the directory structure** - consistency is key
3. **Add new weeks** in the same pattern
4. **Update main README** if you add new sections

## ⚠️ Files Still Needed

Don't forget to add these files:

- `LICENSE` - Your MIT License file
- `_config/README.md` - When you add config files
- `_meta/README.md` - When you add meta information

## 🔧 Git Commands Reference

```bash
# Stage all changes
git add .

# Commit with message
git commit -m "Update Week 1 documentation"

# Push to GitHub
git push origin main
```

## 📞 Need Help?

If links are still broken:
1. Check the filename matches exactly (case-sensitive)
2. Verify the file is in the correct directory
3. Make sure you pushed changes to GitHub
4. Clear your browser cache

---

**Last Updated**: October 30, 2025
**Repository**: humblehungrylife/HL-Zero-Budget-Lab
