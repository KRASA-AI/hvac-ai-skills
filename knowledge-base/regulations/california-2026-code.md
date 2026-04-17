# California 2026 Building Code Cycle — HVAC Reference Notes

Quick-reference facts for any HVAC AI skill that produces customer-facing content, proposals, permit narratives, or installation specs for projects in California. Use as ground truth for the 2026 code cycle that took effect January 1, 2026. Refresh this document at minimum every six months — California amends interim updates more often than the three-year cycle suggests.

## Effective date and scope

- The 2026 code cycle applies to permit applications submitted on or after **January 1, 2026**. Permits applied for in 2025 may continue under the 2022 cycle through their normal expiration window.
- Triggered codes for HVAC work:
  - **2025 California Energy Code (Title 24, Part 6)** — the building energy efficiency standard
  - **2025 California Green Building Standards Code (CALGreen, Part 11)** — sustainability and indoor air quality
  - **2025 California Mechanical Code (CMC)** — equipment installation and ducting
- Local jurisdictions may amend stricter overlays (e.g., Berkeley, San Francisco, Palo Alto local reach codes). Always verify the local AHJ has not added requirements above state baseline.

## The headline change for HVAC

**Heat pumps are now the prescriptive default for space heating across all 16 climate zones.** This is the single most consequential 2026 cycle change for HVAC contractors operating in California.

- Under the prescriptive compliance path, residential new construction and replacement HVAC equipment defaults to a heat pump.
- Gas furnaces are not banned. A gas-furnace project must comply via the **performance compliance path** (energy modeling that demonstrates the gas-furnace design meets or beats the prescriptive heat-pump baseline). This adds documentation, modeling cost, and review time — typically a paid Title 24 consultant is involved.
- The economic effect is that gas-furnace replacements now carry a soft cost premium (modeling fees, longer permit cycles) on top of equipment cost.

## Heat pump control requirements (new in 2026 cycle)

These are the technician-facing settings the installing contractor must commission and certify:

- **Defrost delay:** must be set to **≥90 minutes** between defrost cycles. The installer self-certifies this on the Installation Certificate.
- **Supplemental electric heat lockout:** must be enabled to lock out resistance heat above **35°F outdoor air temperature**, capped at **2.7 kW per ton of cooling capacity**. This prevents resistance-heat over-firing that destroys the energy-savings argument for the heat pump in the first place.
- **Smart thermostat compatibility:** the controller must support the lockout and defrost delay settings. Many older entry-tier thermostats do not — verify model compatibility before the install.

Failure to commission these settings is the most common 2026-cycle field-inspection finding so far. Field reports from Q1 2026 show roughly 1 in 4 first-time inspections being kicked back over defrost delay or supplemental lockout misconfiguration.

## Documentation chain (Certificate workflow)

Every Title 24 HVAC project goes through a three-document chain. Skipping or back-dating any of these is the fastest path to a permit revocation.

1. **Certificate of Compliance (CF1R)** — submitted with the permit application before issuance. Demonstrates the design meets Title 24 (prescriptive or performance path). Typically prepared by a Title 24 consultant or the design engineer.
2. **Installation Certificate (CF2R)** — completed during construction by the installing contractor. Documents that what was actually installed matches the CF1R design. Includes the heat-pump control settings above.
3. **Certificate of Acceptance (CF3R)** — completed by a HERS rater (Home Energy Rating System) for verification-required measures. Required for refrigerant charge verification, duct leakage testing, fan watt draw, and certain other measures.

Forms must be uploaded to the appropriate registry (CHEERS, CalCERTS, or other approved HERS provider) and signed by the responsible party. Local AHJs increasingly require digital uploads — paper-only submissions are being rejected in many jurisdictions.

## CALGreen (Part 11) HVAC-relevant items

- **Whole-house mechanical ventilation** required per ASHRAE 62.2-2019 (referenced) for new dwellings and significant alterations. Most contractors comply via continuous bath-fan ventilation, ERV, or HRV.
- **MERV 13 filtration** required for new construction and many alterations. The system must be designed to handle the additional static pressure — a common field issue when retrofitting MERV 13 onto an existing duct system not sized for it.
- **Refrigerant management plan** required for commercial projects above the size threshold; documents leak prevention and recovery procedures.

## California Mechanical Code (CMC) HVAC-relevant items

- A2L refrigerant equipment is permitted statewide. CMC adopts UL 60335-2-40 by reference for room volume / charge limits and leak detection sensor requirements.
- Combustion appliance zone (CAZ) testing remains required for fuel-burning equipment retrofits — increasingly relevant as gas-furnace replacements push into the performance path and require documentation that the existing CAZ tests post-install.

## Permitting platform landscape (April 2026)

- **Permitio.ai** (launched March 2026) is the first AI agent purpose-built for HVAC permits. Pulls job data from FSM, auto-generates compliant submissions, pays fees, tracks approvals, and books inspections. Early users report ~20× faster approval cycles. California-aware out of the box.
- **PermitFlow** and **AutoHVAC** are platform-style alternatives without the AI-agent layer.
- The prompt-addressable workflow gap: **drafting the narrative / project-description fields inside the permit application**. AI agents like Permitio handle the structured fields; the unstructured narrative fields ("describe scope of work," "system description") are still a contractor-written task and a candidate for a future `admin/permit-narrative-drafter.md` skill.

## Common 2026-cycle proposal language anchors

When generating customer-facing proposals or explainers for California projects, anchor on these defensible facts:

- "Under California's 2025 Energy Code that took effect January 1, 2026, a heat pump is the prescriptive default for space heating in your climate zone."
- "A gas-furnace replacement is still permitted but requires a performance compliance pathway — additional modeling and documentation that adds approximately $400–$1,200 to project soft costs in most jurisdictions, not counting longer permit timelines."
- "Your installer is required to commission specific heat-pump controls (defrost delay, supplemental heat lockout) and certify them on a state-tracked Installation Certificate."
- "MERV 13 filtration is now required by California's green building code on most projects and was sized into our duct design."

Avoid:
- Claiming heat pumps are "mandatory" or "required" — they are the prescriptive default, not the only legal option.
- Citing federal 25C / IRA tax credits without checking expiration status (the 25C credit expired for installations completed after 12/31/2025 — see `customer-service/rebate-and-tax-credit-navigator.md`).
- Quoting any specific local jurisdiction's reach-code without verifying it for that exact city/county.

## Source landscape

- California Energy Commission — official Title 24 Part 6 reference and CF1R/CF2R/CF3R form library
- CALGreen reference — Part 11 of Title 24
- ICC California Mechanical Code — annotated CMC reference
- HERS providers: CHEERS, CalCERTS — registry submissions and rater directories
- AHJ-by-AHJ overlays vary; consult local building department for any project in a known reach-code city

## Refresh cadence

- Reread quarterly. California's interim updates (e.g., emergency standards adoptions, AHJ-level overlays) appear off-cycle.
- Next planned refresh: 2026-07-15.
- Trigger an out-of-cycle refresh if any of: emergency CEC standards adoption, federal preemption ruling on state HVAC codes, or major AHJ reach-code adoption (e.g., a top-50 California city banning a class of equipment).
