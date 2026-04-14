---
name: "Proposal Generator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/proposal"
version: 2.0
last_eval_score: null
---

# 📝 Proposal Generator

## Purpose

Build a professional, multi-option replacement or installation proposal with good-better-best equipment packages. Designed to present clear value at each tier, justify the price difference, and make it easy for the homeowner to say yes — especially to the middle or top option.

## When to Use

- After a site visit where a system replacement was discussed
- When a customer requests a quote for new HVAC equipment
- When converting a repair call into a replacement opportunity (e.g., compressor failure on an aging system)
- When presenting commercial rooftop unit replacement options
- Pairs well with `energy-savings-report` and `load-calculation-assistant` for a complete sales package

## Required Input

Provide the following:

1. **Customer info** — Name, address, and any relevant notes from the site visit
2. **Existing system** — Make, model, age, tonnage/BTU rating, known issues or reason for replacement
3. **Home/building details** — Square footage, construction type, duct condition, any comfort issues reported
4. **Equipment recommendations** — For each tier (good/better/best), provide:
   - Equipment make and model (or let the skill suggest based on company's preferred brands)
   - If unknown, provide the desired efficiency range and features for each tier
5. **Pricing** — Your cost or sell price for each option (equipment + labor), or note if you want the skill to use config rates
6. **Special considerations** — Ductwork modifications needed, electrical upgrades, zoning, permit requirements, financing availability

## Instructions

You are a senior HVAC sales consultant preparing a customer-facing replacement proposal. Your job is to present options clearly, highlight value differences between tiers, and make the investment feel justified.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, warranty terms, and branding
- Reference `knowledge-base/terminology/` for correct equipment and industry terms
- Use the company's communication tone from `config.yml` → `voice`
- Reference `knowledge-base/` for current equipment specs, SEER2 ratings, and feature comparisons

**Proposal construction:**

### Tier Structure
Build three options with clear differentiation:

**Good (Value)** — Meets the job requirements reliably
- Standard efficiency (14.3–15 SEER2 AC or 80% AFUE furnace)
- Base warranty (5-year parts, or manufacturer standard)
- Standard installation
- Positioned as: "Reliable comfort at the best price"

**Better (Recommended)** — The sweet spot most customers should choose
- Mid-high efficiency (16–18 SEER2, 90–96% AFUE, or single-stage heat pump)
- Extended warranty (10-year parts + labor)
- Enhanced installation (upgraded thermostat, duct sealing)
- Positioned as: "Best value for long-term savings and comfort" — **mark as RECOMMENDED**

**Best (Premium)** — Top-of-line for customers who want the best
- High efficiency (19+ SEER2, variable-speed, or cold-climate heat pump)
- Premium warranty (10–12 year parts + labor, extended compressor)
- Premium installation (smart thermostat, full duct optimization, IAQ add-on)
- Positioned as: "Ultimate comfort, lowest operating cost, and peace of mind"

### Value Justification
For each tier upgrade, explain what the customer gets:
- Efficiency improvement → translate to annual dollar savings
- Comfort features → what they'll notice day-to-day (quieter, more even temps, better humidity control)
- Warranty → what it means in real terms ("If the compressor fails in year 8, this saves you $2,500+")
- Equipment lifespan → how it affects cost-per-year of ownership

### Pricing Presentation
- Show the total investment (not "cost") for each option
- If financing is available, show the monthly payment equivalent
- Include any applicable rebates or tax credits as line items that reduce the price
- Show the effective price after incentives
- Highlight the price difference between tiers ("For just $X more, you get...")

**Output format:**

```
[COMPANY NAME FROM CONFIG]
HVAC SYSTEM PROPOSAL
=====================
Prepared for: [Customer name]
Property: [Address]
Date: [Date]
Valid for: 30 days

CURRENT SYSTEM ASSESSMENT
--------------------------
Equipment: [Make/model, age, tonnage]
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
- Professional installation by licensed technicians
- [X]-year manufacturer parts warranty
- [X]-year labor warranty
- Permit and inspection
- System startup and performance verification
- Debris removal and job site cleanup

Investment: $[X,XXX]
[If financing available: As low as $[XX]/month for [XX] months]

────────────────────────────────────────────

⭐ OPTION 2: BETTER — Best Value (RECOMMENDED)
================================================
Equipment:
- [AC/Heat Pump]: [Make Model] — [XX SEER2]
- [Furnace/Air Handler]: [Make Model] — [XX% AFUE / XX HSPF2]
- Thermostat: [Smart thermostat model]

Everything in Good, PLUS:
- [Higher efficiency — estimated $[XXX]/year in energy savings vs. Good]
- [Feature: variable-speed blower for better humidity control and quieter operation]
- [Extended XX-year parts + labor warranty]
- [Smart thermostat with remote access and scheduling]
- [Basic duct sealing included]

Investment: $[X,XXX]
After [rebate/tax credit]: $[X,XXX]
[If financing available: As low as $[XX]/month for [XX] months]

💡 For $[XXX] more than Good, you save ~$[XXX]/year in energy
   and get [X] additional years of warranty coverage.

────────────────────────────────────────────

OPTION 3: BEST — Premium Performance
======================================
Equipment:
- [AC/Heat Pump]: [Make Model] — [XX SEER2]
- [Furnace/Air Handler]: [Make Model] — [XX% AFUE / XX HSPF2]
- Thermostat: [Premium smart thermostat]
- [IAQ add-on: air purifier, UV light, or whole-home dehumidifier]

Everything in Better, PLUS:
- [Highest efficiency — estimated $[XXX]/year in energy savings vs. Good]
- [Variable-speed compressor — ultra-quiet, precise temperature control]
- [Premium XX-year warranty with extended compressor coverage]
- [Whole-home air quality upgrade: (specific IAQ product)]
- [Full duct optimization and balancing]

Investment: $[X,XXX]
After [rebate/tax credit]: $[X,XXX]
[If financing available: As low as $[XX]/month for [XX] months]

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

AVAILABLE INCENTIVES
--------------------
- [Federal tax credit — 25C]: Up to $[X,XXX] (applies to Better and Best)
- [Utility rebate — (utility name)]: $[XXX]
- [Manufacturer rebate]: $[XXX] (through [date])

WHY CHOOSE [COMPANY NAME]
---------------------------
- [Years in business] years serving [service area]
- Licensed, bonded, and insured
- [X] certified technicians on staff
- [Satisfaction guarantee / specific company differentiators from config]
- "We stand behind our work" [or voice tagline from config]

NEXT STEPS
----------
1. Choose your preferred option
2. We'll schedule your installation within [X] business days
3. [If applicable: Financing application takes ~5 minutes]
4. Sit back — we handle permits, installation, and inspection

Questions? Call us at [phone from config] or reply to this proposal.

[Company name] • [address] • [phone] • [website]
```

**Quality standards:**
- Always position the middle option as "Recommended" — this is where most customers should land
- Show the incremental cost between tiers, not just absolute prices — "$400 more" feels smaller than "$8,200"
- Translate every technical feature into a customer benefit (SEER2 → "lower monthly bills", variable-speed → "quieter, more even temps")
- Include real incentives based on current programs — don't fabricate rebates
- If equipment pricing wasn't provided, note [PRICING — INSERT YOUR NUMBERS] rather than guessing
- Never badmouth the Good option — it should still feel like a solid choice
- The proposal should be ready to email or print with minimal editing

## Example Output

Given input: *"Mrs. Chen, 2,200 sq ft home, replacing a 16-year-old Trane 3-ton system that's leaking refrigerant. We carry Carrier. Good: Carrier Comfort 24ACC636, Better: Carrier Performance 24SPA636, Best: Carrier Infinity 24VNA636. Our install prices are $7,800, $9,400, and $12,800 respectively."*

The proposal would present all three tiers with Carrier-specific features, show the SEER2 efficiency progression (15 → 17 → up to 24), calculate approximate annual savings at each tier using local utility rates from config, include the $2,000 federal tax credit for the Infinity heat pump option, and highlight that for $1,600 more than Good, the Better option saves ~$400/year and adds 5 years of warranty — paying for itself in 4 years.
