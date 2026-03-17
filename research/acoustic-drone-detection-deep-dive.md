# Acoustic Drone Detection: Technical Deep Dive

## 1. Physics of Acoustic Drone Detection

### Noise Sources
1. **Aerodynamic noise (dominant):** Blade Passing Frequency (BPF) + harmonics + tip vortex broadband. BPF = (RPM/60) x number_of_blades.
2. **Motor/ESC noise:** Electromagnetic, 600-6000 Hz, adds ~22 dB.
3. **Structural vibration:** Frame resonances from motor vibration.

### Frequency Ranges by Drone Type

| Drone Type | RPM | BPF | Key Energy | Bandwidth |
|---|---|---|---|---|
| DJI Mavic series | 4500-6000 | 150-200 Hz | Up to 4th-5th harmonic | 100 Hz - 8 kHz |
| DJI Phantom/M600 | 3000-5500 | 100-183 Hz | Harmonics to ~1.5 kHz | 80 Hz - 6 kHz |
| FPV racing quads | 8000-30000+ | 267-1500 Hz | Extends to 10-16 kHz | 200 Hz - 16 kHz |
| Fixed-wing (pusher) | 5000-10000 | 167-333 Hz | Narrower harmonic stack | 100 Hz - 4 kHz |
| Large octocopters | 2000-4000 | 67-133 Hz | Multi-rotor interaction tones | 50 Hz - 4 kHz |

Multi-rotors exhibit **rotor-rotor interaction tones** (beat frequencies from mismatched RPM) — strong discriminator from birds/aircraft.

### Propagation
- ~6 dB per doubling of distance (inverse-square)
- Atmospheric absorption: ~1-3 dB/100m above 2 kHz
- Temperature gradients and wind create refraction effects

## 2. Key Academic Papers

- **Al-Emadi et al. (2019)** — "Audio Based Drone Detection and Identification using Deep Learning" (IWCMC). CNN, RNN, CRNN on 1300+ clips. Foundational.
- **EUSIPCO 2017** — "Empirical Study of Drone Sound Detection with DNNs." Code: github.com/sdeva14/eusipco17-drone-sound-detection
- **DronePrint (ACM IMWUT 2021)** — Open-set detection of 13 drone models. MFCC + LSTM. 96% closed-set, 86% open-set.
- **EUSIPCO 2020** — YAMNet transfer learning detector, detection to 500m.
- **ResNet10_CBAM (Drones, 2025)** — Strong low-SNR performance with attention mechanism.

## 3. Open-Source Repos

| Repository | Description |
|---|---|
| sdeva14/eusipco17-drone-sound-detection | RNN/CNN/GMM detection (Python) |
| kbhujbal/SudarshanChakra-acoustic_uav_threat_detection_CNN | Production-grade CNN pipeline with defense-optimized metrics |
| saraalemadi/DroneAudioDataset | Drone audio + noise augmentation |
| Adnan0032/Drone-Sound-Detection-with-TinyML-on-Raspberry-Pi-4 | TinyML on RPi4, MFCC + TFLite |
| augmented-human-lab/DroneAudioSet-code | Code for 23.5-hour DroneAudioset |
| james-godito/sound_localization | GCC-PHAT source localization |

## 4. Microphone Array Design

### How Many Mics for What Task

| Capability | Min Mics | Recommended | Notes |
|---|---|---|---|
| Detection only | 1 | 2-4 | Single mic OK if SNR adequate |
| 2D direction-finding | 3 | 4-8 circular/cross | Delay-and-sum beamforming |
| 3D direction-finding | 4 non-coplanar | 8-16 | Need vertical extent |
| High-res DOA + tracking | 8+ | 16-32 | MUSIC/ESPRIT need N > sources |
| Ranging | 2 distributed arrays | 2+ arrays, each 4+ mics | Triangulation |

### Spacing Rules
- **d < c / (2 * f_max)** where d = spacing, c = 343 m/s
- For 4 kHz max: d < 4.3 cm
- For 8 kHz max: d < 2.1 cm
- Angular resolution improves with larger aperture — tension with aliasing constraint
- Solution: non-uniform/sparse arrays, nested spacing

### Recommended Configuration
- **8-16 MEMS microphones** in cross or circular arrangement
- **Aperture: 30-50 cm diameter**
- **Inner spacing: 2-4 cm** (handles up to 4-8 kHz)
- **Outer spacing: 15-25 cm** (resolution at BPF ~150-300 Hz)
- Mics: ICS-43434 (I2S, 70 dB SNR) or IMP34DT05 (PDM, 64 dB SNR), ~$1-2 each

## 5. Signal Processing Pipeline

