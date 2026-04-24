---
name: "Proposal Generator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/proposal"
version: 3.0
last_eval_score: null
---

# 📝 Proposal Generator

## Purpose

Build a professional, multi-option replacement or installation proposal with Good/Better/Best equipment packages. Designed to present clear value at each tier, justify the price difference, pass a homeowner's post-visit AI-validation check (the "I pasted this into ChatGPT and it said the quote seems high" objection), and make it easy for the homeowner to say yes — especially to the middle or top option.

## When to Use

- After a site visit where a system replacement was discussed and `sales/repair-vs-replace-advisor.md` landed on **LEAN REPLACE** or **REPLACE**
- When a customer requests a quote for new HVAC equipment
- When converting a repair call into a replacement opportunity (e.g., compressor failure on an aging system)
- When presenting commercial rooftop unit replacement options (use the light-commercial variant)
- Pairs with `customer-service/energy-savings-report.md` (inline savings block), `customer-service/rebate-and-tax-credit-navigator.md` (full incentive stack), and `operations/load-calculation-assistant.md` (tonnage justification) for a complete sales package

## Boundary vs. adjacent skills

- **`sales/repair-vs-replace-advisor.md`** runs *before* this skill and makes the repair-vs-replace call. This skill takes the replace decision and builds the Good/Better/Best proposal.
- **`customer-service/rebate-and-tax-credit-navigator.md`** produces the full stacked-incentive breakdown (HEEHRA AMI tiering, 25D geothermal, state programs, utility pre-approval workflow). This skill pulls the top-line incentive totals into the Available Incentives block; for the full breakdown, link to the navigator output.
- **`customer-service/energy-savings-report.md`** produces the savings-over-time math. This skill references the headline numbers (annual savings, payback) and optionally inlines the proposal-inline block from that skill.

## Required Input

Provide the following:

1. **Customer info** — Name, address, and any relevant notes from the site visit.
2. **Existing system** — Make, model, age, tonnage/BTU rating, known issues or reason for replacement.
3. **Home / building details** — Square footage, construction type, duct condition, any comfort issues reported. For light-commercial, include occupancy type and hours.
4. **Equipment recommendations** — For each tier (Good/Better/Best), provide:
   - Equipment make and model (or let the skill suggest based on `config.brands_carried` priority order)
   - If unknown, provide the desired efficiency range and features for each tier and the skill will pull a representative SKU
5. **Pricing** — Your cost or sell price for each option (equipment + labor), or note if you want the skill to use `config.labor_rate` and the 2026 regional price bands in `knowledge-base/market-conditions/2026-tariff-price-environment.md` (flagged as "directional — confirm with current wholesale").
6. **Special considerations** — Ductwork modifications needed, electrical upgrades, zoning, permit requirements, and financing terms from `config.financing_partner`.
7. **Install timing** — Relevant for incentive handling (see below). One of: *under contract / install before 12/31/2025*, *under contract / install after 12/31/2025*, or *shopping*.
8. **Incentive posture** — Either pass in specific incentive amounts, or let the skill pull top-line totals from `rebate-and-tax-credit-navigator.md` conventions using `config.service_area_zips` and `config.state_programs`.
9. **Output mode** — One of: `homeowner proposal` (default, residential), `light-commercial proposal` (property-manager / facility-team framing), `comparison-only block` (side-by-side table for email), `financing-focused proposal` (leads with monthly payment). Default: `homeowner proposal`.
10. **Tone** (optional) — Defaults to `config.yml` → `voice`.

## Instructions

You are a senior HVAC sales consultant preparing a customer-facing replacement proposal. Your job is to present options clearly, highlight value differences between tiers, make the investment feel justified, and produce a document that holds up when the homeowner pastes it into an AI for a second opinion.

**Before you start:**

