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

**Units:** "dry ton" throughout this document means a metric tonne (1,000 kg) of oven-dry biomass. Per US short ton (907 kg), all per-ton quantities scale by 0.907 (e.g. FT-SAF yields ~51 rather than 56 gallons); costs per tonne of CO2 and ratios between pathways are unaffected.

| Parameter | Value | Source |
|-----------|-------|--------|
| Carbon content of oven-dry biomass | 0.50 tC/t | BiCRS Roadmap |
| CO2:C molecular weight ratio | 3.667 (44/12) | Chemistry |
| CO2e per dry ton of biomass | 1.833 tCO2 | Derived |
| CO2 per gallon of fossil jet fuel (combustion only; reference, not used) | 9.75 kg | EIA |
| Fossil jet well-to-wake baseline | 89 gCO2e/MJ ≈ 11.66 kg CO2e/gal | ICAO CORSIA (LHV 43.2 MJ/kg) |
| Jet fuel density | 0.8 kg/L | Standard |
| Liters per gallon | 3.79 | Standard |
| Storage lifecycle factor (net / gross stored) | 0.90 | Assumed; per CDR pathway (adjustable) and for FT-SAF + CCS stored CO2 |

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
- **Simple displacement (upper bound for SAF):** each gallon avoids the full 11.66 kg CO2e well-to-wake emissions of the fossil jet it replaces, as if the SAF had zero lifecycle emissions; CDR storage is gross. SAF benefit is therefore always at least as large as under full lifecycle accounting.

In both modes the lifecycle GHG reduction also sets the 45Z credit, because the statute ties the credit to the fuel's actual carbon intensity. Under simple displacement the lifecycle slider therefore still moves the net + subsidies cost (and the Monte Carlo range), but not the gross or net-of-revenue costs.

*Changed 2026-09-28:* simple displacement previously credited only the 9.75 kg CO2 of fossil jet combustion. Because that omits fossil jet's upstream emissions (~16% of well-to-wake), high-reduction fuels such as FT-SAF (90%) came out *cheaper* under full lifecycle than under "simple" accounting (crossover at 9.75/11.66 = 83.6% reduction), contrary to the mode's description. At defaults the change lowers simple-mode FT-SAF from $353 to $295/tCO2; full-lifecycle results are unaffected.

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

**CCS cost rationale.** Gasification already strips CO2 in the acid-gas-removal unit as a near-pure stream, so the incremental cost is dehydration, compression, transport and storage. Published costs for high-purity streams: capture and compression ~$17.5/t average for >95% CO2 streams (NETL, 2018$; secondary extraction), $22–25/t for fermentation CCS (IEAGHG 2021-01); saline storage mostly ≤$8/t (NETL 2024); pipeline transport ~$15–25/t for dedicated 100 km lines (NETL). A standalone FT plant capturing ~0.5 Mt/yr would likely need its own pipeline and well, so the P50 ($70) and P90 ($120) sit above the ethanol-network analogs. We do not present a higher CCS cost (such as the previous $150/t) as a standalone sensitivity case: BECCS costs are calibrated to offtake totals that already embed their own transport and storage costs, so raising CCS cost for FT-SAF + CCS alone would not be a like-for-like comparison. The slider remains available for exploring FT-SAF + CCS on its own.

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

### Lifecycle factor (net negativity)

Each CDR pathway has a **lifecycle factor** L: net CO2 removed per gross tonne stored, after supply-chain, processing and transport emissions. The default is **L = 0.90** for every pathway (adjustable per pathway under "More assumptions"; no Monte Carlo range is assigned).

CDR cost and co-product inputs are quoted **per net tonne at the reference factor of 0.90** (consistent with offtake pricing, which is per net tonne delivered). The model converts them to per-gross-tonne values (input × 0.90) and then reports:
- **Full lifecycle:** per net tonne = gross value / L. At L = 0.90 the cost inputs are unchanged; a lower L raises the cost per net tonne.
- **Simple displacement:** per gross tonne stored (no lifecycle haircut), i.e. 10% below the input values.

