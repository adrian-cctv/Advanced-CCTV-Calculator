
# 🛡️ CCTV Storage Architect v3.8
> **Enterprise Storage & Design Utilities for the Security Industry.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

**Live Demo:** [https://adrian-cctv.github.io/Advanced-CCTV-Calculator/](https://adrian-cctv.github.io/Advanced-CCTV-Calculator/)

Most storage calculators overlook the **Binary Gap**, **FPS scaling**, and **physical chassis constraints**. This tool is built to bridge the gap between **Sales Estimates** and **Engineering Reality**, now with full camera scheduling, multi-chassis support, and vendor-specific adjustments.



## 🛠️ Key Capabilities

### Core Features
* **⚡ Binary Accuracy:** Automatically corrects the **9.1% decimal-to-binary drive gap** (18TB Label vs 16.37TiB Usable).
* **🎬 Camera Schedule Builder:** Add camera groups with resolution, codec, FPS, retention days, and motion detection percentage.
* **🔄 Dynamic FPS Scaling:** Bitrate automatically adjusts when FPS changes (30fps = 2× storage of 15fps).
* **🖥️ Multi-Chassis Visualizer:** Plan across multiple servers with mixed bay configurations (e.g., 24-bay + 16-bay + 12-bay).
* **📊 VMS Overhead:** Configurable buffer (2-12%) for database indexing, metadata, and AI analytics.
* **🏗️ RAID Set Planning:** Full RAID support including JBOD, RAID0, RAID1, RAID5, RAID6, and RAID10.
* **🏗️ Distribution Logic:** Choose between Balanced (equal load) or Storage-Based (fill-first) drive allocation across chassis.
* **💾 Spare Drive Management:** Add hot spares (active failover) and cold spares (offline replacements).
* **🎯 Dual Design Modes:** 
  - **Camera-Based:** Design from camera schedule to storage requirements
  - **Target Capacity:** Enter desired storage, get drive count and configuration

### Enhanced Features
* **📈 Growth Factor:** Plan for 0-200% future expansion (10% recommended).
* **🏭 Vendor Adjustment:** Global slider with manufacturer reference guide (Axis: 0.65x, Hikvision: 1.0x, Avigilon: 1.2x).
* **✏️ Manual Override:** Per-camera manual bitrate override with checkbox toggle.
* **📊 Per-Chassis Metrics:** Shows Required (with growth+VMS), Raw Provided, and Usable (after RAID).
* **🚦 Bandwidth Gauge:** Real-time network utilization tracking with capacity presets from 100Mb to 10Gb.
* **🎨 Color-Coded Codecs:** H.265 options in green, H.264 in orange for quick identification.
* **📄 Export Reports:** One-click report export as `.txt` file with timestamp.



## 🚀 How to Use

### Quick Start

1. **Add Camera Groups:**
   - Click **"+ Add Group"** to create camera groups
   - Configure resolution, codec, FPS, retention days, and recording percentage
   - FPS changes automatically scale bitrate

2. **Configure Infrastructure:**
   - Select **RAID Type** (JBOD, RAID0, RAID1, RAID5, RAID6, RAID10)
   - Choose **HDD Size** (6TB to 28TB)
   - Set **Growth %** (10% recommended)
   - Set **VMS Overhead %** (5% standard, 8-12% for AI systems)
   - Configure **Hot Spares** and **Cold Spares**

3. **Set Global Bitrate Adjustment:**
   - Adjust slider based on camera manufacturer
   - Reference card shows recommended factors:
     - **Axis (Zipstream):** 0.65x - 0.75x
     - **Bosch (Intelligent Codec):** 0.80x - 0.90x
     - **Hikvision / Dahua:** 0.95x - 1.05x
     - **Avigilon (HDSM):** 1.10x - 1.20x

4. **Configure Multi-Chassis:**
   - Set **Number of Chassis**
   - Enter **Bays per Chassis** (e.g., "24,16,12" for mixed)
   - Choose distribution mode:
     - **Balanced:** Equal cameras per chassis
     - **Storage-Based:** Fill first chassis completely

5. **Review Results:**
   - **Camera Schedule:** View per-group Mbps and TiB with totals
   - **Chassis Topology:** See drive allocation, storage metrics, and color-coded drives
   - **Infrastructure Summary:** Complete report with recommendations
   - **Save JSON** Save JSON for future use

### Manual Bitrate Override

For cameras with specific manufacturer bitrate specifications:
- Check the box next to **Manual kbps**
- Enter the exact bitrate from datasheet
- Uncheck to return to auto-calculation



## 📊 Formula Reference

### Core Calculations

| Parameter | Formula |
|-----------|---------|
| **Scaled Bitrate** | `Base × (FPS ÷ 15) × Vendor_Adjust × Global_Adjust` |
| **Bandwidth** | `(Bitrate × Cameras × Rec%) ÷ 1000` (Mbps) |
| **Raw Storage** | `(Bitrate × 86400 × Days × Cameras × Rec%) ÷ 8,388,608,000` (TiB) |
| **Required Storage** | `Raw × (1 + Growth%) × (1 + VMS%)` |
| **TB → TiB** | `Labeled_TB × 0.90949` |

### RAID Usable Capacity

| RAID Type | Formula | Efficiency | Fault Tolerance |
|-----------|---------|------------|-----------------|
| JBOD | `N × Size` | 100% | 0 drives |
| RAID0 | `N × Size` | 100% | 0 drives |
| RAID1 | `Size` | 50% | 1 drive |
| RAID5 | `(N-1) × Size` | 67-94% | 1 drive |
| RAID6 | `(N-2) × Size` | 50-88% | 2 drives |
| RAID10 | `(N÷2) × Size` | 50% | Multi-drive |



## ⚠️ Professional Disclaimer

*This tool is a design aid provided for professional integrators. While it accounts for binary conversion, FPS scaling, RAID overhead, and VMS metadata, final storage specifications should always be verified against manufacturer-specific design tools to account for specific VMS and hardware throughput limitations.*



## 📋 Version History

### v3.8 (Current)

- Added Save/Load functionality via JSON files.
- Expanded HDD support to 24TB and 28TB.
- Implemented Distribution Modes (Balanced vs. Storage-Based).
- Updated RAID 1 logic to support multi-pair mirroring.
- Enhanced Network Bandwidth Gauge with 2.5G and 10G presets.

### v3.7
- Added RAID1 (Mirror) support
- Added Hot and Cold spare drives
- Fixed dropdown selection colors (green/orange)
- Fixed display artifacts

### v3.6
- Enhanced chassis cards with three storage metrics (Required, Raw, Usable)
- Added color-coded dropdown for H.264/H.265

### v3.5
- Simplified vendor adjustment with reference card
- Removed per-camera vendor dropdown

### v3.4
- Dual bitrate system (Auto + Manual)
- Bosch vendor profile
- Global vendor adjustment slider

### v3.3
- Dynamic FPS bitrate scaling fix
- Per-group Mbps and TiB columns

### v3.2
- Multi-chassis support with mixed bay configurations
- Balanced and storage-based distribution modes

### v3.1
- Camera schedule summary (totals row)
- Improved visual feedback

### v3.0
- JBOD RAID type
- Target capacity design mode
- Expanded HDD options (6-22TB)

### v2.0
- FPS scaling implementation
- Export report functionality
- Input validation

### v1.0
- Initial release with RAID5, RAID6, RAID10
- Basic chassis visualizer
- Binary gap correction


## 🔧 Technical Specifications

### Supported RAID Types
- **JBOD:** No redundancy, all capacity usable
- **RAID0:** Striping, no redundancy
- **RAID1:** Mirroring, 50% capacity
- **RAID5:** Single parity, 1 drive tolerance
- **RAID6:** Double parity, 2 drive tolerance
- **RAID10:** Mirror + Stripe, 50% capacity

### Supported HDD Sizes
- 6TB, 8TB, 10TB, 12TB, 14TB, 16TB, 18TB, 20TB, 22TB, 24TB, 28TB

### Supported Resolutions
- 720p, 2MP (1080p), 4MP (1440p), 5MP, 6MP, 4K (8MP)

### Codecs
- **H.264:** Standard compression (baseline)
- **H.265:** High Efficiency (50% less bitrate)


---

## 🎯 Best Practices

1. **Always use TiB for capacity planning** - Convert TB to TiB (×0.90949)
2. **Add 10-20% growth factor** for 3-5 year planning
3. **Use RAID6 for drives 16TB+** - Rebuild times are critical
4. **Add hot spares** (1 per 10-20 drives) for automatic failover
5. **Account for VMS overhead** (5% standard, 8-12% for AI systems)
6. **Test with actual camera bitrates** using manual override
7. **Document all assumptions** for future reference

---

## 📝 Export Format

Reports are exported as plain text with the following structure:


[ STORAGE DESIGN REPORT ]
═══════════════════════════════════
📋 MODE: CAMERA-BASED | Global Adj: 100%
🏢 CHASSIS: 1 units (24 bays)

📹 CAMERAS: 32 | 📊 126.4 Mbps | 💾 Raw Storage: 59.80 TiB

📈 STORAGE REQUIREMENTS
   Raw Camera Storage : 59.80 TiB
   +10% Growth        : 65.78 TiB
   +5% VMS            : 69.07 TiB (Required)

💾 HARDWARE SUMMARY
   RAID Type          : RAID6 (Double parity, 2 drive failure tolerance)
   HDD Size           : 18 TB (16.37 TiB usable)
   Hot Spares         : 1
   Total Drives       : 7 / 24

📊 CAPACITY ANALYSIS
   Usable (RAID6)     : 81.85 TiB
   Required           : 69.07 TiB
   ✅ Sufficient Capacity


## 🤝 Contributing

Found a bug or have a feature request? Open an issue or submit a pull request.

### Planned Features
- [ ] VMS-specific profiles (Geutebruck, Milestone, Genetec, Avigilon)
- [ ] Bandwidth calculator for network infrastructure
- [ ] Cloud storage tiering calculator


--
**Developed by Adrian C.**  
*Version 3.7 | Last Updated: March 2026*
```