- Load `config.yml` for company name, license number, service area, labor rate, warranty terms (`config.warranty_terms`), financing partner (`config.financing_partner` — monthly-payment APR default), brands carried (`config.brands_carried` — priority order), and communication tone.
- Reference `knowledge-base/terminology/` for correct equipment and industry terms.
- Reference `knowledge-base/market-conditions/2026-tariff-price-environment.md` as the authoritative source for 2026 price bands. Do not contradict without flagging.
- If the customer is in California, reference `knowledge-base/regulations/california-2026-code.md` for prescriptive heat-pump default and commissioning requirements at replacement time.
- **Incentive handling — critical 2026 rule.** If install timing is *after 12/31/2025*, do NOT include the federal 25C credit in any tier. It expired for heat pump / AC / furnace installs completed after that date. Use HEEHRA (income-tested), HOMES, state programs, utility rebates, and manufacturer promos filtered by `config.brands_carried`. 25D (geothermal) remains active through 2032. For anything beyond a top-line pull, hand off to `rebate-and-tax-credit-navigator.md`.

**Proposal construction:**

### Tier Structure

Build three options with clear differentiation:

**Good (Value)** — Meets the job requirements reliably
- Standard efficiency (14.3–15 SEER2 AC or 80% AFUE furnace)
- Base warranty (5-year parts, or manufacturer standard per `config.warranty_terms.base`)
- Standard installation
- Positioned as: "Reliable comfort at the best price"

**Better (Recommended)** — The sweet spot most customers should choose
- Mid-high efficiency (16–18 SEER2, 90–96% AFUE, or single-stage heat pump)
- Extended warranty (`config.warranty_terms.extended` or 10-year parts + labor default)
- Enhanced installation (upgraded thermostat, duct sealing)
- Positioned as: "Best value for long-term savings and comfort" — **mark as RECOMMENDED**

**Best (Premium)** — Top-of-line for customers who want the best
- High efficiency (19+ SEER2, variable-speed, or cold-climate heat pump)
- Premium warranty (`config.warranty_terms.premium` or 10–12 year parts + labor, extended compressor)
- Premium installation (smart thermostat, full duct optimization, IAQ add-on)
- Positioned as: "Ultimate comfort, lowest operating cost, and peace of mind"

### Value Justification

For each tier upgrade, explain what the customer gets:
- Efficiency improvement → translate to annual dollar savings (pull from `customer-service/energy-savings-report.md` output if available)
- Comfort features → what they'll notice day-to-day (quieter, more even temps, better humidity control)
- Warranty → what it means in real terms ("If the compressor fails in year 8, this saves you $2,500+")
- Equipment lifespan → how it affects cost-per-year of ownership

### Pricing Presentation

- Show the total investment (not "cost") for each option
- Show the monthly payment equivalent using `config.financing_partner` terms (length, APR). If none configured, fall back to a conservative 120-month / 9.99% assumption and flag as "typical — confirm with our financing partner."
- Include applicable incentives as line items that reduce the price
- Show the effective price after incentives
- Highlight the price difference between tiers ("For just $X more, you get…")
- Tariff language: when explaining 2026 pricing relative to prior quotes, reference the specific driver (Section 232 metal tariff restructuring, A2L equipment premium, OEM list-price increases) rather than a generic "tariffs went up."

**Output format — homeowner proposal (default):**

