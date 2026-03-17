# Counter-Drone Detection Technology: Detailed Research

## 1. Acoustic Drone Detection Systems

### Companies and Products

**Squarehead Technology (Norway)** — Market leader in acoustic drone detection.
- **Discovair G2+** — 128 MEMS microphones in a passive array with integrated optical camera. IP65-rated. 105-degree FoV per sensor; single portable sensor can provide 360-degree coverage. ML-based classification that recognizes drone types it has never seen before. Partnered with DroneShield. Pricing likely $50K-$200K+ per unit.

**Zvook (Ukraine)** — Battle-proven acoustic detection, deployed covering ~5% of Ukrainian territory.
- Architecture: Parabolic acoustic mirror (~0.5m diameter) + microphone + Android phone + backup battery + AI software
- Detects drones at up to 5 km, cruise missiles at 7 km. FPV-specific model detects at 150-450m range, 360-degree coverage
- **Cost: ~$500 per station.** False positive rate of 1.6%. Detections appear in Ukraine's Delta system within 12 seconds
- **This is the most relevant model for a low-cost startup prototype**

**Sky Fortress (Ukraine)** — Being tested by Lithuanian Armed Forces for deployment alongside radar in 2026.

**Fraunhofer IDMT (Germany)** — Detection/localization range of 50-200m depending on noise environment. Designed for integration with radar, camera, and lidar.

### How Acoustic Detection Works
Microphone arrays use beamforming and direction-of-arrival (DoA) algorithms to isolate drone motor/propeller noise from ambient sound. ML classifiers (typically CNNs trained on Mel-Frequency Cepstral Coefficients or Log-Mel spectrograms) distinguish drone signatures from birds, vehicles, and other noise. Triangulation across multiple nodes provides geolocation.

### Key Army Requirement (January 2026 RFI)
Army C5ISR Center issued RFI seeking acoustic detection for Group 1 (under 20 lbs) and Group 2 (21-55 lbs) UAS for dismounted troops. Requirements: passive detection, determine location/bearing/classification, notify via TAK, function on-the-move, small/light/low power, embedded detection algorithms.

---

## 2. RF-Based Passive Drone Detection Systems

| Company | Product | Key Specs | Price Range |
|---------|---------|-----------|-------------|
| **Dedrone** | DedroneSensor RF | Detects/classifies/locates by RF signature. Covers entire DJI line. ML classification. | ~$20K-$50K/sensor |
| **DroneShield** | RfOne MKII | Passive RF direction-finding. Long-range. No emissions. | ~$30K-$100K+ |
| **CRFS** | RFeye sensors | Passive RF geolocation. Military drones at up to 400 km. 3D geolocation. | Enterprise pricing |
| **D-Fend Solutions** | EnforceAir | RF detection + cyber takeover. VIP/critical infrastructure. | Premium |
| **BlueHalo** | C-UAS RF | Broad RF signature library. Long-range sUAS detection. | Military contract |
| **Guardian RF** | Passive RF sensors | YC W24 startup. Fielded at Vandenberg SFB. Law enforcement deployments. | Startup pricing |
| **AeroDefense** | AirWarden | Uses FAA Remote ID signals. Pinpoints pilot location + drone details. | More affordable |

### Critical Limitation of RF Detection
RF detection **cannot detect autonomous or fiber-optic FPV drones** — these emit no RF signal during terminal attack phase. Russia has deployed fiber-optic FPV drones at scale since spring 2024, striking up to 19 km behind lines with complete immunity to RF detection and EW jamming. NATO issued an urgent Innovation Challenge in April 2025 for this exact problem, with required detection range of only 300-500m.

---

## 3. Computer Vision / Camera-Based Detection

- **Dedrone** — 18M+ image training set. ML with behavior model filters. Near-zero false positives claimed.
- **Pelco** — PTZ cameras with up to 60x zoom. Auto-track from radar/RF cues.
- **Walaris** — AirScout Sentry: 4-6 multispectral cameras per deployment.
- **Teledyne FLIR** — Cooled/uncooled thermal detecting small UAVs at 5+ km.