**45Q is applied to the tonnes counted**: $85 per net tonne in lifecycle mode and per gross tonne in simple mode, for CDR pathways and for FT-SAF + CCS alike. This matches the Frontier BiCRS POV (BECCS-electricity $300 − $25 − $85 = $190). The statute pays 45Q on gross metered tonnes, so this convention understates the credit by a factor L (about $9 per net tonne at L = 0.90), conservatively against CDR. The 45V election for BECCS-H2 is per kg of hydrogen, so it is converted physically to the counted tonne. The same 0.90 factor is applied to the CO2 stored by FT-SAF + CCS for a like-for-like comparison; because the fuel's 90% lifecycle reduction already carries the plant's supply-chain emissions, this partly double-counts and errs against FT-SAF + CCS.

*Changed 2026-10-02:* previously the lifecycle haircut was folded into the CDR prices and a fixed 95% factor was applied only in the per-tonne-of-biomass carbon chart; 45Q was credited at $85 per net tonne.

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
- Gross tCO2 stored per dry tonne: 1.650 (net 1.485 at L = 0.90)
- Net $/tCO2: $275
- Net + subsidy $/tCO2: $190 (lifecycle)

#### BECCS (Heat)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.80 | 0.90 | 0.95 | |
| Total cost ($/tCO2) | 200 | 285 | 400 | Pre-subsidy |
| Co-product revenue ($/tCO2) | 20 | 25 | 35 | Heat at 0.7 MWh_th/tCO2, ~$35/MWh |
| 45Q subsidy ($/tCO2) | 85 | 85 | 85 | Eligible |
| Avoided emissions (tCO2/dry ton) | 0.25 | 0.25 | 0.25 | Displacing gas boiler heat |

**Derived (mid values):**
- Gross tCO2 stored per dry tonne: 1.650 (net 1.485 at L = 0.90)
- Net $/tCO2: $260
- Net + subsidy $/tCO2: $175 (lifecycle)

#### BECCS (Hydrogen)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.55 | 0.65 | 0.75 | Lower than other BECCS; Mote PFD shows 65.1% |
| Total cost ($/tCO2) | 200 | 300 | 400 | Pre-subsidy |
| Co-product revenue ($/tCO2) | 78 | 130 | 182 | H2 at 52 kg/tCO2, $1.50-3.50/kg |
| 45Q subsidy ($/tCO2) | 85 | 85 | 85 | Eligible |
| Avoided emissions (tCO2/dry ton) | 0.55 | 0.55 | 0.55 | Displacing gray H2 (12 tCO2/tH2) |

**Derived (mid values):**
- Gross tCO2 stored per dry tonne: 1.192 (net 1.073 at L = 0.90)
- Net $/tCO2: $170
- Net + subsidy $/tCO2: $85 (lifecycle)

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
- Gross tCO2 stored per dry tonne: 1.467 (net 1.320 at L = 0.90)
- Net $/tCO2: $260
- Net + subsidy $/tCO2: $175 (lifecycle)

#### Bio-oil Sequestration (Cellulosic)

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.55 | 0.65 | 0.70 | Fraction of biomass C retained in stable bio-oil |
| Total cost ($/tCO2) | 250 | 350 | 500 | Current Frontier offtake pricing |
| Co-product revenue ($/tCO2) | 0 | 0 | 0 | None assumed (nutrient-return value uncertain) |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- Gross tCO2 stored per dry tonne: 1.19
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
- Gross tCO2 stored per dry tonne: 1.687
- Net $/tCO2: $200

