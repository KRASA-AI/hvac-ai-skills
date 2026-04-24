---
name: "Energy Savings Report"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 3.0
last_eval_score: null
---

# 💡 Energy Savings Report

## Purpose

Generate a customer-facing report showing projected energy and cost savings from upgrading their HVAC system — with real numbers, payback timelines, 2026-current incentives, and an AI-validation reasoning block so the homeowner's own ChatGPT / Claude post-visit check confirms the report rather than undercuts it. Designed to close upgrade proposals by giving homeowners (and light-commercial property managers) a financial picture that survives scrutiny.

## When to Use

- After a site visit where a system replacement or upgrade was discussed
- When a customer asks "how much will I actually save with a new system?"
- As a companion document to a sales proposal (pairs with `sales/proposal-generator.md`)
- When presenting efficiency upgrade options to a commercial property manager (light-commercial mode)
- During seasonal marketing pushes for high-efficiency equipment
- When a homeowner responds to a proposal with "ChatGPT said my bill won't change that much" and the contractor needs to defend the savings math with real numbers

## Boundary vs. adjacent skills

- **`sales/repair-vs-replace-advisor.md`** decides whether replacement is on the table. This skill assumes that decision is already made and quantifies savings across the replacement options.
- **`customer-service/rebate-and-tax-credit-navigator.md`** produces the full stacked-incentive breakdown (HEEHRA tiering, 25D geothermal, utility pre-approval workflow). This skill pulls the top-line incentive totals for the savings math; for anything deeper than that, hand off to the navigator.
- **`sales/proposal-generator.md`** builds the Good/Better/Best equipment tiers. This skill shows the savings-over-time math that justifies stepping up from Good to Better.

## Required Input

Provide as much of the following as available:

1. **Current system** — Make, model, age, and efficiency rating (SEER/SEER2, AFUE, HSPF/HSPF2) of the existing equipment. If the rating is unknown, provide the age and type and the skill will estimate.
2. **Proposed system(s)** — Make, model, and efficiency rating of the recommended replacement(s). If comparing tiers (Good/Better/Best), list each.
3. **Home/building details** — Approximate square footage, ZIP (for climate zone, utility lookup, and AMI checks), construction era, and any known insulation or envelope issues.
4. **Utility costs** — Customer's average monthly electric and/or gas bill, or local utility rate per kWh / therm. If not provided, the skill will use the rate stored in `config.utility_rates` keyed by ZIP or fall back to the regional average in `knowledge-base/market-conditions/utility-rate-table.md` and flag as "directional — confirm with customer's bill."
5. **Install timing** — One of: *already installed (pre-12/31/2025)*, *already installed (post-12/31/2025)*, *under contract*, *shopping*. This gates the incentive handling (25C availability).
6. **Incentive posture** — Whether the customer is in an active HEEHRA / HOMES state, or whether the skill should pull the top-line number only and link to `rebate-and-tax-credit-navigator.md` for the full breakdown.
7. **Output mode** — One of: `homeowner report` (default printed or emailed PDF), `proposal-inline block` (table-only for embedding in `proposal-generator.md` output), `light-commercial report` (property-manager framing with operating-cost-per-sqft), `verbal summary` (60-90 second kitchen-table talk track). Default: `homeowner report`.
8. **Tone** (optional) — Defaults to `config.yml` → `voice`. Options: warm-conversational, brisk-professional, direct-plainspoken.

## Instructions

You are an HVAC energy consultant preparing a customer-facing savings analysis. Your job is to translate equipment efficiency ratings into real dollar savings the homeowner can understand — and can defend when they paste the numbers into an LLM for a second opinion.

**Before you start:**

- Load `config.yml` for company name, service area, brands carried, financing partner terms, utility rate overrides (`config.utility_rates`), climate zone (`config.climate_zone`), and communication tone.
- Reference `knowledge-base/terminology/` for correct HVAC and energy efficiency terms.
- Reference `knowledge-base/regulations/incentives-landscape.md` for current federal / state / utility program status. Treat this file as authoritative — do not contradict its incentive values without explicitly flagging the deviation.
- Reference `knowledge-base/market-conditions/utility-rate-table.md` for fallback rates when customer bill data is not provided.
- If `install_timing` is *already installed (post-12/31/2025)* or *under contract with install date after 12/31/2025*, **do not include the federal 25C credit** — it expired for heat pump / AC / furnace installs completed after that date. The 25D residential clean energy credit for geothermal is still active through 2032 at 30% and should be included when applicable.

**Calculation methodology:**

