# Edge AI Hardware for Counter-Drone Detection: BOM Research

## 1. Compute Platforms

### NVIDIA Jetson Orin Nano Super — RECOMMENDED
- **AI:** 67 TOPS (INT8)
- **GPU:** 1024-core Ampere, up to 1020 MHz
- **CPU:** 6-core Cortex-A78AE
- **Memory:** 8GB LPDDR5
- **Power:** 7-25W configurable
- **Price:** **$249** (Dev Kit)
- **YOLOv8-nano INT8:** ~43 FPS at 7.8W
- Supported until 2032. Mature SDK (TensorRT, DeepStream, JetPack).

### Raspberry Pi 5 + Hailo-8 HAT+
- **AI:** 26 TOPS
- **Price:** **$205** combined
- **Power:** ~15W
- Lower performance but cheaper. Good for secondary nodes.

### Comparison
| Platform | TOPS | Price | Power |
|---|---|---|---|
| Jetson Orin Nano Super | 67 | $249 | 25W |
| RPi5 + Hailo-8 | 26 | $205 | ~15W |
| RPi5 + Hailo-8L | 13 | $165 | ~14W |

**Jetson wins** — 67 TOPS, 2.5x RPi+Hailo performance, only $45 more.

### Others (not recommended)
- Qualcomm RB3 Gen 2: 12 TOPS, $399. Poor value.
- Rockchip RK3588: 6 TOPS, $69-180. Marginal for real-time detection. OK as sensor hub.
- Google Coral: 4 TOPS, TFLite only. Too limited.

## 2. Microphone Arrays

### miniDSP UMA-16 v2 — RECOMMENDED
- 16 Knowles PDM MEMS mics, rectangular array
- 24-bit, 48 kHz, USB Audio Class 2
- **$275**
- 16 channels enables beamforming + DoA

### Seeed ReSpeaker XVF3800
- 4 MEMS mics, 360-degree, on-chip DoA/AEC
- **$50-55**
- 4 mics limits angular resolution. Good supplementary.

### miniDSP UMA-8
- 8 MEMS mics, circular
- ~$150. Middle ground.

### Custom Build
- Knowles SPH0645LM4H MEMS: ~$2-3 each
- 8-16 mic custom PCB: ~$50-80 in parts
- Requires firmware work

## 3. SDR / RF Detection

### HackRF Pro — RECOMMENDED
- **Freq:** 100 kHz - 6 GHz (covers ALL drone bands)
- **BW:** Up to 20 MHz
- **Price:** **$400**
- Covers 900 MHz, 2.4 GHz, AND 5.8 GHz

### RTL-SDR Blog V4
- **Freq:** 500 kHz - 1.7 GHz (900 MHz band only)
- **Price:** **$30**
- Cannot monitor 2.4/5.8 GHz directly

### Multi-SDR Strategy
- 1x HackRF Pro ($400) for 2.4/5.8 GHz
- 1x RTL-SDR V4 ($30) for dedicated 900 MHz
- **Total: $430** for full spectrum coverage

### Key Drone Frequencies
- 2.4 GHz (Wi-Fi/control)
- 5.8 GHz (video/control)
- 900 MHz (long-range)
- 1.2 GHz (analog video)
- 433 MHz (telemetry)

**Note:** HackRF Pro is half-duplex — can only monitor one band at a time. Need fast frequency hopping or two units for simultaneous monitoring.

## 4. Cameras

### Thermal: FLIR Lepton 3.5 + PureThermal 3
- 160x120 thermal, radiometric
- USB UVC (webcam mode)
- **$250** combo
- Detects drones at night, through haze

### Visible: Arducam IMX477
- 12.3MP, Sony IMX477, CSI/MIPI (native to Jetson)
- Day/night model with auto IR-cut filter
- **$70**
- 50mm lens: detect Mavic-sized drone at ~500m

### Camera Total: **$320**

## 5. Enclosure

### Pelican 1450 — RECOMMENDED
- Interior: 14.62 x 10.18 x 6.00 in
- IP67, MIL-STD-810
- **$196**

### Additional
- IP68 cable glands: $2-5 each (need ~6-8)
- Panel-mount SMA connectors: $5-10 each
- Custom CNC panel (Protocase): $50-100

## 6. Power

### System Power Budget
| Component | Draw |
|---|---|
| Jetson Orin Nano (25W mode) | 25W |
| HackRF Pro | 2-3W |
| RTL-SDR V4 | 0.5W |
| Microphone array | 1-2W |
| Cameras | 2-3W |
| Misc (fans, networking) | 2-3W |
| **Total** | **~33-37W** |

