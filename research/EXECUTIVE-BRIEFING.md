# FORGE: Executive Briefing
## A Counter-Drone Detection Startup for American Defense

---

## The Opportunity in One Paragraph

Cheap drones have transformed warfare and border security. RF-based detection — the dominant technology — is going blind as fiber-optic and autonomous drones proliferate. The SAFER SKIES Act (December 2025) just authorized state and local law enforcement to counter drones for the first time, creating thousands of new buyers with authority but no tools. The DoD, DHS, and FEMA are collectively spending **billions** on counter-drone in 2026. Ukraine proved that dense networks of low-cost acoustic sensors ($500/unit) can detect drones that defeat all other methods. No Western company sells this at scale. **This is the wedge.**

---

## The Product: FORGE Sentinel

A ruggedized, offline, edge-AI-powered acoustic drone detection node designed for networked deployment.

### What It Does
- Detects approaching drones by sound using a MEMS microphone array and on-device ML
- Classifies drone type (multirotor, FPV, fixed-wing) and estimates bearing
- Works against **fiber-optic and autonomous drones** that are invisible to RF detection
- Operates fully offline — no cloud, no network required for detection
- Logs all detections with tamper-evident audit trail
- Networks with other Sentinel nodes for triangulation and area coverage
- Outputs standardized Cursor-on-Target (CoT) messages for integration with TAK and any C2 system

### Why It Wins
1. **Solves the fiber-optic drone problem** — the #1 gap in current C-UAS. NATO issued an urgent challenge for exactly this.
2. **$500-2,000 per unit** — 10-100x cheaper than RF/radar alternatives. Enables dense deployment.
3. **Detection-only = no legal barriers.** No ITAR, no FCC licensing, no jamming authorities needed.
4. **Under $15,000 unit cost** — DoD can buy on a purchase card with zero procurement friction.
5. **Network effects** — one unit is a sensor, 100 units is a detection grid with triangulation.
6. **Dual-use from day one** — prisons, stadiums, critical infrastructure, border, military.

---

## Market Size

| Segment | 2025 | 2030 | CAGR |
|---|---|---|---|
| Drone detection specifically | $858M | $2.8B | 28.9% |
| Counter-UAS total | $3.1-6.6B | $20.3B | 25.1% |

### Immediate Addressable Markets
- **FEMA C-UAS Grant Program:** $500M ($250M FY26, $250M FY27) for state/local detection
- **DHS C-UAS IDIQ:** $1.5B contract vehicle, mandatory across CBP/ICE/USCG/Secret Service
- **DIU Counter-Drone Sensing Initiative:** Active CSO, demos at Yuma spring 2026
- **Army C5ISR Acoustic Detection RFI:** Specifically seeking acoustic C-UAS for dismounted troops
- **xTechCounter Strike:** Up to $350K per winner + G-TEAD Marketplace access
- **Correctional facilities:** 9,000 facilities, 43% increase in drone incidents. Est. $2.5B market.
- **Commercial venues below NFL tier:** 7,000+ venues, est. $3.6B market.

---

## Competitive Landscape

### What exists today (and why there's room)

| Company | Approach | Price Point | Gap |
|---|---|---|---|
| Anduril | Full-spectrum C-UAS (Lattice + Anvil + Pulsar) | $20B Army contract level | Not selling to small buyers |
| Dedrone (Axon) | RF primary, camera secondary | RF-160 at $11,500/unit; systems $250K+ | Blind to fiber-optic/autonomous drones |
| DroneShield | RF + EW + acoustic (secondary) | Systems $30K-100K+ | Acoustic not primary modality |
| Squarehead | Acoustic arrays (128 mics) | Est. $50K-200K/unit | Expensive, not designed for dense mesh |
| ZVOOK (Ukraine) | Acoustic mesh ($500/unit) | $500/node | Not commercially available in the West |

**The gap:** No Western company sells a low-cost ($500-2K) networked acoustic detection node. ZVOOK proved the model at scale (14,000+ sensors, 95% interception support rate) but isn't a commercial product available to US buyers.

---

## Technical Architecture

### Per Node (BOM: ~$2,330)
- **Compute:** NVIDIA Jetson Orin Nano Super (67 TOPS, $249)
- **Acoustic:** miniDSP UMA-16 v2 (16-ch MEMS array, $275)
- **RF:** HackRF Pro + RTL-SDR V4 ($430) — complementary, not primary
- **Visual:** FLIR Lepton 3.5 thermal + Arducam IMX477 visible ($320)
- **Enclosure:** Pelican 1450, IP67 ($196)
- **Power:** 768Wh LiFePO4 + 100W solar ($500) — ~23 hours without sun
- **Total system draw:** ~33-37W

### ML Pipeline
- **Stage 1 (always-on, <100mW):** Harmonic product spectrum trigger on MCU
- **Stage 2 (on-demand, 7-15W):** MobileNetV2 on 64-bin mel spectrogram, INT8, <20ms inference
- **DOA:** GCC-PHAT for real-time bearing estimation
- **Visual:** YOLOv8-nano for drone detection/tracking (~43 FPS)

### Detection Performance (expected)
- Small FPV drone: 150-300m
- Larger platform (Mavic/Phantom class): 300-500m
- Classification range: 50-200m
- Direction-finding: 50-250m
- Alert latency: <2 seconds

