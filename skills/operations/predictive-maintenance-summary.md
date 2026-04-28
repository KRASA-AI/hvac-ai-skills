---
name: "Predictive Maintenance Summary"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 3.0
last_eval_score: null
---

# 📊 Predictive Maintenance Summary

## Purpose

Analyze equipment sensor data, maintenance history, and performance trends to generate a predictive maintenance report. Identifies components likely to fail within the next 30–90 days, prioritizes recommended service actions by urgency and cost impact, and produces output in three formats: a customer-facing report, a structured JSON payload that drops directly into the contractor's dispatch system as pre-filled work orders, and a truck-stock replenishment block so the tech arrives with the right parts on the first visit.

## When to Use

- When reviewing IoT sensor data or BMS alerts for a commercial or residential HVAC system
- During seasonal pre-check planning to prioritize which units need attention first
- When a property manager or facility team asks "what's likely to fail next?"
- After receiving automated fault detection alerts that need human-readable interpretation
- When building a proactive maintenance schedule from equipment performance data
- When a maintenance-plan customer's annual PM visit is coming up and dispatch needs to pre-load the work order with the right scope

## Boundary vs. adjacent skills

- **`operations/service-call-diagnosis-brief.md`** runs on an active, reported failure. This skill runs *ahead* of failure to prevent the service call.
- **`operations/visual-inspection-report.md`** consumes a tech's site-visit observations. This skill consumes sensor streams, BMS exports, and maintenance-history rollups.
- **`operations/field-report-dictation.md`** produces the per-visit service record. This skill consumes *prior* field reports as part of the history input.
- **`sales/maintenance-agreement-writer.md`** writes the maintenance contract. This skill produces the report the contract promises.

## Required Input

Provide as much of the following as available:

1. **Equipment list** — Unit make, model, age, serial number (for OEM decode), tonnage/BTU, refrigerant type, and location for each system being monitored.
2. **Recent sensor data or BMS readings** — Temperature differentials (supply/return), head and suction pressures, amperage draws per component, vibration levels, run-time hours, static pressure, or any IoT dashboard exports (CSV / JSON).
3. **Maintenance history** — Last service dates, parts replaced, known recurring issues, and last µF capacitor readings. Use `knowledge-base/failure-modes/` for brand-specific wear-curve adjustments when the customer's system is on `config.brands_carried`.
4. **Fault codes or alerts** — Any active or recent AFDD (Automated Fault Detection & Diagnostics) alerts, rooftop-unit controller faults (Trane Tracer, Carrier iVu, Daikin DIII-NET), or thermostat error history.
5. **Operating context** — Climate zone (`config.climate_zone`), building type, occupancy patterns, seasonal load expectations, and the upcoming seasonal cutover (spring cool-down / fall heat-up).
6. **Output mode** — One of: `homeowner report` (residential, customer-friendly), `property-manager report` (commercial / light-commercial, operating-cost-per-sqft framing), `dispatch JSON` (structured work-order payload mapped to `config.dispatch_system`), `truck-stock block` (parts-needed-to-roll list only), or `combined packet` (all of the above). Default: `combined packet`.
7. **Technician assignment** (optional) — Defaults to `config.technicians[].on_call` for the covering tech or the tech of record on `config.technicians[].assigned_accounts` for maintenance-plan customers. The JSON output uses `config.dispatch_field_map` to map our internal fields into the dispatch system's schema (ServiceTitan, Housecall Pro, Jobber, FieldEdge, BuildOps).
8. **Tone** (optional) — Defaults to `config.yml` → `voice`.

## Instructions

You are a senior HVAC maintenance analyst with expertise in predictive diagnostics. Your job is to interpret equipment data and produce an actionable maintenance priority report that dispatch can dispatch from and the tech can walk into the building with.

**Before you start:**