### EcoFlow RIVER 2 Pro — RECOMMENDED
- 768 Wh LiFePO4
- 12V DC, USB-C PD, AC outlets
- **$350**
- Runtime at 33W: **~23 hours**

### 100W Folding Solar Panel
- **$150**
- ~500-600 Wh/day at 5-6 peak sun hours
- Enables continuous daytime operation

### Alternative: DIY Battery
- 12V 50Ah LiFePO4 (LiTime): ~$150-200
- 600Wh. Cheaper but more integration work.

## 7. Prototype BOM — Option A: Performance Build

| Item | Part | Price |
|---|---|---|
| Compute | Jetson Orin Nano Super Dev Kit | $249 |
| SDR (wideband) | HackRF Pro | $400 |
| SDR (900MHz) | RTL-SDR V4 | $30 |
| Mic Array | miniDSP UMA-16 v2 | $275 |
| Thermal Camera | FLIR Lepton 3.5 + PureThermal 3 | $250 |
| Visible Camera | Arducam IMX477 Day/Night CSI | $70 |
| Enclosure | Pelican 1450 + foam | $196 |
| Connectors | Cable glands, SMA passthroughs | $50 |
| Antennas | Wideband discone + 2.4/5.8 directional | $80 |
| Power | EcoFlow RIVER 2 Pro (768Wh) | $350 |
| Solar | 100W folding panel | $150 |
| Networking | USB LTE modem | $80 |
| Storage | 512GB NVMe SSD | $50 |
| Misc | Cables, USB hub, heatsinks, fans | $100 |
| **TOTAL** | | **$2,330** |

## 8. Prototype BOM — Option B: Budget Build

| Item | Part | Price |
|---|---|---|
| Compute | RPi 5 8GB + Hailo-8 HAT+ | $205 |
| SDR | HackRF One (legacy stock) | $250 |
| Mic Array | Seeed ReSpeaker XVF3800 | $55 |
| Thermal | FLIR Lepton 3.5 + PureThermal 3 | $250 |
| Visible | RPi Camera Module 3 Wide | $35 |
| Enclosure | Polycase IP67 box | $50 |
| Power | EcoFlow RIVER 3 (245Wh) | $189 |
| Misc | Cables, storage, connectors, antenna | $150 |
| **TOTAL** | | **$1,184** |

## 9. $10K Budget Allocation

| Allocation | Amount |
|---|---|
| 3x Detection Nodes (Option A) | $6,990 |
| Central server / display laptop | $1,000 |
| Software development / cloud costs | $500 |
| Spare parts and consumables | $500 |
| Testing equipment (spectrum analyzer rental) | $500 |
| Contingency | $510 |
| **TOTAL** | **$10,000** |

3 nodes enables triangulation and demonstrates the network effect.

## 10. Technical Notes

1. **HackRF half-duplex:** Can only monitor one band at a time. Two units or fast hopping for simultaneous 2.4 + 5.8 GHz.
2. **Acoustic range:** MEMS arrays detect small drones at 100-300m in quiet environments. Urban noise reduces significantly.
3. **Visual range:** 12MP + 50mm lens detects Mavic at ~500m in good conditions. Thermal effective to ~200-400m.
4. **Sensor fusion:** Acoustic provides bearing, RF confirms transmission, visual/thermal tracks and classifies. Jetson runs all three ML pipelines simultaneously.
5. **YOLOv8 for drones:** Pre-trained models exist (Anti-UAV dataset). Fine-tune YOLOv8-nano, export TensorRT INT8, expect ~40-65 FPS on Orin Nano.

## Sources
- [Jetson Orin Nano Super](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/nano-super-developer-kit/)
- [RPi AI HAT+](https://www.raspberrypi.com/news/raspberry-pi-ai-hat/)
- [miniDSP UMA-16](https://www.minidsp.com/products/usb-audio-interface/uma-16-microphone-array)
- [HackRF Pro](https://greatscottgadgets.com/hackrf/pro/)
- [RTL-SDR V4](https://www.rtl-sdr.com/rtl-sdr-blog-v4-dongle-initial-release/)
- [FLIR Lepton 3.5](https://oem.flir.com/products/lepton)
- [PureThermal 3](https://groupgets.com/products/purethermal-3)
- [Pelican 1450](https://www.pelican.com/us/en/product/cases/protector/1450)
- [EcoFlow RIVER 2 Pro](https://us.ecoflow.com/pages/river-portable-power-stations)
- [Arducam IMX477](https://www.amazon.com/Arducam-Vision-Automatic-Switching-All-Day/dp/B08PFJDJC9)