### Key Advantage Over RF
Fiber-optic FPV drones (the fastest-growing threat class) emit **zero RF**. Autonomous waypoint drones emit **zero RF**. These are completely invisible to Dedrone, DroneShield's RF sensors, and most deployed C-UAS. They still have motors and propellers that make sound. Acoustic detection is the primary viable approach.

---

## Go-to-Market Strategy

### Phase 1: Prototype & Validate (Months 1-6, $10K)

| Action | Timeline | Cost |
|---|---|---|
| Incorporate Delaware C-Corp | Week 1 | $1,000 |
| SAM.gov registration (UEI, CAGE) | Weeks 2-5 | $0 |
| Basic insurance (CGL) | Week 4 | $1,500 |
| Join NSTXL + SOSSEC consortia | Weeks 2-4 | $750 |
| Build 3 prototype nodes | Weeks 4-12 | $6,990 |
| Field test (demonstrate triangulation) | Weeks 10-14 | $0 |
| Submit DIU solution brief (C-UAS sensing CSO) | Month 2 | $0 |
| Enter xTechCounter Strike competition | When open | $0 |
| **Total** | | **~$10,240** |

### Phase 2: First Revenue (Months 6-18)
- Win DIU prototype OTA ($100K-500K) or xTech prize ($50-350K)
- Respond to DHS $1.5B C-UAS IDIQ solicitation
- Target FEMA-funded state/local agencies (detection-only, $5K-15K/unit)
- Sell direct to correctional facilities and small critical infrastructure
- Apply to Army SBIR for acoustic C-UAS when reauthorized

### Phase 3: Scale (Months 18-36)
- SBIR Phase II ($500K-1.5M)
- AFWERX STRATFI ($3M-15M with matching)
- MIL-STD testing and certification
- Production manufacturing partnership
- Build champion inside a program office for APFIT nomination ($10M-50M)

### Pricing Model
| Tier | What | Price | Margin |
|---|---|---|---|
| Single node (COTS) | Detection + classification + bearing | $12,000 | ~80% |
| 3-node network kit | Triangulation + area coverage | $30,000 | ~75% |
| Software subscription | Updates, new drone signatures, cloud dashboard | $200/node/month | ~95% |
| Enterprise | Custom integration, TAK/C2 support, training | Negotiated | ~60% |

At $12K/unit with $2.3K BOM, gross margin is ~80% — healthy for defense hardware.
Under $15K means DoD units can buy on a **government purchase card** with minimal friction.

---

## Funding Pathway

| Source | Amount | Timeline | Notes |
|---|---|---|---|
| Self-funded (current) | $10K | Now | Prototype + registration |
| xTech prize | $50K-350K | Months 3-9 | Non-dilutive |
| DIU prototype OTA | $100K-500K | Months 4-8 | 60-90 day award |
| SBIR Phase I (when reauthorized) | $50K-150K | Months 6-10 | 6-month performance period |
| Direct commercial sales | Variable | Months 6+ | Prisons, infrastructure |
| SBIR Phase II | $500K-1.5M | Months 14-20 | 24-month period |
| Seed VC (if desired) | $1M-5M | Months 12-18 | Defense tech VC is hot ($49B in 2025) |

---

## Risk Assessment

| Risk | Severity | Mitigation |
|---|---|---|
| Acoustic range insufficient in noisy environments | High | Dense mesh deployment (more nodes = more coverage). Multi-modal fusion with thermal/RF. |
| Established player copies approach | Medium | Speed to market. Build operational dataset (competitive moat). Get on SAFER SKIES approved list early. |
| SBIR never reauthorized | Medium | OTA and DIU pathways don't depend on SBIR. Direct commercial sales. |
| False positive rate too high for operational use | High | Multi-modal confirmation (acoustic trigger -> visual/thermal confirm). ML model improvement with operational data. |
| Single founder, no hardware background | High | Leverage open-source (repos exist). Contract PCB design. Focus on system integration and compliance, not component design. |

---

## What Makes This Different

Most defense startups start with technology looking for a problem. This starts with:

1. **A validated operational model** (Ukraine's 14,000 acoustic sensors)
2. **A market gap** (no Western commercial equivalent)
3. **Perfect timing** (SAFER SKIES Act, FEMA $500M grants, DHS $1.5B IDIQ, fiber-optic drone proliferation)
4. **A founder whose strengths match the market need** (compliance, traceability, audit trails — exactly what DoD buyers require)
5. **A product that costs $2,300 to build and sells for $12,000** with a clear path to volume

The narrow wedge is acoustic drone detection. The network effect is mesh deployment. The platform play is sensor fusion and C2 integration. The moat is operational data and early regulatory approval.

---

## First Week Actions

- [ ] Incorporate Delaware C-Corp
- [ ] Get EIN from IRS (same day)
- [ ] Open business bank account
- [ ] Begin SAM.gov registration
- [ ] Order Jetson Orin Nano Super Dev Kit ($249)
- [ ] Order miniDSP UMA-16 v2 ($275)
- [ ] Download and run open-source drone audio detection repo
- [ ] Start collecting drone audio samples (buy a $50 toy drone for recording)

---

*This briefing synthesizes research from 7 parallel investigations conducted March 17, 2026. Full research documents available in /forge/research/.*
