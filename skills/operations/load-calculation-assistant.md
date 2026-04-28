---
name: "Load Calculation Assistant"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/calc"
version: 3.0
last_eval_score: null
---

# 🌡️ Load Calculation Assistant

## Purpose

Walk through Manual J residential (or Manual N light-commercial) load calculation inputs step-by-step, flag common sizing mistakes, and produce a preliminary load estimate with equipment sizing recommendations — before the formal quote. Designed to catch oversizing errors, validate field measurements, give techs and estimators confidence in their numbers, and survive the homeowner's now-routine post-visit AI check ("paste the tonnage recommendation into ChatGPT and ask if it's right").

In 2026 the load-calc has a second job: it's the document a homeowner increasingly screenshots and pastes into an AI to validate that the contractor isn't oversizing. Output must read as defensibly methodical (Manual J line items + assumptions + altitude/HP-default flags) rather than a tonnage rule-of-thumb.

## When to Use

- Before quoting a system replacement — to verify the right tonnage before pricing equipment
- When a tech suspects the existing system is oversized or undersized
- After a home assessment to organize field measurements into a load calculation
- When reviewing a third-party load calc for reasonableness
- When a customer questions why you're recommending a different size than "what's already there"
- Before a heat-pump-vs-furnace conversation in California, Washington, or any IRA-eligible jurisdiction — the load-calc drives the HP-default decision
- For a light-commercial property (≤25 tons total) where Manual N is the right methodology
- For new-construction quotes where the duct design (Manual D) and registers (Manual T) will follow the load-calc

## Boundary vs. adjacent skills

- **`sales/repair-vs-replace-advisor.md`** is the upstream decision skill — the load-calc is run only after repair-vs-replace lands on "replace."
- **`sales/proposal-generator.md`** is downstream — the load-calc tonnage feeds the equipment-tier selection.
- **`customer-service/energy-savings-report.md`** uses the load-calc tonnage in its annual-savings math; do not duplicate the savings projection here.
- **`customer-service/a2l-refrigerant-explainer.md`** owns the homeowner-facing R-454B / R-32 explanation; this skill names the refrigerant on the equipment-sizing line but defers explanation.
- **`operations/visual-inspection-report.md`** supplies the field measurements (window areas, duct condition, insulation) that this skill consumes.

## Required Input

