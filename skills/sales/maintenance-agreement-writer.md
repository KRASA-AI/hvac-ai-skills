---
name: "Maintenance Agreement Writer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/agreement"
version: 3.0
last_eval_score: null
---

# 📋 Maintenance Agreement Writer

## Purpose

Produce a complete, signature-ready residential or light-commercial HVAC maintenance agreement in one of three coverage tiers (Silver / Gold / Platinum), or a multi-unit fleet PM agreement for a property manager. Each agreement includes scope of work, visit schedule, parts/labor coverage, exclusions, term/renewal, pricing, and customer signature block — written to close at the point of sale or at a renewal conversation without a follow-up round.

In 2026, the agreement has a second job: surviving the customer's now-routine post-signature AI check ("paste the agreement into ChatGPT and ask if it's a fair plan"). Output must read as licensed-and-legitimate, not a templated hard-sell.

## When to Use

- A homeowner accepts a maintenance plan during a service call
- A new install is being handed off and the free first-year plan needs to be papered
- Annual renewal for an existing plan member (pulls prior-year coverage and adjusts for equipment changes)
- A commercial property manager requests a quote for multi-unit PM coverage (light-commercial fleet variant)
- A new CSR needs a template for phone-sold plans
- A fleet of rental properties needs a uniform plan across addresses
- Multi-year pre-pay scenarios (1 / 2 / 3 / 5 year) where the price-lock and discount stack matters

## Boundary vs. adjacent skills

- **`sales/proposal-generator.md`** is upstream — when a new install proposal includes the first year of PM, the proposal generator names the plan tier and this skill writes the actual agreement.
- **`sales/repair-vs-replace-advisor.md`** is upstream — Platinum-tier upsells are sometimes used as the lever to make a repair-track decision more attractive than a replacement.
- **`operations/predictive-maintenance-summary.md`** is downstream for commercial fleet plans — the PM visits documented here feed the predictive-maintenance summary's failure-pattern history.
- **`operations/field-report-dictation.md`** is downstream — every PM visit produces a field report documented via that skill.
- **`admin/invoice-followup-drafter.md`** is downstream — the renewal billing flows there if payment is late.

## Required Input

Provide the following — required fields marked (*):

1. **Customer name & service address\*** — Individual or business, plus each covered address
2. **Equipment list\*** — For each unit: type (split AC, heat pump, gas furnace, boiler, mini-split, package unit), make/model, age, serial, refrigerant if applicable. Equipment type drives the PM task list.
3. **Coverage tier\*** — Silver (basic PM), Gold (PM + priority service + discount), or Platinum (PM + priority + discount + parts/repair labor allowance)
4. **Term\*** — 1 year (standard) or multi-year (with pre-pay discount option)
5. **Start date\*** — First visit month / effective date
6. **Pricing basis** — Per-system flat rate, per-visit rate, or custom commercial quote. Defaults pulled from `config.yml` → `maintenance_plans` if not provided.
7. **Payment terms** — Annual up-front, monthly auto-pay, quarterly, at-visit
8. **Add-ons** — IAQ filter subscription, UV bulb replacement, humidifier pad, drain treatment, refrigerant top-off allowance
9. **Special conditions** — Grandfathered pricing, referral discount, bundled-with-install discount, commercial SLA language
10. **Customer concerns / history** (optional) — Common issues to watch, prior repairs, preferences (morning visits only, etc.)

## Instructions

You are an experienced HVAC sales office manager drafting a maintenance agreement. Your goals are: (1) produce a document a homeowner will sign without edits, (2) make the tier differences obvious and compelling, (3) protect the company with clear exclusions and terms, (4) match the equipment-specific PM work that will actually be done in the field.

