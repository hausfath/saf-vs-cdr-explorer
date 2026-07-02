# SAF vs CDR Explorer

**Live tool: https://hausfath.github.io/saf-vs-cdr-explorer/**

Given a ton of waste biomass, is it a better climate investment to make sustainable aviation
fuel (SAF) or to remove carbon (CDR)? This interactive tool compares the cost per tonne of
CO₂ benefit across 14 pathways — five SAF production routes (Fischer-Tropsch, FT+CCS, HEFA,
alcohol-to-jet) and nine biomass carbon removal routes (BECCS variants, waste-to-energy+CCS,
bio-oil sequestration, biochar, biomass injection, biomass burial) — with every major
assumption adjustable.

![Screenshot of the explorer](docs/screenshot.png)

## What you can adjust

- **SAF production cost (MFSP)** for each pathway — the primary driver
- **Jet fuel price** (SAF's displacement revenue)
- **CDR total cost** for each pathway, calibrated by default to actual offtake pricing
- **Tax credits**: 45Q amount, 45Z Clean Fuel Production Credit ceiling (post-OBBBA $1.00/gal,
  pre-OBBBA $1.75/gal, or $0 for post-2029 expiry), and the BECCS-hydrogen 45Q-vs-45V
  election (the two credits cannot be stacked at one facility under §45V(d)(2))
- **Per-pathway details**: mass yield, lifecycle GHG reduction, capture efficiency,
  co-product revenue
- **Accounting basis**: simple displacement vs full lifecycle (CORSIA-style lifecycle
  factors for SAF, 95% net-negativity and co-product displacement credit for CDR)

Results update live: benchmark tiles (FT-SAF vs BECCS-electricity, the most common pathways
today), a $/tCO₂ comparison chart with P10–P90 uncertainty ranges, a carbon-fate-per-dry-ton
chart, and a full data table. **Copy scenario link** encodes all assumptions in the URL so a
specific scenario can be shared.

## How it works

The page is fully self-contained (no dependencies, no build step). The model runs a
4,000-draw Monte Carlo per pathway with PERT-distributed inputs; sliders set the central
(P50) estimate and each input's P10–P90 range scales proportionally. The 45Z credit is
computed per draw from the sampled carbon intensity, `(50 − CI)/50`, as under current law.

See [METHODOLOGY.md](METHODOLOGY.md) for the full set of assumptions, formulas, pathway
definitions, and literature sources.

## Running locally

Open `index.html` in any browser. That's it.

## Caveats

- SAF benefits are *avoided* emissions (counterfactual); CDR benefits are *removals*.
  These are not interchangeable if aviation can decarbonize another way.
- Non-CO₂ aviation effects (contrails) are not modeled; crediting them could roughly
  double SAF's effective climate benefit.
- No commercial FT-SAF+CCS plant exists; those costs are techno-economic estimates.
- HEFA uses waste fats/oils, which mostly don't compete with cellulosic CDR for feedstock.

## Credits

Analysis and tool by [Zeke Hausfather](https://github.com/hausfath). Assumptions current as
of July 2026 (post-OBBBA tax credit treatment).
