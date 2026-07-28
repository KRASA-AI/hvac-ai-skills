---
name: "Energy Savings Report"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 4.4
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

**Minimum viable input (fastest path — everything else below is estimate-and-flag):** ZIP code +
current system age/type + install timing. That's enough to generate a directionally useful
`homeowner report` with every unmeasured number marked `[ESTIMATE — VERIFY WITH UTILITY BILLS]`.
Don't hold up the report chasing all ten fields below — gather what's on hand, run it, and let the
Assumptions section carry the gaps. Provide as much of the following as available:

1. **Current system (required — or estimate from age)** — Make, model, age, and efficiency rating (SEER/SEER2, AFUE, HSPF/HSPF2) of the existing equipment. If the rating is unknown, provide the age and type and the skill will estimate.
2. **Proposed system(s) (required)** — Make, model, and efficiency rating of the recommended replacement(s). If comparing tiers (Good/Better/Best), list each.
3. **Home/building details (recommended — estimate from ZIP + regional averages if omitted)** — Approximate square footage, ZIP (for climate zone, utility lookup, and AMI checks), construction era, and any known insulation or envelope issues.
4. **Utility costs (optional — falls back to config/regional table)** — Customer's average monthly electric and/or gas bill, or local utility rate per kWh / therm. If not provided, the skill will use the rate stored in `config.utility_rates` keyed by ZIP or fall back to the regional average in `knowledge-base/market-conditions/utility-rate-table.md` and flag as "directional — confirm with customer's bill."
5. **Install timing (required)** — One of: *already installed (pre-12/31/2025)*, *already installed (post-12/31/2025)*, *under contract*, *shopping*. This gates the incentive handling (25C availability).
6. **Incentive posture (optional)** — Whether the customer is in an active HEEHRA / HOMES state, or whether the skill should pull the top-line number only and link to `rebate-and-tax-credit-navigator.md` for the full breakdown.
7. **Output mode (optional — defaults to `homeowner report`)** — One of: `homeowner report` (default printed or emailed PDF), `proposal-inline block` (table-only for embedding in `proposal-generator.md` output), `light-commercial report` (single-property property-manager framing with operating-cost-per-sqft), `commercial-portfolio` (multi-building energy roll-up for a property-management entity — adds per-property energy table, portfolio-wide OpEx-per-sqft, 10-year operating savings projection by property, and a CapEx-by-quarter rollout sequence), `verbal summary` (60-90 second kitchen-table talk track). Default: `homeowner report`.
8. **Tone** (optional) — Defaults to `config.yml` → `voice`. Options: warm-conversational, brisk-professional, direct-plainspoken.
9. **Commercial-portfolio inputs (only when `output_mode = commercial-portfolio`)** — Building roster (`property_id`, address, sqft, year built, current equipment, current annual energy spend or utility-account ID), filing-entity tax structure (LLC / S-corp / C-corp / non-profit / municipal — drives 179D / Section 48 / §6417 elective-pay handling on the proposal hand-off), `config.crm_record_id` for prior-bid linkback, `config.energy_modeling_partner` if a third-party modeler will validate the projection. If a property's current annual energy spend is unknown, the skill will model it from sqft + climate zone and flag the row `[MODELED — VERIFY WITH UTILITY ACCOUNT]`.
10. **TOU / time-of-use overlay (optional but recommended)** — Whether the customer's utility tariff is time-of-use, tiered, or flat. If TOU, provide on-peak / mid-peak / off-peak rates and hours from `config.utility_rates[zip].tou` if present; otherwise the skill will pull the regional default from `knowledge-base/market-conditions/utility-rate-table.md` and flag as "directional — confirm with customer's bill." Heat-pump pre-cool / pre-heat scheduling savings is a separate line in the savings table when this overlay is on.

## Instructions

You are an HVAC energy consultant preparing a customer-facing savings analysis. Your job is to translate equipment efficiency ratings into real dollar savings the homeowner can understand — and can defend when they paste the numbers into an LLM for a second opinion.