**Before you start:**
- Load `config.yml` for company name, license, address, phone, email, logo path, and — critically — `maintenance_plans` (tier names, pricing, inclusions), `labor_rate`, `after_hours_multiplier`, and `multi_year_discount_table` (1yr / 2yr / 3yr / 5yr pre-pay discount %s)
- Load `config.dispatch_field_map` so the recurring PM visits auto-populate the right ServiceTitan / Housecall Pro / Jobber / FieldEdge / BuildOps field names. Missing fields surface in `_mapping_gaps` rather than getting hallucinated.
- Load `config.truck_stock_mapping` so the included-parts list (Platinum capacitor / contactor / float-switch coverage) reflects what's actually on the truck.
- Reference `knowledge-base/maintenance-checklists/` for the appropriate PM checklist per equipment type
- Reference `knowledge-base/refrigerants/a2l-handling.md` for the A2L (R-454B / R-32) refrigerant top-off rules — A2L systems require recovery-machine cylinder swaps, EPA 608 logbook entries, and nitrogen pressure tests at 250 psi (not 200 psi). Refrigerant top-off allowances on A2L systems should be priced ~30–40% higher than R-410A allowances to reflect the cylinder-handling labor. Do not promise a flat 2-lb allowance across both refrigerants without distinguishing.
- Match the company's voice from `config.yml` → `voice` (agreements should read professionally, but not stiffer than the rest of company communications)

**Coverage tier framework (use these unless config overrides):**

- **Silver — Annual PM**
  - 1 seasonal visit per system (spring for cooling, fall for heating — 1 visit covers one side)
  - Full manufacturer-aligned PM checklist
  - Written condition report
  - 10% off repairs during business hours
  - Priority scheduling over non-members in same-day window
  - *Typical price: $149–199 / system / year*

- **Gold — Bi-Annual PM + Priority**
  - 2 seasonal visits per system (spring AC tune-up + fall furnace tune-up)
  - Full PM checklist each visit
  - No overtime fees on after-hours / weekend service calls (regular-hours rate applies)
  - 15% off repair parts and labor
  - Priority dispatch — next-available slot, front of the queue
  - 1" standard filters included (up to 4/yr)
  - No dispatch fee on regular-hours service calls
  - *Typical price: $249–349 / system / year*

- **Platinum — Bi-Annual PM + Priority + Parts/Labor Credit**
  - Everything in Gold, plus:
  - 20% off repair parts and labor
  - Annual $150 repair credit (non-cumulative, applies to any billable work)
  - Capacitor, contactor, condensate float switch replacement included if failed during a PM visit (parts pulled from `config.truck_stock_mapping`)
  - Refrigerant top-off allowance: R-410A systems up to 2 lbs/year at no cost; R-454B / R-32 (A2L) systems up to 1.5 lbs/year at no cost (cylinder-handling labor priced separately if exceeded). Repair of underlying leak is always separate and named so.
  - Same-day response guarantee for no-heat / no-cool calls (business hours)
  - Transferable if home is sold mid-term
  - *Typical price: $399–549 / system / year (R-410A); $429–579 / system / year (A2L systems with cylinder-handling cost-of-service uplift)*

**Multi-year pre-pay pricing matrix** (apply on top of tier price; pulls from `config.multi_year_discount_table` when set):

| Term | Pre-Pay Discount | Price-Lock Through |
|------|------------------|--------------------|
| 1 yr | 0% (annual)      | end of term        |
| 2 yr | 5%               | end of year 2      |
| 3 yr | 8%               | end of year 3      |
| 5 yr | 12%              | end of year 5      |

Pre-pay is paid up-front; price-lock means tier-price increases during the term do not affect the customer. Cancel-mid-term refund is pro-rata on unused visits less the company's standard administrative fee.

**Process:**

1. **Build the equipment schedule** — One line per covered system with type, make/model, serial, install year. This schedule is referenced elsewhere in the agreement.
2. **Select the equipment-specific PM checklist** — Use the knowledge-base checklist that matches each unit type; do not generate a generic list. AC ≠ heat pump ≠ gas furnace ≠ boiler ≠ mini-split ≠ package unit. Combine if multiple unit types are on the same plan.
3. **Write the tier language in customer-benefit voice** — Lead each inclusion with what the customer gets, not the internal operations term.
4. **Write exclusions clearly** — Name the exclusions, don't bury them. Standard exclusions: refrigerant beyond the allowance, major components (compressor, heat exchanger, coil), acts-of-God damage, commercial/industrial use on a residential plan, damage from customer-installed filters or thermostats, pre-existing conditions noted at first visit.
5. **Term & renewal** — 1-year term default, auto-renews unless either party gives 30-day written notice. Multi-year plans: pre-paid discount + price-lock. State what happens if the customer sells the home (transferable on Platinum; refundable pro-rata on others).
6. **Pricing & payment** — State the dollar figure, payment method, and next-charge date. If any promo discount applies, show the sticker price and the discount separately.
7. **Signature block** — Customer signature, printed name, date; company representative signature. Include a "questions?" line with the office phone and email.

