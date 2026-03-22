# 🛡️ CCTV Storage Architect v1.0
> **Enterprise Storage & Design Utilities for the Security Industry.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

Available to Play Here : https://adrian-cctv.github.io/Advanced-CCTV-Calculator/


Most storage calculators overlook the "Binary Gap" and physical chassis constraints. This tool is built to bridge the gap between **Sales Estimates** and **Engineering Reality**.

---

## 🛠️ Key Capabilities

* **⚡ Binary Accuracy:** Automatically corrects the **9.1% decimal-to-binary drive gap** (10TB Label vs 9.09TiB Usable).
* **🖥️ Chassis Visualizer:** Real-time 2D topology preview to plan physical server bay population.
* **📊 VMS Guardrail:** Includes a hard-coded **5% buffer** for database indexing and VMS metadata.
* **🏗️ RAID Set Planning:** Smart logic to ensure remaining bays can accommodate secondary RAID sets for future expansion.

---

## 🚀 How to Use

1. **Define Load:** Enter your total usable TB required based on your camera schedule.
2. **Configure RAID:** Select your RAID level (**RAID 6** is strongly recommended for drives 16TB and larger).
3. **Plan Physicals:** Enter the max bays of your server (e.g., 12, 24, or 60).
4. **Audit:** Review the **Chassis Topology** to ensure you aren't "orphaning" drives or overfilling the server.

---

## ⚠️ Professional Disclaimer
*This tool is a design aid provided for professional integrators. While it accounts for binary conversion and RAID overhead, final storage specifications should always be verified against manufacturer-specific design tools to account for specific VMS and hardware throughput limitations.*

---
**Developed by Adrian C.
