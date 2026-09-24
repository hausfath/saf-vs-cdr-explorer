# SAF vs CDR Cost-Effectiveness Analysis: Methodology & Assumptions

## Overview

This analysis compares the cost-effectiveness ($/tCO2) of using waste and residue biomass for Sustainable Aviation Fuel (SAF) production versus Carbon Dioxide Removal (CDR) pathways. It accounts for the co-benefit value of SAF (energy content and displaced fossil jet fuel revenue) and the co-benefit value of CDR approaches (clean energy from BECCS, biochar products, etc.).

The core question: **When, if ever, does SAF provide a cost-competitive decarbonization strategy compared to more direct CDR approaches?**

> **September 2026 update.** SAF-side assumptions were recalibrated against current literature, market data and statute, and several model bugs were fixed. CDR costs are unchanged (calibrated to Frontier offtake data). Every number in this document is generated from the explorer (`index.html`) defaults. See [Changelog: September 2026](#changelog-september-2026) at the end for the full list and the effect of each change.

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
| CO2 per gallon of fossil jet fuel (combustion) | 9.75 kg | EIA |
| Fossil jet well-to-wake baseline | 89 gCO2e/MJ ≈ 11.66 kg CO2e/gal | ICAO CORSIA (LHV 43.2 MJ/kg) |
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

For **SAF + CCS**, the MFSP is the FT-SAF MFSP plus the full CCS chain cost (compression, transport, and geological storage; capture is part of the gasification process) of $40–120/tCO2 (P50 $70), applied to the CO2 stored per gallon.

### Emissions Accounting

Two accounting bases are offered. **Full lifecycle is the default** (September 2026 onward).

- **Full lifecycle (default):** each gallon of SAF avoids `lifecycle GHG reduction × 11.66 kg CO2e`, where 11.66 kg CO2e/gal is the ICAO CORSIA well-to-wake fossil jet baseline (89 gCO2e/MJ × 43.2 MJ/kg × 3.03 kg/gal). CDR storage (and the stored CO2 of FT-SAF+CCS) is multiplied by a 95% net-negativity factor, and CDR energy co-products are credited with the fossil emissions they displace in the per-dry-ton carbon chart.
- **Simple displacement:** each gallon avoids the 9.75 kg CO2 released by burning fossil jet (EIA), with no upstream emissions on either side; CDR storage is gross.

*Bug fixed September 2026:* earlier versions multiplied the lifecycle reduction by the 9.75 kg combustion-only figure, mixing a well-to-wake percentage with a combustion-only baseline. That understated SAF's lifecycle benefit by 16% (9.75 / 11.66) and overstated its $/tCO2 by ~19%.

### Tax credits (post-OBBBA, Pub. L. 119-21)

**45Z Clean Fuel Production Credit.** For fuel produced after 2025-12-31 the SAF-specific rate is gone; SAF earns the general applicable amount of **$1.00/gal** with prevailing-wage and apprenticeship compliance ($0.20/gal otherwise), inflation-adjusted, for fuel sold through **2029-12-31** (26 USC 45Z(g)). The credit is `applicable amount × (50 − CI)/50`, CI in kg CO2e/mmBTU. OBBBA added: a zero floor on emissions rates except manure-derived fuels (45Z(b)(1)(E)); exclusion of ILUC emissions (45Z(b)(1)(B)(iv)); and a requirement that feedstock be "produced or grown in the United States, Mexico, or Canada" (45Z(f)(1)(A)(iii)). SAF CI may be determined with CORSIA or a similar methodology (45ZCF-GREET) (45Z(b)(1)(B)(iii)). We convert each pathway's lifecycle reduction to CI against 89 gCO2e/MJ (93.9 kg/mmBTU). At the new default reductions: FT-SAF ~$0.81/gal, HEFA ~$0.59/gal, AtJ ~$0.34/gal, FT-SAF (switchgrass) ~$0.62/gal. The credit is recomputed for every Monte Carlo draw.

**No 45Z + 45Q stacking.** A "qualified facility" for 45Z "does not include any facility for which ... (iii) The credit for carbon oxide sequestration under section 45Q" is allowed for the taxable year (26 USC 45Z(d)(4)(B)). Treasury's proposed 45Z regulations (91 FR 5160, Feb 2026) call 45V, the 48(a)(15) election and 45Q the "anti-stacking credits." *Bug fixed September 2026:* earlier versions credited FT-SAF+CCS with both 45Z and 45Q, overstating its subsidy by ~$16/tCO2. FT-SAF+CCS now carries an election: **45Q on stored CO2 (default; ~$84 per dry ton)** or **45Z alone with CCS counted in the fuel's CI** (CCS drives CI to the zero floor, so the factor caps at 1.0: ~$56 per dry ton at $1.00/gal). Whether 45ZCF-GREET/CORSIA will score a specific FT+CCS pathway this way has not been confirmed.

**45Q:** $85/tCO2 for geologic storage with prevailing wage (non-DAC), construction start before 2033, 12,500 t/yr minimum capture for industrial facilities. **45V** (BECCS-H2 election, unchanged): construction start before 2028, up to $3.00/kg, barred where 45Q is claimed (45V(d)(2)).

### Carbon Fate in FT-SAF Production

Based on Liu et al. (Table 4) and the BiCRS pathway comparison spreadsheet:

| Carbon destination | Fraction of biomass C | tCO2 per dry ton |
|-------------------|----------------------|------------------|
| Fuel (kerosene — burned on use) | 32.4% | 0.59 |
| CCS storage (if integrated) | 53.7% | 0.98 |
| Process losses | ~14% | 0.26 |
| **Total** | **100%** | **1.83** |

Note: at 0.17 kg fuel per kg biomass, roughly 29–32% of the biomass *carbon* ends up in the fuel (fuel is ~85% carbon, biomass ~50%). The mass yield (17%) and the carbon fraction (~30%) are different quantities. The ~54% captured share is consistent with the Energy Transitions Commission's (2021) statement that gasification routes "allow the capture of up to 55% of total biomass carbon," and with Jiang & Bhattacharyya (2016, coal-biomass case, 56.9% captured); we could not verify it directly against a biomass-only FT table.

### SAF Pathway Parameters

All ranges are (P10, P50, P90).

**Jet fuel price (all SAF pathways): $2.10 / $2.75 / $4.00 per gallon** (was $2.50 / $3.00 / $3.50). US Gulf Coast kerosene-type jet spot (FRED `DJFUELUSGULF`, computed from daily data): 2022 $3.37, 2023 $2.70, 2024 $2.34, 2025 $2.11, 2026 January–September $3.36 (September 2026 ~$4.38, reflecting the Strait of Hormuz disruption). The P50 is the 2022–2026 mean ($2.78, rounded); P10 matches the 2025 average and P90 the 2026 crisis level.

| Pathway | Mass yield (kg/kg) | MFSP ($/gal) | Lifecycle GHG reduction | Notes |
|---|---|---|---|---|
| FT-SAF, ag/forest residues | 0.13 / 0.17 / 0.21 | 4.00 / 7.00 / **10.00** | **0.78 / 0.90 / 0.94** | CORSIA defaults: ag residues 7.7 g/MJ (91.3%), forestry residues 8.3 (90.7%) |
| FT-SAF + CCS | same as FT | FT MFSP + CCS adder | same as FT | CCS **$40 / $70 / $120** per tCO2 stored (was $100 / $150 / $250) |
| FT-SAF, switchgrass | 0.13 / 0.17 / 0.21 | 4.50 / 7.50 / **10.50** | **0.70 / 0.80 / 0.90** | CORSIA switchgrass 17.0 g/MJ incl. global ILUC (80.9%); US ILUC is lower |
| HEFA, waste fats/oils | 0.40 / 0.50 / 0.60 | **4.00 / 5.25 / 7.00** | **0.65 / 0.78 / 0.85** | CORSIA UCO 13.9 g/MJ (84.4%), beef tallow 29.7 (66.6%) |
| AtJ-SAF, ag/forest residues | 0.08 / 0.12 / 0.16 | 5.00 / 9.00 / 12.00 | **0.55 / 0.65 / 0.73** | CORSIA ethanol-AtJ residues 24.6–39.7 g/MJ (55–72%); isobutanol 67–73% |

Bold = changed in September 2026. Previous lifecycle ranges were 60/70/85% (FT, HEFA), 55/65/80% (switchgrass) and 50/65/80% (AtJ); the FT ranges sat entirely below the CORSIA default values (ICAO, *CORSIA Default Life Cycle Emissions Values*, 8th ed., Nov 2025). The new FT P10 (78%) allows for plants doing worse than the default in practice (fossil process energy, long feedstock hauls).

**MFSP rationale.**
- *FT:* nth-plant TEAs cluster around $3–6/gal. First-of-a-kind estimates run higher: EASA's 2025 production-cost estimate for "advanced aviation biofuels" is €2,760/t (range €1,790–3,130), roughly $9/gal, and Langholtz et al. (2026) cite current SAF costs of $9.40–10.96/gal. Real FOAK plants have failed (Fulcrum's Sierra plant shut in May 2024; Red Rock never produced fuel). P90 was raised to $10.
- *HEFA:* 2025 aviation-biofuel (HEFA) market price in the EU was €1,925/t versus €640/t for conventional jet (EASA 2025 reference prices, real index pricing), about $6.5–7/gal, which includes margin. Used-cooking-oil prices of roughly $1,000–1,300/t (market reports, unverified) put feedstock alone near $4/gal. The previous $4.50 P50 gave HEFA a net+subsidy cost ($109/t) well below the literature: El-Houjeiri et al. (2026) find a HEFA abatement-cost median of $362/t (range $112–526) across harmonized studies.
- *AtJ:* unchanged; LanzaJet's Freedom Pines ethanol-to-jet plant now operates, but on non-cellulosic ethanol, and cellulosic ethanol has not reached commercial scale.