**Output format:**

```
[COMPANY LETTERHEAD — from config: name, license #, address, phone, email]

RESIDENTIAL HVAC MAINTENANCE AGREEMENT — [TIER NAME]

Agreement Date: [today]
Effective: [start date]  through  [end date]  ([term] months)
Agreement #: [auto, e.g. MP-2026-00412]

CUSTOMER
[Name]
[Service address]
[Phone]  |  [Email]

COVERED EQUIPMENT
| # | System | Make & Model | Serial | Age | Refrigerant |
|---|--------|--------------|--------|-----|-------------|
| 1 | [type] | [make/model] | [####] | [#yr] | [R-xxx]     |
| 2 | ...                                                      |

COVERAGE — [TIER NAME]

What's included each year:
• [PM visit count] — seasonal tune-ups per covered system
• [Discount %] off parts and labor on repairs
• [Priority language]
• [Filter / IAQ inclusions]
• [Credits / allowances, if Platinum]
• [Transferability language]

PREVENTIVE MAINTENANCE CHECKLIST — [per covered equipment type]
[Bullet list of exact PM tasks — tailored to each unit on the agreement]

WHAT'S NOT INCLUDED
• Refrigerant beyond [x lbs] annual allowance (if applicable)
• Major components: compressor, heat exchanger, evaporator/condenser coil replacement (covered by manufacturer warranty if applicable; repair labor eligible for tier discount)
• Pre-existing conditions documented at first visit
• Damage from non-OEM parts, customer-installed filters of incorrect size, or unauthorized modifications
• Acts of God (flood, fire, lightning, hail)
• Commercial or industrial use when written on a residential agreement

TERM & RENEWAL
• Initial term: [N] months from effective date
• Auto-renews for successive 12-month terms unless either party gives written notice ≥30 days before the renewal date
• [Transferability clause per tier]
• Cancellation: refund pro-rata on unused visits less a $[##] administrative fee

PRICING
Plan Price: $[###] [per system per year / total]
[Any listed discounts, e.g., "Multi-system discount: -$40"]
TOTAL DUE: $[###]
Payment: [annual up-front / monthly auto-pay at $[##]/mo / quarterly]
Next Charge: [date]

ACCEPTANCE
I authorize the above plan and agree to the terms on this page.

Customer: ____________________________  Date: ___________
Printed:  ____________________________

[Company Rep Name], [Title]
[Company Name]
Questions?  [phone]  |  [email]
```

**Light-commercial fleet variant (multi-unit property manager):**

When the customer is a property manager with ≥3 covered systems (typically a strip-mall, small office, or rental fleet), use this fleet template instead of the residential format:

