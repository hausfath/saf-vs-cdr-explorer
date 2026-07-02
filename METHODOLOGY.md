# SAF vs CDR Cost-Effectiveness Analysis: Methodology & Assumptions

## Overview

This analysis compares the cost-effectiveness ($/tCO2) of using waste and residue biomass for Sustainable Aviation Fuel (SAF) production versus Carbon Dioxide Removal (CDR) pathways. It accounts for the co-benefit value of SAF (energy content and displaced fossil jet fuel revenue) and the co-benefit value of CDR approaches (clean energy from BECCS, biochar products, etc.).

The core question: **When, if ever, does SAF provide a cost-competitive decarbonization strategy compared to more direct CDR approaches?**

---

## Key Metric

All pathways are compared on **$/tCO2 avoided or removed**, calculated as:

- **SAF (without CCS)**: (Production cost − Fuel revenue) / tCO2 avoided
- **SAF + CCS**: (Production cost − Fuel revenue − 45Q credit on stored CO2) / (tCO2 avoided + tCO2 removed)
- **CDR**: (Production cost − Co-product revenue) / tCO2 removed

Three cost tiers are reported for each pathway:
1. **Gross**: Total production cost per tCO2
2. **Net**: After subtracting co-product/fuel revenue
3. **Net + Subsidy**: After additionally subtracting applicable government subsidies

For SAF+CCS, the denominator includes **both** CO2 avoided (fuel displacement) and CO2 removed (CCS storage), since both contribute to total climate benefit per dry ton.

---

## Fundamental Constants

| Parameter | Value | Source |
|-----------|-------|--------|
| Carbon content of oven-dry biomass | 0.50 tC/t | BiCRS Roadmap |
| CO2:C molecular weight ratio | 3.667 (44/12) | Chemistry |
| CO2e per dry ton of biomass | 1.833 tCO2 | Derived |
| CO2 per gallon of fossil jet fuel | 9.75 kg | EIA |
| Jet fuel density | 0.8 kg/L | Standard |
| Liters per gallon | 3.79 | Standard |
| Net negativity (CDR process efficiency) | 95% | Assumed; net removals = 95% of gross stored |

---

## SAF Pathway Assumptions

### Cost Basis

SAF costs are expressed as **Minimum Fuel Selling Price (MFSP)** in $/gallon. MFSP is **feedstock-inclusive** — it already incorporates:
- Feedstock procurement and transport to facility
- Conversion capital expenditure (amortized)
- Conversion operating expenditure
- Return on investment

No separate feedstock cost is added to avoid double-counting.

For **SAF + CCS**, the MFSP includes the full CCS chain cost (capture, compression, transport, and geological storage). The CCS increment is based on $100-250/tCO2 for the full chain, applied to the CO2 captured per dry ton.

### Emissions Accounting

The **base analysis** uses simple displacement: each gallon of SAF produced avoids 9.75 kg CO2 that would have been emitted by burning fossil jet fuel. This is a simplification — SAF has upstream lifecycle emissions from feedstock collection, transport, and conversion energy.

A **sensitivity analysis** (Figure 7) applies lifecycle GHG reduction factors to show the impact of upstream emissions on results. A 95% net negativity factor is also applied to all CDR pathways.

### 45Z Clean Fuel Production Credit (SAF subsidy)

The SAF subsidy is the **Section 45Z Clean Fuel Production Credit**, not the legacy blender's tax credit (§40B/§6426), which expired at the end of 2024. Following the One Big Beautiful Bill Act (Pub. L. 119-21, July 2025):

- The SAF-specific rate (formerly up to $1.75/gal) was **removed**. For fuel produced after 2025-12-31, SAF uses the general applicable amount of **$1.00/gal** with prevailing-wage & apprenticeship compliance ($0.20/gal otherwise); we assume compliance.
- The credit is extended to fuel sold through **2029**.
- The credit is **not flat**: it equals the applicable amount × an emissions factor `(50 − CI) / 50`, where CI is the fuel's carbon intensity in kg CO2e/mmBTU, floored at 0 and capped at 1.0.

