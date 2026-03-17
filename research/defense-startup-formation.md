# Starting a Defense Technology Company: Practical Guide

## 1. Business Entity Formation

**Delaware C-Corp.** VCs require it for stock classes and ISOs. DoD doesn't care about entity type, but C-Corp is future-proof. Use Stripe Atlas or Clerky ($500-800). Total with registered agent: under $1,500.

## 2. SAM.gov Registration

- **Free.** Takes 7-21 business days.
- Need: EIN, bank account, NAICS codes
- UEI replaces DUNS (generated automatically)
- CAGE code assigned automatically during validation
- Counter-drone NAICS: 334511 (Search/Detection/Navigation), 334290 (Communications Equipment), 541715 (R&D Physical/Engineering Sciences)
- **Must renew annually**

## 3. Security Clearances

**Start without one.** Counter-drone detection at the unclassified level doesn't require clearance. Many contracts don't.

When you need a Facility Clearance (FCL):
- Cannot self-sponsor — need a government contracting officer to sponsor
- All Key Management Personnel must be U.S. citizens
- Timeline: 45 days to 12 months (6 months typical)
- Outsourced FSO services: $2K-5K/month

## 4. ITAR/EAR

**Detection-only devices** (passive RF, acoustic, optical): likely EAR-controlled, possibly EAR99 (minimal restrictions). Reasonable argument for Commerce jurisdiction.

**Active countermeasures** (jamming, spoofing): almost certainly ITAR (USML Category XI or XII). Much more burdensome.

If in doubt, file a free Commodity Jurisdiction request with DDTC (30-60 days). ITAR registration costs $2,250/year.

**Bottom line:** Stay detection-only to avoid ITAR complexity initially.

## 5. CMMC Compliance

- **Level 1** (FCI only): 17 practices, self-assessment. ~$3K-5K to implement.
- **Level 2** (CUI): 110 practices, third-party assessment. **$50K-200K first year.**

On $10K: Start compliant from day one (cheaper than retrofitting). Use Microsoft 365 GCC High for CUI ($30-50/user/month). Document everything. Don't need certification before first contract — phased rollout still in progress.

## 6. Insurance

FAR minimums:
- Workers' Comp: per state law + $100K employer liability
- Commercial General Liability: $500K per occurrence
- Auto Liability: $200K/$500K

Practically also need: E&O ($1M-5M), Cyber Liability ($1M-2M), Product Liability ($1M-5M).

**Startup cost: $3,000-8,000/year** for basic package.

## 7. Key Standards for Defense Electronics

- **MIL-STD-810H** (ruggedization): No certifying body — self-declare after testing. Test lab: $10K-50K+.
- **MIL-STD-461G** (EMI/EMC): Critical for RF devices. Testing: $15K-40K.
- **IP67/IP68**: Ingress protection (water/dust).

**Don't need these for first contract.** Many prototype contracts accept commercial-grade hardware. Invest in MIL-STD when production contract demands it.

## 8. The Valley of Death

Why startups die between prototype and production:
- SBIR Phase II ends with no Phase III
- Champion PM rotates assignments
- Budget cycles 2+ years out
- Investors lose patience with lumpy government revenue

How to survive:
1. **Dual-use from day one** — commercial airports, prisons, events = non-DoD revenue
2. **DIU pathway** — 51% of completed prototypes transition to production
3. **AFWERX TACFI/STRATFI** — bridges Phase II to Phase III
4. **Find your champion** — operational end-users, not just acquisition professionals
5. **Stay lean** — don't staff up before production revenue
6. **Multiple contract vehicles** — SBIR + DIU + OTA + GSA + commercial simultaneously

## 9. Case Studies

### Anduril
- Founded 2017. First contract within ~1 year.
- Started with surveillance towers, demonstrated to CBP at the border
- Now: $20B Army contract (March 2026), $14B+ valuation
- Key: Simpler demonstrable product first, not a moonshot

### Shield AI
- Founded 2015. Turned down $5M contingent on ditching military focus.
- Nova autonomous drone (no GPS/comms). $198M Coast Guard contract (2024).
- Now: $5.6B valuation.

