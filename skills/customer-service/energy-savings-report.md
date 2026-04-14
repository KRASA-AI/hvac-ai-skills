---
name: "Energy Savings Report"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 2.0
last_eval_score: null
---

# 💡 Energy Savings Report

## Purpose

Generate a customer-facing report showing projected energy and cost savings from upgrading their HVAC system — with real numbers, payback timelines, and available rebates. Designed to help close upgrade proposals by giving homeowners a clear financial picture.

## When to Use

- After completing a site visit where a system replacement or upgrade was discussed
- When a customer asks "how much will I save with a new system?"
- As a companion document to a sales proposal (pairs well with `proposal-generator`)
- When presenting efficiency upgrade options to a commercial property manager
- During seasonal marketing pushes for high-efficiency equipment

## Required Input

Provide as much of the following as available:

1. **Current system** — Make, model, age, and efficiency rating (SEER/SEER2, AFUE, HSPF/HSPF2) of the existing equipment. If the rating is unknown, provide the age and type and the skill will estimate.
2. **Proposed system(s)** — Make, model, and efficiency rating of the recommended replacement(s). If comparing tiers (good/better/best), list each.
3. **Home/building details** — Approximate square footage, location (for climate zone), and any known insulation or envelope issues.
4. **Utility costs** (if available) — Customer's average monthly electric and/or gas bill, or local utility rate per kWh / therm.
5. **Rebates/incentives** (if known) — Any manufacturer, utility, or federal tax credit incentives that apply.

## Instructions

You are an HVAC energy consultant preparing a customer-facing savings analysis. Your job is to translate equipment efficiency ratings into real dollar savings the homeowner can understand.

**Before you start:**
- Load `config.yml` from the repo root for company details, service area, and branding
- Reference `knowledge-base/terminology/` for correct HVAC and energy efficiency terms
- Use the company's communication tone from `config.yml` → `voice`

**Calculation methodology:**

1. **Determine baseline efficiency** — Use the current system's SEER (cooling) and AFUE or HSPF (heating) rating. If unknown, estimate based on age:
   - Pre-2006: ~10 SEER, ~80% AFUE
   - 2006–2015: ~13 SEER, ~80–90% AFUE
   - 2015–2023: ~14–16 SEER, ~90–96% AFUE
   - Post-2023 (SEER2): Convert using SEER2 ≈ SEER × 0.95

2. **Estimate annual energy usage** — Use regional cooling/heating degree days and approximate load:
   - Cooling cost estimate: (Square footage × 1.0 BTU/hr per sq ft × Cooling hours) / (SEER × 1000) × electricity rate
   - Heating cost estimate: (Square footage × heating load factor × Heating hours) / (AFUE × therms conversion) × gas rate
   - If customer provided actual utility bills, use those as the baseline instead

3. **Calculate savings for each proposed option** — Apply the efficiency improvement ratio:
   - Cooling savings: Baseline cooling cost × (1 − old SEER / new SEER)
   - Heating savings: Baseline heating cost × (1 − old AFUE / new AFUE)
   - Total annual savings = cooling savings + heating savings

4. **Factor in incentives** — Include applicable:
   - Federal tax credits (25C credit: up to $2,000 for qualifying heat pumps, $600 for qualifying AC/furnace)
   - Utility rebates (check local utility programs for the service area in config)
   - Manufacturer rebates (seasonal promotions)

5. **Calculate payback period** — (Equipment cost − total incentives) / annual savings = years to payback

**Output format:**

```
ENERGY SAVINGS ANALYSIS
========================
Prepared for: [Customer name]
Prepared by: [Company name from config]
Date: [today's date]
Property: [address / description]

CURRENT SYSTEM
--------------
Equipment: [make/model/age]
Cooling efficiency: [X SEER/SEER2]
Heating efficiency: [X AFUE or X HSPF]
Estimated annual energy cost: $[X,XXX]

PROPOSED UPGRADE OPTIONS
------------------------

[For each option:]

### Option [A/B/C]: [Equipment description]
Cooling efficiency: [X SEER2]
Heating efficiency: [X AFUE / HSPF2]
Estimated annual energy cost: $[X,XXX]
Annual savings: $[XXX]
Equipment investment: $[X,XXX]

Available incentives:
- [Federal tax credit]: −$[XXX]
- [Utility rebate]: −$[XXX]
- [Manufacturer rebate]: −$[XXX]
Net investment after incentives: $[X,XXX]

Simple payback period: [X.X years]
10-year net savings: $[X,XXX]
15-year net savings: $[X,XXX]

SAVINGS COMPARISON SUMMARY
---------------------------
[Side-by-side table comparing all options: annual savings, net cost, payback, 10-year savings]

ADDITIONAL BENEFITS
-------------------
- [Improved comfort / humidity control / noise reduction]
- [Better air filtration / indoor air quality]
- [Smart thermostat compatibility]
- [Increased home resale value]
- [Reduced carbon footprint: ~X,XXX fewer lbs CO2/year]

ASSUMPTIONS & NOTES
--------------------
- Electricity rate: $[X.XX]/kWh (based on [source])
- Gas rate: $[X.XX]/therm (based on [source])
- Cooling degree days: [X,XXX] (based on [city/climate zone])
- Heating degree days: [X,XXX] (based on [city/climate zone])
- Calculations are estimates. Actual savings depend on usage, weather, and home characteristics.

[Company name] • [phone] • [website]
"[Company tagline or closing from config voice]"
```

**Quality standards:**
- Always show your math — customers and salespeople need to trust the numbers
- Round savings to nearest $25 to avoid false precision
- If estimating efficiency from age, clearly note "estimated" and the basis
- Never guarantee exact savings — use "estimated" and "projected" language
- Include the assumptions section so the report has credibility
- If data is insufficient for a reliable estimate, flag it with [ESTIMATE — VERIFY WITH UTILITY BILLS]

## Example Output

Given input: *"Mrs. Garcia, 1,800 sq ft home in Denver. Current system is a 15-year-old Carrier furnace (80% AFUE) and 10 SEER AC. Proposing a Carrier Infinity 24VNA6 (up to 24 SEER2) heat pump with gas backup. Also considering mid-range option: Carrier Performance 25HCC6 (17 SEER2). Xcel Energy charges about $0.14/kWh and $0.85/therm."*

The report would show:
- Current estimated annual cost: ~$2,400 (heating + cooling)
- Option A (17 SEER2): ~$525/year savings, ~7-year payback after incentives
- Option B (24 SEER2 heat pump): ~$875/year savings, ~6-year payback after $2,000 federal tax credit
- 10-year net savings comparison clearly favoring the heat pump option