**CCS cost rationale.** Gasification already strips CO2 in the acid-gas-removal unit as a near-pure stream, so the incremental cost is dehydration, compression, transport and storage. Published costs for high-purity streams: capture and compression ~$17.5/t average for >95% CO2 streams (NETL, 2018$; secondary extraction), $22–25/t for fermentation CCS (IEAGHG 2021-01); saline storage mostly ≤$8/t (NETL 2024); pipeline transport ~$15–25/t for dedicated 100 km lines (NETL). A standalone FT plant capturing ~0.5 Mt/yr would likely need its own pipeline and well, so the P50 ($70) and P90 ($120) sit above the ethanol-network analogs. The old $150/t is kept as a sensitivity case: it raises FT-SAF+CCS from $147 to $198/tCO2.

**Derived values (defaults, full lifecycle):**
- FT-SAF: 56.1 gal/dry ton; 0.59 tCO2e avoided per dry ton; gross $667, net $405, net+subsidy $328/tCO2.
- FT-SAF + CCS: MFSP $7.00 + $1.23 CCS = $8.23/gal; 0.59 avoided + 0.93 net stored = 1.52 tCO2e per dry ton; net+subsidy $147/tCO2 (45Q election); $165/tCO2 with the 45Z election.
- HEFA: 165 gal per ton of oil; 1.50 tCO2e avoided per ton of oil; net+subsidy $210/tCO2.