**Step 0 — pick the mode first, then read only that path.** This skill has five output modes, but the four residential/single-property modes are lightweight; the commercial-portfolio mode is the only heavy one. Route before you read further so you don't carry the portfolio machinery into a simple homeowner job:

| If the job is… | Use mode | Inputs you need | Inputs/sections you can IGNORE |
|----------------|----------|-----------------|-------------------------------|
| One home, full report (DEFAULT) | `homeowner report` | Inputs 1–8 | #9 (portfolio roster), #10 TOU unless flagged, the commercial-portfolio template, the CFO block |
| A table to drop into a proposal | `proposal-inline block` | 1–7 | same as above + the AI-validation/Assumptions prose |
| One commercial building, one owner | `light-commercial report` | 1–8 + per-sqft framing | #9 portfolio roster, CapEx-by-quarter |
| A 60–90s talk track | `verbal summary` | 1–6 | everything else |
| **Multiple buildings / one entity** | `commercial-portfolio` | 1–8 **plus #9 roster and #10 TOU** | nothing — this is the full path |

Only `commercial-portfolio` uses the per-property roster (#9), the 179D/Section 48/§6417 hand-off, the portfolio roll-up template, the CapEx tables, and the "What your CFO will see" block. If you are in any of the other four modes, treat those sections as not present. The default is `homeowner report`; when in doubt for a single residence, use it.

**Before you start:**

- Load `config.yml` for company name, service area, brands carried, financing partner terms, utility rate overrides (`config.utility_rates`), climate zone (`config.climate_zone`), and communication tone.
- Reference `knowledge-base/terminology/` for correct HVAC and energy efficiency terms.
- Reference `knowledge-base/regulations/incentives-landscape.md` for current federal / state / utility program status. Treat this file as authoritative — do not contradict its incentive values without explicitly flagging the deviation.
- Reference `knowledge-base/market-conditions/utility-rate-table.md` for fallback rates when customer bill data is not provided.
- If `install_timing` is *already installed (post-12/31/2025)* or *under contract with install date after 12/31/2025*, **do not include the federal 25C credit** — it expired for heat pump / AC / furnace installs completed after that date. **Do not include the 25D geothermal credit either for post-2025 installs** — the OBBBA (signed July 2025) terminated 25D for expenditures made after 12/31/2025. 25D may be claimed only for systems paid and placed in service by 12/31/2025. Verify current IRS guidance before including either credit.

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
   - *Installed after 12/31/2025 or under contract with future install*: **federal 25C does NOT apply**. Pivot to HEEHRA (income-tested, point-of-sale rebate up to $8,000 at <80% AMI, 50% up to $8,000 at 80–150% AMI, not eligible >150% AMI — **but see the fuel-switching gate immediately below, which disqualifies a large share of the replacement jobs this skill models**), HOMES (performance-based, state-administered), state programs from `config.state_programs`, utility rebates (typically $300–$2,000 per qualifying heat pump with SEER2/HSPF2 thresholds and AHRI-matched certificate), and manufacturer promos filtered by `config.brands_carried`.
   - 🚨 **HEEHR fuel-switching gate — run this BEFORE putting a HEEHRA dollar figure on any option.** Under 2026 DOE guidance, HEEHR **no longer funds fuel switching** (see `knowledge-base/regulations/incentives-landscape.md`). Apply this test to *each modeled option independently*, because a single report routinely contains options that land on both sides of the line:
     - **AC-for-AC replacement** (no heat pump anywhere in the option) → **HEEHRA does not apply at all.** HEEHR funds heat pumps and other electric appliances; it has never funded a straight AC changeout. Do not put a HEEHRA line on an AC option.
     - **Heat pump replacing an existing fossil furnace / boiler, fossil equipment removed** (full electrification) → **HEEHRA does NOT apply.** This is the fuel switch DOE pulled out of the program. Say so plainly on the option; do not hedge it as "may qualify."
     - **Dual-fuel / hybrid — heat pump added, existing fossil furnace retained as backup** → **HEEHRA may apply.** The fossil system stays, so this is not a fuel switch. This is now frequently the *only* HEEHR-eligible option in a gas-heated home, and the report should say that out loud rather than letting the customer infer it.
     - **Electric-to-electric** (heat pump or high-efficiency electric replacing existing electric resistance / an old heat pump) → **HEEHRA may apply.** Not a fuel switch.
     - In all "may apply" cases the figure still carries the AMI test and the "as of [date] — your state is revising its program" caveat. Route the dollar figure to `rebate-and-tax-credit-navigator.md`; this skill states eligibility posture, not the final number.
   - *Geothermal installs*: federal 25D is **terminated for expenditures after 12/31/2025** (OBBBA, July 2025). It applies only to systems paid and placed in service by 12/31/2025 (30% of project cost, no cap). For 2026+ geothermal installs, do not include 25D — pivot to state/utility incentives and, for commercial, Section 48 ITC via the navigator skill.
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

**Output format — commercial-portfolio:**

For multi-property accounts (HOAs, REITs, property-management companies, school districts, municipal facilities). Replaces the single-property structure with an entity-level roll-up:

```
[COMPANY NAME] — PORTFOLIO ENERGY SAVINGS ANALYSIS
Prepared for: [Entity name]    CRM record: [config.crm_record_id]
Filing entity: [LLC / S-corp / C-corp / non-profit / municipal]
Properties in scope: [N]    Total sqft: [X,XXX,XXX]
Date: [Today's date]    Modeled by: [Advisor name] (third-party validator: [config.energy_modeling_partner or "n/a"])

PORTFOLIO BASELINE
------------------
Total annual energy spend: $[XXX,XXX]  (current portfolio)
Portfolio OpEx per sqft: $[X.XX]/sqft/yr  (current)

Per-property baseline:
| Property | Sqft | Equipment age | Current SEER2 / efficiency | Annual energy $ | $/sqft | Source |
|----------|------|---------------|----------------------------|-----------------|--------|--------|
| [property_id] | [sqft] | [yrs] | [eff] | $[XX,XXX] | $[X.XX] | [bill / modeled] |
…

PROPOSED PORTFOLIO UPGRADE
--------------------------
[Brief description of equipment standardization across portfolio — e.g., "All RTUs upgraded to 14.8 IEER R-454B over 3 years, prioritized by age + condition + tenant impact."]

Per-property savings projection:
| Property | New equipment | Year of replace | Annual savings | Net investment | Simple payback | 10-yr op savings |
|----------|---------------|-----------------|-----------------|----------------|----------------|------------------|
| [property_id] | [eq] | Y[1/2/3] | $[X,XXX] | $[XX,XXX] | [X.X yr] | $[XX,XXX] |
…

PORTFOLIO ROLL-UP
-----------------
Total project cost (all 3 years): $[X,XXX,XXX]
Cash incentives (utility + manufacturer rebates):                     −$[XX,XXX]
179D deduction: $[X.XX]/sqft (PW/A-enhanced band) × [sqft] = $[XXX,XXX] deduction,
  CAPPED at project cost, × [entity marginal rate ~21%]           ≈ −$[XXX,XXX] cash-equivalent
Section 48 ITC:  $0 — [state why: air-source RTUs are not §48 energy property]
                       [only populate this line for geothermal / ground-source / CHP scope]
Total cash-equivalent incentives:                                    −$[XXX,XXX]   (see Rebate & Tax Credit Navigator commercial-portfolio output)
Net portfolio investment: $[X,XXX,XXX]
Annual energy savings at full deployment: $[XX,XXX]/yr
Portfolio OpEx per sqft after deployment: $[X.XX]/sqft/yr  (down from $[X.XX])
Simple payback at portfolio level: [X.X years]
10-year operating savings: $[XXX,XXX]
15-year operating savings: $[XXX,XXX]

CAPEX BY QUARTER (proposed rollout)
-----------------------------------
| Year/Q | Properties | CapEx | Cumulative CapEx | Cumulative annual savings |
| Y1Q1 | [list] | $[XXX,XXX] | $[XXX,XXX] | $[XX,XXX] |
…

CAPEX-VS-OPEX SUMMARY
---------------------
| Horizon | Cumulative CapEx (cash + financed) | Cumulative operating savings | Net cash position |
| Year 3 | $[X,XXX,XXX] | $[XXX,XXX] | $([XXX,XXX]) |
| Year 5 | $[X,XXX,XXX] | $[XXX,XXX] | $([XXX,XXX]) |
| Year 10 | $[X,XXX,XXX] | $[X,XXX,XXX] | $[XXX,XXX] |

WHAT YOUR CFO / ACCOUNTANT WILL SEE
-----------------------------------
We encourage you to share this report with your finance team and run the numbers. Here's what an honest review will see and say:
1. Portfolio OpEx-per-sqft baseline ($[X.XX]/sqft/yr): [one sentence — e.g., "In line with BOMA 2025 benchmark for [building class] in [climate zone]."]
2. Per-property savings methodology: [one sentence — "Uses (1 − old/new) efficiency ratio plus IEER-vs-SEER2 conversion for RTU; consistent with ASHRAE 90.1-2019 baseline assumptions used by the 179D modeler."]
3. Incentive posture: [one sentence — "179D taken at the PW/A-**enhanced** $2.50–$5.00/sqft band (that band is already the ~5×-multiplied figure — no further multiplier is applied on top of it), capped at project cost and reported at its ~21% cash-equivalent value because it is a deduction and not a credit; Section 48 ITC is $0 on air-source RTU scope and is only populated for geothermal / CHP property; all handed to the navigator for accountant-facing detail and CPA confirmation."]
4. Payback horizon: [one sentence — "[X.X]-year simple payback at portfolio level on a 15+ year useful-life standardization is in the defensible range for a [filing entity] under 2026 equipment pricing."]

If your CFO or accountant pushes back on any of the four, call me at [phone] and I'll bring the per-property model and the navigator output to the next finance review.

ASSUMPTIONS, MAPPING GAPS, AND VALIDATOR HAND-OFF
-------------------------------------------------
- Per-property energy spend marked `[MODELED — VERIFY WITH UTILITY ACCOUNT]`: [list]
- Properties on R-410A (refrigerant-transition note triggered): [list]   (cross-link `customer-service/a2l-refrigerant-explainer.md`)
- Tenant-impact / vacancy-window assumption per property: [list]
- 179D modeler / Section 48 PW/A confirmation: [config.energy_modeling_partner or "PENDING"]
- _mapping_gaps: [array of fields the entity needs to confirm before this becomes a proposal — utility account IDs, current capital-plan amounts, lease pass-through structure, etc.]

For the entity-level federal stack (179D + Section 48 + §6417 elective-pay) and per-property utility / manufacturer / demand-response stacking, see the commercial-portfolio output of `rebate-and-tax-credit-navigator.md`.

[Company name] • [phone] • [website] • license #[X]
```

**Output format — verbal summary:**

A 60-90 second spoken-voice track. Lead with the annual savings number. Give the payback horizon. Name the incentives that apply. Close with: "Want me to send you the full report with the math? I'll have it in your inbox in the next hour."

**TOU / time-of-use overlay (when on):**

When the customer's tariff is time-of-use, add a separate "TOU pre-cool / pre-heat schedule savings" line to the savings table for each option that includes a smart thermostat or heat-pump variable-speed equipment:
- Annual TOU savings = (on-peak kWh shifted to off-peak per day) × (on-peak rate − off-peak rate) × cooling/heating days
- Use a default 30% of cooling kWh shiftable / 20% of heating kWh shiftable for variable-speed heat pumps with a smart thermostat unless the user provides a measured shift figure
- Flag as "estimate — depends on schedule the homeowner actually keeps; revisit at first-year tune-up"
- For tiered tariffs, calculate baseline-tier-creep savings instead (load reduction keeps the customer in a lower kWh tier longer)
- For demand-charge tariffs (more common on light-commercial and commercial-portfolio), add a "peak-demand-kW reduction × demand charge × 12 months" line and note that the realized savings depends on whether the new equipment actually reduces peak-coincident demand (variable-speed wins; single-stage at full load does not)

**Quality standards:**

- Always show your math — customers and salespeople need to trust the numbers.
- Round savings to the nearest $25 to avoid false precision (nearest $250 on commercial-portfolio totals).
- If estimating efficiency from age, clearly note "estimated" and the basis.
- Never guarantee exact savings — use "estimated" and "projected" language.
- Include the Assumptions section so the report has credibility.
- If data is insufficient for a reliable estimate, flag with `[ESTIMATE — VERIFY WITH UTILITY BILLS]`.
- **Never claim the federal 25C credit for installs completed after 12/31/2025.** It expired. Use HEEHRA / HOMES / utility / manufacturer stacks instead — subject to the HEEHR gate below.
- **Never put a HEEHRA figure on an AC-only option, or on a heat-pump option that removes a fossil furnace.** HEEHR does not fund AC-for-AC changeouts, and 2026 DOE guidance removed fuel switching from the program. In a gas-heated home the dual-fuel option (furnace retained) is typically the only HEEHR-eligible one in the set — say that explicitly instead of leaving the customer to assume the $8,000 applies to whichever option they like best. A savings report that books $8,000 against a furnace-removal option is the fastest way to get a signed proposal unwound at rebate-denial time.
- **Never show a heat pump saving money on *heating* against an existing gas furnace without checking the local fuel-cost spread.** In most 2026 gas markets, delivered heat from a ~2.5-COP heat pump costs *more* per MMBtu than from an 80% AFUE furnace. Heat-pump savings in those markets come from the cooling side and from dual-fuel switchover logic. Model it; don't assume it.
- Always include the "What your AI check will see" block in the homeowner report and the "What your CFO / accountant will see" block in the commercial-portfolio report. This is the 2026-specific piece that makes the report survive the customer's own LLM (or accountant's) check.
- For any incentive dollar figure, always note "estimate — confirm with current program terms" unless sourced from a point-of-sale program the contractor controls.
- When the proposed option is a heat pump in California, reference `knowledge-base/regulations/california-2026-code.md` for the prescriptive default and commissioning requirements.
- On commercial-portfolio mode, every property must have an `equipment_age` flag and an `r410a_present` flag. If `r410a_present = true` for any property, the refrigerant-transition note is mandatory and a cross-link to `customer-service/a2l-refrigerant-explainer.md` is required.
- On commercial-portfolio mode, never omit the `_mapping_gaps` array — that's the field the property manager's CFO uses to plan the data-collection sprint before the proposal goes to bid.
- Hand off to the navigator skill for the accountant-facing 179D / Section 48 / §6417 detail; do not duplicate the navigator's commercial-portfolio output here.

