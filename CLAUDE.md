# Forge

## What This Is

A business planning and product specification exercise for a defense technology startup. The goal is to identify a concrete, buildable hardware product that serves the Department of Defense, can be manufactured on a limited budget, and is designed from day one to integrate into broader networked systems.

## Guiding Principles

- **Profitability and honor** — we build things that make money *and* strengthen American defense. These are not in tension.
- **Network effects by design** — individual units should be useful, but a fleet of them should be transformatively more useful. Design for integration from the start.
- **Budget-constrained execution** — we don't have Anduril's funding. Plans must be executable with limited capital, Claude Code instances, and sweat equity. Prefer approaches where a small team can punch above its weight.
- **Specificity over ambition** — Anduril started with border intrusion detection, not "revolutionize defense." We need to find our equivalent: a real problem, a buildable device, a willing buyer.

## The Product: FORGE Sentinel

A ruggedized, offline, edge-AI-powered **acoustic drone detection node** designed for networked mesh deployment. Detects drones (including fiber-optic and autonomous drones invisible to RF) by sound using MEMS microphone arrays and on-device ML. $2,300 BOM, $12K sell price, under the $15K purchase card threshold.

**The wedge:** Ukraine proved 14,000+ acoustic sensors at $500/unit achieve 95% interception support rates. No Western company sells this commercially. The SAFER SKIES Act (Dec 2025) just created thousands of new buyers. DHS has $1.5B in counter-drone contracts. This is the gap.

## Current Phase

**Phase 1: Prototype & Validate**

1. ~~Identify the problem~~ — Counter-drone detection, specifically acoustic detection of RF-silent drones
2. ~~Research the market~~ — See /research/ directory (7 deep-dive documents)
3. Incorporate Delaware C-Corp, SAM.gov registration
4. Build 3 prototype nodes and demonstrate networked triangulation
5. Submit DIU solution brief and enter xTech competitions
6. Write plan/spec compelling enough to secure OTA or SBIR funding

## Research Completed

See `/research/` directory:
- `EXECUTIVE-BRIEFING.md` — Full synthesis and go-to-market strategy
- `counter-drone-detection-tech.md` — Existing products, specs, prices, gaps
- `counter-drone-market-landscape.md` — Market size, players, underserved segments
- `acoustic-drone-detection-deep-dive.md` — Physics, ML models, array design, open-source tools
- `edge-ai-hardware-bom.md` — Components, BOMs, budget allocation
- `dod-procurement-pathways.md` — OTA, DIU, SBIR, purchase cards, DHS contracts
- `defense-startup-formation.md` — Incorporation, SAM.gov, clearances, ITAR, CMMC, insurance
- `border-security-drone-threat.md` — Cartel drones, deployed systems, contracts, legal framework

## How to Help

When working in this repo, Claude should:
- Push toward specificity — vague strategy is worthless, concrete specs are valuable
- Challenge assumptions about what's technically feasible at our budget
- Bring knowledge of DoD procurement, SBIR/STTR programs, defense market dynamics
- Help research real capability gaps and real programs of record
- Draft documents that could actually be submitted (white papers, SBIR proposals, product specs)