HEFA uses waste fats/oils rather than cellulosic biomass and is not comparable per dry ton. Global collection of used cooking oil (~12 Mt/yr) and animal fats (~13 Mt/yr) is roughly 25 Mt/yr (IEA 2023), most of it already used for biofuels. Earlier versions said "~5–10 Mt/yr," which appears to have been a units mix-up (GlobalData's 2030 forecast is 5–10 *billion gallons*) or a regional subset. AtJ fermentation produces a very pure CO2 stream, but only ~15% of biomass carbon (ETC 2021); AtJ+CCS is not modeled separately.

### Non-CO2 sensitivity (new)

The *SAF non-CO2 credit* slider (default 0) adds a benefit equal to a chosen percentage of the CO2 displacement benefit. Calibration for 100% SAF:
- Contrail radiative forcing falls 26% (Märkl et al. 2024, ECLIF3, 100% HEFA, "conservatively") to 44% (Teoh et al. 2022, fleet-wide 100% SAF model).
- Lee et al. (2021, Table 5) give contrail-cirrus CO2-equivalent emissions of 0.63× aviation CO2 under GWP100, 1.77× under GWP*, and 2.32× under GWP20.
- The implied non-CO2 benefit is therefore ~16–28% of the CO2 benefit (GWP100) or ~46–78% (GWP*). We use 20% and 60% as sensitivity cases. The earlier statement that crediting contrails "could roughly double SAF's benefit" is only reached under GWP20 or with optimistic contrail assumptions and has been removed.
- Caveats: contrail forcing uncertainty is very large (non-CO2 terms contribute ~8× more than CO2 to uncertainty in aviation ERF; Lee et al. 2021); benefits depend on blend level, routes and time of day; flight rerouting may avoid many warming contrails more cheaply than SAF; no accounting framework currently credits these benefits.

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
| BECCS (electricity) | 0.92 MWh per dry ton | 0.33 | NGCC at 0.36 tCO2/MWh |
| BECCS (heat) | 0.7 MWh_th/tCO2 | 0.25 | Gas boiler |
| BECCS (hydrogen) | ~50 kg H2 per dry ton | 0.55 | Gray H2 at 12 tCO2/tH2 |
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
| CDR efficiency | 0.55 | 0.65 | 0.70 | Fraction of biomass C retained in stable bio-oil |
| Total cost ($/tCO2) | 250 | 350 | 500 | Current Frontier offtake pricing |
| Co-product revenue ($/tCO2) | 0 | 0 | 0 | None assumed (nutrient-return value uncertain) |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- tCO2 removed per dry ton: 1.19
- Net $/tCO2: $350

#### Bio-oil Sequestration (HTL, Lipid Feedstock)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.55 | 0.65 | 0.70 | Fraction of biomass C retained in stable bio-oil |
| Total cost ($/tCO2) | 225 | 325 | 475 | Slightly lower than cellulosic |
| Co-product revenue ($/tCO2) | 0 | 0 | 0 | None (no nutrient-return co-product from HTL) |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- Net $/tCO2: $325

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

Each (P10, P50, P90) triple defines a **two-piece (split) normal distribution** whose 10th, 50th and 90th percentiles match the stated values exactly, with separate spreads below and above the median, clamped to physical bounds (costs ≥ 0, fractions in [0, 0.99]).

*Bug fixed September 2026:* earlier versions (and the original Python analysis) passed the triple to a PERT distribution as its minimum, mode and maximum. The resulting output "P10–P90" ranges were only ~45% as wide as the stated input ranges (for example, a $200 / $300 / $400 cost input produced a $256–345 P10–P90 band). Central estimates are unaffected; uncertainty ranges are now correspondingly wider.

### Monte Carlo Simulation

- **4,000 draws** per pathway in the explorer (seeded, so ranges are stable while adjusting inputs)
- Parameters drawn from split-normal distributions fitted to (P10, P50, P90); FT-SAF + CCS samples the FT base MFSP and the CCS cost independently
- The **45Z credit is stochastic**, computed per draw from that draw's sampled lifecycle GHG reduction (higher reduction → lower CI → larger credit), so it is perfectly correlated with the fuel's carbon intensity.
- Figure 1 P10–P90 error bars are taken directly as percentiles of the Monte Carlo output distribution (rather than deterministic percentile-input estimates), so they propagate the full stochastic model including the CI-scaled credit.

### Tornado Sensitivity

One-at-a-time (OAT) sensitivity analysis on the **net + subsidy $/tCO2** metric, varying each parameter from its P10 to P90 value while holding others at P50 (static Python analysis, May 2026; not regenerated for the September 2026 parameters).

**Note on mass yield:** For SAF pathways, mass yield does not affect $/tCO2 because MFSP is expressed per gallon — both cost and CO2 avoided scale identically with yield. Yield matters for tCO2 per dry ton (carbon flow) but cancels in the cost ratio.

### LCA / Full Carbon Accounting (Figure 7)

Figure 7 shows the complete carbon budget per dry ton of feedstock, including:
- **CO2 permanently stored** (green) — net after 95% net negativity factor
- **Upstream process emissions** (orange) — 5% of gross stored for CDR; lifecycle reduction for SAF
- **CO2 released to atmosphere** (gray) — biomass carbon not captured
- **CO2 avoided** (blue, hatched, stacked above) — additional abatement from displacing fossil energy

At September 2026 defaults, BECCS (electricity) delivers 1.90 tCO2e per dry ton (1.57 net stored + 0.33 avoided) against 0.59 for FT-SAF, a 3.2× ratio; FT-SAF + CCS delivers 1.52.

### LCA Lifecycle GHG Reduction Factors

See the SAF pathway table above (CORSIA 8th edition defaults, November 2025). In full-lifecycle mode these multiply the 11.66 kg CO2e/gal well-to-wake baseline.

---

## Key Caveats & Limitations

1. **Avoided ≠ Removed.** SAF prevents fossil CO2 emissions (avoidance); CDR physically removes atmospheric CO2 (removal). If aviation decarbonizes via other means (electric, hydrogen flight), SAF's displacement value goes to zero while CDR removal is permanent.

2. **Non-CO2 aviation effects are a sensitivity, not a default.** Non-CO2 terms were 66% of aviation's net ERF in 2018 (Lee et al. 2021), and aviation warms at ~3× the rate of its CO2 alone under GWP*. 100% SAF cuts ice-crystal numbers by ~56% and contrail forcing by ~26–44% (Märkl et al. 2024; Teoh et al. 2022), worth roughly +16–28% (GWP100) to +46–78% (GWP*) of SAF's CO2 benefit. See the non-CO2 sensitivity section. The effect is route-, weather- and blend-dependent and is not yet credited in any accounting framework.

3. **Permanence varies across CDR pathways.** BECCS with geologic storage is effectively permanent (10,000+ years). Biochar is 100-1,000 years. Biomass burial durability is unproven at scale.

4. **MRV costs not included** in CDR total costs — may add $5-20/tCO2.

5. **SAF distribution costs not included** — transport from production facility to airport not in MFSP.

6. **Subsidies reflect current US policy** (45Q at $85/tCO2; 45Z up to $1.00/gal, CI-scaled, ~$0.34–0.81/gal at default lifecycle reductions, through 2029; no 45Z at facilities claiming 45Q). Policy is subject to change. EU and UK SAF mandates (ReFuelEU: 2% in 2025, 6% in 2030; UK: 2% in 2025, 10% in 2030) create compliance demand independent of $/tCO2 and are not modeled; ReFuelEU penalties are at least twice the SAF–fossil price gap.

7. **Feedstock competition effects not modeled.** If demand for waste biomass increases, prices would rise for all pathways.

8. **HEFA feedstock supply is limited.** Global collection of used cooking oil and animal fats is roughly 25 Mt/yr (IEA 2023), most of it already used for biofuels, with documented fraud risk in imported UCO. ETC (2021) puts waste lipids' ceiling at ~5% of global jet fuel.

9. **Cost estimates reflect current/near-term pricing.** SAF mid-values represent pre-commercial current costs; P10 values represent at-scale commercial targets. CDR mid-values are calibrated to actual Frontier offtake pricing (CO280 ~$285, Celsio ~$300, burial projects ~$150).

10. **No operational FT-SAF or FT-SAF+CCS plants exist.** Fulcrum's Sierra plant shut down in May 2024 and the company filed for Chapter 11 that September; Red Rock Biofuels never produced fuel. Velocys Bayou Fuels and DG Fuels (Louisiana, in FEED as of 2026) remain in development. SAF+CCS cost estimates combine FT TEA literature with CCS costs for high-purity CO2 streams.

---

## Summary Results (Central Estimates, September 2026 defaults)

Generated from the explorer defaults. P10–P90 are Monte Carlo percentiles of the net + subsidy cost. "tCO2 benefit/dry ton" is per ton of oil for the lipid pathways and per ton of MSW or wet waste for those pathways.

**Full lifecycle accounting (default):**

| Pathway | Type | Gross $/tCO₂ | Net $/tCO₂ | Net+Sub $/tCO₂ | P10–P90 (net+sub) | tCO₂ benefit/dry ton |
|---|---|---|---|---|---|---|
| FT-SAF (ag/forest waste) | SAF | $667 | $405 | $328 | $12 – $640 | 0.59 |
| FT-SAF + CCS | SAF+CDR | $303 | $202 | $147 | $29 – $269 | 1.52 |
| FT-SAF (switchgrass) | SAF | $804 | $509 | $442 | $94 – $777 | 0.52 |
| HEFA (waste fats/oils) | SAF | $577 | $275 | $210 | $17 – $436 | 1.50 |
| AtJ-SAF (ag/forest waste) | SAF | $1188 | $825 | $780 | $231 – $1238 | 0.30 |
| BECCS (electricity) | CDR | $300 | $275 | $190 | $91 – $289 | 1.90 |
| BECCS (heat) | CDR | $285 | $260 | $175 | $89 – $287 | 1.82 |
| BECCS (hydrogen) | CDR | $300 | $170 | $85 | −$27 – $197 | 1.68 |
| WtE + CCS | CDR | $300 | $260 | $175 | $69 – $273 | 1.69 |
| Bio-oil seq. (cellulosic) | CDR | $350 | $350 | $350 | $251 – $500 | 1.13 |
| Bio-oil seq. (HTL, lipid) | CDR | $325 | $325 | $325 | $228 – $478 | 1.13 |
| Biomass injection | CDR | $200 | $200 | $200 | $123 – $283 | 1.60 |
| Biochar | CDR | $200 | $180 | $180 | $73 – $330 | 0.52 |
| Biomass burial | CDR | $150 | $150 | $150 | $92 – $199 | 1.65 |

**Simple displacement accounting:**

| Pathway | Type | Gross $/tCO₂ | Net $/tCO₂ | Net+Sub $/tCO₂ | P10–P90 (net+sub) | tCO₂ benefit/dry ton |
|---|---|---|---|---|---|---|
| FT-SAF (ag/forest waste) | SAF | $718 | $436 | $353 | $13 – $662 | 0.55 |
| FT-SAF + CCS | SAF+CDR | $301 | $201 | $146 | $28 – $266 | 1.53 |
| FT-SAF (switchgrass) | SAF | $769 | $487 | $423 | $92 – $723 | 0.55 |
| HEFA (waste fats/oils) | SAF | $538 | $256 | $196 | $16 – $386 | 1.61 |
| AtJ-SAF (ag/forest waste) | SAF | $923 | $641 | $606 | $182 – $911 | 0.39 |
| BECCS (electricity) | CDR | $300 | $275 | $190 | $91 – $289 | 1.65 |
| BECCS (heat) | CDR | $285 | $260 | $175 | $89 – $287 | 1.65 |
| BECCS (hydrogen) | CDR | $300 | $170 | $85 | −$27 – $197 | 1.19 |
| WtE + CCS | CDR | $300 | $260 | $175 | $69 – $273 | 1.47 |
| Bio-oil seq. (cellulosic) | CDR | $350 | $350 | $350 | $251 – $500 | 1.19 |
| Bio-oil seq. (HTL, lipid) | CDR | $325 | $325 | $325 | $228 – $478 | 1.19 |
| Biomass injection | CDR | $200 | $200 | $200 | $123 – $283 | 1.69 |
| Biochar | CDR | $200 | $180 | $180 | $73 – $330 | 0.55 |
| Biomass burial | CDR | $150 | $150 | $150 | $92 – $199 | 1.74 |

### Key Ratios and break-evens (full lifecycle, net + subsidies)

- **BECCS (electricity) / FT-SAF benefit per dry ton:** 3.2× (1.90 vs 0.59 tCO2e)
- **FT-SAF matches BECCS (electricity, $190/t)** at a jet price of ~$4.19/gal (MFSP $7.00), or an MFSP of ~$5.56/gal (jet $2.75), or a non-CO2 credit of ~72%
- **FT-SAF at nth-plant MFSP ($4.00/gal):** ~$42/tCO2
- **FT-SAF + CCS** ($147/t) is below BECCS (electricity) at defaults; it matches BECCS at an FT base MFSP of ~$8.18/gal, and rises to $198/t at a $150/t CCS cost
- **Without 45Z** (post-2029 expiry): FT-SAF $405/t

### Key Finding

At current costs, cellulosic SAF without carbon capture costs roughly $330/tCO2 avoided, well above most biomass CDR ($150–350/t before subsidies), and delivers about a third of the climate benefit per ton of biomass. The comparison is highly sensitive to three uncertain inputs: whether FT plants reach nth-plant costs, the jet fuel price, and whether non-CO2 benefits are counted. FT-SAF + CCS, which both displaces fossil jet and stores ~54% of the biomass carbon, is cost-competitive with the benchmark BECCS pathway at central assumptions, but no such plant exists, and its advantage depends on the FT process itself becoming bankable.

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
- ICAO (2025). CORSIA Default Life Cycle Emissions Values for CORSIA Eligible Fuels, 8th edition (November 2025).
- EASA (2026). 2025 Aviation Fuels Reference Prices for ReFuelEU Aviation (briefing note).
- US EIA / FRED. U.S. Gulf Coast Kerosene-Type Jet Fuel Spot Price FOB (DJFUELUSGULF).
- 26 U.S.C. §45Z, §45Q, §45V as amended by Pub. L. 119-21 (2025); Treasury proposed regulations REG-121244-23, 91 FR 5160 (2026).
- IEAGHG (2021). Biorefineries with CCS. Technical Report 2021-01.
- IEA (2023). Is the biofuel industry approaching a feedstock crunch?
- Energy Transitions Commission (2021). Bioresources within a Net-Zero Emissions Economy.
- UK Climate Change Committee (2018). Biomass in a Low-Carbon Economy.
- Lee, D.S. et al. (2021). The contribution of global aviation to anthropogenic climate forcing for 2000 to 2018. *Atmospheric Environment* 244, 117834.
- Teoh, R. et al. (2022). Targeted use of sustainable aviation fuel to maximize climate benefits. *Environmental Science & Technology* 56, 17246–17255.
- Märkl, R. et al. (2024). Powering aircraft with 100% sustainable aviation fuel reduces ice crystals in contrails. *Atmospheric Chemistry and Physics* 24, 3813–3837.
- El-Houjeiri, H., Brandt, A., Masnadi, M. (2026). Carbon compensation or fuel displacement? Biomass allocation trade-offs for decarbonizing aviation. *iScience*. doi:10.1016/j.isci.2026.116848
- Langholtz, M. et al. (2026). Biomass carbon removal can help sustainable aviation fuels achieve on-time arrival. *iScience* 29, 115956. doi:10.1016/j.isci.2026.115956
- Jiang, Y. & Bhattacharyya, D. (2016). Techno-economic analysis of a biomass-to-liquids plant with CCS.

---


## Addendum: The Interactive Explorer

The web tool (`index.html`) implements this methodology as a client-side JavaScript model,
verified to reproduce the original Python analysis' central estimates to <$0.001/tCO2.
Differences and extensions relative to the static analysis:

1. **User-adjustable central estimates.** Sliders set each input's P50; the P10 and P90
   scale proportionally with the user's value (bounded parameters such as lifecycle
   reduction and capture efficiency are clamped at 0.95/0.98).