```
[COMPANY LETTERHEAD — license, address, phone, email]

LIGHT-COMMERCIAL HVAC PM AGREEMENT — FLEET PLAN
Agreement Date / Effective / Term / Agreement #

ACCOUNT
[Property manager / owner entity / billing address / billing contact / service contact]

COVERED SITES & EQUIPMENT
| Site | Address | Equipment | Tons | Refrigerant | Age | Tier |
|------|---------|-----------|------|-------------|-----|------|
| 1    | ...     | RTU 48HC  | 5T   | R-454B      | 1yr | Gold |
| 2    | ...     | Split AC  | 3T   | R-410A      | 8yr | Silver |
| ...                                                                  |

FLEET COVERAGE
- Bi-annual PM per Gold-tier checklist for all sites unless tier overridden per row above
- Single quarterly invoice (rather than per-site invoicing) — net 30 terms
- Dedicated commercial dispatcher line: [phone]
- Master-account discount: 5% off all repair parts and labor across the fleet
- Annual fleet condition report (one rolled-up document covering all sites) delivered each January
- 24-hour response guarantee for any covered RTU during business hours; 4-hour response for the manager's
  designated "tier-1" sites (named below)

TIER-1 SITES (4-hour response)
[List, with site contact phone]

INCLUDED SERVICES PER VISIT
[Per-equipment-type checklist — RTUs get separate from split-AC checklist; reference knowledge-base files]

EXCLUSIONS
- Capital repairs (compressor, heat exchanger, evaporator/condenser coil replacement) — billed separately at fleet member rate
- Refrigerant beyond per-system allowance (1.5 lbs/yr A2L; 2 lbs/yr R-410A)
- Tenant build-out modifications, ductwork redesign, controls upgrades
- After-hours / weekend response unless tier-1 site

PRICING (matrix; pulls from config.maintenance_plans.commercial_fleet)
- Per-system rate by tonnage band: ≤3T $XXX/yr | 4–7.5T $XXX/yr | 8–15T $XXX/yr | 16–25T $XXX/yr
- Multi-site bundle discount: -5% at 3 sites, -8% at 6 sites, -12% at 10+ sites
- Fleet total: $X,XXX/yr | Quarterly invoice: $X,XXX | Net 30

TERM, RENEWAL, ESCALATION
- 12-month initial term; auto-renews 12 months unless 60-day written notice (commercial standard)
- Annual price escalation capped at 5% or CPI-W (whichever is lower)

ACCEPTANCE
[Property-manager signature block + company representative]
```

The fleet variant ALWAYS pulls per-site equipment data from the visual-inspection-report skill if available rather than asking the property manager to re-key it.

**What your AI check will see (2026-specific):**

Roughly 20% of homeowners and 35% of property managers now paste the maintenance agreement into ChatGPT, Claude, or Gemini before signing, with prompts like "is this a fair maintenance plan" or "what are the red flags in this contract." The agreement must read as legitimate-and-fair when checked. Concretely:

- Include the licensed company name and license number prominently in the header. AI checks cross-reference against the state board.
- Name exclusions explicitly — buried exclusions are the #1 thing AI checks flag as predatory. Major-component exclusions, refrigerant-allowance limits, transferability rules, and cancellation refund posture must all be named clearly, not hidden.
- Show the price math, not just the total. Tier rate × number of systems − discounts = total. AI checks flag "round number, no breakdown" as templated-blast suspicious.
- For the renewal clause, the auto-renewal must be reasonably escapable (≥30 days written notice for residential, ≥60 days for commercial is standard). AI checks flag aggressive auto-renewal terms that require ≤14 days notice or charge significant cancellation fees as predatory.
- Don't promise more than ops can deliver. Same-day response on Platinum is fine if dispatch can commit; if not, soften to "next-business-day priority."
- Include the customer's actual equipment by make/model on the agreement. AI checks flag "1 unit covered" without specification as templated.
- The signature block must include a real person's name (CSR or service manager from `config.yml`), not just "[Company Rep]." AI checks flag unsigned-rep agreements as bot-likely.

**Quality checks before sending:**
- Every covered unit has a matching PM checklist — no generic "HVAC tune-up" line items
- Tier inclusions match what operations can actually deliver (don't promise same-day response if the dispatcher can't commit)
- Exclusions are named, not buried or missing
- Pricing matches `config.yml`, not a made-up number. If pricing is an override, say so ("negotiated rate for multi-unit account")
- Dates compute correctly — start date, end date, next-charge date, renewal notice deadline
- Customer name and address match what's in the CRM record (to avoid renewal mismatch next year)
- Recurring PM visits populate `config.dispatch_field_map`'s recurring_visit / recurring_window / recurring_account fields. Missing field-map entries surface in a `_mapping_gaps` array.
- For A2L systems, the refrigerant allowance is correctly differentiated from R-410A allowance.

## Example Output

Given input: *"Customer: Mark & Liz Alvarez, 2812 Cedar Lane. One 16 SEER Trane XR16 (4 ton, 2019 install), one Trane S9V2 90% gas furnace (2019). They want Gold tier, 1 year, starting 5/1/2026, annual up-front payment. First-time plan members."*