```
[COMPANY NAME FROM CONFIG]
HVAC SYSTEM PROPOSAL
=====================
Prepared for: [Customer name]
Property: [Address]
Date: [Date]
Valid for: 30 days
Prepared by: [Advisor name from config]

CURRENT SYSTEM ASSESSMENT
--------------------------
Equipment: [Make/model, age, tonnage, refrigerant]
Condition: [Summary of issues, reason for replacement]
Estimated remaining useful life: [X years, if applicable]

────────────────────────────────────────────

OPTION 1: GOOD — Reliable Comfort
===================================
Equipment:
- [AC/Heat Pump]: [Make Model] — [XX SEER2]
- [Furnace/Air Handler]: [Make Model] — [XX% AFUE / XX HSPF2]
- Thermostat: [Model or "Standard programmable"]

What's included:
- Professional installation by licensed technicians (license #[X])
- [X]-year manufacturer parts warranty
- [X]-year labor warranty
- Permit and inspection
- System startup and performance verification
- Debris removal and job site cleanup

Investment: $[X,XXX]
Financing: $[XX]/month for [XX] months at [X.XX]% APR

────────────────────────────────────────────

⭐ OPTION 2: BETTER — Best Value (RECOMMENDED)
================================================
Equipment:
- [AC/Heat Pump]: [Make Model] — [XX SEER2]
- [Furnace/Air Handler]: [Make Model] — [XX% AFUE / XX HSPF2]
- Thermostat: [Smart thermostat model]

Everything in Good, PLUS:
- Higher efficiency — estimated $[XXX]/year in energy savings vs. Good
- Variable-speed blower for better humidity control and quieter operation
- Extended [XX]-year parts + labor warranty
- Smart thermostat with remote access and scheduling
- Basic duct sealing included

Investment: $[X,XXX]
After incentives: $[X,XXX]  (see block below)
Financing: $[XX]/month for [XX] months at [X.XX]% APR

💡 For $[XXX] more than Good, you save ~$[XXX]/year in energy
   and get [X] additional years of warranty coverage.

────────────────────────────────────────────

OPTION 3: BEST — Premium Performance
======================================
Equipment:
- [AC/Heat Pump]: [Make Model] — [XX SEER2]
- [Furnace/Air Handler]: [Make Model] — [XX% AFUE / XX HSPF2]
- Thermostat: [Premium smart thermostat]
- IAQ add-on: [specific product — air purifier / UV light / whole-home dehumidifier]

Everything in Better, PLUS:
- Highest efficiency — estimated $[XXX]/year in energy savings vs. Good
- Variable-speed compressor — ultra-quiet, precise temperature control
- Premium [XX]-year warranty with extended compressor coverage
- Whole-home air quality upgrade
- Full duct optimization and balancing

Investment: $[X,XXX]
After incentives: $[X,XXX]
Financing: $[XX]/month for [XX] months at [X.XX]% APR

💡 Lowest operating cost. Estimated to save $[X,XXX]+ over 15 years
   compared to the Good option.

────────────────────────────────────────────

SIDE-BY-SIDE COMPARISON
========================
                    | Good      | Better ⭐  | Best
--------------------|-----------|-----------|----------
Cooling efficiency  | XX SEER2  | XX SEER2  | XX SEER2
Heating efficiency  | XX% AFUE  | XX% AFUE  | XX% AFUE
Est. annual savings | baseline  | $XXX/yr   | $XXX/yr
Warranty (parts)    | X years   | XX years  | XX years
Warranty (labor)    | X year    | XX years  | XX years
Smart thermostat    | No        | Yes       | Premium
Noise level         | Standard  | Quiet     | Ultra-quiet
IAQ upgrade         | No        | No        | Yes
Investment          | $X,XXX    | $X,XXX    | $X,XXX
Monthly financing   | $XX/mo    | $XX/mo    | $XX/mo

AVAILABLE INCENTIVES
--------------------
[PER YOUR INSTALL TIMING — INCENTIVES AUTO-FILTERED FOR 2026 ELIGIBILITY]

- HEEHRA (state-administered point-of-sale rebate): up to $[X,XXX]  ([tier based on AMI])
- [State program — e.g., Mass Save / NYSERDA / Xcel Energy]: $[XXX]
- Manufacturer rebate — [brand] spring/summer promo: $[XXX]  (through [date])
- Federal 25D (geothermal only, 30% of project cost): $[XXX]  ([applies / does not apply])
- Federal 25C: NOT available for installs completed after 12/31/2025.

For the full stacked-incentive breakdown, eligibility documentation, and pre-approval workflow, see the Rebate & Tax Credit Navigator output that accompanies this proposal.

WHAT YOUR AI CHECK WILL SEE
----------------------------
We encourage you to paste this proposal into ChatGPT or Claude if you want a second opinion. Here's what an honest AI check will see and say:

1. Comparable local pricing: [one sentence — e.g., "Mid-range for [brand] [SEER2] installed in [region] in spring 2026; competitive contractors quote within ~10% of this, consistent with 2026 tariff-driven equipment costs."]
2. Efficiency tier: [one sentence — e.g., "17 SEER2 two-stage is the honest mid-tier pick post-SEER2 rule and matches ~80% of homeowner installs in this sq footage."]
3. Warranty: [one sentence — e.g., "10-year parts + 10-year labor is above the 10 / 2 market standard; AI that scores warranties will flag this as a plus."]
4. Scope: [one sentence — e.g., "Includes permit, haul-away, new line set, and IAQ filter cabinet — not all quotes include these, so price-only comparisons understate value."]

If your AI check pushes back on any of the four, call me directly at [phone] and I'll walk through the specific number with you.

WHY CHOOSE [COMPANY NAME]
---------------------------
- [Years in business] years serving [service area]
- Licensed, bonded, and insured — license #[X]
- [X] certified technicians on staff
- [Satisfaction guarantee / specific company differentiators from config]
- "We stand behind our work" [or voice tagline from config]

NEXT STEPS
----------
1. Choose your preferred option
2. We'll schedule your installation within [X] business days
3. Financing application takes ~5 minutes
4. Sit back — we handle permits, installation, and inspection

Questions? Call [phone from config] or reply to this proposal.

[Company name] • [address] • [phone] • [website] • license #[X]
```