2. **In-browser Monte Carlo.** Uncertainty ranges are P10-P90 from a 4,000-draw Monte Carlo
   per pathway using split-normal inputs matched to each P10/P50/P90 (seeded, so ranges are
   stable while dragging sliders). The 45Z credit is stochastic, computed from each draw's
   sampled lifecycle reduction.
3. **FT-SAF+CCS cost is derived, not independent.** Its MFSP = FT-SAF (ag/forest) MFSP +
   a CCS cost slider ($/tCO2 stored, default $70, P10–P90 $40–120) applied to the ~0.018
   tCO2 stored per gallon. Moving the base FT-SAF cost moves FT-SAF+CCS with it. It carries
   a 45Q-or-45Z election (§45Z(d)(4)).
4. **BECCS-hydrogen 45Q/45V election.** The 45V clean hydrogen credit survived the OBBBA
   with a shortened window (construction start before January 1, 2028; up to $3.00/kg).
   Because 45V and 45Q cannot be claimed at the same facility (26 USC 45V(d)(2)), the tool
   offers an election: 45Q on stored CO2 (default) or 45V on the ~50 kg H2 produced per dry
   ton (~$126/tCO2 removed at 65% capture efficiency). The hydrogen co-product revenue
   (default $130/tCO2, implying ~$3.1/kg H2) is a market price assumption between gray
   (~$1-2/kg) and green (~$4.50+/kg) hydrogen, independent of either credit.