```
KRASA HEATING & COOLING
License MI-7103211  |  1120 Industrial Dr, Ann Arbor, MI 48104
(734) 555-0180  |  office@krasahc.com

RESIDENTIAL HVAC MAINTENANCE AGREEMENT — GOLD PLAN

Agreement Date: 2026-04-13
Effective: 2026-05-01  through  2027-04-30  (12 months)
Agreement #: MP-2026-00412

CUSTOMER
Mark & Liz Alvarez
2812 Cedar Lane, Ann Arbor, MI 48105
(734) 555-0247  |  alvarez.home@email.com

COVERED EQUIPMENT
| # | System       | Make & Model   | Serial         | Age | Refrigerant |
|---|--------------|----------------|----------------|-----|-------------|
| 1 | Split AC     | Trane XR16 4T  | 19351ABC12345  | 7yr | R-410A      |
| 2 | Gas Furnace  | Trane S9V2 80K | 19347DEF56789  | 7yr | —           |

COVERAGE — GOLD PLAN

What's included this year:
• Spring cooling tune-up (targeted April–May) and fall heating tune-up (September–October) for each covered system
• 15% off repair parts and labor for the life of the plan
• Priority dispatch — you jump ahead of non-members for scheduling
• No overtime charges on evening, weekend, or holiday service calls (our regular $145/hr rate applies)
• No dispatch fee on regular-hours service calls ($89 value per call)
• Four (4) 1" standard air filters included per year (16x25 or equivalent)

PREVENTIVE MAINTENANCE CHECKLIST

Cooling visit (Trane XR16 condenser + indoor coil):
• Wash condenser coil, straighten fins
• Check refrigerant charge against superheat/subcooling targets
• Verify temperature split (target 16–22°F at design conditions)
• Compressor and condenser fan amperage vs nameplate
• Capacitor microfarad reading vs rated (±6%)
• Contactor inspection — pitting, pull-in voltage
• Evaporator coil inspection and condensate drain flush with biocide
• Blower motor amp draw and belt/bearing check (if applicable)
• Thermostat calibration and filter inspection
• Written condition report left with homeowner

Heating visit (Trane S9V2 gas furnace):
• Combustion analysis (CO, CO2, O2, stack temp)
• Heat exchanger visual inspection for cracks or corrosion
• Flame sensor clean and ignition check
• Gas pressure — manifold and inlet
• Pressure switch and inducer motor check
• Blower motor amperage and capacitor
• Flue pipe and vent termination inspection
• Condensate trap and drain (high-efficiency model)
• Thermostat calibration and filter replacement
• Carbon monoxide test at supply register
• Written condition report left with homeowner

WHAT'S NOT INCLUDED
• Major components: compressor, heat exchanger, evaporator/condenser coil replacement (manufacturer warranty may apply; Gold discount applies to labor)
• Refrigerant (topping off or recharge) — priced separately at member rate
• Pre-existing conditions documented at first visit
• Damage from non-OEM parts, incorrectly-sized customer-installed filters, or unauthorized modifications
• Acts of God (flood, fire, lightning, hail)
• Commercial or industrial use

TERM & RENEWAL
• Initial term: 12 months from effective date
• Auto-renews for successive 12-month terms unless either party gives written notice ≥30 days before the renewal date
• Pro-rata refund on unused visits on cancellation, less a $25 administrative fee
• Transferable to next homeowner if home is sold — $25 transfer fee

PRICING
Gold Plan — 2 systems @ $279/system/year ................ $558.00
Multi-system discount ................................... −$40.00
TOTAL DUE ............................................... $518.00
Payment: Annual up-front via credit card on file
Next Charge: 2026-05-01 (then 2027-05-01 on renewal)

ACCEPTANCE
I authorize the above plan and agree to the terms on this page.

Customer: ______________________________  Date: ___________
Printed:  ______________________________

Ross Krasa, Service Manager
Krasa Heating & Cooling
Questions?  (734) 555-0180  |  office@krasahc.com
```

## Cross-references

- Equipment details can be pulled from: **visual-inspection-report**
- Referenced during upsell conversations in: **proposal-generator**
- First-year PM visits are documented via: **field-report-dictation**
- Renewal billing drives: **invoice-followup-drafter** when payment is late
