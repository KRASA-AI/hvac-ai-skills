---
name: "Maintenance Agreement Writer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/agreement"
version: 2.0
last_eval_score: null
---

# 📋 Maintenance Agreement Writer

## Purpose

Produce a complete, signature-ready residential or light-commercial HVAC maintenance agreement in one of three clearly-defined coverage tiers (Silver / Gold / Platinum). Each agreement includes scope of work, visit schedule, parts/labor coverage, exclusions, term/renewal, pricing, and customer signature block — written to close at the point of sale or at a renewal conversation without a follow-up round.

## When to Use

- A homeowner accepts a maintenance plan during a service call
- A new install is being handed off and the free first-year plan needs to be papered
- Annual renewal for an existing plan member (pulls prior-year coverage and adjusts for equipment changes)
- A commercial property manager requests a quote for multi-unit PM coverage
- A new CSR needs a template for phone-sold plans
- A fleet of rental properties needs a uniform plan across addresses

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
- Load `config.yml` for company name, license, address, phone, email, logo path, and — critically — `maintenance_plans` (tier names, pricing, inclusions) and `labor_rate` / `after_hours_multiplier`
- Reference `knowledge-base/maintenance-checklists/` for the appropriate PM checklist per equipment type
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
  - Capacitor, contactor, condensate float switch replacement included if failed during a PM visit
  - Refrigerant top-off allowance: up to 2 lbs/year at no cost (repair of leak is separate)
  - Same-day response guarantee for no-heat / no-cool calls (business hours)
  - Transferable if home is sold mid-term
  - *Typical price: $399–549 / system / year*

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

**Quality checks before sending:**
- Every covered unit has a matching PM checklist — no generic "HVAC tune-up" line items
- Tier inclusions match what operations can actually deliver (don't promise same-day response if the dispatcher can't commit)
- Exclusions are named, not buried or missing
- Pricing matches `config.yml`, not a made-up number. If pricing is an override, say so ("negotiated rate for multi-unit account")
- Dates compute correctly — start date, end date, next-charge date, renewal notice deadline
- Customer name and address match what's in the CRM record (to avoid renewal mismatch next year)

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