```
Raw Audio (16-48 kHz)
    |
[Windowing + Pre-emphasis]
    |
[STFT/FFT] -> Power Spectrum
    |
[Harmonic Product Spectrum] -> BPF extraction (fast trigger)
    |
[Mel Spectrogram / MFCC] -> Feature tensor
    |
[CNN / CRNN classifier] -> Detection / Classification
    |
[GCC-PHAT Beamforming] -> Direction of Arrival
    |
[Kalman filter tracking] -> Bearing track over time
```

### Best Features
- **MFCC (13-40 coefficients):** Best overall. SVM + 13 MFCCs = 96.4-96.7% accuracy. Cheap to compute.
- **Log Mel Spectrograms (64-128 bins):** Best for CNN input. Low false alarm rate.
- **Harmonic Product Spectrum:** Not ML — extremely fast first-stage trigger.

### DOA Algorithms
- **GCC-PHAT:** Workhorse. Robust, computationally light. Best for real-time edge.
- **Delay-and-Sum:** Simplest. Good enough for 8-16 mic arrays.
- **MUSIC:** Super-resolution. Higher compute, resolves multiple drones.
- **SRP-PHAT:** Grid search with GCC-PHAT. More robust for 3D localization.

## 6. ML Models for Edge Hardware

### Architecture Comparison

| Model | Accuracy | Size | Inference (edge) | Viability |
|---|---|---|---|---|
| Small CNN (3-5 layers) on mel spec | 92-96% | 0.5-5 MB | 5-20 ms Jetson Nano | **Excellent** |
| MobileNetV2 on mel spec | 94-97% | 3.5 MB INT8 | 8-15 ms Jetson Orin | **Excellent** |
| YAMNet (transfer learning) | 95-98% | ~14 MB | 20-40 ms Jetson | Good |
| ResNet10_CBAM | Strong low-SNR | ~10 MB | 20-30 ms Jetson | Good |
| Transformer | Highest raw | 80+ MB | 100+ ms | Marginal |

### Recommended: Two-Stage Pipeline

**Stage 1 — Always-on trigger (<100 mW):** Harmonic product spectrum on raw FFT. Runs on STM32/ESP32. Detects harmonic stack in 100-2000 Hz range, wakes Stage 2.

**Stage 2 — Classification (7-15W when active):** MobileNetV2 or custom 4-layer CNN on 64-bin mel spectrogram, INT8 quantized. Runs on Jetson Orin Nano (40 TOPS) or Hailo-8 (26 TOPS). <20 ms inference.

## 7. Detection Range (Empirical)

| System | Drone | Max Detection | Conditions |
|---|---|---|---|
| Simple energy detector | DJI Mavic | ~100-150 m | Quiet rural |
| YAMNet transfer learning | Various | Up to 500 m | Low wind |
| **Squarehead Discovair G2+** | DJI S1000 | **180 m** | Operational |
| Squarehead Discovair G2+ | DJI Mavic Pro | **120 m** | Operational |
| Squarehead Discovair G2+ | DJI Spark | **90 m** | Operational |
| 256-element research array | Medium quad | **1300 m** | Ideal |
| Ukrainian ZVOOK | FPV drones | ~200-500 m/node | **Combat** |

**Plan for:** 100-300 m reliable detection, 50-200 m classification, 50-250 m direction-finding.

### Environmental Degradation

| Factor | Impact |
|---|---|
| Wind 10-15 km/h | -30 to -50% range |
| Wind 25+ km/h | Severe degradation |
| Windscreen (foam/fur) | +31-131% range extension |
| Urban ambient (60-70 dBA) | Range drops to 50-100 m for small drones |
| Rain | Moderate broadband noise increase |

## 8. Acoustic vs. RF vs. Radar vs. Visual

| Criterion | Acoustic | RF | Radar | Visual (EO/IR) |
|---|---|---|---|---|
| Range | 100-500 m | 1-5 km | 5-30 km | 500m-2 km |
| **Fiber-optic drones** | **YES** | **NO** | YES | YES |
| **Autonomous drones** | **YES** | **NO** | YES | YES |
| Passive (no emissions) | **YES** | YES | NO | YES (EO) |
| Cost per sensor | **$400-$5K** | $5K-50K | $100K-$10M+ | $10K-100K |
| Night operation | **YES** | YES | YES | IR only |
| All-weather | Degraded wind/rain | YES | YES | Degraded fog/rain |
| SWaP | **Very low** | Low-mod | High | Moderate |
| Licensing needed | **None** | Possible | Usually | None |

### Why Acoustic is the Wedge