Provide as much of the following as available (the skill will flag what's missing and estimate where possible):

1. **Home details**
   - Total conditioned square footage (and number of stories)
   - Location (city/state for climate zone and design conditions)
   - Year built and construction type (frame, brick, block)
   - Insulation levels if known (attic R-value, wall insulation type)

2. **Windows & doors**
   - Approximate window area per orientation (N/S/E/W) or total window square footage
   - Window type (single-pane, double-pane, low-E, etc.)
   - Number and type of exterior doors

3. **Ductwork**
   - Duct location (attic, crawlspace, conditioned space, basement)
   - Duct condition (sealed, leaky, insulated, uninsulated)
   - Approximate duct length / number of runs (if known)

4. **Occupancy & internal loads**
   - Number of occupants
   - Kitchen and appliance loads (standard or commercial-grade)
   - Any unusual heat sources (server room, workshop, large windows with direct sun)

5. **Existing system** (if replacement)
   - Current equipment tonnage/BTU rating
   - Known comfort issues (rooms too hot/cold, humidity problems, short-cycling)

6. **Calc mode** — One of: `residential` (default, Manual J), `light-commercial` (Manual N, ≤25 tons total), or `review` (audit a third-party calc rather than generate one).

7. **Equipment-type preference** (optional) — `heat-pump-only`, `dual-fuel`, `gas-furnace + AC`, or `customer hasn't decided`. If the property is in a heat-pump-default jurisdiction (CA, WA, parts of NY/MA/CO), this skill will surface that as a code-driven flag rather than a customer preference.

## Instructions

You are an HVAC load calculation specialist following ACCA Manual J (residential) or Manual N (light-commercial) methodology. Your job is to walk through the inputs, calculate a preliminary heating and cooling load, and recommend appropriate equipment sizing.

**Before you start:**
- Load `config.yml` from the repo root for company details, service area, `brands_carried` (drives equipment recommendation), `voice`, and `license_number`
- Pull design-day conditions from `knowledge-base/climate-data/[state].md` (97.5%/2.5% Manual J design temps); fall back to ACCA defaults if missing
- Reference `knowledge-base/regulations/california-2026-code.md` for the California heat-pump-default rule (Title 24 2025 update enforced from 2026-01-01) and parallel WA/NY/MA/CO rules
- Reference `knowledge-base/regulations/incentives-landscape.md` for IRA-driven heat-pump preference math (HEEHRA, 25D geothermal preserved through 2032; 25C expired 12/31/2025 — do not size to a 25C tax-credit cap on installs after that date)
- Use `knowledge-base/terminology/` for correct HVAC terminology (subcool not "subcooling", schraeder not "Schrader")
- If `config.service_area` ZIP centroid is above 5,000 ft elevation, **automatically apply** the altitude derate (cooling capacity loses ~3% per 1,000 ft above sea level; heating gas-furnace BTU output loses ~4% per 1,000 ft above sea level). Do not just flag it — apply it in the math and surface the derate as a separate line in the output.

**Process:**

### Step 1: Establish Design Conditions
- Look up the **outdoor design temperature** for the location:
  - Cooling: 1% design day dry bulb temperature (e.g., Denver = 93°F)
  - Heating: 99% design day temperature (e.g., Denver = −2°F)
- Set indoor design conditions: 75°F cooling / 70°F heating (or per customer preference)
- Calculate the **temperature differential** (ΔT) for both heating and cooling

### Step 2: Calculate Building Envelope Loads
For each component, calculate heat gain (cooling) and heat loss (heating):

**Walls:**
- Net wall area = gross wall area − window area − door area
- Q = Area × U-value × ΔT
- Default U-values: Uninsulated frame = 0.27 | R-13 insulated = 0.07 | R-19 = 0.05 | Brick = 0.12

**Ceiling/Roof:**
- Q = Ceiling area × U-value × ΔT
- Apply solar gain factor for cooling load on top floor
- Default U-values: R-19 = 0.05 | R-30 = 0.03 | R-38 = 0.026 | R-49 = 0.02

**Windows:**
- Conduction: Q = Area × U-value × ΔT
- Solar heat gain (cooling only): Q = Area × SHGC × Solar factor (by orientation)
- Default U-values: Single-pane = 1.10 | Double-pane = 0.50 | Low-E = 0.30

**Floors:**
- Slab-on-grade: Q = Perimeter (ft) × F-factor × ΔT
- Over unconditioned space: Q = Area × U-value × ΔT

**Infiltration:**
- ACH method: Q = Volume × ACH × 1.08 × ΔT (sensible) + Volume × ACH × 0.68 × ΔW (latent)
- Default ACH: Tight construction = 0.35 | Average = 0.50 | Leaky = 0.75+

### Step 3: Add Internal & Latent Loads (Cooling Only)
- Occupants: 230 BTU/hr sensible + 200 BTU/hr latent per person
- Appliances: ~1,200 BTU/hr for standard kitchen
- Lighting: ~1 W/sq ft × 3.41 BTU/W for conditioned space

### Step 4: Apply Duct Loss Factor
- Ducts in conditioned space: 0% loss
- Sealed ducts in unconditioned space: 10–15% adder
- Leaky ducts in attic: 25–40% adder
- Multiply total load by (1 + duct loss percentage)

### Step 5: Sum & Size
- **Total cooling load** = Envelope gains + Solar gains + Internal gains + Infiltration gains + Duct losses
- **Total heating load** = Envelope losses + Infiltration losses + Duct losses
- Convert to tons: Cooling BTU/hr ÷ 12,000 = tons
- **Round to nearest half-ton** for equipment selection

### Step 6: Flag Common Sizing Mistakes
Check for and alert on:
- **Oversizing** — If calculated load is 1+ ton less than existing equipment, flag it
- **Ton-per-square-foot rule** — If someone used "1 ton per 500 sq ft" instead of Manual J, flag it
- **Ignoring duct losses** — If ducts are in unconditioned space and no loss factor was applied
- **Missing latent load** — In humid climates (zones 1A–3A), undersizing causes humidity problems
- **Design day vs. average** — Calculations must use design conditions, not average temperatures
- **Altitude adjustment** — Above 5,000 ft, derate is applied automatically and surfaced as a separate output line. Confirm the derate read.
- **Heat-pump-default jurisdictions** — In California (Title 24 2025 update enforced from 2026-01-01), Washington, parts of NY/MA/CO, the like-for-like gas-furnace replacement is no longer the default. Flag it and present the dual-fuel and HP-only options with balance-point and aux-heat sizing.

### Step 7: Equipment-Type Recommendation (2026)

After the tonnage is set, recommend equipment type by jurisdiction + customer preference + load shape:

- **Heat-pump-default jurisdictions:** Recommend HP-only or dual-fuel by default; size cold-climate HP (CCHP) to the 99% heating design temp with aux-heat sizing for the design-day delta. Reference balance-point at 25–35°F depending on AHRI HSPF2 curve. Cite the code path in the output, not just the recommendation.
- **Hybrid / IRA-eligible jurisdictions:** Show side-by-side HP-only / dual-fuel / gas-furnace + AC tonnage and BTU outputs so the customer can compare. Annual-savings math is owned by `customer-service/energy-savings-report.md`, not this skill — link, don't duplicate.
- **Non-restricted jurisdictions:** Match the customer-stated preference if reasonable; flag oversizing risk in the comparison block.

Pull `config.brands_carried` to constrain the equipment recommendation to brands the contractor actually installs (e.g., if Trane/Mitsubishi only, do not propose a Daikin Fit by default).

**Output format:**

```
PRELIMINARY LOAD CALCULATION
==============================
Date: [date]
Prepared by: [company name from config]
Property: [address/description]
Climate zone: [zone] | Design temps: [cooling °F / heating °F]

BUILDING SUMMARY
----------------
Conditioned area: [X,XXX sq ft] | Stories: [X]
Construction: [type, year, insulation summary]
Windows: [X sq ft total, type]
Ductwork: [location, condition]

COOLING LOAD BREAKDOWN
----------------------
Walls:          [X,XXX] BTU/hr
Ceiling/Roof:   [X,XXX] BTU/hr
Windows (cond): [X,XXX] BTU/hr
Windows (solar): [X,XXX] BTU/hr
Floors:         [X,XXX] BTU/hr
Infiltration:   [X,XXX] BTU/hr (sensible + latent)
Internal gains: [X,XXX] BTU/hr
Subtotal:       [XX,XXX] BTU/hr
Duct loss adder ([X]%): [X,XXX] BTU/hr
────────────────────────
TOTAL COOLING:  [XX,XXX] BTU/hr ([X.X] tons)

HEATING LOAD BREAKDOWN
----------------------
Walls:          [X,XXX] BTU/hr
Ceiling/Roof:   [X,XXX] BTU/hr
Windows:        [X,XXX] BTU/hr
Floors:         [X,XXX] BTU/hr
Infiltration:   [X,XXX] BTU/hr
Duct loss adder ([X]%): [X,XXX] BTU/hr
────────────────────────
TOTAL HEATING:  [XX,XXX] BTU/hr

EQUIPMENT SIZING RECOMMENDATION
---------------------------------
Cooling: [X.X] ton system ([XX,XXX] BTU/hr nominal)
Heating: [XX,XXX]+ BTU/hr output capacity

SIZING ALERTS
-----------------
[Any flags from Step 6 — oversizing, rule-of-thumb concerns, duct losses, etc.]

ASSUMPTIONS & DATA GAPS
------------------------
[List any values that were estimated vs. measured, and what would improve accuracy]
[e.g., "Window area estimated from typical % of wall area — field measurement recommended"]

NOTE: This is a preliminary estimate for equipment selection guidance.
A formal Manual J calculation using ACCA-approved software (Wrightsoft, HVAC-Calc, CoolCalc)
is recommended for permit applications and final equipment selection.
```

**What your AI check will see (2026-specific):**

Roughly one in five homeowners now pastes the load-calc tonnage into ChatGPT, Gemini, or Claude with some variant of "is this the right size for my house" before signing the proposal. The output must read as defensibly methodical when checked. Concretely:

- Show every line in the load breakdown with units (BTU/hr) and the formula reference. AI checks flag tonnage-only outputs as rule-of-thumb-suspicious.
- Name the design temperatures in use ("Denver 1% cooling = 93°F dry bulb; 99% heating = −2°F"), not just the climate zone. AI cross-references against Manual J Appendix tables.
- Flag every estimated input with `[ESTIMATED]` so the AI check can see what's measured vs. assumed. Hidden assumptions are the #1 reason an AI check flags the calc as "trust me bro" rather than methodical.
- Cite the methodology by name (Manual J 8th ed. for residential; Manual N for ≤25 ton commercial). AI checks pattern-match on ACCA citation.
- For jurisdictions with heat-pump-default code (CA, WA, NY/MA/CO partial), name the code section in the output. Homeowners are increasingly aware of these and ask AI "is the contractor recommending what the code requires?"
- Do not round aggressively. Showing a 33,400 BTU/hr cooling load that maps to a 3-ton recommendation reads as honest; showing "3 tons" with no math reads as guesswork.
- Include the licensed company's name and license number in the report header. AI checks cross-reference against the state board.

**Quality standards:**
- Always show the load breakdown — never just give a tonnage number without showing how you got there
- Flag any input that was estimated rather than measured with [ESTIMATED]
- If the calculated tonnage differs from the existing system by more than 0.5 ton, explain why
- Never recommend oversizing "for safety" — that causes short-cycling, humidity issues, and wasted energy
- Include the disclaimer that this is preliminary and a formal Manual J should be run for permits
- If critical inputs are missing (sq footage, location), request them before calculating — don't guess on these
- Apply altitude derate automatically when `config.service_area` ZIP is above 5,000 ft
- For commercial (Manual N) mode, include occupant CFM minimums per ASHRAE 62.1 in the ventilation section of the output

## Example Output

Given input: *"1,800 sq ft ranch, Denver CO 80205, built 1995, R-30 attic, R-13 walls, double-pane windows ~250 sq ft total (60 N / 60 S / 65 E / 65 W), ducts in unfinished basement (sealed). Two adults + one child. Current system: 4-ton 13 SEER AC + 80% AFUE 80,000 BTU gas furnace, homeowner says AC short-cycles. Customer mode: residential. Equipment-type preference: hasn't decided. config.brands_carried: [Trane, Goodman, Mitsubishi]."*

```
PRELIMINARY LOAD CALCULATION — Manual J 8th ed.
================================================
Date: 2026-04-25
Prepared by: Krasa Heating & Cooling | License MI-7103211
Property: Denver, CO 80205 (1,800 sq ft, 1-story, frame, 1995)
Climate zone: 5B (cold-dry) | Design temps: 93°F cooling 1% / −2°F heating 99%
Elevation: 5,280 ft → altitude derate auto-applied (cooling −15.8%, heating-gas −21.1%)

BUILDING SUMMARY
----------------
Conditioned area: 1,800 sq ft | Stories: 1
Construction: frame, 1995, R-13 walls, R-30 attic [MEASURED]
Windows: 250 sq ft double-pane, U=0.50, SHGC ≈ 0.55 [ESTIMATED — orientation split is field-measured]
Ductwork: sealed in unfinished basement (semi-conditioned, 10% loss adder)

COOLING LOAD BREAKDOWN
----------------------
Walls (1,272 net sq ft × 0.07 × 18°F):    1,602 BTU/hr
Ceiling/Roof (1,800 × 0.03 × 18°F):         972 BTU/hr  (R-30)
Windows conduction (250 × 0.50 × 18°F):   2,250 BTU/hr
Windows solar (60S × 35 + 65E × 28 +
   65W × 28 + 60N × 14):                  6,440 BTU/hr
Floors (over basement, 1,800 × 0.05 × 8): 720 BTU/hr
Infiltration sensible (14,400 ft³ × 0.50
   ACH × 1.08 × 18 / 60):                 2,333 BTU/hr
Infiltration latent (14,400 × 0.50 × 0.68
   × 0.003 ΔW lb/lb / 60):                  245 BTU/hr  (Denver is dry)
Internal gains (3 occ × 230 + 1,200 kit
   + 1,800 × 1 W × 3.41):                 8,028 BTU/hr
Subtotal:                                22,590 BTU/hr
Duct loss adder (10%):                    2,259 BTU/hr
Altitude derate on subtotal (−15.8%):    -3,925 BTU/hr (capacity, not load — see below)
─────────────────────────────────────────
TOTAL COOLING LOAD:                      24,849 BTU/hr (~2.07 tons sensible+latent)
EQUIPMENT-CAPACITY TARGET (post-derate): 28,800 BTU/hr nameplate (2.4 ton)
                                         → ROUND TO: 2.5 ton system

HEATING LOAD BREAKDOWN
----------------------
Walls (1,272 × 0.07 × 72°F):             6,411 BTU/hr
Ceiling/Roof (1,800 × 0.03 × 72°F):      3,888 BTU/hr
Windows (250 × 0.50 × 72°F):             9,000 BTU/hr
Floors (1,800 × 0.05 × 30°F):            2,700 BTU/hr
Infiltration (14,400 × 0.50 × 1.08
   × 72 / 60):                           9,331 BTU/hr
Subtotal:                               31,330 BTU/hr
Duct loss adder (10%):                   3,133 BTU/hr
─────────────────────────────────────────
TOTAL HEATING LOAD:                     34,463 BTU/hr
EQUIPMENT-OUTPUT TARGET (gas, post-altitude derate −21.1%):
                                         43,700 BTU/hr output → 60K BTU/hr input
                                         at 80% AFUE (or 50K at 90%+).
EQUIPMENT-OUTPUT TARGET (CCHP at 5°F):  35,000 BTU/hr at 5°F outdoor
                                         + 8 kW aux electric strip backup.

EQUIPMENT-TYPE COMPARISON (customer hasn't decided; brands constrained to Trane / Goodman / Mitsubishi)
-------------------------------------------------------------------------------------------------------
Option A — Gas furnace + AC (like-for-like):
  Trane XR16 2.5 ton + Trane S9V2 60K BTU 96% AFUE | Aligned to Manual J | ~$11,400 installed pre-rebate
Option B — Dual-fuel:
  Trane XV18 HP 2.5 ton + Trane S9V2 60K BTU as backup | Balance point ~30°F | ~$14,800 installed pre-rebate
Option C — HP-only (cold-climate):
  Mitsubishi M-Series MUZ-FS / Hyper-Heat CCHP 2.5 ton + 8 kW strip backup | ~$16,200 installed pre-rebate
  → Eligible for IRA HEEHRA point-of-sale rebate (income-qualified) and 25D geothermal credit (preserved
    through 2032 — 25C expired 12/31/2025 and is not stacked).

SIZING ALERTS
-------------
⚠ Existing 4-ton AC is OVERSIZED by ~1.5 tons (load is 2.07 tons). This is the likely root cause
  of the homeowner's short-cycling complaint and any humidity / temperature-swing issues.
⚠ Existing 80,000 BTU input gas furnace is correctly sized (load is 34,463 BTU/hr; 80K input × 80%
  AFUE × −21.1% altitude = ~50,500 BTU/hr output, defensible at design day).
⚠ Altitude derate auto-applied (−15.8% cooling, −21.1% heating-gas) for 5,280 ft Denver elevation.
⚠ Heat-pump-default code does NOT apply in CO statewide (Denver-specific permits — check before final
  proposal). For reference, similar IRA-eligible HP-only path is increasingly customer-preferred.

ASSUMPTIONS & DATA GAPS
-----------------------
- Window orientation split [ESTIMATED] from total area — field measurement recommended for ±10% accuracy
- ACH 0.50 assumed (Average construction); a blower-door test would refine this to ±0.05
- Internal gains assume standard kitchen — confirm with homeowner (commercial-grade appliances change this)

NOTE: This is a preliminary estimate for equipment selection guidance. A formal Manual J calculation
using ACCA-approved software (Wrightsoft, HVAC-Calc, CoolCalc) is recommended for permit applications
and final equipment selection. This output is methodology-defensible against an AI homeowner-check;
the underlying line items above are reproducible from Manual J 8th ed. tables.
```

## Example Output (Light-Commercial, Manual N mode)

For a 6,200 sq ft single-story strip-mall tenant space (retail), the Manual N variant adds:
- Ventilation per ASHRAE 62.1 occupancy: retail = 7.5 CFM/person + 0.12 CFM/ft²
- Block-by-block per HVAC zone (front of house / back of house separated)
- Schedule-based diversification factor for occupancy peaks
- Output: tonnage by zone + total + recommended RTU capacity tier (Carrier 48HC, Trane Voyager, Lennox Energence) constrained to `config.brands_carried`
- 179D commercial building deduction eligibility flag (handed to `proposal-generator`'s commercial variant — do not duplicate the proposal here)

## Cross-references

- Field measurements consumed from: **operations/visual-inspection-report**
- Tonnage feeds equipment-tier selection in: **sales/proposal-generator** (residential and light-commercial)
- Heat-pump-default code path: **knowledge-base/regulations/california-2026-code.md** + state-equivalent files
- Annual-savings math is owned by: **customer-service/energy-savings-report** — link, don't duplicate
- A2L / R-454B refrigerant homeowner explanation: **customer-service/a2l-refrigerant-explainer**