Vision-based systems struggle with false positives when drones have poor contrast or complex shapes (birds). Night/thermal extends capability but resolution drops with range.

---

## 4. Technical Specifications for a Prototype

### Microphone Array Hardware
- **MEMS Microphones:** InvenSense ICS-40800, Infineon IM69D130 (69 dB SNR, 130 dB AOP), ST MP34DT01TR-M
- **Array Configuration:** Minimum 4 mics for 2D DoA; 8-16 recommended; research uses 32-128
- **Pre-built Arrays:** Seeed Studio ReSpeaker USB Mic Array v3.0 (4 mics, XMOS XVF3800, 360-degree, built-in beamforming/DoA)
- **Sampling:** Minimum 48 kHz; drone propeller harmonics typically 100 Hz - 8 kHz

### SDR / RF Detection Hardware
| Device | Frequency Range | Price | Notes |
|--------|----------------|-------|-------|
| **RTL-SDR v4** | 24-1766 MHz | ~$35 | Receive-only, good for initial experiments |
| **KrakenSDR** | 24-1766 MHz (5 coherent channels) | **$750** (+$199 for antennas) | **Best value for direction-finding** |
| **HackRF One** | 1 MHz - 6 GHz | ~$300 | Covers 2.4/5.8 GHz drone frequencies |
| **ADALM-PLUTO** | 325 MHz - 3.8 GHz | ~$250 | Full-duplex |
| **USRP B210** | DC - 6 GHz | ~$2,000+ | Research-grade |

Key drone frequencies: **2.4 GHz** (Wi-Fi/control), **5.8 GHz** (video/control), **900 MHz** (long-range), **1.2 GHz** (analog video), **433 MHz** (telemetry).

### Camera / EO-IR Hardware
| Device | Specs | Price |
|--------|-------|-------|
| **FLIR Lepton 3.5** | 160x120 thermal, radiometric | **$164** (module) |
| **PureThermal 3** | Carrier board for Lepton | ~$200 |
| **Arducam IMX477** | 12.3 MP visible, C/CS mount | ~$60-80 |

### Compute Platform
| Device | AI Performance | Price |
|--------|---------------|-------|
| **NVIDIA Jetson Orin Nano Super** | **67 TOPS** | **$249** |
| **Raspberry Pi 5 (8 GB)** | CPU-only inference | ~$80 |

---

## 5. Sub-$10K Prototype BOM

### Option A: Multi-Modal Detection Node (~$2,700-$3,100)

| Component | Purpose | Est. Cost |
|-----------|---------|-----------|
| KrakenSDR + 5 antennas | RF direction-finding | $950 |
| HackRF One | 2.4/5.8 GHz monitoring | $300 |
| ReSpeaker XVF3800 x2 | Acoustic array | $100 |
| Additional MEMS mics + custom PCB | Extended acoustic | $200-500 |
| NVIDIA Jetson Orin Nano Super | Edge AI inference | $249 |
| FLIR Lepton 3.5 + PureThermal 3 | Thermal detection | $365 |
| Arducam IMX477 + telephoto lens | Visible-light detection | $130 |
| Raspberry Pi 5 (8 GB) | RF processing node | $80 |
| Enclosure, cables, power | Integration | $300-500 |
| Software (GNU Radio, YOLOv8, PyTorch) | Detection algorithms | $0 |
| **TOTAL** | | **~$2,700-$3,100** |

### Option B: Distributed Acoustic Network (Zvook-inspired, ~$640-$2,040)

| Component | Purpose | Est. Cost |
|-----------|---------|-----------|
| Parabolic reflectors (3D-printed, 0.5m) x4 | Acoustic concentration | $100-200 |
| High-sensitivity MEMS mics x4 | Pickup elements | $40 |
| Raspberry Pi x4 | Edge processing nodes | $320-600 |
| Central server (Pi 5) | Triangulation + display | $80 |
| Custom ML pipeline | Detection/classification | $0 |
| LoRa modules or Wi-Fi mesh | Node communication | $100-200 |
| **TOTAL** | | **~$640-$2,040** |

---

## 6. DoD Programs and Contracts