We convert each pathway's lifecycle GHG reduction to an absolute CI against the ICAO CORSIA petroleum-jet baseline of **89 gCO2e/MJ (≈93.9 kg CO2e/mmBTU)**. A 70% reduction yields ~$0.44/gal; 85% yields ~$0.72/gal; below ~47% reduction the credit is zero. In the Monte Carlo the credit is **stochastic**, computed from each draw's sampled lifecycle reduction (perfectly correlated with the fuel's carbon intensity). This replaces the previous flat $1.25/gal assumption and materially raises SAF's net+subsidy $/tCO2.

### Carbon Fate in FT-SAF Production

Based on Liu et al. (Table 4) and the BiCRS pathway comparison spreadsheet:

| Carbon destination | Fraction of biomass C | tCO2 per dry ton |
|-------------------|----------------------|------------------|
| Fuel (kerosene — burned on use) | 32.4% | 0.59 |
| CCS storage (if integrated) | 53.7% | 0.98 |
| Process losses | ~14% | 0.26 |
| **Total** | **100%** | **1.83** |

The CO2 in the fuel is released upon combustion but offset by the displacement credit (avoiding 0.547 tCO2 of fossil jet emissions per dry ton).

### SAF Pathway Parameters

All ranges represent (P10, P50, P90) — the 10th, 50th, and 90th percentile estimates.

#### Fischer-Tropsch (FT-SAF) — Agricultural/Forest Waste

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| Mass yield (kg SAF / kg dry feedstock) | 0.13 | 0.17 | 0.21 | Dimitriou 2018, Wang 2022 |
| MFSP ($/gal) | 4.00 | 7.00 | 9.00 | Mid reflects current pre-commercial; P10 = at-scale |
| Jet fuel price ($/gal) | 2.50 | 3.00 | 3.50 | Wholesale Jet-A |
| 45Z credit ($/gal), CI-scaled | 0.33 | 0.44 | 0.57 | Derived from lifecycle reduction; $1.00/gal max (see 45Z note) |
| Lifecycle GHG reduction | 60% | 70% | 85% | CORSIA defaults, GREET |

**Derived (mid values):**
- Gallons per dry ton: 56.1
- tCO2 avoided per dry ton: 0.547
- Gross $/tCO2: $718
- Net $/tCO2 (after fuel revenue): $410
- Net + subsidy $/tCO2: $365

#### Fischer-Tropsch + CCS (FT-SAF + CCS) — Agricultural/Forest Waste

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| Mass yield (kg SAF / kg dry feedstock) | 0.13 | 0.17 | 0.21 | Same as FT |
| MFSP ($/gal) | 8.75 | 9.65 | 11.40 | Base FT + CCS at $100-250/tCO2 (mid $150) |
| CDR efficiency | 0.537 | 0.537 | 0.537 | Liu et al. Table 4; fraction of biomass C stored |
| CCS cost basis | $100/tCO2 | $150/tCO2 | $250/tCO2 | Full chain: capture + transport + storage |
| 45Q credit on stored CO2 | $85/tCO2 | $85/tCO2 | $85/tCO2 | Eligible (gaseous capture from acid gas removal) |
| Lifecycle GHG reduction | 60% | 70% | 85% | CORSIA defaults, GREET |

**CCS cost derivation:** Gasification produces a concentrated CO2 stream (~95% pure) from the acid gas removal unit. Capture is essentially free (already part of the process); the cost is compression + transport + storage = $100-250/tCO2 for the full chain. At 0.984 tCO2 stored per dry ton, this adds $1.76-4.39/gal to MFSP.

**Derived (mid values):**
- tCO2 avoided per dry ton: 0.547 (fuel displacement)
- tCO2 removed per dry ton: 0.984 (CCS storage)
- **Total tCO2 abated per dry ton: 1.531**
- Gross $/tCO2: $353
- Net $/tCO2 (after fuel revenue): $243
- Net + subsidy $/tCO2 (45Z + 45Q): $173

#### Fischer-Tropsch (FT-SAF) — Switchgrass

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| Mass yield (kg SAF / kg dry feedstock) | 0.13 | 0.17 | 0.21 | Same as FT waste |
| MFSP ($/gal) | 4.50 | 7.50 | 10.00 | Higher feedstock cost vs waste |
| Lifecycle GHG reduction | 55% | 65% | 80% | CORSIA defaults |

**Derived (mid values):**
- Net $/tCO2: $462
- Net + subsidy $/tCO2: $426

#### HEFA — Waste Fats/Oils

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| Mass yield (kg SAF / kg oil feedstock) | 0.40 | 0.50 | 0.60 | Per ton of waste oil, not cellulosic |
| MFSP ($/gal) | 3.00 | 4.50 | 6.00 | Already commercial; UCO price-dependent |
| Lifecycle GHG reduction | 60% | 70% | 85% | ICCT 2019, CORSIA |

**Derived (mid values):**
- Gallons per ton oil: 165.0
- tCO2 avoided per ton oil: 1.608
- Net $/tCO2: $154
- Net + subsidy $/tCO2: $109

**Note:** HEFA uses waste fats/oils (used cooking oil, animal fats), not cellulosic biomass. It is not directly comparable to cellulosic pathways on a per-dry-ton basis and is presented separately in figures. HEFA does not produce a concentrated CO2 stream and is not a natural candidate for CCS integration.

#### Alcohol-to-Jet (AtJ-SAF) — Agricultural/Forest Waste

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| Mass yield (kg SAF / kg dry feedstock) | 0.08 | 0.12 | 0.16 | Lower yield than FT |
| MFSP ($/gal) | 5.00 | 9.00 | 12.00 | Mid reflects current pre-commercial |
| Lifecycle GHG reduction | 50% | 65% | 80% | CORSIA defaults, Tanzil 2021 |

**Derived (mid values):**
- Gallons per dry ton: 39.6
- tCO2 avoided per dry ton: 0.386
- Net $/tCO2: $615
- Net + subsidy $/tCO2: $580

**Note:** AtJ fermentation produces a very pure CO2 stream (~99%) as a byproduct, making it another natural CCS candidate (similar to ADM's ethanol+CCS project). Not modeled separately here but would have similar CCS economics to FT+CCS.

---

## CDR Pathway Assumptions

### Cost Basis

CDR costs are expressed as **total cost per tCO2 removed** ($/tCO2). These are **feedstock-inclusive** and represent the full system cost including:
- Feedstock procurement and transport
- Conversion/processing capital and operating costs
- CO2 transport and geological storage (for CCS pathways)

MRV (monitoring, reporting, verification) costs are **not** included and may add $5-20/tCO2.

### Net Negativity

All CDR pathways are assumed to achieve **95% net negativity** — meaning net CO2 removed = 95% of gross CO2 stored. The 5% reduction accounts for upstream process emissions (energy inputs for feedstock processing, transport, facility operations) that are not fully offset by the storage. This is already reflected in the $/tCO2 cost figures (which are per net tCO2 removed) but is shown explicitly in Figure 7.

### Avoided Emissions from Co-products

CDR pathways that produce useful co-products (electricity, heat, hydrogen) generate **additional avoided emissions** by displacing fossil energy sources. These are shown in Figure 7 as additive to the stored CO2.

| CDR Pathway | Co-product | Avoided emissions (tCO2/dry ton) | Displaced source |
|-------------|-----------|----------------------------------|-----------------|
| BECCS (electricity) | 0.92 MWh/tCO2 | 0.33 | NGCC at 0.36 tCO2/MWh |
| BECCS (heat) | 0.7 MWh_th/tCO2 | 0.25 | Gas boiler |
| BECCS (hydrogen) | 52 kg H2/tCO2 | 0.55 | Gray H2 at 12 tCO2/tH2 |
| WtE + CCS | Electricity + heat | 0.30 | Fossil grid mix |

These avoided emissions are not included in the $/tCO2 cost metric (which is per tCO2 *removed* only) but are relevant for total climate benefit accounting per dry ton of feedstock.

### Subsidy Eligibility

**45Q tax credit ($85/tCO2)** requires gaseous CO2 capture from a point source. Only pathways with CCS (combustion/gasification + CO2 capture + geological storage) qualify:
- BECCS (electricity, heat, hydrogen) — **eligible**
- WtE + CCS — **eligible**
- FT-SAF + CCS — **eligible** (gaseous capture from acid gas removal)
- Bio-oil sequestration — **not eligible** (no gaseous capture)
- Biomass injection — **not eligible**
- Biochar — **not eligible**
- Biomass burial — **not eligible**

### CDR Pathway Parameters

#### BECCS (Electricity)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.80 | 0.90 | 0.95 | Fraction of biomass C permanently stored |
| Total cost ($/tCO2) | 200 | 300 | 400 | Pre-subsidy; CO280 ~$285 as reference |
| Co-product revenue ($/tCO2) | 18 | 25 | 35 | Electricity at 0.5 MWh/tCO2, $35-70/MWh |
| 45Q subsidy ($/tCO2) | 85 | 85 | 85 | Eligible (gaseous CO2 capture) |
| Avoided emissions (tCO2/dry ton) | 0.33 | 0.33 | 0.33 | Displacing NGCC electricity |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.650
- Net $/tCO2: $275
- Net + subsidy $/tCO2: $190

#### BECCS (Heat)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.80 | 0.90 | 0.95 | |
| Total cost ($/tCO2) | 200 | 285 | 400 | Pre-subsidy |
| Co-product revenue ($/tCO2) | 20 | 25 | 35 | Heat at 0.7 MWh_th/tCO2, ~$35/MWh |
| 45Q subsidy ($/tCO2) | 85 | 85 | 85 | Eligible |
| Avoided emissions (tCO2/dry ton) | 0.25 | 0.25 | 0.25 | Displacing gas boiler heat |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.650
- Net $/tCO2: $260
- Net + subsidy $/tCO2: $175

#### BECCS (Hydrogen)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.55 | 0.65 | 0.75 | Lower than other BECCS; Mote PFD shows 65.1% |
| Total cost ($/tCO2) | 200 | 300 | 400 | Pre-subsidy |
| Co-product revenue ($/tCO2) | 78 | 130 | 182 | H2 at 52 kg/tCO2, $1.50-3.50/kg |
| 45Q subsidy ($/tCO2) | 85 | 85 | 85 | Eligible |
| Avoided emissions (tCO2/dry ton) | 0.55 | 0.55 | 0.55 | Displacing gray H2 (12 tCO2/tH2) |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.192
- Net $/tCO2: $170
- Net + subsidy $/tCO2: $85

**Note:** BECCS-H2 has lower CDR efficiency (65% vs 90%) because the gasification/reforming process diverts more carbon into the hydrogen product stream rather than capturing it as CO2. However, the avoided emissions from displacing gray H2 are substantial (0.55 tCO2/dry ton), partially compensating for the lower direct CDR.

#### WtE + CCS (Waste-to-Energy with Carbon Capture)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.70 | 0.80 | 0.88 | Lower due to MSW heterogeneity |
| Total cost ($/tCO2) | 200 | 300 | 400 | Pre-subsidy; Celsio ~$300/ton |
| Co-product revenue ($/tCO2) | 25 | 40 | 60 | Electricity + heat |
| 45Q subsidy ($/tCO2) | 85 | 85 | 85 | Eligible |
| Avoided emissions (tCO2/dry ton) | 0.30 | 0.30 | 0.30 | Displacing fossil grid |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.467
- Net $/tCO2: $260
- Net + subsidy $/tCO2: $175

#### Bio-oil Sequestration (Cellulosic)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.35 | 0.45 | 0.55 | Limited by pyrolysis conversion |
| Total cost ($/tCO2) | 250 | 350 | 500 | Current Frontier offtake pricing |
| Co-product revenue ($/tCO2) | 0 | 10 | 20 | Nutrient return to soil |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- tCO2 removed per dry ton: 0.825
- Net $/tCO2: $340

#### Bio-oil Sequestration (HTL, Lipid Feedstock)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.35 | 0.45 | 0.55 | |
| Total cost ($/tCO2) | 225 | 325 | 475 | Slightly lower than cellulosic |
| Co-product revenue ($/tCO2) | 0 | 10 | 20 | Nutrient return |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- Net $/tCO2: $315

#### Biomass Injection (Slurry)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.85 | 0.92 | 0.95 | High — direct sequestration |
| Total cost ($/tCO2) | 125 | 200 | 285 | |
| Co-product revenue ($/tCO2) | 0 | 0 | 0 | PFAS destruction (value not monetized) |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.687
- Net $/tCO2: $200

#### Biochar

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.20 | 0.30 | 0.40 | Low — much carbon lost as gas during pyrolysis |
| Total cost ($/tCO2) | 100 | 200 | 350 | Wide range due to scale variation |
| Co-product revenue ($/tCO2) | 0 | 20 | 50 | Conservative estimate; biochar market uncertain |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- tCO2 removed per dry ton: 0.550
- Net $/tCO2: $180

#### Biomass Burial

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.85 | 0.95 | 0.98 | High if properly sealed |
| Total cost ($/tCO2) | 90 | 150 | 200 | Current ~$150; NOAK target ~$90-100 |
| Co-product revenue ($/tCO2) | 0 | 0 | 0 | None |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.742
- Net $/tCO2: $150

---

## Feedstock Competition Map

The analysis identifies which SAF and CDR pathways compete for the **same** feedstock supply:

| Feedstock | SAF Pathways | CDR Pathways | Direct Competition? |
|-----------|-------------|--------------|---------------------|
| Agricultural residues | FT-SAF, FT-SAF+CCS, AtJ-SAF | BECCS (all), Bio-oil, Biochar | **Yes** — core comparison |
| Forest waste | FT-SAF, FT-SAF+CCS, AtJ-SAF | BECCS (all), Bio-oil, Biomass burial, Biochar | **Yes** — core comparison |
| Switchgrass (marginal land) | FT-SAF, AtJ-SAF | BECCS (all), Biochar | **Yes** |
| MSW | — | WtE + CCS | No SAF competitor |
| Waste fats/oils | HEFA | Bio-oil seq. (HTL) | **Yes** — lipid competition |
| Wet waste (manure) | — | Biomass injection | No SAF competitor |

---

## Uncertainty & Sensitivity Analysis

### Parameter Distributions

All (P10, P50, P90) values are used to fit **PERT distributions** for Monte Carlo simulation. PERT distributions weight the mode more heavily than triangular distributions, better representing cost distributions where extreme values are possible but unlikely.

### Monte Carlo Simulation

- **10,000 draws** per pathway
- Parameters drawn from PERT distributions fitted to (P10, P50, P90)
- The **45Z credit is stochastic**, computed per draw from that draw's sampled lifecycle GHG reduction (higher reduction → lower CI → larger credit), so it is perfectly correlated with the fuel's carbon intensity.
- Figure 1 P10–P90 error bars are taken directly as percentiles of the Monte Carlo output distribution (rather than deterministic percentile-input estimates), so they propagate the full stochastic model including the CI-scaled credit.

### Tornado Sensitivity

One-at-a-time (OAT) sensitivity analysis on the **net + subsidy $/tCO2** metric, varying each parameter from its P10 to P90 value while holding others at P50.

**Note on mass yield:** For SAF pathways, mass yield does not affect $/tCO2 because MFSP is expressed per gallon — both cost and CO2 avoided scale identically with yield. Yield matters for tCO2 per dry ton (carbon flow) but cancels in the cost ratio.

### LCA / Full Carbon Accounting (Figure 7)

Figure 7 shows the complete carbon budget per dry ton of feedstock, including:
- **CO2 permanently stored** (green) — net after 95% net negativity factor
- **Upstream process emissions** (orange) — 5% of gross stored for CDR; lifecycle reduction for SAF
- **CO2 released to atmosphere** (gray) — biomass carbon not captured
- **CO2 avoided** (blue, hatched, stacked above) — additional abatement from displacing fossil energy

This figure demonstrates that high-efficiency CDR pathways with energy co-products (e.g. BECCS-H2: 1.19 stored + 0.55 avoided = 1.74 total) achieve substantially greater total abatement per dry ton than SAF alone (0.55 avoided only).

### LCA Lifecycle GHG Reduction Factors

| SAF Pathway | Lifecycle GHG Reduction (P10/P50/P90) | Source |
|-------------|---------------------------------------|--------|
| FT (waste) | 60% / 70% / 85% | CORSIA, GREET |
| FT (switchgrass) | 55% / 65% / 80% | CORSIA |
| HEFA (waste fats) | 60% / 70% / 85% | ICCT 2019, CORSIA |
| AtJ (waste) | 50% / 65% / 80% | CORSIA, Tanzil 2021 |

These reduce the effective tCO2 avoided, increasing SAF's $/tCO2 by 15-50% depending on pathway.

---

## Key Caveats & Limitations

1. **Avoided ≠ Removed.** SAF prevents fossil CO2 emissions (avoidance); CDR physically removes atmospheric CO2 (removal). If aviation decarbonizes via other means (electric, hydrogen flight), SAF's displacement value goes to zero while CDR removal is permanent.

2. **Non-CO2 aviation effects not modeled.** Aviation's total climate forcing is ~2-4x its CO2 impact alone (contrails, NOx, water vapor at altitude). SAF from FT/HEFA can reduce contrails by 50-70% via lower aromatic content (Voigt et al. 2021). This is the strongest unmodeled argument in SAF's favor — if credited, it could roughly double SAF's effective climate benefit. However, the effect is highly route/weather-dependent and not yet included in any carbon accounting framework.

3. **Permanence varies across CDR pathways.** BECCS with geologic storage is effectively permanent (10,000+ years). Biochar is 100-1,000 years. Biomass burial durability is unproven at scale.

4. **MRV costs not included** in CDR total costs — may add $5-20/tCO2.

5. **SAF distribution costs not included** — transport from production facility to airport not in MFSP.

6. **Subsidies reflect current US policy** (45Q at $85/tCO2; 45Z Clean Fuel Production Credit for SAF — up to $1.00/gal, CI-scaled, ~$0.34–0.44/gal at mid lifecycle reductions — post-OBBBA and effective through 2029). Policy is subject to change. EU subsidies not modeled.

7. **Feedstock competition effects not modeled.** If demand for waste biomass increases, prices would rise for all pathways.

8. **HEFA feedstock supply is limited.** Waste fats/oils are a constrained resource (~5-10 Mt/year globally). HEFA cannot scale to meet aviation fuel demand alone.

9. **Cost estimates reflect current/near-term pricing.** SAF mid-values represent pre-commercial current costs; P10 values represent at-scale commercial targets. CDR mid-values are calibrated to actual Frontier offtake pricing (CO280 ~$285, Celsio ~$300, burial projects ~$150).

10. **No operational FT-SAF+CCS plants exist.** Velocys Bayou Fuels is the most advanced integrated project (completed FEED) but remains in development. Fulcrum BioEnergy went bankrupt in 2024. SAF+CCS cost estimates are based on TEA literature and CCS cost estimates applied to the FT gasification process.

---

## Summary Results (Mid Estimates)

| Pathway | Type | Gross $/tCO2 | Net $/tCO2 | Net+Sub $/tCO2 | tCO2/dry ton |
|---------|------|-------------|-----------|----------------|--------------|
| FT-SAF (ag/forest waste) | SAF | $718 | $410 | $365 | 0.547 |
| **FT-SAF + CCS** | **SAF+CDR** | **$353** | **$243** | **$173** | **1.531** |
| FT-SAF (switchgrass) | SAF | $769 | $462 | $426 | 0.547 |
| HEFA (waste fats/oils) | SAF | $462 | $154 | $109 | 1.608 |
| AtJ-SAF (ag/forest waste) | SAF | $923 | $615 | $580 | 0.386 |
| BECCS (electricity) | CDR | $300 | $275 | $190 | 1.650 |
| BECCS (heat) | CDR | $285 | $260 | $175 | 1.650 |
| BECCS (hydrogen) | CDR | $300 | $170 | $85 | 1.192 |
| WtE + CCS | CDR | $300 | $260 | $175 | 1.467 |
| Bio-oil seq. (cellulosic) | CDR | $350 | $340 | $340 | 0.825 |
| Bio-oil seq. (HTL, lipid) | CDR | $325 | $315 | $315 | 0.825 |
| Biomass injection | CDR | $200 | $200 | $200 | 1.687 |
| Biochar | CDR | $200 | $180 | $180 | 0.550 |
| Biomass burial | CDR | $150 | $150 | $150 | 1.742 |

### Key Ratios

- **BECCS (elec/heat) / SAF carbon ratio:** 3.02x (BECCS removes ~3x more CO2 per dry ton than SAF avoids)
- **Breakeven CDR efficiency:** 29.8% (CDR must achieve at least this efficiency to match SAF's carbon benefit per dry ton)
- **FT-SAF+CCS total abatement:** 1.531 tCO2/dry ton — comparable to CDR pathways
- **Cheapest cellulosic SAF (net+sub):** FT-SAF+CCS at $173/tCO2
- **Cheapest cellulosic CDR (net+sub):** BECCS-H2 at $85/tCO2, Biomass burial at $150/tCO2

### Key Finding

FT-SAF+CCS is the only SAF pathway approaching cost-competitiveness with CDR on a $/tCO2 basis ($173 net+sub), benefiting from dual revenue streams (fuel sales + 45Q). However, it still sits above the cheapest CDR options and requires both functional SAF production (no commercial FT-SAF plants today) and CCS infrastructure — adding significant execution risk relative to proven CDR approaches. The narrower 45Z credit ($1.00/gal max, CI-scaled) under the post-OBBBA regime raises SAF's net+subsidy costs relative to the prior $1.25/gal blender's credit, widening the gap for SAF pathways without CCS.

---

## References

- Liu, G., Larson, E., Williams, R., Kreutz, T. (2011). Making Fischer-Tropsch fuels and electricity from coal and biomass. *Energy & Fuels*.
- Dimitriou, I. et al. (2018). Techno-economic assessment of Fischer-Tropsch synthesis. *Bioresource Technology*.
- Wang, W.-C. et al. (2022). Review of biojet fuel conversion technologies. *Renewable and Sustainable Energy Reviews*.
- DOE SAF Grand Challenge Roadmap (2022).
- ICCT (2019). The cost of supporting alternative jet fuels in the European Union.
- Tanzil, A.H. et al. (2021). Strategic assessment of sustainable aviation fuel production technologies. *Biomass and Bioenergy*.
- Fuss, S. et al. (2018). Negative emissions—Part 2: Costs, potentials and side effects. *Environmental Research Letters*.
- NASEM (2019). Negative Emissions Technologies and Reliable Sequestration.
- Bui, M. et al. (2018). Carbon capture and storage (CCS): the way forward. *Energy & Environmental Science*.
- Voigt, C. et al. (2021). Cleaner burning aviation fuels can reduce contrail cloudiness. *Communications Earth & Environment*.
- Frontier Climate (2024). Purchasing POV: BiCRS.
- Frontier Climate (2024). BiCRS pathway comparison spreadsheet.
- EIA (2024). Carbon Dioxide Emissions Coefficients.
- CORSIA (2024). Default Life Cycle Emissions Values for CORSIA Eligible Fuels.
- Jiang, Y. & Bhattacharyya, D. (2016). Techno-economic analysis of a biomass-to-liquids plant with CCS.

---


## Addendum: The Interactive Explorer

The web tool (`index.html`) implements this methodology as a client-side JavaScript model,
verified to reproduce the original Python analysis' central estimates to <$0.001/tCO2.
Differences and extensions relative to the static analysis:

1. **User-adjustable central estimates.** Sliders set each input's P50; the P10 and P90
   scale proportionally with the user's value (bounded parameters such as lifecycle
   reduction and capture efficiency are clamped at 0.95/0.98).
2. **In-browser Monte Carlo.** Uncertainty ranges are P10-P90 from a 4,000-draw PERT
   Monte Carlo per pathway (seeded, so ranges are stable while dragging sliders). The 45Z
   credit is stochastic, computed from each draw's sampled lifecycle reduction.
3. **FT-SAF+CCS cost is derived, not independent.** Its MFSP = FT-SAF (ag/forest) MFSP +
   a CCS cost slider ($/tCO2 stored, default $150, per the $100-250/tCO2 full-chain range)
   applied to the ~0.018 tCO2 stored per gallon. Moving the base FT-SAF cost moves
   FT-SAF+CCS with it.
4. **BECCS-hydrogen 45Q/45V election.** The 45V clean hydrogen credit survived the OBBBA
   with a shortened window (construction start before January 1, 2028; up to $3.00/kg).
   Because 45V and 45Q cannot be claimed at the same facility (26 USC 45V(d)(2)), the tool
   offers an election: 45Q on stored CO2 (default) or 45V on the ~50 kg H2 produced per dry
   ton (~$126/tCO2 removed at 65% capture efficiency). The hydrogen co-product revenue
   (default $130/tCO2, implying ~$3.1/kg H2) is a market price assumption between gray
   (~$1-2/kg) and green (~$4.50+/kg) hydrogen, independent of either credit.
5. **Benchmark tiles** compare the most common pathways today - FT-SAF (ag/forest waste)
   and BECCS (electricity) - rather than the cheapest options.

---

*Static analysis conducted May 2026 (code: `saf_vs_cdr_analysis.py`, not included in this
repository). Interactive tool and tax-credit updates: July 2026.*