## Example Output

Given input: *"Mrs. Garcia, 1,800 sqft home in Denver (ZIP 80202). Current: 15-year-old Carrier furnace (80% AFUE) + 10 SEER AC. Proposing Good: Carrier Performance 25HCC6 (17 SEER2), Better: Carrier Performance 17 SEER2 heat pump, Best: Carrier Infinity 24VNA6 (up to 24 SEER2) heat pump with gas backup. Xcel Energy: $0.14/kWh, $0.85/therm. Install timing: under contract, install June 2026. Household income: ~$95K (family of 4). Output: homeowner report."*

The report would show:
- Current annual cost: ~$2,400 (modeled from sqft + Denver CDD/HDD; flagged as verify-against-bill)
- Option A (17 SEER2 AC, gas furnace retained): ~$325/year savings, ~9-year payback (no federal 25C post-12/31/2025; **no HEEHRA line — HEEHR does not fund an AC-for-AC changeout at all**; utility rebate + manufacturer promo applied)
- Option B (17 SEER2 heat pump, gas furnace removed — full electrification): ~$300–$400/year savings, payback deliberately shown as a range because the *heating* side of this swap is not automatically a saving in Denver (see the fuel-cost reality check below). **No HEEHRA line: removing the furnace is a fuel switch, which 2026 DOE guidance excludes from HEEHR.** The report says that in one plain sentence rather than omitting it silently, because the customer will otherwise assume the $8,000 is on the table.
- Option C (24 SEER2 heat pump with the existing gas furnace retained as backup — dual-fuel): ~$450–$600/year savings, ~7-year payback after the full stack — and this is **the only option in the set that HEEHRA can pay for** (fossil system retained ⇒ not a fuel switch), at the 50%-up-to-$8,000 / 80–150% AMI tier for **Denver County** (family of 3 at ~$95K). Lead with this. The incentive rule and the engineering both point the same direction here, which is the report's headline finding.
- **Fuel-cost reality check the report must show, not bury** (this is the number that makes or breaks trust on a heat-pump recommendation in a gas market): at Xcel's Denver 2026 residential rates, gas at ~$0.85/therm and 80% AFUE delivers useful heat at roughly **$10–$11 per MMBtu**, while electricity at ~$0.14/kWh through a heat pump at a seasonal COP of ~2.5 delivers it at roughly **$16–$17 per MMBtu**. **Heating on the heat pump costs more per delivered BTU than the existing gas furnace at these rates.** So the savings on Options B and C come from the *cooling* side (and, on C, from running the heat pump only above its balance point and letting gas carry the cold hours) — not from a heating win. Any report that shows a heat pump beating a gas furnace on *heating* cost in this rate environment is wrong, and an AI second-pass will say so.
- AI-validation block correctly notes 25C is *not* included (install is post-12/31/2025), states the HEEHRA fuel-switching gate and why Option C is the eligible one, and defends the payback math with the (1 − old/new) efficiency methodology — including the SEER→SEER2 conversion (the existing unit's 10 SEER is converted at ≈×0.95 before it is compared against a 17 SEER2 rating, so the ratio is computed on like units)
- Assumptions section cites Xcel's 2026 residential rate, Denver climate zone, and links to the `rebate-and-tax-credit-navigator.md` output for the full stacked-incentive breakdown

### Commercial-portfolio worked example

Given input: *"Sunbelt Property Management LLC, 6 garden-style apartment buildings in Phoenix metro (ZIP 85016, 85254, 85283), total 312,000 sqft. Current equipment: 18 RTUs (3-ton to 7.5-ton), all R-410A, ages 9–17 years, mixed Carrier + Trane + Lennox. Current portfolio annual energy spend (utility-account verified): $487,000. Filing entity: LLC taxed as S-corp, PW/A confirmed for the projected install crew. config.energy_modeling_partner: Sustainability Solutions Group. config.crm_record_id: SBM-2026-0412. APS commercial tariff is TOU with demand charges. Proposed: standardize on Carrier WeatherExpert 14.8 IEER R-454B over 3 years (Y1: 8 oldest RTUs at 4 properties, Y2: 6 RTUs at 2 properties, Y3: 4 newest-but-still-aging RTUs across portfolio). Output: commercial-portfolio."*

The report would produce:
- Portfolio baseline: $487K total, $1.56/sqft/yr OpEx (with BOMA Phoenix 2025 benchmark referenced as $1.49–$1.72/sqft/yr for garden-style multi-family at this vintage so the figure is calibrated)
- Per-property baseline table with all 6 properties, sqft, equipment age range, current SEER2-equivalent IEER, annual $ (utility-account verified, no `[MODELED]` flags), $/sqft per property, surfacing that two properties run at $1.81–$1.93/sqft/yr (the worst — and the prioritized Y1 targets)
- Per-property savings projection with each of the 18 RTUs scheduled into Y1/Y2/Y3, annual savings per RTU calculated from (1 − old IEER / new 14.8 IEER) × current cooling kWh × $/kWh, plus the TOU demand-charge-reduction line for variable-capacity equipment
- Portfolio roll-up (every figure below reconciles — the arithmetic is shown because a property manager *will* check it): total project **~$645K** (12 RTUs at the Better tier ≈ $390K, 6 at the Best tier with VFD + BMS integration ≈ $255K — i.e. roughly $32K–$43K per installed RTU including curb adapter, crane, controls and commissioning, which is the band a 3–7.5-ton commercial changeout actually lands in); annual energy savings at full deployment **~$71K/yr** (HVAC is ~45% of the $487K spend; going from a blended ~10.8 IEER to 14.8 IEER is a ~27% cut on that portion ≈ $59K, plus ~$12K of TOU demand-charge reduction from the variable-capacity units); portfolio OpEx **$1.56 → $1.33/sqft/yr**; **simple payback ~9.1 years pre-incentive, ~6.7 years post-incentive**
- Incentive stack, stated in **cash-equivalent** terms and handed to the navigator for CPA-facing detail — the three rules this block exists to enforce:
  - **179D is a deduction, not a credit.** The PW/A-**enhanced** rate is the $2.50–$5.00/sqft band (that band is *already* the multiplied figure — do **not** apply a further 5× to it; base is the ~$0.50–$1.00/sqft band). And **179D is capped at the cost of the qualifying property**, so on a $645K project the deduction caps at $645K, not at $2.50 × 312K sqft = $780K. Cash value ≈ $645K × the entity's ~21% marginal rate ≈ **$135K**.
  - **Section 48 ITC does not apply here at all.** These are packaged air-source rooftop units; §48 energy property means geothermal/ground-source, CHP and similar. A 6%/30% ITC line on an air-source RTU portfolio is a hard error a CPA will strike. Removed from this example on purpose.
  - Cash incentives that *do* apply: APS commercial RTU rebate ≈ $27K (18 units) + Carrier commercial promo ≈ $7K. **Total cash-equivalent stack ≈ $170K; net portfolio investment ≈ $475K.**
- ⚠️ **179D timing flag on a 3-year rollout:** OBBBA terminates 179D for property whose construction begins after the 2026 cutoff. A Y1 start may qualify while the **Y2 and Y3 tranches may not** — the report must flag this rather than modeling the deduction flat across all three years, because it is the single largest line in the stack and the one most likely to evaporate.
- CapEx-by-quarter table sequencing the 8 / 6 / 4 RTU replacements across 12 quarters with cumulative CapEx and cumulative annual savings columns, respecting the stated CapEx ceiling
- CapEx-vs-OpEx summary showing the cumulative cash position turning positive in **Y7** on the post-incentive basis (~$475K net ÷ ~$71K/yr ≈ 6.7 years) — and stating the pre-incentive figure (~9.1 years) alongside it, so the manager sees what the incentives are actually buying
- "What your CFO will see" block calibrated to the BOMA benchmark, the ASHRAE 90.1-2019 baseline for the 179D modeler hand-off, and the explicit CPA-confirm caveat on the 179D cap, rate band, and begin-construction cutoff
- Mapping gaps: lease pass-through structure (does the property absorb savings or pass to tenants?), prior capital-plan amounts pulled from `config.crm_record_id`, Sustainability Solutions Group sign-off on the ASHRAE 90.1-2019 baseline model, energy-community designation per property
- Refrigerant-transition note triggered (all 18 RTUs are R-410A) with cross-link to `a2l-refrigerant-explainer.md` for the property-manager-facing tenant-communications sequence