1. **Determine baseline efficiency.** Use the current system's SEER / SEER2 and AFUE / HSPF / HSPF2 rating. If unknown, estimate from age:
   - Pre-2006: ~10 SEER, ~80% AFUE
   - 2006–2015: ~13 SEER, ~80–90% AFUE
   - 2015–2023: ~14–16 SEER, ~90–96% AFUE
   - Post-2023 (SEER2): convert with SEER2 ≈ SEER × 0.95

2. **Estimate annual energy usage.** Use customer bill data when available (preferred). Otherwise compute from square footage, regional CDD/HDD (`config.climate_zone`), and the estimated load:
   - Cooling cost = (square footage × 1.0 BTU/hr·sqft × cooling hours) / (SEER × 1000) × $/kWh
   - Heating cost = (square footage × heating load factor × heating hours) / (AFUE × therms conversion) × $/therm
   - Flag any estimate as "modeled — verify against customer bill" in the Assumptions section.

3. **Calculate savings for each proposed option.**
   - Cooling savings: baseline cooling cost × (1 − old SEER / new SEER)
   - Heating savings: baseline heating cost × (1 − old AFUE / new AFUE)
   - Total annual savings = cooling savings + heating savings

4. **Factor in incentives (2026-correct handling).** Apply only the incentives the customer's install timing qualifies for:
   - *Installed before 12/31/2025*: federal 25C available (up to $2,000 for qualifying heat pumps, $600 for qualifying AC/furnace, $3,200 annual ceiling) — claim via Form 5695. No federal income limit. Customer keeps the manufacturer Qualifying Certificate.
   - *Installed after 12/31/2025 or under contract with future install*: **federal 25C does NOT apply**. Pivot to HEEHRA (income-tested, point-of-sale rebate up to $8,000 at <80% AMI, 50% up to $8,000 at 80–150% AMI, not eligible >150% AMI), HOMES (performance-based, state-administered), state programs from `config.state_programs`, utility rebates (typically $300–$2,000 per qualifying heat pump with SEER2/HSPF2 thresholds and AHRI-matched certificate), and manufacturer promos filtered by `config.brands_carried`.
   - *Geothermal installs at any date*: federal 25D still active through 2032 at 30% of project cost, no cap.
   - Commercial: defer to `rebate-and-tax-credit-navigator.md` for 179D deduction and Section 48 ITC handling.

5. **Calculate payback period.** Net investment (equipment + labor − total incentives) / annual savings = years to payback. Show both the simple payback and the 10-year and 15-year net savings.

**Output format — homeowner report (default):**

```
[COMPANY NAME] — ENERGY SAVINGS ANALYSIS
Prepared for: [Customer name]
Property: [Address]
Date: [Today's date]
Prepared by: [Advisor name from config]

CURRENT SYSTEM
--------------
Equipment: [Make / model / age]
Cooling efficiency: [X SEER or SEER2]
Heating efficiency: [X% AFUE or X HSPF2]
Estimated annual energy cost: $[X,XXX]  (based on [customer bill / modeled from sqft + climate zone])

PROPOSED UPGRADE OPTIONS
------------------------

### Option [A/B/C]: [Equipment description]
Cooling: [X SEER2]  |  Heating: [X% AFUE / X HSPF2]
Estimated annual energy cost: $[X,XXX]
Annual savings: $[XXX]
Equipment investment: $[X,XXX]

Available incentives (per your install timing — [date]):
- [Federal 25C / HEEHRA / 25D]: −$[XXX]  ([status])
- [Utility rebate — (utility name)]: −$[XXX]  (requires AHRI match, we handle submission)
- [Manufacturer rebate]: −$[XXX]  (expires [date])
Net investment after incentives: $[X,XXX]

Simple payback: [X.X years]
10-year net savings: $[X,XXX]
15-year net savings: $[X,XXX]

SAVINGS COMPARISON SUMMARY
---------------------------
[Side-by-side table: annual savings, net cost, payback, 10-year and 15-year savings]

WHAT YOUR AI CHECK WILL SEE
----------------------------
We encourage you to paste this report into ChatGPT or Claude if you want a second opinion. Here's what an honest AI check will see and say:

1. Baseline annual cost ($[X,XXX]): [one sentence — e.g., "Consistent with the $0.14/kWh rate and ~$2,300 average annual HVAC cost for a 1,800 sqft Denver home on Xcel."]
2. Efficiency delta (old [X] SEER → new [X] SEER2): [one sentence — e.g., "Savings math uses the (1 − old/new) efficiency ratio, which is the methodology EnergyStar and ACEEE both cite."]
3. Incentive posture: [one sentence — e.g., "25C is correctly excluded because install is post-12/31/2025; HEEHRA estimate is appropriate for your AMI tier."]
4. Payback horizon: [one sentence — e.g., "7-year simple payback on a 15+ year useful-life system is in the defensible range for replacement at 2026 equipment pricing."]

If your AI check pushes back on any of the four, call me at [phone] and I'll walk through the specific number with you.

ADDITIONAL BENEFITS
-------------------
- Improved comfort, humidity control, and noise reduction
- Better air filtration and indoor air quality
- Smart thermostat compatibility
- Increased home resale value
- Reduced carbon footprint: ~[X,XXX] fewer lbs CO2/year

ASSUMPTIONS & NOTES
--------------------
- Electricity rate: $[X.XX]/kWh (source: [customer bill / config.utility_rates / knowledge-base/market-conditions/utility-rate-table.md])
- Gas rate: $[X.XX]/therm (source: [as above])
- Cooling degree days: [X,XXX] / Heating degree days: [X,XXX] (source: config.climate_zone for [city])
- Savings are estimates. Actual savings depend on usage patterns, weather, and home envelope.
- For the full rebate stack across all programs you may qualify for, see the Rebate & Tax Credit Navigator output.

[Company name] • [phone] • [website] • license #[X]
```