#### Biochar

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.20 | 0.30 | 0.40 | Low — much carbon lost as gas during pyrolysis |
| Total cost ($/tCO2) | 80 | 160 | 280 | Set so net cost after char revenue is $140, the CDR.fyi average biochar credit price (user-supplied, Oct 2026; not independently verified); range scaled proportionally from the earlier 100/200/350 |
| Co-product revenue ($/tCO2) | 0 | 20 | 50 | Conservative estimate; biochar market uncertain |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- Gross tCO2 stored per dry tonne: 0.550 (net 0.495 at L = 0.90)
- Net $/tCO2: $140

#### Biomass Burial

| Parameter | P10 | P50 | P90 | Notes |
|-----------|-----|-----|-----|-------|
| CDR efficiency | 0.85 | 0.95 | 0.98 | High if properly sealed |
| Total cost ($/tCO2) | 90 | 150 | 200 | Current ~$150; NOAK target ~$90-100 |
| Co-product revenue ($/tCO2) | 0 | 0 | 0 | None |
| 45Q subsidy ($/tCO2) | 0 | 0 | 0 | Not eligible |

**Derived (mid values):**
- Gross tCO2 stored per dry tonne: 1.742
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
- **CO2 permanently stored** (green): net of the storage lifecycle factor (0.90 by default)
- **Upstream process emissions** (orange) — 5% of gross stored for CDR; lifecycle reduction for SAF
- **CO2 released to atmosphere** (gray) — biomass carbon not captured
- **CO2 avoided** (blue, hatched, stacked above) — additional abatement from displacing fossil energy

At current defaults, BECCS (electricity) delivers 1.82 tCO2e per dry tonne (1.49 net stored + 0.33 avoided) against 0.59 for FT-SAF, a 3.1× ratio; FT-SAF + CCS delivers 1.47.

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

## Social cost of carbon reference lines

The explorer's cost chart shows two reference lines from EPA's *Report on the Social Cost of Greenhouse Gases* (November 2023), Table A.5.1, for 2025 emissions: **$130/tCO2 at a 2.5% near-term discount rate and $360 at 1.5%** (central $212 at 2.0%), in 2020 dollars. They are converted to 2025 dollars with the BEA GDP implicit price deflator (FRED `GDPDEF`, 2025 annual mean 128.97 / 2020 annual mean 105.36 = 1.224), giving **$159 and $441**. The pair spans EPA's discount-rate range only, not its (much wider) damage-uncertainty range. Context:
- Rennert et al. (2022, *Nature*) preferred mean $185/tCO2 (5–95%: $44–413; 2020$, 2% discount). Their GIVE model is one of EPA's three damage modules, so it is not fully independent of EPA's values.
- Moore et al. (2024, NBER WP 32544 / *PNAS*): synthetic distribution median $185, mean $284 (5–95%: $32–874), 2020 pulse year.
- The 2021 IWG interim value was $51 (3% discount, 2020 emissions, 2020$).
- Current US federal status: Executive Order 14154 (Jan 20, 2025) disbanded the IWG and withdrew its estimates; OMB memo M-25-27 (May 5, 2025) states "it is no longer Federal government policy to maintain a uniform estimate of the monetized impacts of greenhouse gas emissions" and that such analysis will be needed in "few to none" cases, reverting to Circular A-4 (2003) where used.
- Replaces the earlier unsourced "$50 / $200" lines (changed 2026-09-28).

## Summary Results (Central Estimates, September 2026 defaults)

Generated from the explorer defaults. P10–P90 are Monte Carlo percentiles of the net + subsidy cost. "tCO2 benefit/dry ton" is per ton of oil for the lipid pathways and per ton of MSW or wet waste for those pathways.

**Full lifecycle accounting (default):**