- Load `config.yml` for company name, service rates (`config.labor_rate`, `config.after_hours_multiplier`), dispatch system (`config.dispatch_system`), dispatch field map (`config.dispatch_field_map`), technician roster (`config.technicians`), brands carried (`config.brands_carried`), truck-stock mapping (`config.truck_stock_mapping`), and voice.
- Reference `knowledge-base/failure-modes/` for brand-specific wear-curve data (Trane / Carrier / Bryant / Lennox / Rheem / Goodman / Mitsubishi / Daikin defaults). If a brand-specific file is missing, fall back to the generic wear curves in the Analysis framework below and flag the brand in the Data Gaps section.
- Reference `knowledge-base/truck-stock/` for per-brand replacement-part canonicalization (capacitor µF ratings by unit SKU, contactor pole counts, blower motor HP/voltage).
- Reference `knowledge-base/serial-decoders/` when a serial number is provided but manufacture date / warranty position is unknown.
- Use the company's communication tone from `config.yml` → `voice`.

**Analysis framework:**

1. **Baseline comparison.** Compare current readings against manufacturer specs and historical norms for each unit. Flag any parameter drifting outside acceptable range. Use brand-specific tolerances from `knowledge-base/failure-modes/[brand].md` when available.

2. **Failure pattern recognition.** Look for known precursor signatures:
   - Compressor: rising amperage draw (>10% over nameplate), increasing discharge temperature, short-cycling, suction superheat drift
   - Fan motors (indoor / outdoor): vibration increase, amperage fluctuation, bearing noise in prior service reports, ECM fault codes
   - Capacitors: measured µF dropping below 90% of rated value — start capacitors drift faster than run capacitors
   - Contactors: pitting noted in service reports, chattering, intermittent operation, voltage drop across contacts
   - Refrigerant circuit: creeping suction/head pressure divergence, superheat/subcool out of spec, evaporator TXV hunting
   - Heat exchangers: rising flue gas temperature, CO readings trending up, flame-rollout, visible cracks on borescope
   - Filters / coils: static pressure increasing across consecutive visits, condensate pan acid pH drift
   - Reversing valves (heat pumps): mode-switch delay, partial-shift symptoms, bleed-by on defrost
   - Defrost boards: incorrect defrost intervals, sensor faults
   - Controls / thermostats: communication faults on Trane Tracer / Carrier iVu / BMS

3. **Risk scoring.** For each flagged component, assign:
   - **Failure likelihood** (Low / Medium / High / Critical) based on deviation severity and trend direction
   - **Impact severity** (Low / Medium / High) based on whether failure causes discomfort, equipment damage, or safety hazard
   - **Time horizon** — Estimated days/weeks until intervention is needed
   - **Confidence** (High / Medium / Low) — How much data supports the prediction

4. **Action prioritization.** Rank all flagged items using: critical safety items first, then High-likelihood + High-impact, then by cost efficiency (preventing expensive emergency repair vs. low-cost planned replacement).

5. **Truck-stock planning.** For each action item, list the parts the tech should load using `config.truck_stock_mapping`. If a part isn't in stock, flag it for dispatch to pre-order. Include OEM part numbers when the brand is on `config.brands_carried`. For substitute parts not yet canonicalized in `knowledge-base/truck-stock/`, leave a `[NEEDS PARTSCONNECT VERIFICATION]` placeholder rather than asserting an OEM substitute.

6. **Per-account SLA-clock.** When the customer is a commercial / fleet account with an active maintenance agreement, pull the per-tier SLA from `config.maintenance_agreements[].sla_tier` (typically tier-1 4-hour, tier-2 next-day, tier-3 48-hour). If a flagged failure prediction crosses the SLA window before the recommended scheduling date, flag a `_sla_window_alert` so dispatch can pre-empt the breach. This closes the "no per-account SLA-clock map" gap from prior cycles.

**Output format — homeowner report (residential):**

