# 🛡️ CCTV Storage Architect v3.9.4
> **Enterprise Storage & Design Utilities for the Security Industry.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

**Live Demo:** [https://adrian-cctv.github.io/Advanced-CCTV-Calculator/](https://adrian-cctv.github.io/Advanced-CCTV-Calculator/)

Most storage calculators overlook the **Binary Gap**, **FPS scaling**, and **physical chassis constraints**. This tool is built to bridge the gap between **Sales Estimates** and **Engineering Reality**, now with full camera scheduling, multi-chassis support, and vendor-specific adjustments.



## 🛠️ Key Capabilities

### Core Features
* **⚡ Binary Accuracy:** Automatically corrects the **9.1% decimal-to-binary drive gap** (18TB Label vs 16.37TiB Usable).
* **🎬 Camera Schedule Builder:** Add camera groups with resolution, codec, FPS, retention days, and motion detection percentage.
* **🔄 Dynamic FPS Scaling:** Bitrate automatically adjusts when FPS changes using log-linear model (sub-linear scaling with FPS).
* **📐 Log-Linear Bitrate Model:** Statistical model calibrated against Axis calculator data (R² ≈ 0.98) for accurate bitrate prediction across all resolutions.
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
* **🏭 Vendor Adjustment:** Global slider with manufacturer reference guide (Axis: 1.0x, Bosch/Hanwha: 1.2x, Hikvision: 1.3x, Avigilon: 2.5x).
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
         - **Axis (Zipstream):** 0.9x - 1.1x
         - **Bosch (Intelligent Codec):** 1.0 x - 1.3x
         - **Hanwha (WiseStream II):** 1.0 x - 1.3x
         - **Hikvision / Dahua:** 1.0x - 1.3x
         - **Avigilon (HDSM):** 2.2x - 2.6x 

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
| **Log-Linear Bitrate** | `Mbps = exp(a + b·log(MP) + c·log(FPS) + d·isH265) × Global_Adjust` |
| **Bandwidth** | `(Bitrate × Cameras × Rec%) ÷ 1000` (Mbps) |
| **Raw Storage** | `(Bitrate × 86400 × Days × Cameras × Rec%) ÷ 8,388,608,000` (TiB) |
| **Required Storage** | `Raw × (1 + Growth%) × (1 + VMS%)` |
| **TB → TiB** | `Labeled_TB × 0.90949` |

### Log-Linear Model Coefficients (Current)

Calibrated against Axis bitrate calculator data (R² ≈ 0.98, average error < 3%):

| Coefficient | Value | Description |
|-------------|-------|-------------|
| a (intercept) | -1.7232 | Base calibration constant |
| b (MP exponent) | 0.9718 | Near-linear resolution scaling |
| c (FPS exponent) | 0.5920 | Sub-linear frame rate scaling |
| d (H.265 effect) | -0.2169 | ~20% efficiency gain over H.264 |

### Example Bitrate Outputs (15 FPS)

| Resolution | H.264 | H.265 | H.265 Savings |
|------------|-------|-------|---------------|
| 1MP | 0.89 Mbps | 0.71 Mbps | ~20% |
| 2MP | 1.74 Mbps | 1.40 Mbps | ~20% |
| 4MP | 3.41 Mbps | 2.75 Mbps | ~19% |
| 8MP | 6.69 Mbps | 5.39 Mbps | ~19% |



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

### v3.9.4 - Current
- **Removed unused `BRAND_MULTIPLIERS` object** - Cleaned up dead code
  - The `BRAND_MULTIPLIERS` object was defined but never used in bitrate calculations
  - Bitrate calculations use `globalAdjustment` slider instead of brand-specific multipliers
  - Vendor reference values remain in UI as static guidance for users
  - Impact: Cleaner codebase, no functional change to calculations

### v3.9.3 -
- **Corrected Avigilon brand multiplier** - Updated from 1.10x to 2.50x based on empirical data
  - Avigilon HDSM cameras use significantly higher bitrates than Axis baseline
  - Sample data shows 2.54x average ratio vs Axis (range: 1.82x-3.15x)
  - Fix: Vendor reference card now shows 2.50x for Avigilon (was 1.10x)
  - Impact: Avigilon bitrate calculations now accurate within ~21% error (down from 55%)

### v3.9.2 -
- **Recalibrated log-linear bitrate model** against Axis bitrate calculator data
  - New coefficients: a=-1.7232, b=0.9718, c=0.5920, d=-0.2169
  - Improved accuracy: average error reduced from up to 41% to within ±5%
  - R² ≈ 0.98 correlation with Axis reference data
- Updated model documentation and verification test cases

### v3.9.1 -
- **Fixed critical bug** in log-linear model coefficient
  - Corrected intercept from 7.6009 to -3.0052
  - Fixed astronomical bitrate calculations (was producing ~59,000 Mbps instead of ~1.47 Mbps)

### v3.9 -
- **Implemented log-linear bitrate model** for more accurate bitrate calculations
  - Model: `log(Mbps) = a + b·log(MP) + c·log(FPS) + d·isH265`
  - R² ≈ 0.962 in log space - explains 96.2% of variance
  - Naturally handles nonlinear bandwidth scaling with resolution and frame rate
- Bitrate calculation: replaced linear scaling with log-linear power-law model
  - MP scaling: `bitrate ∝ MP^0.75` (sub-linear, more realistic)
  - FPS scaling: `bitrate ∝ FPS^1.05` (near-linear)
  - H.265 efficiency: 23% gain (e^-0.2603 ≈ 0.77)
  - Brand-specific multipliers via global adjustment slider
- Model Coefficients (v3.9 initial):
| Parameter | Value | Description |
|-----------|-------|-------------|
| a (intercept) | -3.0052 | Calibrated to 2MP@15fps@H.264 = 1470 kbps |
| b (MP exponent) | 0.75 | Sub-linear resolution scaling |
| c (FPS exponent) | 1.05 | Near-linear frame rate scaling |
| d (H.265 effect) | -0.2603 | 23% efficiency gain over H.264 |
- Brand Multipliers (v3.9 initial):
| Brand | Multiplier | Notes |
|-------|------------|-------|
| Axis | 1.00x | Baseline (Zipstream) |
| Bosch | 1.20x | 20% higher bitrate |
| Hanwha | 1.20x | 20% higher bitrate |
| Hikvision | 1.30x | 30% higher bitrate |
| Dahua | 1.30x | 30% higher bitrate |
| Avigilon | 1.10x | 10% higher bitrate |
| Generic | 1.30x | 30% higher bitrate |

### v3.8 -
- Added Save/Load functionality via JSON files.
- Expanded HDD support to 24TB and 28TB.
- Implemented Distribution Modes (Balanced vs. Storage-Based).
- Updated RAID 1 logic to support multi-pair mirroring.
- Enhanced Network Bandwidth Gauge with 2.5G and 10G presets.

### v3.7 -
- Added RAID1 (Mirror) support
- Added Hot and Cold spare drives
- Fixed dropdown selection colors (green/orange)
- Fixed display artifacts

### v3.6 -
- Enhanced chassis cards with three storage metrics (Required, Raw, Usable)
- Added color-coded dropdown for H.264/H.265

### v3.5 -
- Simplified vendor adjustment with reference card
- Removed per-camera vendor dropdown

### v3.4 -
- Dual bitrate system (Auto + Manual)
- Bosch vendor profile
- Global vendor adjustment slider

### v3.3 -
- Dynamic FPS bitrate scaling fix
- Per-group Mbps and TiB columns

### v3.2 -
- Multi-chassis support with mixed bay configurations
- Balanced and storage-based distribution modes

### v3.1 -
- Camera schedule summary (totals row)
- Improved visual feedback

### v3.0 -
- JBOD RAID type
- Target capacity design mode
- Expanded HDD options (6-22TB)

### v2.0 -
- FPS scaling implementation
- Export report functionality
- Input validation

### v1.0 -
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
- 720p, 2MP (1080p), 4MP (1440p), 5MP, 4K (8MP), 12MP

### Codecs
- **H.264:** Standard compression (baseline)
- **H.265:** High Efficiency (~20% less bitrate, model-calibrated)



---

## 🎯 Best Practices

1. **Always use TiB for capacity planning** - Convert TB to TiB (×0.90949)
2. **Add 10-20% growth factor** for 3-5 year planning
3. **Use RAID6 for drives 16TB+** - Rebuild times are critical
4. **Add hot spares** (1 per 10-20 drives) for automatic failover
5. **Account for VMS overhead** (5% standard, 8-12% for AI systems)
6. **Test with actual camera bitrates** using manual override
7. **Document all assumptions** for future reference



## 📝 Export Format

Reports are exported as plain text with the following structure:


[ STORAGE DESIGN REPORT ]
══════════════════════════════════
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
- [ ] Cloud storage tiering calculator


--
**Developed by Adrian C.**
*Version 3.9.4 | Last Updated: April 2026*