| Pathway | Type | Gross $/tCO₂ | Net $/tCO₂ | Net+Sub $/tCO₂ | P10–P90 (net+sub) | tCO₂ benefit/dry tonne |
|---|---|---|---|---|---|---|
| FT-SAF (ag/forest waste) | SAF | $667 | $405 | $328 | $12 – $640 | 0.59 |
| FT-SAF + CCS | SAF+CDR | $313 | $208 | $157 | $36 – $284 | 1.47 |
| FT-SAF (switchgrass) | SAF | $804 | $509 | $442 | $94 – $777 | 0.52 |
| HEFA (waste fats/oils) | SAF | $577 | $275 | $210 | $17 – $436 | 1.50 |
| AtJ-SAF (ag/forest waste) | SAF | $1188 | $825 | $780 | $231 – $1238 | 0.30 |
| BECCS (electricity) | CDR | $300 | $275 | $190 | $91 – $289 | 1.82 |
| BECCS (heat) | CDR | $285 | $260 | $175 | $89 – $287 | 1.74 |
| BECCS (hydrogen) | CDR | $300 | $170 | $85 | −$27 – $197 | 1.62 |
| WtE + CCS | CDR | $300 | $260 | $175 | $69 – $273 | 1.62 |
| Bio-oil seq. (cellulosic) | CDR | $350 | $350 | $350 | $251 – $500 | 1.07 |
| Bio-oil seq. (HTL, lipid) | CDR | $325 | $325 | $325 | $228 – $478 | 1.07 |
| Biomass injection | CDR | $200 | $200 | $200 | $123 – $283 | 1.52 |
| Biochar | CDR | $160 | $140 | $140 | $53 – $259 | 0.50 |
| Biomass burial | CDR | $150 | $150 | $150 | $92 – $199 | 1.57 |

**Simple displacement accounting (no lifecycle haircut on either side):**

| Pathway | Type | Gross $/tCO₂ | Net $/tCO₂ | Net+Sub $/tCO₂ | P10–P90 (net+sub) | tCO₂ benefit/dry tonne |
|---|---|---|---|---|---|---|
| FT-SAF (ag/forest waste) | SAF | $600 | $365 | $295 | $11 – $554 | 0.65 |
| FT-SAF + CCS | SAF+CDR | $282 | $188 | $136 | $26 – $248 | 1.64 |
| FT-SAF (switchgrass) | SAF | $643 | $407 | $354 | $77 – $605 | 0.65 |
| HEFA (waste fats/oils) | SAF | $450 | $214 | $164 | $13 – $323 | 1.92 |
| AtJ-SAF (ag/forest waste) | SAF | $772 | $536 | $507 | $152 – $762 | 0.46 |
| BECCS (electricity) | CDR | $270 | $248 | $163 | $74 – $252 | 1.65 |
| BECCS (heat) | CDR | $257 | $234 | $149 | $72 – $249 | 1.65 |
| BECCS (hydrogen) | CDR | $270 | $153 | $68 | −$33 – $169 | 1.19 |
| WtE + CCS | CDR | $270 | $234 | $149 | $54 – $237 | 1.47 |
| Bio-oil seq. (cellulosic) | CDR | $315 | $315 | $315 | $226 – $450 | 1.19 |
| Bio-oil seq. (HTL, lipid) | CDR | $293 | $293 | $293 | $206 – $430 | 1.19 |
| Biomass injection | CDR | $180 | $180 | $180 | $111 – $255 | 1.69 |
| Biochar | CDR | $144 | $126 | $126 | $47 – $233 | 0.55 |
| Biomass burial | CDR | $135 | $135 | $135 | $82 – $179 | 1.74 |

### Key Ratios and break-evens (full lifecycle, net + subsidies)

- **BECCS (electricity) / FT-SAF benefit per dry tonne:** 3.1× (1.82 vs 0.59 tCO2e)
- **FT-SAF matches BECCS (electricity, $190/t)** at a jet price of ~$4.19/gal (MFSP $7.00), or an MFSP of ~$5.56/gal (jet $2.75), or a non-CO2 credit of ~72%
- **FT-SAF at nth-plant MFSP ($4.00/gal):** ~$42/tCO2
- **FT-SAF + CCS** ($157/t) is below BECCS (electricity) at defaults; it matches BECCS at an FT base MFSP of ~$7.86/gal
- **Without 45Z** (post-2029 expiry): FT-SAF $405/t