1. **Fiber-optic drones** make RF blind — acoustic becomes primary detection modality
2. **Lowest cost per node** — enables dense coverage impossible with radar/RF
3. **Truly passive** — no emissions, critical for forward positions
4. **Complementary** — ideal first layer that cues expensive sensors
5. **Works against autonomous drones** — another growing threat invisible to RF
6. **No regulatory burden** — no licensing
7. Weakness (limited range, wind sensitivity) mitigated by **dense networked mesh** — the Ukrainian model

## 9. Open Datasets

| Dataset | Drones | Size | License |
|---|---|---|---|
| **DroneAudioset** | Multiple types | **23.5 hours** | MIT |
| **Multiclass Acoustic (2025)** | **32 UAV classes** | Comprehensive | Open |
| **Al-Emadi DroneAudioDataset** | Multiple | 1300+ clips | Open |
| **DroneDetectionThesis** | Multiple | Varied | Open |

**Data gap:** Almost no public datasets of fiber-optic FPV drone audio. Building operational dataset = competitive moat.

## 10. Fiber-Optic Drones and the Detection Equation

Fiber-optic FPV drones use 0.1-0.3mm cable for video/control. **Eliminates RF command link entirely.**

**What breaks:** RF detection, RF jamming, RF direction-finding.

**What still works:** Acoustic (drones are often *louder* due to cable spool weight), Visual/IR, Radar (if RCS sufficient).

Ukraine deployed 14,000+ acoustic sensors achieving 95% interception support rate specifically because of this shift.

### Implications for Product Spec
1. Acoustic detection is now **primary**, not supplementary
2. Every RF-based C-UAS system has a blind spot your product fills
3. Fiber-optic FPVs have distinctive signatures (cable spool motor, higher-thrust RPM)
4. Mesh networking essential — dense cheap sensors > fewer expensive ones
5. **Latency critical** — FPV at 100 km/h covers 100m in 3.6 seconds. Detection-to-alert must be <1-2 seconds.
6. Integration API essential — output standardized CoT/MQTT messages for any C2 system

## Product Specification Summary

- **Array:** 8-16 MEMS mics, cross/circular, 30-50 cm aperture, non-uniform spacing
- **Processing:** Two-stage: STM32 trigger (<100 mW) + Jetson Orin Nano classification (7-15W)
- **ML:** MobileNetV2 or custom CNN on 64-bin mel spectrogram, INT8, <20 ms inference
- **DOA:** GCC-PHAT real-time, optional MUSIC for multi-target
- **Detection range:** 150-300 m (small FPV), 300-500 m (larger platforms)
- **Networking:** Mesh (LoRa, Wi-Fi mesh, tactical radio) for triangulation
- **Output:** Bearing, classification, confidence, track ID via MQTT or CoT for C2
- **Enclosure:** IP67, MIL-STD-810G, windscreen mandatory
- **Power:** <20W average, battery + solar rechargeable or vehicle power
- **Unit cost target:** $500-2,000 (compete at ZVOOK tier for dense deployment)

## Sources
- [Squarehead Drone Detection](https://www.sqhead.com/drone-detection)
- [ZVOOK](https://www.zvook.tech/en)
- [Sky Fortress - United24](https://united24media.com/war-in-ukraine/sky-fortress-ukraines-acoustic-detection-system-that-tracks-drones-cheap-and-fast-9451)
- [Army Acoustic Detection RFI - DefenseScoop](https://defensescoop.com/2026/01/21/army-counter-drone-small-uas-acoustic-detection-systems/)
- [Fiber-Optic Drones - Atlantic Council](https://www.atlanticcouncil.org/blogs/ukrainealert/fiber-optics-drones-have-emerged-as-critical-kit-for-both-russia-and-ukraine/)
- [NATO Fiber-Optic FPV Challenge](https://www.jedonline.com/2025/05/15/nato-seeks-solutions-for-fiber-optic-fpv-drones/)
- [Ukraine Acoustic Network - The War Zone](https://www.twz.com/air/ukraines-acoustic-drone-detection-network-eyed-by-u-s-as-low-cost-air-defense-option)
- [DroneAudioset - HuggingFace](https://huggingface.co/datasets/ahlab-drone-project/DroneAudioSet/)
- [Multiclass Acoustic Dataset](https://arxiv.org/abs/2509.04715)
- [Acoustic UAV Detection CNN - GitHub](https://github.com/kbhujbal/SudarshanChakra-acoustic_uav_threat_detection_CNN)
- [TinyML Drone Detection RPi4 - GitHub](https://github.com/Adnan0032/Drone-Sound-Detection-with-TinyML-on-Raspberry-Pi-4)