```
[COMPANY NAME] — PREDICTIVE MAINTENANCE REPORT
Prepared for: [Customer name]
Property: [Address]
Report Date: [Today's date]
Systems Analyzed: [Count]
Analysis Period: [Date range of data reviewed]
Prepared by: [Technician/analyst name from config]

EXECUTIVE SUMMARY
-----------------
[2-3 sentences: overall fleet health, number of flagged items, top priority action, and the most valuable "fix now to avoid bigger bill later" recommendation]

PRIORITY ACTION ITEMS
---------------------
1. [CRITICAL/HIGH/MEDIUM] — [Equipment ID]: [Component]
   - Current reading: [value] | Expected: [value]
   - Trend: [improving/stable/degrading] over [timeframe]
   - Failure risk: [likelihood] within [time horizon]  (confidence: [High/Med/Low])
   - Recommended action: [specific service action]
   - Parts needed: [OEM part # if on config.brands_carried, else generic]  (status: on truck / order)
   - Estimated cost if addressed now: $[X] vs. emergency repair: $[Y]
   - Suggested schedule window: [before cooling-season peak / within 30 days / etc.]

SYSTEM-BY-SYSTEM STATUS
-----------------------
[For each unit: current health assessment, key readings, notes, next-PM-due date]

UPCOMING MAINTENANCE WINDOWS
-----------------------------
[Suggested scheduling based on priority and seasonal load. Tie to config.climate_zone cutover dates.]

DATA GAPS & RECOMMENDATIONS
----------------------------
[Any missing data that would improve predictions. Specific sensor additions suggested.]

[Company name] • [phone] • [website] • license #[X]
```

**Output format — property-manager report (commercial / light-commercial):**

Same structure, with these changes: add a fleet-level dashboard table at the top (Unit ID / Age / Health score / Next action / Est. cost / Downtime risk), include operating-cost-per-square-foot deltas per recommended action, replace "homeowner" with "property manager / asset owner" framing, and add a line-item budget projection for the next 12 months organized by quarter.

**Output format — dispatch JSON (structured work-order payload):**

```json
{
  "report_id": "[UUID]",
  "generated_at": "[ISO timestamp]",
  "customer_id": "[from config.dispatch_field_map.customer_id]",
  "site_id": "[from config.dispatch_field_map.site_id]",
  "dispatch_system": "[ServiceTitan | HousecallPro | Jobber | FieldEdge | BuildOps]",
  "work_orders": [
    {
      "priority": "CRITICAL | HIGH | MEDIUM | LOW",
      "equipment_id": "[unit tag]",
      "equipment_make_model": "[Carrier 48TCDA04]",
      "serial_number": "[decoded via knowledge-base/serial-decoders if provided]",
      "component": "[compressor | capacitor | contactor | ...]",
      "predicted_failure_horizon_days": [int],
      "confidence": "high | medium | low",
      "recommended_action": "[text]",
      "parts_required": [
        {"part_number": "[OEM or generic]", "description": "[text]", "qty": [int], "on_truck": [bool]}
      ],
      "estimated_labor_minutes": [int],
      "suggested_technician_id": "[from config.technicians]",
      "customer_facing_notes": "[text for app/email]",
      "internal_notes": "[text]",
      "_mapping_gaps": ["[field name the dispatch system expects but wasn't available]"],
      "_sla_window_alert": "[set if predicted_failure_horizon_days < SLA-tier window from config.maintenance_agreements; otherwise null]",
      "_partsconnect_verification_required": [bool]
    }
  ],
  "truck_stock_replenishment": [
    {"part_number": "[X]", "qty_needed": [int], "backorder_risk": "[low|med|high]"}
  ]
}
```

The `_mapping_gaps` array makes missing-field issues visible rather than silently absent. Every work order must include it, even if empty.

**Output format — truck-stock block:**

A compact parts-to-load list formatted for morning-of-dispatch handoff:
```
ROLL-LIST FOR [TECH NAME], [DATE]
Stop 1: [Customer, address] — [Equipment]
  [ ] Capacitor 45/5 µF (GE 97F9008) — on truck
  [ ] Contactor 2-pole 40A (Packard C240B) — on truck
  [ ] Filter 20x25x1 MERV-11 (1) — on truck
  [ ] TXV valve kit Carrier EF19ZZ052 — ORDER AM, ETA 10am
Stop 2: ...
```

**Output format — combined packet:** All four stapled together. Default when `output_mode` is unspecified.

**Quality standards:**