5. **Benchmark tiles** compare FT-SAF (ag/forest waste) and BECCS (electricity), two
   established routes that use the same residues, rather than the cheapest options.
6. **Non-CO2 slider** (default 0%) credits SAF with an extra benefit as a share of its CO2
   displacement benefit.

---

*Static analysis conducted May 2026 (code: `saf_vs_cdr_analysis.py`, not included in this
repository). Interactive tool and tax-credit updates: July 2026. SAF recalibration and bug
fixes: September 2026.*

---

## Changelog: September 2026

**Bugs fixed**

| Issue | Direction | Effect at defaults |
|---|---|---|
| Monte Carlo treated P10/P90 as PERT min/max | Ranges too narrow | Output P10–P90 bands were ~45% of stated input width; central values unchanged |
| Lifecycle mode applied SAF % reduction to the 9.75 kg combustion-only figure instead of the 11.66 kg well-to-wake baseline | Against SAF | SAF lifecycle benefit understated 16% ($/t overstated ~19%) |
| FT-SAF+CCS claimed 45Z and 45Q together, barred by §45Z(d)(4) | For SAF+CCS | Subsidy overstated ~$16/tCO2 |
| Lifecycle mode: table and tiles used gross CDR storage while the carbon chart used net; FT-SAF+CCS storage was netted in the chart but not in costs | Inconsistent | ≤5% |
| Negative-zero axis tick label ("$-0"); gap between stored and released segments in lifecycle carbon chart | Display | — |
| Unsupported superlatives in benchmark tiles ("most common", "leading") | Copy | — |