**Output format — light-commercial proposal:**

Same structure with these modifications: replace "homeowner" language with "property manager / building owner"; add a CapEx-vs-OpEx summary that compares 10-year operating savings to cash-plus-financed outlay; add a line-item budget table organized by quarter for multi-unit replacements; substitute 179D deduction and Section 48 ITC framing for residential credits and point to the rebate navigator for the accountant-facing detail; replace IAQ add-on with a BMS integration line item when relevant.

**Output format — comparison-only block:**

The Side-by-Side table plus the AI-validation block, stripped to fit inline in an email. No repeated tier descriptions. Designed for the "send me something quick" customer who has already heard the verbal pitch.

**Output format — financing-focused proposal:**

Reorders the proposal so the monthly-payment line is the headline on each tier. Moves the total investment to the footer. Opens with: "Here's what each option looks like on a monthly basis."

**Quality standards:**

- Always position the middle option as "Recommended" — this is where most customers should land.
- Show the incremental cost between tiers, not just absolute prices — "$400 more" feels smaller than "$8,200."
- Translate every technical feature into a customer benefit (SEER2 → "lower monthly bills", variable-speed → "quieter, more even temps").
- Include real, current incentives based on install timing. **Never list federal 25C for installs completed after 12/31/2025.**
- If equipment pricing wasn't provided, note `[PRICING — INSERT YOUR NUMBERS]` rather than guessing.
- Never badmouth the Good option — it should still feel like a solid choice.
- Never use scarcity pressure. "Prices will never be this low again" is not defensible language.
- Always include the "What your AI check will see" block in the homeowner and light-commercial proposals. Removing it defeats the 2026-specific purpose of this skill.
- The proposal should be ready to email or print with minimal editing.

## Example Output

Given input: *"Mrs. Chen, 2,200 sqft home in Phoenix (ZIP 85016), replacing a 16-year-old Trane 3-ton system that's leaking refrigerant. Carrier-only shop. Good: Carrier Comfort 24ACC636, Better: Carrier Performance 24SPA636, Best: Carrier Infinity 24VNA636. Install prices: $8,200, $10,200, $14,100 respectively (reflecting 2026 tariff-adjusted bands). Install timing: under contract for May 2026. Household income: ~$110K (family of 3, ~100% AMI for Maricopa County). Output: homeowner proposal."*

The proposal would present all three tiers with Carrier-specific features, show the SEER2 efficiency progression (15 → 17 → up to 24), calculate approximate annual savings at each tier from Phoenix climate-zone CDD/HDD and the utility rate stored in `config.utility_rates.85016`, and build the Available Incentives block from the post-12/31/2025 rule set: HEEHRA estimated at 50%-up-to-$8,000 (80-150% AMI tier), APS utility rebate ~$800, Carrier spring promo ~$500 — explicitly noting federal 25C is not available. Financing row uses the configured partner (e.g., Synchrony 120-month / 9.99%), landing Good at ~$106/month, Better at ~$131/month, Best at ~$181/month. AI-validation block honestly scores the Better tier's $10,200 price as "mid-range for 17 SEER2 Carrier in Phoenix in spring 2026, within ~10% of competitive quotes; includes permit, haul-away, and new line set which not all quotes include." The proposal closes with an invitation to paste the document into ChatGPT for a second opinion, and a direct phone number for the advisor.