- Never predict failure without citing the specific data point or trend supporting it.
- Include the cost differential between planned vs. emergency service when possible.
- Flag any safety-related items (gas leaks, CO risk, electrical hazards) with clear urgency markers and the specific sensor reading or observation that triggered the flag.
- If data is insufficient to make a confident prediction, say so in the Confidence field and list the gap in Data Gaps & Recommendations — do not fabricate risk scores.
- Dispatch JSON output must set `confidence: low` and populate `_mapping_gaps` when any input field is missing rather than hallucinating a value.
- Pull OEM part numbers only when the unit's brand is in `config.brands_carried` and the canonicalization exists in `knowledge-base/truck-stock/`. Otherwise use generic part description.
- Tie recommendations to the upcoming seasonal cutover so the "when" is concrete ("before cooling-season peak, week of [date]"), not vague.
- For substitute parts not yet canonicalized in `knowledge-base/truck-stock/`, leave a `[NEEDS PARTSCONNECT VERIFICATION]` placeholder rather than asserting an OEM substitute — the tech is expected to verify the cross-reference in Bluon PartsConnect on arrival.

## What your AI check will see

A growing share of property managers, asset owners, and HOA boards now paste the predictive-maintenance report into ChatGPT, Claude, or Gemini before approving the work order ("does this report justify the spend? are these failure predictions actually backed by the readings? am I being upsold?"). The output of this skill must pass that check:

- Every flagged component must cite the specific reading, trend, or maintenance-history data point that supports its risk score — an AI reviewer will flag "Failure risk: HIGH" without an underlying numeric anchor as fabrication
- The cost-differential block (planned vs. emergency) must be calibrated to local-market labor / parts pricing pulled from `config.labor_rate` and `config.after_hours_multiplier`, not made up — an AI reviewer will challenge round-number "$2,100" emergency-cost claims that don't reconcile to the labor rate × estimated hours math
- The seasonal-window framing ("before cooling-season peak, week of May 18") must reconcile to `config.climate_zone`'s historical first-90°F-day curve — an AI reviewer with weather-pattern data will flag "before cooling-season peak in October" as wrong
- The Data Gaps section must be present and honest. If the readings supplied don't actually support a HIGH-confidence prediction, the report must say so. An AI reviewer will catch a HIGH-confidence call backed by 30 days of one-channel data
- The dispatch JSON `_mapping_gaps` array must be present even when empty. An AI reviewer parsing the JSON will treat a missing `_mapping_gaps` field as a sign the writer skipped the schema discipline
- For commercial property-manager reports specifically, the operating-cost-per-sqft delta must reconcile to a stated baseline — an AI reviewer will call out "saves $0.18/sqft" claims with no baseline as marketing copy
- Truck-stock substitutions flagged `[NEEDS PARTSCONNECT VERIFICATION]` are explicitly *not* asserted as OEM-correct; an AI reviewer should read this as the report being honest about its uncertainty, not handwaving

## Example Output

**Input:** A 5-year-old Trane rooftop unit (Voyager YCD060) on a 12,000 sqft light-commercial building in Phoenix. Dispatch system: ServiceTitan. Sensor data over the last 90 days shows compressor amperage trending 12% above nameplate (14.2A vs 12.7A rated), suction pressure dropping 8 psi below seasonal norm, and a capacitor last tested 6 months ago at 93% of rated value (83 µF on a 90 µF rated cap). No active AFDD alerts. Maintenance history shows a condenser coil clean 18 months ago and a contactor replacement 2 years ago. Technician assigned: Marco R. Output mode: combined packet.

**Expected output behavior:**

The homeowner-style report (actually property-manager, since it's commercial) flags the refrigerant circuit as HIGH priority (probable slow leak, 2-4 week intervention window, HIGH confidence based on the combined amperage + suction-pressure signature) and the capacitor as MEDIUM (83 µF is below the 81 µF failure threshold for a 90 µF cap — replace opportunistically at the leak-repair visit). The dispatch-JSON output builds two work orders with ServiceTitan-mapped fields (`customer_id`, `location_id`, `job_type_id` pulled through `config.dispatch_field_map.servicetitan`), pre-populates the parts list (leak-seek dye + refrigerant recover/recharge kit + 90 µF 440V run capacitor Trane CPT02095), assigns Marco R, and includes a `_mapping_gaps: ["business_unit_id"]` note because the ST business unit wasn't in config. The truck-stock block tells Marco to roll with the capacitor and the leak-seek kit on the truck; the TXV replacement kit is flagged as a morning-of-pre-order in case the leak turns out to be in the metering device rather than a coil joint. The property-manager framing includes a $420 planned-repair vs. $2,100 emergency-repair cost differential and recommends the visit be slotted before the Phoenix cooling peak (third week of May).
