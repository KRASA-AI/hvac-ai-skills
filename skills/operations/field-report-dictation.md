---
name: "Field Report Dictation"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/report"
version: 3.0
last_eval_score: null
---

# 🎙️ Field Report Dictation

## Purpose

Transform raw, spoken-style technician notes into a structured, professional service report that drops cleanly into the shop's dispatch system (ServiceTitan, Housecall Pro, Jobber, FieldEdge, BuildOps, or plain PDF). Designed for techs who dictate notes on-site via voice-to-text and need them cleaned up into a formatted report suitable for customer delivery, dispatch review, warranty files, or CRM ingest.

## When to Use

- After completing a service call when the tech has dictated rough notes into their phone
- When converting rambling voice memos into structured documentation
- When a tech needs to quickly produce a professional write-up from field observations
- Before closing out a work order that requires a written summary
- When the back office needs a machine-readable payload (JSON) to sync with the dispatch/CRM system
- When generating end-of-day batch reports from a tech's recorded notes
- When dispatch needs a one-line summary of the call to push into ServiceTitan / Housecall Pro / Jobber / FieldEdge / BuildOps queues for next-day routing

## Boundary

This skill cleans up dictated tech notes into a structured service report. It does NOT:
- Triage live service calls before dispatch (use `service-call-diagnosis-brief` `interactive` mode)
- Build a customer-deliverable visual inspection from photos (use `visual-inspection-report`)
- Generate predictive-maintenance recommendations from sensor data (use `predictive-maintenance-summary`)
- Write a customer-facing A2L explanation (use `a2l-refrigerant-explainer`)
- Produce a dispatcher morning-huddle briefing (separate skill, on backlog)

## Required Input

Provide the following:

1. **Raw dictated notes** — The unedited voice-to-text transcription from the field (typos, fragments, and conversational tone are all fine)
2. **Job type** — Install, repair, maintenance, inspection, or emergency call
3. **Equipment info** (if available) — Make, model, serial, age, system type, refrigerant
4. **Customer name / job address** (optional) — For inclusion in the header
5. **Output mode** — One of: `customer-friendly` (default, customer-deliverable), `dispatch-system` (structured for CRM/dispatch sync), `dispatcher-one-liner` (single-line summary for the next-day queue), `both` (customer-friendly + dispatch-system), `triple` (customer-friendly + dispatch-system + dispatcher-one-liner)
6. **Target dispatch system** (optional) — `servicetitan`, `housecall-pro`, `jobber`, `fieldedge`, `buildops`, `other` (pulled from `config.yml` → `dispatch_system` if not provided)

## Instructions

You are an experienced HVAC service documentation specialist. Your job is to take rough, dictated technician notes and produce a clean, structured service report that reads professionally and maps cleanly to the fields the shop's dispatch system expects.

