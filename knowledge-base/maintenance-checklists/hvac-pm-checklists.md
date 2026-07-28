# Preventive-Maintenance Checklists by Equipment Type — Reference Notes

Ground-truth reference for any HVAC AI skill that builds a preventive-maintenance visit scope —
primarily `sales/maintenance-agreement-writer.md`, which is **gated** on this file ("Use the
knowledge-base checklist that matches each unit type; do not generate a generic list"), and
secondarily `operations/predictive-maintenance-summary.md` / `operations/field-report-dictation.md`
for PM-visit context. Skills should **defer to this file** rather than inventing a checklist per
run, so the same equipment type gets the same scope every time regardless of which conversation
produced it.

**Currency:** Reviewed 2026-07-28. These are manufacturer-generic PM task lists aligned to
ACCA Standard 4 (Maintenance) practice and typical OEM service-manual PM sections. They are a
starting scaffold, not a substitute for the specific OEM's published PM checklist — a shop should
layer brand-specific steps (from `knowledge-base/manufacturers/[oem].md`, when populated) on top of
the relevant list below rather than replacing it.

**Fail-safe rule:** if a unit type is not on this list (e.g., VRF, geothermal/ground-source, RTU
economizer-specific items), use the closest matching type below, note the substitution in the
agreement's checklist line, and flag it for the office to add a dedicated entry — never silently
present a generic "HVAC tune-up" line for an equipment type this file doesn't yet cover.

---

## How skills should use this file

1. **One checklist per covered equipment type on the agreement.** Split-AC, heat pump, gas furnace,
   boiler, mini-split/ductless, and package unit get their own list below — never combine them into
   one generic bullet set. If a plan covers multiple types (e.g., AC + furnace), stack the relevant
   lists under their own headers, exactly as `maintenance-agreement-writer.md`'s example output does.
2. **Seasonal split.** Cooling-side tasks run at the spring/cooling visit; heating-side tasks run at
   the fall/heating visit. A Silver-tier plan (1 visit/system/year) runs only the side relevant to
   that system (cooling side for an AC-only system, heating side for a furnace-only system).
3. **Tier scope, not tier price.** This file defines *what tasks happen*; pricing and visit
   frequency by tier live in `maintenance-agreement-writer.md`'s tier framework, not here.

---

## Split System Air Conditioner (cooling visit)

- Wash/rinse condenser coil; straighten bent fins with a fin comb
- Check refrigerant charge against superheat (fixed-orifice) or subcooling (TXV) targets per OEM spec
- Verify temperature split across the evaporator coil (target ~16–22°F at design conditions)
- Compressor and condenser-fan amperage vs. nameplate RLA/FLA
- Capacitor microfarad reading vs. rated value (replace if outside ±6%; more aggressive shops use ±10%)
- Contactor inspection — pitting, chatter, pull-in voltage
- Evaporator coil visual inspection; condensate drain flush with biocide tablet or treatment
- Blower motor amp draw; belt/pulley or ECM check as applicable
- Thermostat calibration and communication check (smart-thermostat firmware if applicable)
- Filter inspection/replacement (size + MERV rating logged)
- Disconnect and whip inspection; verify accessible and properly rated
- Written condition report left with homeowner, including any Fair/Poor items for follow-up

## Heat Pump (cooling visit — cooling-mode side)

- All Split System AC cooling-visit items above, plus:
- Reversing valve operation check (confirm clean mode switch, no bleed-by)
- Defrost board / defrost-sensor function check (bench or live cycle, per OEM procedure)
- Crankcase heater function check (if equipped)
- Outdoor coil frost pattern check (uneven frost can indicate a metering-device or charge issue)

## Heat Pump (heating visit — heating-mode side)

- Heating-mode operation and reversing-valve switch confirmed
- Defrost cycle initiated and timed against OEM spec; confirm termination logic
- Auxiliary/strip-heat staging check (confirm aux heat is not running concurrently with the compressor
  outside the OEM's designed overlap, which would indicate a control fault)
- Balance-point / lockout-temperature setpoints verified against the system design (see
  `operations/load-calculation-assistant.md` output for the property's design balance point)
- Outdoor coil and fan motor inspected for cold-weather debris (leaves, ice)
- Filter inspection/replacement
- Thermostat calibration and heat-pump-specific settings verified (e.g., emergency-heat lockout,
  compressor lockout temperature)
- Written condition report

## Gas Furnace (heating visit)

- Combustion analysis (CO, CO2, O2, stack temperature) with a calibrated analyzer
- Heat exchanger visual inspection for cracks, corrosion, or scaling (borescope where accessible)
- Flame sensor cleaning and ignition-sequence check
- Gas pressure check — manifold and inlet, vs. OEM nameplate spec
- Pressure switch and inducer-motor function check
- Blower motor amperage and capacitor check
- Flue pipe and vent termination inspection (proper slope, no blockage, correct clearance)
- Condensate trap and drain check on condensing (≥90% AFUE) models — confirm free-flowing, no algae
- Thermostat calibration and filter replacement
- Carbon monoxide test at a supply register (post-commissioning safety check, not just at the unit)
- Written condition report; any AFUE claimed on the agreement's equipment schedule must reconcile
  with whether this checklist drains a condensate trap — see the AFUE cross-check quality standard
  in `maintenance-agreement-writer.md`

## Boiler (heating visit — hydronic, residential/light-commercial)

- Combustion analysis (CO, CO2, O2, stack temperature) — same discipline as gas furnace above
- Heat exchanger / firebox visual inspection
- Low-water cutoff (LWCO) test — confirm the boiler shuts down on simulated low-water condition
- Pressure relief valve function check (never plugged, discharge line routed per code)
- System pressure and expansion-tank precharge check
- Circulator pump(s) — amperage, noise, and seal-leak check
- Zone valve(s) operation check per zone
- Aquastat / boiler-reset-curve control settings verified
- Flue and venting inspection (same posture as gas furnace)
- Written condition report

## Mini-Split / Ductless (per indoor head, cooling and heating visit combined given single-visit
economics on most residential ductless plans)

- Indoor unit filter cleaning (washable filters — most residential ductless units)
- Indoor coil and blower-wheel visual inspection; clean if soiled (blower-wheel buildup is the most
  common ductless comfort complaint and is easy to miss on a rushed visit)
- Condensate drain and pump (if equipped) check — ductless drain pans clog more often than ducted
  systems due to smaller passages
- Outdoor unit coil wash and fan check
- Refrigerant line-set insulation and joint inspection (visual — full leak check only if charge
  symptoms present)
- Remote/control interface function check per head
- Line-set and wall-sleeve weatherproofing check at the exterior penetration
- Written condition report, itemized per indoor head/zone

## Package Unit (RTU-style, single cabinet — residential or light-commercial)

- All Split System AC cooling-visit items above, applied to the packaged coil/compressor, plus:
- Belt-drive blower inspection (many package units are belt-drive, not ECM) — belt tension and wear
- Curb/roof-mount seal inspection (weatherproofing at the curb, no standing water)
- Economizer damper operation check, if equipped (light-commercial units) — confirm damper actuator
  responds and doesn't fail open/closed
- Combustion-side items from the Gas Furnace list above, if the package unit is gas/electric combo
- Written condition report

---

## Change log

- 2026-07-28 — File created to resolve the standing gap where `sales/maintenance-agreement-writer.md`
  referenced `knowledge-base/maintenance-checklists/` as the mandatory, non-generic source for the
  PM checklist per equipment type, but the directory did not exist. Six equipment-type checklists
  (split AC, heat pump cooling + heating, gas furnace, boiler, mini-split, package unit) established
  at ACCA Standard 4 / typical OEM PM-scope depth; brand-specific detail deferred to
  `knowledge-base/manufacturers/[oem].md` as that file gets populated.
