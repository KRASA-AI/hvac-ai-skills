# A2L Refrigerant Service-Handling Reference (R-454B / R-32)

Service-and-warranty-facing companion to `regulations/a2l-r454b-transition.md`. That file covers the
regulatory/customer-communication story; **this file covers the hands-on service, top-off, recovery, and
recordkeeping rules** that maintenance-agreement and warranty-claim skills defer to.

> **Posture:** Directional reference, not a substitute for the equipment's installation instructions.
> Specific test pressures, charge limits, and torque values **vary by OEM and model — always follow the
> unit's installation manual and nameplate, and applicable code (UL 60335-2-40, local mechanical code)
> over any universal figure quoted here.** Where this file gives a number, treat it as "typical —
> verify against OEM spec." A2L handling requirements are current EPA 608 + A2L safety training territory;
> this file does not replace certification.

## Who may service A2L systems

- Current EPA **Section 608** certification (Type II or Universal for most residential/light-commercial work).
- Completed **A2L-specific safety training** (ACCA, RSES, or OEM channel program) covering handling,
  charging, leak detection, and emergency procedures.
- **A2L-rated service equipment**: manifold gauges, recovery machine, vacuum pump, and refrigerant
  identifier all labeled A2L-compatible. Do not use R-410A-only recovery equipment on A2L.

## Charging and top-off rules (the part contracts get wrong)

- **R-454B is a near-azeotropic (zeotropic) blend — charge in the liquid state** to preserve composition.
  Vapor charging is not acceptable. R-32 is single-component and less sensitive, but follow OEM guidance.
- **Do not "top off" an A2L system of unknown composition.** If composition is in doubt, recover,
  evacuate, and recharge to the nameplate weighed-in charge. This is the default for any A2L leak call.
- **No drop-in / no mixing.** An R-454B condenser requires a matched R-454B-rated indoor coil and metering
  device. Never mix A2L and R-410A components or refrigerants.
- **Charge limits** per UL 60335-2-40 apply based on room volume for some indoor/ductless applications —
  confirm the system's allowable charge for the conditioned space before adding refrigerant.
- **Top-off allowances in maintenance agreements should be priced ~30–40% higher for A2L than for R-410A**
  (directional) to reflect the added cylinder-handling, recovery, and matched-component labor. Do **not**
  promise a single flat lb allowance (e.g., "2 lb included") across both refrigerants without
  distinguishing R-410A from A2L — the handling cost is not the same.

## Leak testing and evacuation

- **Pressure/standing leak test with dry nitrogen (never oxygen), to the value in the equipment's
  installation manual.** Test pressures are OEM- and side-specific; **do not rely on a single universal
  number.** As a rule of thumb the A2L line-set test pressure runs at or above the corresponding R-410A
  procedure for the same equipment, but the manual is authoritative — quote the manual's psig, not a
  memorized figure. (Older shop habits carried lower nitrogen test pressures for R-410A; A2L OEM
  procedures generally specify higher — confirm per model.)
- **Recover refrigerant before brazing/torch work** on a charged line set where code or the OEM requires
  it; provide stronger ventilation and control ignition sources (torches, switchgear) during service.
- Pull a proper vacuum and verify with a micron gauge before recharging.

## Recordkeeping / warranty documentation

- Log A2L service in the **Section 608 recovery/charging logbook**: refrigerant type and quantity
  recovered/added, cylinder IDs, date, technician cert.
- For **warranty refrigerant/leak claims**, an electronic-leak-search alone is generally insufficient
  documentation — record the leak location, the repair, the weighed-in recharge, and the post-repair
  standing-pressure/vacuum verification. Many OEMs require the matched-component AHRI certificate and the
  install-date/serial to adjudicate an A2L compressor or coil claim.

## Flammability figures — canonical, and deliberately hedged

**This section is the single source of truth for LFL numbers in this repo.** Any skill that states an
LFL must cite this file rather than asserting its own figure. Two skills previously carried *different*
and *both-wrong* numbers (9.5% v/v in the customer explainer, 6.2% v/v in the diagnosis brief), which is
exactly the kind of contradiction an AI second-pass catches.

- **R-454B — published LFL values genuinely disagree across sources**, roughly **7.7% to 11.8% by
  volume** depending on whether you read the ASHRAE 34 addendum, the ISO 817 figure, or an OEM/blend
  supplier datasheet. **Do not assert a single precise number.** State the band, or say "roughly 8–12%
  by volume depending on the reference," and move on. The comparison that actually matters to a customer
  holds across the entire band and is what should be said out loud:
  - natural gas (methane) LFL ≈ **5%** v/v
  - propane LFL ≈ **2.1%** v/v
  - → R-454B needs **several times the concentration of natural gas** in the air before it can burn at
    all, and it needs far more ignition energy. That statement is true at 7.7% and at 11.8%, so it never
    has to be walked back.
- **R-32** — LFL ≈ **14%** v/v (higher, i.e. harder to ignite, than R-454B).
- **R-1234yf** — LFL ≈ **6.2%** v/v. ⚠️ This is the figure that was mistakenly copied into a skill as
  "R-454B's LFL." R-1234yf is a *component* of R-454B (~31%), not the blend. Do not use its LFL for the
  blend.
- **Safety-procedure posture:** where a number drives a *safety* decision (ventilation, ignition-source
  standoff), design to the **most conservative published figure**, not the most favorable one. Where a
  number drives *customer reassurance*, use the band plus the natural-gas/propane comparison.

## Cross-references

- Regulatory/customer-facing framing, GWP figures, and the 2026 install-deadline reconsideration:
  `regulations/a2l-r454b-transition.md`. (LFL figures live **here**, not there.)
- Incentive/rebate handling for A2L-ready replacements: `regulations/incentives-landscape.md`.

## Source anchors (verify current)

- EPA Section 608 program and AIM Act rulemaking.
- UL 60335-2-40 (A2L charge-limit standard) and equipment OEM installation instructions.
- ACCA / RSES / OEM A2L safety-training curricula.

*Last reviewed 2026-07-06. Figures are directional; the equipment installation manual and current code
win over anything in this file. Update when OEM procedures or the A2L standard change.*