### Skydio
- Founded 2014 as consumer drone company. Pivoted to defense 2021.
- Won Army Short Range Reconnaissance Program of Record via DIU evaluation
- $20.2M base year, up to $99.8M/5yr via OTA.

### Common thread
Every successful startup either adapted a working commercial product for defense, or built a demonstrable prototype extremely quickly and got it in front of end-users fast.

## 10. Timeline: Incorporation to First Revenue

| Milestone | Timeline |
|-----------|----------|
| Incorporation + EIN | Week 1 |
| SAM.gov registration | Weeks 2-5 |
| First proposal submitted | Months 1-4 |
| SBIR Phase I award (if won) | Months 6-10 ($50K-150K) |
| DIU prototype OTA (if won) | Months 4-8 |
| **First revenue received** | **Months 8-14 (optimistic)** |
| SBIR Phase II | Months 14-20 ($500K-1.5M) |
| Facility clearance (if needed) | Months 12-24 |
| Program of Record | **Years 2-5** |

### What $10K Actually Buys

- Incorporation + registered agent: ~$1,000
- SAM.gov: $0
- Basic insurance: ~$1,500/year
- Domain/website/marketing: ~$500
- **Remaining ~$7,000**: prototype materials, travel to demo days, conferences

## 11. Current Political/Procurement Environment (March 2026)

**Executive Order "Prioritizing the Warfighter" (Jan 7, 2026):** Targets underperforming primes. Net positive for startups — political top-cover for alternative suppliers.

**DOGE and DoD review:** Double-edged — could streamline procurement or freeze spending during review.

**Replicator/DAWG:** Biden-era Replicator rebranded as Defense Autonomous Warfare Group. Replicator 2 specifically targets C-sUAS. First contracts Jan 2026.

**SBIR/STTR expired Sept 30, 2025.** Reauthorization expected but uncertain.

**Defense tech funding boom:** $49.1B in VC deals in 2025, up from $27.2B in 2024.

**"Department of War" rebranding** signals sustained/increased defense spending.

## First 90-Day Action Plan on $10K

1. **Week 1:** Incorporate Delaware C-Corp. Get EIN. Open bank account.
2. **Week 2:** Begin SAM.gov registration. Select NAICS codes.
3. **Weeks 2-4:** Identify open DIU CSOs for C-UAS. Monitor defensesbirsttr.mil.
4. **Week 4:** Get basic CGL insurance ($1,500/year).
5. **Weeks 4-8:** Write and submit first SBIR/DIU proposal.
6. **Weeks 4-12:** Build prototype with remaining funds. Document for CMMC from day one.
7. **Ongoing:** Attend defense tech events (AUSA, SOF Week, DIU Demo Days). Network with PMs and end-users.

## Sources
- [SAM.gov Registration Guide](https://blogs.usfcr.com/sam-registration-2025)
- [Facility Clearances for Startups](https://industryfso.com/defense-startup-fcls)
- [Drone Export Control Laws](https://jrupprechtlaw.com/drone-export-control-laws-ear-itar/)
- [CMMC Cost Breakdown 2026](https://www.cmmc.com/newsroom/cost-of-cmmc)
- [FAR 28.307-2 Liability](https://www.acquisition.gov/far/28.307-2)
- [MIL-STD-810H Guide](https://freemindtronic.com/mil-std-810h-rugged-device-certification/)
- [Valley of Death - War University](https://www.waru.edu/library/damag/january-february2022/valley-death)
- [Defense Tech Startups Record 2025](https://www.defensenews.com/industry/2026/01/20/defense-tech-startups-had-their-best-funding-year-ever-in-2025/)
- [Anduril $20B Contract](https://techcrunch.com/2026/03/14/us-army-announces-contract-with-anduril-worth-up-to-20b/)
- [Skydio Army SRR](https://www.skydio.com/blog/skydio-selected-sole-platform-for-us-army-srr)
- [DIU Work With Us](https://www.diu.mil/work-with-us)
- [Prioritizing the Warfighter EO](https://www.whitehouse.gov/presidential-actions/2026/01/prioritizing-the-warfighter-in-defense-contracting/)
- [DAWG / Replicator Continuation](https://breakingdefense.com/2025/12/its-alive-biden-era-replicator-drone-initiative-lives-on-as-dawg-looking-at-bigger-uass/)