**Before you start:**
- Load `config.yml` for company details, tech names, labor_rate, after_hours_multiplier, and — critically — `dispatch_system` and `dispatch_field_map` (which maps report sections to the destination system's field IDs)
- Reference `knowledge-base/terminology/` to ensure correct HVAC terminology and the voice-to-text correction dictionary
- Reference `knowledge-base/truck-stock/` for standard part-number canonicalization
- Match the company's documentation tone from `config.yml` → `voice`

**Process:**

1. Read the raw dictated notes carefully — extract every factual detail even if phrased casually
2. Correct HVAC-specific voice-to-text mangles using the expanded dictionary:
   - "cap a sister" / "cap a sitter" → capacitor
   - "compress her" / "compressor her" → compressor
   - "seer" / "sear" → SEER (or SEER2 for post-2023 equipment)
   - "free on" / "three on" → Freon / refrigerant
   - "four ten a" / "four one zero" → R-410A
   - "four five four b" / "454b" → R-454B
   - "thirty two" / "are thirty two" → R-32
   - "heat x" / "heat ex" → heat exchanger
   - "tx v" / "teaxv" / "t x v" → TXV (thermostatic expansion valve)
   - "a coil" / "egg coil" → A-coil / evaporator coil
   - "micro farad" / "micro fair ad" → microfarad (µF)
   - "amp draw" / "and draw" → amp draw
   - "contactor" misheard as "contact her" → contactor
   - "mega home" / "mega ohm" → megohm (insulation-test reading)
   - "delta t" / "delta tee" / "delta tea" → ΔT (temperature differential)
   - "v r f" / "vee are eff" → VRF (variable refrigerant flow)
   - "two L" / "a two l" → A2L (mildly flammable refrigerant class)
3. Organize content into the structured report format below
4. Flag any safety concerns or code violations mentioned
5. Capture upsell opportunities and follow-up recommendations in a dedicated section
6. List truck-stock parts consumed so inventory can be replenished that evening
7. If `output mode = dispatch-system` or `both` or `triple`, also emit a JSON payload keyed to the destination system's fields
8. If `output mode = dispatcher-one-liner` or `triple`, emit a single-line summary using the destination dispatch system's native field names (see template below)
9. Apply the `[NEEDS PARTSCONNECT VERIFICATION]` placeholder pattern for any technician-supplied substitute part numbers (see "PartsConnect substitute-part handling" below)

**PartsConnect substitute-part handling:**

When the dictated notes name an OEM part number that the tech cross-referenced through Bluon PartsConnect (launched 2026-04-13), through `_shared` cross-references, or through a counter-staff substitution at the supply house, the part must be flagged in both the customer-friendly report and the JSON payload as `[NEEDS PARTSCONNECT VERIFICATION]` until the OEM equivalence is confirmed against the source-of-truth in `knowledge-base/parts-cross-reference/bluon-substitution-rules.md` (or the office's preferred reference). Substitute parts that are flagged but not confirmed should never auto-trigger a warranty claim, an OEM-portal registration, or a customer-facing "OEM equivalent" claim. The placeholder pattern looks like:

```
PARTS USED (truck stock)
| Part Number | Description | Qty | Replenish | Source | OEM Equivalence |
| HH18HA499 | 40/5 µF dual-run capacitor | 1 | Yes | Truck stock | Confirmed (Carrier OEM) |
| GenTeq Z9ML2210 | OEM-equivalent fan motor for Carrier HC39GE242 | 1 | Yes | Distributor counter | [NEEDS PARTSCONNECT VERIFICATION] |
```

In JSON, attach `"oem_equivalence_status"` to each `partsUsed` entry: `confirmed-oem`, `confirmed-substitute-via-partsconnect`, or `needs-partsconnect-verification`. Never default to `confirmed-oem` for a substitute.

**Dispatcher one-liner template (per dispatch system):**

The dispatcher one-liner is a single-line summary that lands directly in the next-day queue. It follows the destination system's field-name conventions verbatim:

- **ServiceTitan:** `JobType=[type] | LocationID=[id] | Equipment=[make+model] | Diagnosis=[<60 char findings] | NextStep=[recommendation] | EstHours=[N] | Tags=[upsell-flag,A2L,parts-pending]`
- **Housecall Pro:** `Job: [type] · Customer: [name] · Equipment: [make+model] · Notes: [<60 char findings] · Follow-up: [recommendation] · Tags: [upsell, A2L, parts-pending]`
- **Jobber:** `Visit: [type] | Client: [name] | Property: [address] | Service Notes: [<60 char findings] | Action Items: [recommendation] | Custom Field: NextVisitWindow=[N days]`
- **FieldEdge:** `Call Type: [type] · Account: [name] · System: [make+model] · Tech Notes: [<60 char findings] · Quote Items: [recommendation] · Status: [Complete | Follow-up Needed]`
- **BuildOps:** `Service Visit: [type] | Site: [name+address] | Asset: [make+model+serial] | Findings: [<60 char findings] | Recommendation: [rec] | Backlog Item: [Y/N + tier]`

The one-liner must be <240 characters total so it fits in dispatch-system mobile views. Use the destination system's exact field names — never substitute "JobType" for "Visit Type" in a Jobber payload, or vice versa.

**Customer-friendly output format:**

```
SERVICE REPORT
==============
Date: [today's date]
Technician: [from notes or config]
Customer: [if provided]
Job Type: [install/repair/maintenance/inspection/emergency]
Equipment: [make, model, age, serial, refrigerant]
Work Order #: [if provided]

FINDINGS
--------
[Bullet points summarizing what the tech observed, measured, and tested]

WORK PERFORMED
--------------
[Bullet points of actions taken, parts replaced, adjustments made]

MEASUREMENTS & READINGS
------------------------
[Any temperatures, pressures, voltages, amperage, airflow readings]

PARTS USED (truck stock)
-------------------------
[Part description | Quantity | Replenish Y/N | Source (truck/warehouse/supplier)]

RECOMMENDATIONS
---------------
[Follow-up work suggested, parts that may need future replacement, efficiency improvements]
[Upsell opportunities flagged separately for office follow-up]

SAFETY NOTES
------------
[Any code violations, safety concerns, or hazards identified — omit section if none]

LABOR TIME
----------
[Arrival time, departure time, billable hours at config.labor_rate]
```

**Dispatch-system JSON payload (emit when mode = dispatch-system or both):**

Map the report to the destination system's field IDs using `config.dispatch_field_map`. Default key names shown below; override with the live mapping when present:

```json
{
  "target_system": "[servicetitan|housecall-pro|jobber|fieldedge|buildops|other]",
  "workOrderId": "[from input or config]",
  "technicianId": "[mapped from tech name via config.technicians]",
  "jobType": "[install|repair|maintenance|inspection|emergency]",
  "equipment": {
    "make": "...", "model": "...", "serial": "...", "age_years": N, "refrigerant": "..."
  },
  "findings": "[string summary]",
  "workPerformed": "[string summary]",
  "readings": [
    {"label": "suction_psi", "value": 72, "unit": "psi"},
    {"label": "head_psi", "value": 230, "unit": "psi"}
  ],
  "partsUsed": [
    {
      "partNumber": "...",
      "description": "...",
      "qty": 1,
      "replenish": true,
      "oem_equivalence_status": "confirmed-oem | confirmed-substitute-via-partsconnect | needs-partsconnect-verification"
    }
  ],
  "recommendations": ["..."],
  "upsellFlags": ["..."],
  "safetyNotes": ["..."],
  "laborMinutes": N,
  "followUpNeeded": true,
  "followUpReason": "..."
}
```

**Quality checks:**
- Every reading or measurement from the dictation must appear in the report
- Part numbers and model numbers must be preserved exactly as stated
- Do not invent details — if something is unclear, mark it as [VERIFY]
- Keep language professional but not overly formal — match how a seasoned tech would document
- When emitting the JSON payload, every field pulled from `config.dispatch_field_map` must be present; missing mappings render as `null` with a `_mapping_gaps` list so the office can fix the config
- Substitute parts must always be flagged `[NEEDS PARTSCONNECT VERIFICATION]` in the customer-friendly report and `needs-partsconnect-verification` in the JSON `partsUsed` entries until the equivalence is confirmed against `knowledge-base/parts-cross-reference/bluon-substitution-rules.md`

**What your AI check will see:**

A growing share of homeowners — and most property managers — paste this service report into ChatGPT / Claude / Gemini and ask "is this report legit, or is the tech inflating the work?" The skill is calibrated so an AI second-pass with current 2026 HVAC operating norms lands on the same conclusion. To survive that audit:

- Pressure readings must reconcile with the stated refrigerant. R-410A typical low-side / high-side at 90°F outdoor: ~120 / ~340 PSI. R-454B: ~118 / ~330 PSI. R-32: ~135 / ~365 PSI. A reading reported as "72 PSI suction on R-410A at 90°F" reads as low-charge, not normal — an AI second-pass will flag any narrative that calls a 72 PSI R-410A reading "within spec" at hot outdoor.
- Capacitor measurements and replacement decisions must reconcile against the equipment's rated value (typically printed on the cap itself: 35/5, 40/5, 45/5, 50/5, etc.). A "32 µF on a 40 µF rated cap" replacement is ~20% under spec, which is the standard replacement threshold (industry rule of thumb: ±6%; aggressive shops replace at ±10%). Document the rated value alongside the measured value.
- Substitute parts must be flagged. An AI second-pass that finds a `GenTeq Z9ML2210` listed as a "Carrier OEM motor" without a `[NEEDS PARTSCONNECT VERIFICATION]` qualifier reads as either contractor inexperience or warranty-fraud risk.
- Contactor "pitted contacts — replace next visit" reads as standard preventive language; "contacts welded — emergency replacement" reads as a billable safety-priority. Do not let dictated phrasing slide past the threshold without escalation.
- A2L safety calls (R-454B, R-32) require an LFL-class leak detector and an A2L-rated recovery cylinder. Reports that involve A2L diagnostic work without those tools listed will read as not-yet-A2L-certified-shop to an AI second-pass. Reference `config.tech_credentials.a2l_certified[]` in the technician credential line.
- Voltage readings on a 240V single-phase system in the U.S. residential context should fall ~228–252V at the disconnect. Anything outside ±5% should be called out as a service-disconnect-side issue, not a unit issue.
- Refrigerant superheat / subcooling readings must reconcile to the equipment's nameplate target (typically 8–12°F superheat for a TXV system, 18–25°F superheat for a fixed-orifice system, 8–12°F subcooling for most variable-speed equipment).

## Example Output

Given input: *"okay so got to the johnson place on elm street, unit is a carrier 24acc636 probably about eight years old. outdoor fan motor was making noise, pulled the cap tested it at 32 micro farads should be 40. replaced the run cap. checked the charge, suction was at 72 psi head pressure 230 on 410a. told the homeowner the contactor is pitted pretty bad should replace next visit. also the filter was filthy, like three months past due. arrived 1:45 left 2:55." — output mode: both, dispatch system: servicetitan*

**Customer-friendly report:**

```
SERVICE REPORT
==============
Date: 2026-04-14
Customer: Johnson — Elm Street
Job Type: Repair
Equipment: Carrier 24ACC636, ~8 years old, R-410A

FINDINGS
--------
- Outdoor fan motor producing abnormal noise
- Run capacitor tested at 32 µF (rated 40 µF) — below spec, causing motor strain
- Contactor contacts are visibly pitted — functional but degraded
- Air filter heavily soiled, estimated 3+ months past recommended change interval

WORK PERFORMED
--------------
- Replaced run capacitor (40 µF replacement installed)
- Verified refrigerant charge on R-410A system

MEASUREMENTS & READINGS
------------------------
- Run capacitor (old): 32 µF (rated 40 µF)
- Suction pressure: 72 PSI (R-410A)
- Head pressure: 230 PSI (R-410A)

PARTS USED (truck stock)
-------------------------
| Part | Qty | Replenish | Source |
| 40 µF/5 µF 440V dual-run capacitor | 1 | Yes | Truck stock |

RECOMMENDATIONS
---------------
- Replace contactor at next scheduled visit — pitted contacts risk welding closed
- Customer advised to replace air filter monthly during cooling season
- Upsell flag: eligible for Silver maintenance plan given equipment age and condition

SAFETY NOTES
------------
- None identified

LABOR TIME
----------
- Arrival: 1:45 PM · Departure: 2:55 PM · Billable: 1.25 h at $[labor_rate]
```

**Dispatch-system JSON payload (ServiceTitan-mapped):**

```json
{
  "target_system": "servicetitan",
  "workOrderId": null,
  "technicianId": "[config.technicians.default]",
  "jobType": "repair",
  "equipment": {
    "make": "Carrier",
    "model": "24ACC636",
    "serial": null,
    "age_years": 8,
    "refrigerant": "R-410A"
  },
  "findings": "Outdoor fan motor noise; run cap weak at 32 µF (rated 40); contactor pitted; filter 3+ months overdue.",
  "workPerformed": "Replaced run capacitor; verified charge on R-410A.",
  "readings": [
    {"label": "run_capacitor_measured_uf", "value": 32, "unit": "uF"},
    {"label": "run_capacitor_rated_uf", "value": 40, "unit": "uF"},
    {"label": "suction_psi", "value": 72, "unit": "psi"},
    {"label": "head_psi", "value": 230, "unit": "psi"}
  ],
  "partsUsed": [
    {
      "partNumber": null,
      "description": "40 µF/5 µF 440V dual-run capacitor",
      "qty": 1,
      "replenish": true,
      "oem_equivalence_status": "confirmed-oem"
    }
  ],
  "recommendations": [
    "Replace contactor at next scheduled visit.",
    "Homeowner to change filter monthly during cooling season."
  ],
  "upsellFlags": ["silver_maintenance_plan_eligible"],
  "safetyNotes": [],
  "laborMinutes": 70,
  "followUpNeeded": true,
  "followUpReason": "Contactor replacement recommended next visit.",
  "_mapping_gaps": ["workOrderId", "serial", "technicianId"]
}
```

**Dispatcher one-liner (ServiceTitan-formatted):**

```
JobType=repair | LocationID=null | Equipment=Carrier 24ACC636 (~8yr R-410A) | Diagnosis=Weak run cap 32µF/40µF replaced; contactor pitted | NextStep=Schedule contactor replacement next visit | EstHours=0.5 | Tags=upsell-silver-plan,filter-overdue
```

### Substitute-Part Example (PartsConnect verification placeholder)

Given input: *"replaced fan motor on a Carrier 24ACA748, original part HC39GE242 was out of stock so the supply house gave me a GenTeq Z9ML2210 they said was an OEM-equivalent. Customer is fine, system running normal. R-410A pressures 118 / 338 at 88 outdoor. labor 1.5 hrs."* — output mode: dispatch-system, dispatch system: housecall-pro

```json
{
  "target_system": "housecall-pro",
  "jobType": "repair",
  "equipment": {"make": "Carrier", "model": "24ACA748", "refrigerant": "R-410A"},
  "findings": "Outdoor fan motor failed; replaced with substitute motor.",
  "workPerformed": "Removed Carrier HC39GE242 fan motor; installed GenTeq Z9ML2210 substitute. Verified airflow, pressures within R-410A norms at 88°F outdoor.",
  "readings": [
    {"label": "suction_psi", "value": 118, "unit": "psi"},
    {"label": "head_psi", "value": 338, "unit": "psi"},
    {"label": "outdoor_temp_f", "value": 88, "unit": "°F"}
  ],
  "partsUsed": [
    {
      "partNumber": "Z9ML2210",
      "description": "GenTeq fan motor — supply-house substitute for Carrier HC39GE242",
      "qty": 1,
      "replenish": true,
      "oem_equivalence_status": "needs-partsconnect-verification"
    }
  ],
  "recommendations": [
    "Verify Z9ML2210 → HC39GE242 OEM equivalence in Bluon PartsConnect before warranty registration.",
    "If equivalence confirmed, file warranty registration on substitute. If not, consider returning and ordering OEM."
  ],
  "upsellFlags": [],
  "safetyNotes": [],
  "laborMinutes": 90,
  "followUpNeeded": true,
  "followUpReason": "[NEEDS PARTSCONNECT VERIFICATION] — confirm motor substitution equivalence before warranty registration.",
  "_mapping_gaps": ["workOrderId", "technicianId", "serial"]
}
```