Validation: restoring the pre-September parameters in the updated code reproduces the previous central values exactly (FT-SAF $365, HEFA $109, AtJ $580, FT switchgrass $426/tCO2, simple accounting); FT-SAF+CCS gives $188 versus $172 previously, the difference being the removed 45Z stack.

**Parameter changes (SAF only):** jet price $2.50/$3.00/$3.50 → $2.10/$2.75/$4.00; lifecycle reductions to CORSIA 8th-edition defaults (FT 90%, switchgrass 80%, HEFA 78%, AtJ 65% central); HEFA MFSP $3.00/$4.50/$6.00 → $4.00/$5.25/$7.00; FT MFSP P90 $9 → $10 (switchgrass $10 → $10.50); CCS cost for FT-SAF+CCS $100/$150/$250 → $40/$70/$120; default accounting simple → full lifecycle. Waste-lipid supply and non-CO2 statements corrected (see caveats).

**Effect on central net + subsidy costs ($/tCO2):** FT-SAF $365 → $328; FT-SAF+CCS $172 → $147; HEFA $109 → $210; AtJ $580 → $780; FT switchgrass $426 → $442 (old values simple accounting, new values full lifecycle). Sequential attribution for FT-SAF: lifecycle default (−$25 vs simple), jet price (+$26), CORSIA lifecycle values (−$38).

**External check:** El-Houjeiri et al. (2026) harmonize 16 waste/residue SAF studies to 2024 USD; their abatement-cost medians (before subsidies) are FT $342, HEFA $362 and AtJ $938/tCO2. The explorer's net-of-revenue lifecycle values are FT $405, HEFA $275 and AtJ $825.

**Open items:** BECCS-H2 documentation used 52 kg H2/tCO2 while the code uses 50 kg H2 per dry ton (≈42 kg/tCO2 at 65% capture), so the $130/tCO2 co-product default implies ~$3.1/kg rather than $2.50/kg; CDR values were not changed pending review. The 53.7% FT carbon-capture share was not verified against a biomass-only primary source.