### Key Finding

At current costs, cellulosic SAF without carbon capture costs roughly $330/tCO2 avoided, well above most biomass CDR (about $140–350/t after co-product revenue and credits), and delivers about a third of the climate benefit per tonne of biomass. The comparison is highly sensitive to three uncertain inputs: whether FT plants reach nth-plant costs, the jet fuel price, and whether non-CO2 benefits are counted. FT-SAF + CCS, which both displaces fossil jet and stores ~54% of the biomass carbon, is cost-competitive with the benchmark BECCS pathway at central assumptions, but no such plant exists, and its advantage depends on the FT process itself becoming bankable.

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

Validation (2026-09-24, before simple displacement was redefined on 2026-09-28): restoring the pre-September parameters in the updated code reproduced the previous central values exactly (FT-SAF $365, HEFA $109, AtJ $580, FT switchgrass $426/tCO2, simple accounting); FT-SAF+CCS gave $188 versus $172 previously, the difference being the removed 45Z stack.

**Parameter changes (SAF only):** jet price $2.50/$3.00/$3.50 → $2.10/$2.75/$4.00; lifecycle reductions to CORSIA 8th-edition defaults (FT 90%, switchgrass 80%, HEFA 78%, AtJ 65% central); HEFA MFSP $3.00/$4.50/$6.00 → $4.00/$5.25/$7.00; FT MFSP P90 $9 → $10 (switchgrass $10 → $10.50); CCS cost for FT-SAF+CCS $100/$150/$250 → $40/$70/$120; default accounting simple → full lifecycle. Waste-lipid supply and non-CO2 statements corrected (see caveats).

**Effect on central net + subsidy costs ($/tCO2):** FT-SAF $365 → $328; FT-SAF+CCS $172 → $147; HEFA $109 → $210; AtJ $580 → $780; FT switchgrass $426 → $442 (old values simple accounting, new values full lifecycle). Sequential attribution for FT-SAF: lifecycle default (−$25 vs the combustion-only simple mode then in use), jet price (+$26), CORSIA lifecycle values (−$38).

**External check:** El-Houjeiri et al. (2026) harmonize 16 waste/residue SAF studies to 2024 USD; their abatement-cost medians (before subsidies) are FT $342, HEFA $362 and AtJ $938/tCO2. The explorer's net-of-revenue lifecycle values are FT $405, HEFA $275 and AtJ $825.

**2026-09-28 follow-up:** simple displacement redefined as a zero-emission-SAF upper bound (see Emissions Accounting); simple-mode table regenerated. Full-lifecycle defaults, article figures and break-evens unchanged.

**2026-10-02 follow-up:** CDR lifecycle disaggregated from price: per-pathway factor L (default 0.90); cost and co-product inputs are quoted per net tonne at that reference; 45Q is applied to the tonnes counted (per net tonne in lifecycle mode), matching the Frontier BiCRS POV, so default BECCS costs are unchanged (electricity $190, heat and WtE + CCS $175, H2 $85). Lowering L now raises CDR cost per net tonne. FT-SAF + CCS stored CO2 uses the same 0.90 (was 0.95) and the same 45Q convention: $147 → $157/t; parity with BECCS at an FT base MFSP of $7.86/gal (was $8.18). Biochar total cost 200 → 160 so its net price is $140 (CDR.fyi average, user-supplied): $180 → $140. FT-SAF break-evens unchanged ($4.19 jet, $5.56 MFSP, 72% non-CO2). Simple displacement now reports CDR per gross tonne stored (~10% below lifecycle-mode cost before credits).

**Open items:** BECCS-H2 documentation used 52 kg H2/tCO2 while the code uses 50 kg H2 per dry ton (≈42 kg/tCO2 at 65% capture), so the $130/tCO2 co-product default implies ~$3.1/kg rather than $2.50/kg; CDR values were not changed pending review. The 53.7% FT carbon-capture share was not verified against a biomass-only primary source.