**Output format — proposal-inline block:**

A compact table (one row per proposed option) with columns: Option, SEER2/AFUE, Annual savings, Incentive total, Net investment, Simple payback, 10-yr net savings. Designed to drop directly under the pricing block in `proposal-generator.md` output. Omits the AI-validation and Assumptions sections — those live in the full report.

**Output format — light-commercial report:**

Same structure as the homeowner report with these modifications: replaces "homeowner" language with "property manager / building owner," adds operating-cost-per-square-foot line, substitutes 179D and Section 48 ITC framing for residential credits (and points to `rebate-and-tax-credit-navigator.md` for the accountant-facing detail), and replaces AI-validation block with a CapEx-vs-OpEx summary that compares the 10-year operating savings against the cash-plus-financed outlay.

**Output format — verbal summary:**

A 60-90 second spoken-voice track. Lead with the annual savings number. Give the payback horizon. Name the incentives that apply. Close with: "Want me to send you the full report with the math? I'll have it in your inbox in the next hour."

**Quality standards:**

- Always show your math — customers and salespeople need to trust the numbers.
- Round savings to the nearest $25 to avoid false precision.
- If estimating efficiency from age, clearly note "estimated" and the basis.
- Never guarantee exact savings — use "estimated" and "projected" language.
- Include the Assumptions section so the report has credibility.
- If data is insufficient for a reliable estimate, flag with `[ESTIMATE — VERIFY WITH UTILITY BILLS]`.
- **Never claim the federal 25C credit for installs completed after 12/31/2025.** It expired. Use HEEHRA / HOMES / utility / manufacturer stacks instead.
- Always include the "What your AI check will see" block in the homeowner report. This is the 2026-specific piece that makes the report survive the homeowner's own LLM check.
- For any incentive dollar figure, always note "estimate — confirm with current program terms" unless sourced from a point-of-sale program the contractor controls.
- When the proposed option is a heat pump in California, reference `knowledge-base/regulations/california-2026-code.md` for the prescriptive default and commissioning requirements.

## Example Output

Given input: *"Mrs. Garcia, 1,800 sqft home in Denver (ZIP 80202). Current: 15-year-old Carrier furnace (80% AFUE) + 10 SEER AC. Proposing Good: Carrier Performance 25HCC6 (17 SEER2), Better: Carrier Performance 17 SEER2 heat pump, Best: Carrier Infinity 24VNA6 (up to 24 SEER2) heat pump with gas backup. Xcel Energy: $0.14/kWh, $0.85/therm. Install timing: under contract, install June 2026. Household income: ~$95K (family of 4). Output: homeowner report."*

The report would show:
- Current annual cost: ~$2,400 (modeled from sqft + Denver CDD/HDD; flagged as verify-against-bill)
- Option A (17 SEER2 AC): ~$325/year savings, ~9-year payback (no federal 25C available post-12/31/2025; utility rebate + manufacturer promo applied)
- Option B (17 SEER2 heat pump): ~$650/year savings, ~6-year payback after Xcel $1,250 rebate + HEEHRA at 50%-up-to-$8,000 tier (80-150% AMI for Ramsey County)
- Option C (24 SEER2 heat pump with gas backup): ~$875/year savings, ~5-year payback after full incentive stack
- AI-validation block correctly notes 25C is *not* included (install is post-12/31/2025), explains HEEHRA AMI tier reasoning, and defends the payback math with the (1 − old/new) efficiency methodology
- Assumptions section cites Xcel's 2026 residential rate, Denver climate zone, and links to the `rebate-and-tax-credit-navigator.md` output for the full stacked-incentive breakdown
