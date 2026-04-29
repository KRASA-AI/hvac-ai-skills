---
name: "Proposal Generator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/proposal"
version: 4.0
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
9. **Output mode** — One of: `homeowner proposal` (default, residential), `light-commercial proposal` (single-property property-manager / facility-team framing), `commercial-portfolio` (multi-property entity-level proposal — adds per-property line-item pricing, entity-level tier roll-up, CapEx-by-quarter rollout sequence, 179D + Section 48 framing handed to the navigator, and CPA hand-off block), `comparison-only block` (side-by-side table for email), `financing-focused proposal` (leads with monthly payment). Default: `homeowner proposal`.
10. **Multi-zone / multi-system flag (optional)** — One of: `single system` (default — one indoor + one outdoor unit), `dual-fuel` (heat pump + gas furnace backup), `multi-zone ductless` (one outdoor + 2–8 indoor heads), `multi-system whole-home` (more than one outdoor unit), `whole-home electrification` (replacing fossil heating + adding heat pump + electrical-panel scope). Each non-default value triggers a clarification block in the proposal (see Multi-zone handling under Instructions) rather than the skill silently averaging across systems.
11. **Commercial-portfolio inputs (only when `output_mode = commercial-portfolio`)** — Building roster (`property_id`, address, sqft, current equipment summary, target tier, target install quarter), filing-entity tax structure (LLC / S-corp / C-corp / non-profit / municipal / REIT), `config.crm_record_id` for prior-bid linkback, `config.energy_modeling_partner` if a third-party modeler will validate the projection, and the property-manager's CapEx ceiling per fiscal year if known.
12. **Incentive auto-stacking** — One of: `pull from navigator` (the skill reads the most recent `customer-service/rebate-and-tax-credit-navigator.md` output for this customer keyed by `config.crm_record_id` and stacks the per-program totals automatically into the Available Incentives block), `pass-in` (specific incentive amounts provided in this prompt), `top-line only` (the skill pulls top-line totals from the navigator's conventions and notes "see navigator output for line-item breakdown"). Default: `pull from navigator` if a navigator output exists, otherwise `top-line only`. When `pull from navigator` is on, the skill must echo the navigator's "as of [date]" caveat for state-program figures verbatim.
13. **Tone** (optional) — Defaults to `config.yml` → `voice`.

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

### Multi-zone handling

When `multi_zone_flag` is anything other than `single system`, the proposal must:
- Surface the per-system breakdown on every tier (one indoor + outdoor row per zone or per system) — never average across systems
- For `dual-fuel`: present the gas-furnace backup as a separate line item with its own SEER2/AFUE/HSPF2 and warranty, and note the dual-fuel switchover temperature default per `config.climate_zone` (e.g., 35°F default for cold-climate, 25°F for moderate)
- For `multi-zone ductless`: list each indoor head with its room/zone label, BTU rating, and head style (wall / ceiling cassette / floor / concealed), and bundle the line-set, pad/bracket, and condensate work into a separate "Installation scope per head" block so the homeowner can see why the labor figure is higher than a single-zone install
- For `multi-system whole-home`: present each system as its own Good/Better/Best mini-block under a common Why Two Systems section that justifies the count (square footage, multi-floor, separate occupancy patterns, zoning vs. multi-system trade-off)
- For `whole-home electrification`: include an Electrical Scope block (panel upgrade $X, 240V circuit and disconnect $X, optional EV-charger-ready provision $X) and a Decommissioning Scope block (gas line cap, water heater swap if in scope, venting closure) with each line priced separately so the homeowner sees what's HVAC vs. what's adjacent
- In every multi-zone case, end with a single Clarification Question block — "Before we lock in numbers we need to confirm: [1–3 specific items]" — instead of guessing. This is the one-clarifying-turn the multi-zone case earns.

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

**Output format — commercial-portfolio:**

For multi-property accounts (HOAs, REITs, property-management companies, school districts, municipal facilities). Replaces the single-property tier structure with an entity-level proposal:

```
[COMPANY NAME] — PORTFOLIO HVAC PROPOSAL
Prepared for: [Entity name]    CRM record: [config.crm_record_id]
Filing entity: [LLC / S-corp / C-corp / non-profit / municipal / REIT]
Properties in scope: [N]    Total sqft: [X,XXX,XXX]    Total RTU/equipment count: [N]
CapEx ceiling per fiscal year (entity-stated): $[XXX,XXX]/yr  or  "not stated"
Date: [Today's date]    Valid for: 60 days    Prepared by: [Advisor name from config]

PORTFOLIO ASSESSMENT
--------------------
[2–3 sentences: total equipment count, age range, refrigerant mix (R-22 / R-410A / R-454B), worst-condition properties named, prioritization logic.]

PER-PROPERTY TIER ASSIGNMENT
----------------------------
| Property | Sqft | Equipment | Age | Tier (Good / Better / Best) | Target install Q | Investment | After incentives |
| [property_id] | [sqft] | [eq] | [yrs] | [tier] | [Y1Q1 / Y1Q2 / Y2Q3 / etc.] | $[XX,XXX] | $[XX,XXX] |
…

PORTFOLIO ROLL-UP
-----------------
| Tier | # properties | Total investment | Total incentives | Net portfolio investment | Annual energy savings (cross-link energy-savings-report) |
| Good | [N] | $[XXX,XXX] | $[XX,XXX] | $[XXX,XXX] | $[XX,XXX]/yr |
| Better ⭐ | [N] | $[XXX,XXX] | $[XX,XXX] | $[XXX,XXX] | $[XX,XXX]/yr |
| Best | [N] | $[XXX,XXX] | $[XX,XXX] | $[XXX,XXX] | $[XX,XXX]/yr |
| Total | [N] | $[X,XXX,XXX] | $[XXX,XXX] | $[X,XXX,XXX] | $[XX,XXX]/yr |

CAPEX BY QUARTER (proposed rollout)
-----------------------------------
| Year/Q | Properties | Tier | CapEx | Cumulative CapEx | Within entity ceiling? |
| Y1Q1 | [list] | [tier] | $[XXX,XXX] | $[XXX,XXX] | [yes / no — flag] |
…

ENTITY-LEVEL FEDERAL STACK (handed to navigator for CPA-facing detail)
----------------------------------------------------------------------
- 179D ($[X.XX]–$[X.XX]/sqft sliding scale × [X,XXX,XXX] sqft × 5× PW/A multiplier if confirmed): est. $[XXX,XXX]
- Section 48 ITC base 6% / enhanced 30% with energy-community + domestic-content adders: est. $[XX,XXX] — base-vs-enhanced-vs-adder breakdown lives in the navigator output
- IRA §6417 elective-pay (if filing entity is non-profit / municipal): applicable / not applicable
- 179D allocation rules for designer-claim (if filing entity is government / non-profit): captured in navigator output

For the full entity stack including 179D modeler hand-off, Section 48 base-vs-enhanced-rate breakdown, IRA §6417 elective-pay handling, and the CPA-confirm worksheet, see the commercial-portfolio output of `customer-service/rebate-and-tax-credit-navigator.md`.

CAPEX-VS-OPEX SUMMARY
---------------------
| Horizon | Cumulative CapEx (cash + financed) | Cumulative operating savings | Net cash position |
| Year 3 | $[X,XXX,XXX] | $[XXX,XXX] | $([XXX,XXX]) |
| Year 5 | $[X,XXX,XXX] | $[XXX,XXX] | $([XXX,XXX]) |
| Year 10 | $[X,XXX,XXX] | $[X,XXX,XXX] | $[XXX,XXX] |

WHAT YOUR CFO / ACCOUNTANT WILL SEE
-----------------------------------
1. Tier assignment logic: [one sentence — e.g., "Better is recommended for 4 of 6 properties because the equipment-age + tenant-impact profile makes the ~12% incremental CapEx pay back inside the holding-period assumption."]
2. Federal stack defensibility: [one sentence — e.g., "179D figure uses ASHRAE 90.1-2019 baseline at the $2.50–$5.00/sqft sliding scale with PW/A 5× multiplier; the navigator output carries the modeler hand-off and the CPA-confirm worksheet."]
3. Pricing benchmarks: [one sentence — e.g., "Per-RTU pricing is at the 2026 tariff-adjusted regional median per `knowledge-base/market-conditions/2026-tariff-price-environment.md`; competitive bids should land within ±10% on the equipment line and ±15% on the install line."]
4. CapEx rollout: [one sentence — e.g., "Quarterly sequencing fits inside the stated $[XXX,XXX]/yr ceiling and concentrates Y1 spend on the two worst-condition properties; if the ceiling is breached the rollout can shift one Q1 property to Q3."]

If your CFO or accountant pushes back on any of the four, call me at [phone] and I'll bring the per-property model + the navigator output + the energy-savings-report commercial-portfolio output to the next finance review.

CLARIFICATIONS NEEDED BEFORE BID-LOCK
-------------------------------------
- [list — e.g., lease pass-through structure, energy-community designation per property, PW/A confirmation for crew, tenant-coordination windows for occupied buildings, prior bid amounts pulled from `config.crm_record_id`]

_mapping_gaps: [array of fields the entity needs to confirm before this becomes a signed contract — utility account IDs, capital-plan amounts, energy-community designation, PW/A compliance documentation, etc.]

[Company name] • [address] • [phone] • [website] • license #[X]
```

**Output format — comparison-only block:**

The Side-by-Side table plus the AI-validation block, stripped to fit inline in an email. No repeated tier descriptions. Designed for the "send me something quick" customer who has already heard the verbal pitch.

**Output format — financing-focused proposal:**

Reorders the proposal so the monthly-payment line is the headline on each tier. Moves the total investment to the footer. Opens with: "Here's what each option looks like on a monthly basis."

**Incentive auto-stacking from the navigator (when on):**

When `incentive_auto_stacking = pull from navigator`, the skill must:
1. Load the most recent `customer-service/rebate-and-tax-credit-navigator.md` output keyed by `config.crm_record_id`. If no navigator output exists for this customer, fall back to `top-line only` mode and emit a flag `[NAVIGATOR OUTPUT NOT FOUND — RUN NAVIGATOR FIRST FOR LINE-ITEM STACKING]`.
2. For each tier, pull the navigator's per-program totals (HEEHRA / utility / manufacturer / 25D / 179D / Section 48 / state programs) that are tier-eligible and sum them into the After-Incentives line.
3. Echo the navigator's "as of [date]" caveat verbatim on every state-program line.
4. Echo the navigator's HEEHRA AMI tier (e.g., "<80% AMI: full $8,000", "80–150% AMI: 50% up to $8,000") so the customer sees the same figure in both documents.
5. Emit any `_mapping_gaps` from the navigator output into the proposal's Clarifications block — never silently drop them.
6. If the navigator output's commercial-portfolio variant exists for this `config.crm_record_id` and `output_mode = commercial-portfolio`, use it (179D + Section 48 + §6417 + per-property utility / manufacturer stacking) rather than the residential-pull defaults.
7. Never invent or contradict an incentive figure that the navigator did not produce.

**Quality standards:**

- Always position the middle option as "Recommended" — this is where most customers should land. (On commercial-portfolio, the recommendation is per-property and the rationale appears in the Tier Assignment column.)
- Show the incremental cost between tiers, not just absolute prices — "$400 more" feels smaller than "$8,200."
- Translate every technical feature into a customer benefit (SEER2 → "lower monthly bills", variable-speed → "quieter, more even temps").
- Include real, current incentives based on install timing. **Never list federal 25C for installs completed after 12/31/2025.**
- If equipment pricing wasn't provided, note `[PRICING — INSERT YOUR NUMBERS]` rather than guessing.
- Never badmouth the Good option — it should still feel like a solid choice.
- Never use scarcity pressure. "Prices will never be this low again" is not defensible language.
- Always include the "What your AI check will see" block in the homeowner and light-commercial proposals, and the "What your CFO / accountant will see" block in the commercial-portfolio proposal. Removing it defeats the 2026-specific purpose of this skill.
- For multi-zone / dual-fuel / multi-system / electrification cases, never silently average across systems. Always emit the Multi-zone breakdown and the single Clarification Question block.
- For commercial-portfolio, never duplicate the navigator's federal-stack line-item detail — hand it off and link, so the entity has one source of truth and the CPA reviews one document, not two.
- The proposal should be ready to email or print with minimal editing.

## Example Output

Given input: *"Mrs. Chen, 2,200 sqft home in Phoenix (ZIP 85016), replacing a 16-year-old Trane 3-ton system that's leaking refrigerant. Carrier-only shop. Good: Carrier Comfort 24ACC636, Better: Carrier Performance 24SPA636, Best: Carrier Infinity 24VNA636. Install prices: $8,200, $10,200, $14,100 respectively (reflecting 2026 tariff-adjusted bands). Install timing: under contract for May 2026. Household income: ~$110K (family of 3, ~100% AMI for Maricopa County). Output: homeowner proposal."*

The proposal would present all three tiers with Carrier-specific features, show the SEER2 efficiency progression (15 → 17 → up to 24), calculate approximate annual savings at each tier from Phoenix climate-zone CDD/HDD and the utility rate stored in `config.utility_rates.85016`, and build the Available Incentives block from the post-12/31/2025 rule set: HEEHRA estimated at 50%-up-to-$8,000 (80-150% AMI tier), APS utility rebate ~$800, Carrier spring promo ~$500 — explicitly noting federal 25C is not available. Financing row uses the configured partner (e.g., Synchrony 120-month / 9.99%), landing Good at ~$106/month, Better at ~$131/month, Best at ~$181/month. AI-validation block honestly scores the Better tier's $10,200 price as "mid-range for 17 SEER2 Carrier in Phoenix in spring 2026, within ~10% of competitive quotes; includes permit, haul-away, and new line set which not all quotes include." The proposal closes with an invitation to paste the document into ChatGPT for a second opinion, and a direct phone number for the advisor.

### Commercial-portfolio worked example

Given input: *"Sunbelt Property Management LLC, 6 garden-style apartment buildings in Phoenix metro, total 312,000 sqft, 18 RTUs (3-ton to 7.5-ton), all R-410A, ages 9–17 years. Filing entity: LLC taxed as S-corp. PW/A confirmed. config.crm_record_id: SBM-2026-0412 (rebate navigator commercial-portfolio output exists, dated 2026-04-26). Recommended tier per property: 4 properties → Better (Carrier WeatherExpert 14.8 IEER R-454B), 2 properties → Best (Carrier WeatherExpert with VFD + BMS integration). CapEx ceiling stated: $600,000/yr. Three-year rollout. Output: commercial-portfolio with incentive_auto_stacking = pull from navigator."*

The proposal would produce:
- Per-property tier assignment table for all 6 properties with rationale (worst-condition + tenant-impact = Y1, lower priority = Y2/Y3)
- Portfolio roll-up with Better at 4 properties / 12 RTUs / ~$890K investment / ~$72K incentives, Best at 2 properties / 6 RTUs / ~$640K investment / ~$58K incentives, totals ~$1.53M / ~$130K / ~$1.40M net / ~$73K/yr energy savings cross-linked to the energy-savings-report commercial-portfolio output for SBM-2026-0412
- CapEx-by-quarter table sequencing 8 RTUs in Y1 / 6 in Y2 / 4 in Y3 within the $600K/yr ceiling (Y1Q1 + Y1Q3 each at ~$300K)
- Entity-level federal stack line-items pulled verbatim from the navigator output: 179D at $2.50–$3.50/sqft sliding scale × 312K sqft × 5× PW/A multiplier = est. ~$780K–$1.09M (CPA-confirm flagged); Section 48 ITC base 6% / enhanced 30% with energy-community adder if any property qualifies (per-property eligibility deferred to navigator); §6417 not applicable (S-corp); 179D allocation rules not applicable (not government / non-profit)
- CapEx-vs-OpEx summary showing cash position turns positive between Y6 and Y7
- "What your CFO will see" block calibrated to the navigator's 179D ASHRAE 90.1-2019 baseline reference and the "as of 2026-04-26" caveat on Section 48 adders
- Clarifications block carrying every navigator `_mapping_gaps` entry forward (energy-community designation per property, lease pass-through structure, prior bid amounts from CRM SBM-2026-0412, tenant-coordination windows for occupied buildings)