- **Anduril $20B Army Contract** (March 2026): Enterprise-wide counter-drone
- **DIU + NORTHCOM Partnership** (May 2025): Counter-drone for homeland defense
- **DIU Counter-Drone Sensing Initiative** (Feb 2026): Phase 2 demos at Yuma Proving Ground
- **MatrixSpace** won DIU C-UAS Low-Cost Sensing Challenge (Dec 2025, $500K prize)
- **Guardian RF** and **Squarehead** were DIU finalists ($100K prizes)
- **Army C5ISR Center Acoustic Detection RFI** (Jan 2026)
- **Replicator 2**: Low-collateral C-sUAS, first contract Jan 2026
- **C-UAS Grant Program**: $500M FY2026 for state/local detection

## 7. SBIR/Funding Opportunities

| Program | Topic | Funding |
|---------|-------|---------|
| Army SBIR | Open Multi-Sensor C-UAS Software | Phase I |
| Army SBIR | UAS Passive Detection for Ground Vehicles | Phase I/II |
| Army SBIR | Low-Altitude sUAS Detection | **$2M** |
| **xTechCounter Strike** | C-UAS innovation competition | Up to $350K per winner + G-TEAD access |
| **xTech Adaptive Strike** | Deadline March 13, 2026 | Up to $1.5M |
| **NATO Innovation Challenge** | Fiber-optic FPV drone detection | Varies |

**Note:** SBIR/STTR expired Oct 1, 2025 — new solicitations paused pending reauthorization.

## 8. Market Gaps — Where a Startup Wins

1. **Autonomous / RF-Silent Drone Detection** — Fiber-optic and autonomous drones emit zero RF. Current RF systems are blind. Acoustic + thermal/vision is the viable approach. **Highest-value wedge.**

2. **Low-Cost Distributed Acoustic Networks** — Zvook proves concept at $500/node. No Western commercial equivalent at that price. Army C5ISR RFI specifically seeks this.

3. **Multi-Modal Sensor Fusion at the Edge** — Most products are single-modality. True multi-modal fusion on edge compute in small form factor doesn't exist at affordable price.

4. **Detection-Only Tier** — Many customers just need to KNOW a drone is present, can't legally use defeat. Simpler, cheaper product.

5. **State/Local/Critical Infrastructure Market** — $500M FEMA grant program creates buyers who can't afford $100K+ military systems. A $5K-$15K package is the gap.

## Sources
- [Squarehead Drone Detection](https://www.sqhead.com/drone-detection)
- [Zvook Tech](https://www.zvook.tech/en)
- [Sky Fortress / United24](https://united24media.com/war-in-ukraine/sky-fortress-ukraines-acoustic-detection-system-that-tracks-drones-cheap-and-fast-9451)
- [Fraunhofer IDMT](https://www.idmt.fraunhofer.de/en/Press_and_Media/press_releases/2025/acoustic-drone-detection.html)
- [Army Acoustic Detection RFI - DefenseScoop](https://defensescoop.com/2026/01/21/army-counter-drone-small-uas-acoustic-detection-systems/)
- [DroneShield RfOne MKII](https://www.droneshield.com/c-uas-products/rfone-mk2)
- [CRFS Drone Detection](https://www.crfs.com/solutions/drone-detection)
- [AeroDefense AirWarden](https://aerodefense.tech/drone-detection-system-products/)
- [NVIDIA Jetson Orin Nano Super](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/nano-super-developer-kit/)
- [KrakenSDR](https://www.crowdsupply.com/krakenrf/krakensdr)
- [FLIR Lepton 3.5](https://groupgets.com/products/flir-lepton-3-5)
- [Anduril $20B Contract](https://defensescoop.com/2026/03/14/anduril-20-billion-dollar-army-contract/)
- [DIU Counter-Drone Sensing Feb 2026](https://dronelife.com/2026/02/13/diu-launches-new-counter-drone-sensing-initiative-for-homeland-and-mobile-defense/)
- [Army SBIR Low-Altitude sUAS Detection](https://armysbir.army.mil/announcement/launch-2m-funding-opportunity-low-altitude-suas-detection/)
- [xTechCounter Strike](https://xtech.army.mil/competition/xtechcounterstrike/)
