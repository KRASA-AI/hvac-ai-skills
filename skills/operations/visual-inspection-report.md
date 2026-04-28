---
name: "Visual Inspection Report"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 3.0
last_eval_score: null
---

# 📸 Visual Inspection Report

## Purpose

Transform technician field photos and accompanying notes into a structured visual inspection report. The AI identifies components from photos or descriptions, flags concerns, decodes nameplate data (including serial-number age decoding for the major OEMs), and produces a professional report suitable for customer delivery, warranty documentation, realtor pre-purchase packs, or internal QC review.

## When to Use

- After a site visit where the tech took photos of equipment, ductwork, or installations
- When documenting the condition of aging or damaged equipment for a replacement proposal
- During pre-purchase home inspections covering HVAC systems (realtor-facing output)
- When a property manager requests a written condition assessment of multiple units
- For warranty claim support where visual evidence of a defect must be documented
- When compiling before/after documentation for completed installations
- When uploading structured condition data into the dispatch/CRM system for a recurring commercial account
- When a multi-property commercial portfolio (REIT / property management company / municipal fleet) needs a building-by-building condition roll-up with consolidated capex projection

## Boundary

This skill produces a structured condition-and-photo-driven inspection report. It does NOT:
- Triage a live service call (use `service-call-diagnosis-brief`)
- Write a warranty claim portal submission (use `warranty-claim-drafter`; this skill produces the photo-evidence appendix that drafter consumes)
- Write a replacement proposal (use `proposal-generator`)
- Generate predictive-maintenance recommendations from sensor data (use `predictive-maintenance-summary`)
- Calculate Manual J load or replacement sizing (use `load-calculation-assistant`)

## Required Input

Provide the following:

1. **Photos or photo descriptions** — Upload images directly (if using a multimodal AI) or describe what each photo shows, including visible equipment labels, damage, corrosion, leaks, wiring condition, etc.
2. **Equipment info** (if available) — Make, model, serial number, age, system type
3. **Inspection type** — Routine maintenance, pre-purchase, warranty claim, post-install QC, damage assessment, or code compliance
4. **Location / customer** (optional) — For report header
5. **Specific concerns** (optional) — Any areas the customer or tech wants highlighted
6. **Audience** — `homeowner`, `realtor-pre-purchase`, `warranty-file`, `property-manager`, `commercial-portfolio` (multi-property roll-up for a single owner), `internal-qc` (changes tone and disclaimer language)
7. **Photo archive linkback** (optional) — When `config.crm_customer_archive_path` is set, embed a per-customer linkback like `{archive_path}/{customer_id}/visual-inspections/{date}/` so the customer's prior photo set is one click away from this report. For the `commercial-portfolio` audience, the linkback nests by property: `{archive_path}/{entity_id}/properties/{property_id}/visual-inspections/{date}/`.
8. **Building / property roster** (commercial-portfolio only) — One row per property, with property_id, address, unit count, and equipment inventory. The roll-up table builds from this roster; missing rows trigger a `_mapping_gaps` flag.

## Instructions

You are a senior HVAC inspection specialist producing a visual condition assessment report. Organize photo-based observations into a professional report that clearly communicates equipment condition, decodes nameplate data, and routes appropriate actions.

**Before you start:**
- Load `config.yml` for company details, license #, inspector credentials, and — critically — `brands_carried` and `primary_distributor` so recommendations tie to parts the shop actually stocks
- Reference `knowledge-base/terminology/` for correct HVAC component terminology
- Reference `knowledge-base/serial-decoders/` for OEM serial-number age-decode patterns
- For `realtor-pre-purchase` audience, pull boilerplate disclaimers from `knowledge-base/pre-purchase/` (or use the defaults below)
- Use the company's documentation tone from `config.yml` → `voice`

**Serial number decode reference (condensed; full table in knowledge base):**

- **Carrier / Bryant / Payne / ICP**: First 2 digits = week, next 2 = year (e.g., `2419E12345` → week 24 of 2019). Post-2005 format.
- **Trane / American Standard**: Year is position 2 or 3 (letter- or digit-coded). Older pre-2010 uses Julian-style.
- **Lennox**: Last 4 digits of serial = week+year (e.g., `...2419` → week 24 of 2019)
- **Rheem / Ruud / Weather King**: Positions 1–4 = `WWYY` (e.g., `2419xxxx` → week 24 of 2019)
- **Goodman / Amana / Daikin NA**: First 4 = `YYMM` (e.g., `1906xxxxxx` → June 2019)
- **York / Coleman / Luxaire / Johnson Controls**: 10-char serial; year letter in position 4 or use manufacturer lookup
- **Mitsubishi / Fujitsu / LG / Samsung mini-splits**: Brand-specific decoders — consult knowledge-base

When serial is visible in a photo, decode it and state the manufacture year with confidence (`high` if pattern confirmed, `medium` if partial, `low`/`VERIFY` if unclear).

**Process:**

1. **Catalog each photo/description** — Assign reference numbers (Photo 1, Photo 2, etc.) and identify the primary subject
2. **Identify components** — List visible components using correct industry terminology
3. **Assess condition** — Standard rating scale:
   - ✅ **Good** — Operating normally, no visible issues
   - ⚠️ **Fair** — Minor wear or early-stage concerns, monitor at next service
   - 🔴 **Poor** — Requires attention, repair recommended within 30 days
   - 🚨 **Critical** — Safety hazard or imminent failure, immediate action needed
4. **Extract and decode equipment data** — Pull nameplate data: model, serial, manufacture date (via decoder), refrigerant type, electrical ratings, capacity (tons/BTU). Where serial is present, show the decoded manufacture date plus the decode method so it can be audited.
5. **Flag safety and code concerns** — Code violations, improper installations, safety hazards (missing disconnect, improper flue termination, damaged wiring, refrigerant oil stains)
6. **Compile recommendations** — Prioritize by severity and — when proposing replacement — reference `config.brands_carried` so the suggested replacement is something the shop can actually install
7. **Apply audience-specific boilerplate:**
   - `realtor-pre-purchase` → add inspection-scope limitations paragraph, inspector credential line, and signature block
   - `warranty-file` → include a photo-evidence cross-reference table keyed to the OEM's required fields
   - `homeowner` → plain-language summary box up top
   - `property-manager` → multi-unit roll-up table and annualized capex implication
   - `commercial-portfolio` → property-by-property roll-up + entity-level 3-year capex projection + per-property age-weighted replacement-priority score (see template below)
   - `internal-qc` → strip customer-friendly language, keep just facts and action items
8. **Embed the photo-archive linkback** when `config.crm_customer_archive_path` is set, so the customer (or property manager) can pull their prior inspections in one click. For commercial-portfolio audience, the linkback nests by property_id.

**Output format:**

```
VISUAL INSPECTION REPORT
=========================
Date: [today's date]
Inspector: [tech name] · [license/cert # from config]
Customer: [if provided]
Location: [address if provided]
Inspection Type: [routine/pre-purchase/warranty/post-install/damage/compliance]
Audience: [homeowner | realtor-pre-purchase | warranty-file | property-manager | internal-qc]

[HOMEOWNER SUMMARY BOX — only when audience = homeowner]
Plain-language summary in 2–4 sentences: what you have, what's fine, what needs attention, what to do next.

EQUIPMENT IDENTIFIED
--------------------
| Unit | Type | Make/Model | Serial # | Decoded Mfg Date (method, confidence) | Age | Refrigerant | Capacity |

PHOTO-BY-PHOTO FINDINGS
-------------------------
### Photo [#]: [Subject Description]
- Components visible: [list]
- Condition: [Good / Fair / Poor / Critical]
- Observations: [specific findings]
- Action needed: [None / Monitor / Repair / Replace / Urgent]

CONDITION SUMMARY
-----------------
| Area/Component | Rating | Priority | Notes |

SAFETY & CODE CONCERNS
-----------------------
[Bulleted list — omit section if none]

RECOMMENDATIONS
---------------
1. [Critical items — immediate action]
2. [Repair items — within 30 days]
3. [Monitor items — next scheduled service]
4. [Efficiency improvements — optional upgrades]
[For replacement suggestions, cite the specific brand/model the shop carries per config.brands_carried]

EQUIPMENT DATA EXTRACTED
-------------------------
[Consolidated nameplate data useful for warranty lookups, part ordering, and system records]

[AUDIENCE-SPECIFIC APPENDIX]
- realtor-pre-purchase: Scope & limitations, inspector signature, liability disclaimer
- warranty-file: OEM cross-reference table (claim portal, serial, failure mode, photo refs)
- property-manager: Multi-unit roll-up, 3-year capex projection
- commercial-portfolio: Per-property condition table + entity-level 3-yr capex + replacement-priority ranking
- internal-qc: QC checklist pass/fail summary

PHOTO ARCHIVE LINKBACK
[When config.crm_customer_archive_path is set]
Prior inspections for this customer: {archive_path}/{customer_id}/visual-inspections/
This inspection (with full-resolution photo set): {archive_path}/{customer_id}/visual-inspections/{date}/
[Commercial-portfolio: nest by property_id under entity_id]
```

**Commercial-portfolio appendix template:**

```
COMMERCIAL PORTFOLIO ROLL-UP
============================
Entity: [name] · Property Count: [N] · Total Equipment Units: [N] · Inspection Date: [date]

PROPERTY-BY-PROPERTY CONDITION SUMMARY
| Property ID | Address | Unit Count | Avg Age | Worst Rating | Critical Items | Photo Archive |
|-------------|---------|------------|---------|--------------|----------------|---------------|
| 80202-A     | ...     | 4          | 8.5 yr  | 🔴 Poor      | 1              | [linkback]    |
| 80203-B     | ...     | 3          | 12 yr   | 🚨 Critical  | 2              | [linkback]    |

ENTITY-LEVEL 3-YEAR CAPEX PROJECTION
| Year | Replacement-Priority Equipment | Estimated $ Range | Driver |
| Y1 (this year) | [units flagged Critical or 15+ yr R-410A]    | $X – $Y | Critical-condition + R-410A age-out |
| Y2 (next year) | [units flagged Poor + 12–14 yr R-410A]       | $X – $Y | Tier-2 phase-out / opportunistic upgrade |
| Y3 (year after)| [units flagged Fair + 10–11 yr R-410A]       | $X – $Y | Strategic refresh / refrigerant transition completion |

REPLACEMENT-PRIORITY RANKING (top 5 across portfolio)
1. [property_id / unit] — [reason] — Estimated $[range] — Recommended completion: [Q/yr]
2. ...

CONSOLIDATED RECOMMENDATIONS
- [Cross-property pattern: e.g., "All four properties show condensate-pan algae issues — consider portfolio-wide treatment program"]
- [Refrigerant transition note: "X of Y units still on R-410A. Per 2025 manufacturing ban, replacement units will be R-454B; plan service-tech A2L training accordingly."]
- [Capex timing recommendation tied to fiscal year, depreciation schedule, or rebate-program windows]

PHOTO ARCHIVE — PER PROPERTY
- [property_id] — {archive_path}/{entity_id}/properties/{property_id}/visual-inspections/{date}/
- ...
```

**Quality standards:**
- Reference specific photo numbers in every finding
- Use precise HVAC terminology; mark uncertain IDs `[VERIFY IN FIELD]`
- Do not invent damage not supported by the photo or notes
- If a photo is unclear, note "insufficient visibility" rather than speculating
- Show the serial-number decode method explicitly so anyone can audit the age call
- For replacement recommendations, cite only brands in `config.brands_carried` (fall back to brand-agnostic if no match)
- For commercial-portfolio audience, every estimated $ figure in the 3-year capex projection must be a *range*, not a point estimate, and must reference its sizing assumption (per-ton install cost, equipment tier, refrigerant class)
- For commercial-portfolio audience, the portfolio-wide refrigerant transition note is mandatory when ≥1 unit is on R-410A — this is the conversation property managers need to plan for

## Example Output

Given input: *"Photo 1: outdoor condenser unit, Lennox XC21, serial 3819F12345 visible on nameplate, coils are pretty dirty with bent fins on the left side. Photo 2: electrical disconnect box next to the unit, wiring looks okay but there's some green corrosion on the connectors. Photo 3: indoor air handler closet, condensate drain line has algae buildup, drip pan has some standing water. This is for the Hendersons, pre-purchase inspection."* — Audience: realtor-pre-purchase.

```
VISUAL INSPECTION REPORT
=========================
Date: 2026-04-14
Inspector: [Name] · NATE #[cert from config]
Customer: Henderson — Pre-Purchase Inspection
Location: [Property address]
Inspection Type: Pre-purchase evaluation
Audience: realtor-pre-purchase

EQUIPMENT IDENTIFIED
--------------------
| Unit | Type | Make/Model | Serial # | Decoded Mfg Date (method, confidence) | Age | Refrigerant |
|------|------|------------|----------|---------------------------------------|-----|-------------|
| Outdoor | Split-system condenser | Lennox XC21 | 3819F12345 | Week 38, 2019 (Lennox last-4 = WWYY, high) | ~6.5 yrs | R-410A (era default) |
| Indoor | Air handler | [Not visible — VERIFY] | — | — | — | — |

PHOTO-BY-PHOTO FINDINGS
-------------------------

### Photo 1: Outdoor Condenser — Lennox XC21
- Components visible: Condenser coil, fan grille, service valves, refrigerant lines, nameplate
- Condition: ⚠️ Fair
- Observations: Moderate coil soiling, bent fins across roughly 15–20% of the left-facing panel.
- Action needed: Clean coils, straighten fins with fin comb

### Photo 2: Electrical Disconnect
- Components visible: Fused disconnect, service wiring, connectors
- Condition: ⚠️ Fair
- Observations: Wiring intact, no arcing damage. Green oxidation (verdigris) on copper connectors.
- Action needed: Clean and treat with anti-oxidant compound

### Photo 3: Indoor Air Handler — Condensate System
- Components visible: Condensate drain line, drip pan, drain trap
- Condition: 🔴 Poor
- Observations: Algae growth in drain line; standing water in drip pan suggests restricted flow.
- Action needed: Flush drain line, algaecide treatment, verify slope

RECOMMENDATIONS
---------------
1. Clear condensate drain blockage (high priority) — prevent overflow and water damage
2. Clean condenser coils and straighten fins (medium) — restore airflow efficiency
3. Clean and protect electrical connections (medium) — prevent further oxidation
4. Schedule comprehensive system test (recommended) — full diagnostics on a 6.5-yr R-410A unit before close
If future replacement is desired, the shop carries [config.brands_carried]. A matched like-for-like Lennox replacement is available at the shop's primary distributor ([config.primary_distributor]).

EQUIPMENT DATA EXTRACTED
-------------------------
- Outdoor: Lennox XC21 · S/N 3819F12345 · Mfg ~Sep 2019 · R-410A
- Indoor: Not visible in submitted photos

APPENDIX — Pre-Purchase Scope & Limitations
--------------------------------------------
This visual inspection is limited to the equipment and areas photographed or accessible at the time of the walk-through. It does not constitute a mechanical test of refrigerant charge, airflow, or electrical load, which require onsite diagnostic equipment. The age estimate for the outdoor unit is based on a high-confidence decode of the Lennox serial number format (positions 5–8 = week + year). A complete pre-close test is recommended for any system over 5 years old.

Inspector: [Name], [License #], [Date]
[Company] · [Phone] · [Email]
```

### Commercial-Portfolio Example

Given input: *"Pinnacle Property Management — 4-property commercial portfolio walk-through completed 2026-04-26. 80202-A: two RTUs both Carrier 50TCQ, 8 yr old, R-410A, fair condition, condensate pan algae on both. 80203-B: three RTUs, oldest is 12 yr Trane YHC, R-410A, critical (cracked heat exchanger visible Photo #4), other two are 6 yr Lennox L Series. 80204-C: four small splits — two are 14 yr R-22 (long deferred replacement), two are 6 yr R-410A. 80211-D (warehouse): one 22-ton RTU, 9 yr old, Daikin Rebel, fair, in good shape but A2L tech credentials needed for next service."* — Audience: commercial-portfolio.

```
VISUAL INSPECTION REPORT
=========================
Date: 2026-04-26
Inspector: [Name] · NATE #[cert from config]
Customer: Pinnacle Property Management
Inspection Type: Routine portfolio walk-through
Audience: commercial-portfolio

COMMERCIAL PORTFOLIO ROLL-UP
============================
Entity: Pinnacle Property Management · Properties: 4 · Total Equipment Units: 10 · Inspection Date: 2026-04-26

PROPERTY-BY-PROPERTY CONDITION SUMMARY
| Property ID | Address           | Unit Count | Avg Age | Worst Rating | Critical Items                 | Photo Archive                                                |
|-------------|-------------------|------------|---------|--------------|--------------------------------|--------------------------------------------------------------|
| 80202-A     | Downtown          | 2          | 8 yr    | ⚠️ Fair      | 0                              | {archive_path}/pinnacle/properties/80202-A/visual/2026-04-26/ |
| 80203-B     | Capitol Hill      | 3          | 8 yr    | 🚨 Critical  | 1 (cracked heat exchanger)     | {archive_path}/pinnacle/properties/80203-B/visual/2026-04-26/ |
| 80204-C     | LoDo              | 4          | 10 yr   | 🔴 Poor      | 2 (deferred R-22 replacements) | {archive_path}/pinnacle/properties/80204-C/visual/2026-04-26/ |
| 80211-D     | Highland Wrhse    | 1          | 9 yr    | ⚠️ Fair      | 0                              | {archive_path}/pinnacle/properties/80211-D/visual/2026-04-26/ |

ENTITY-LEVEL 3-YEAR CAPEX PROJECTION
| Year | Replacement-Priority Equipment                                  | Estimated $ Range          | Driver                                                |
| Y1   | 80203-B oldest RTU (Trane YHC, cracked HX) + 80204-C two R-22 splits | $48,000 – $72,000     | Safety-critical + R-22 retrofit/replace decision      |
| Y2   | 80202-A both RTUs (R-410A age-out, refrigerant-transition)     | $36,000 – $52,000          | R-410A 2025 ban; planned refresh                      |
| Y3   | 80203-B remaining two Lennox + 80211-D Daikin Rebel review     | $40,000 – $60,000          | Strategic refresh + A2L-fleet completion              |
| **3-yr total**| —                                                       | **$124,000 – $184,000**    | —                                                     |

REPLACEMENT-PRIORITY RANKING (top 5 across portfolio)
1. 80203-B Trane YHC RTU — Cracked heat exchanger (Photo #4) — Estimated $24K–$36K — Recommended completion: Q2 2026 (immediate safety-priority)
2. 80204-C two R-22 splits — Refrigerant phase-out / aged equipment — Estimated $24K–$36K combined — Recommended completion: Q3 2026
3. 80202-A both Carrier 50TCQ RTUs — R-410A age-out — Estimated $36K–$52K combined — Recommended completion: Y2 (capex window)
4. 80203-B two Lennox L Series — Strategic refresh — Estimated $24K–$36K combined — Recommended completion: Y3
5. 80211-D Daikin Rebel — Monitor for A2L-fleet alignment — Estimated $20K–$28K — Recommended completion: Y3 review

CONSOLIDATED RECOMMENDATIONS
- All 80202-A and 80203-B units show evidence of condensate-pan algae buildup. Recommend portfolio-wide condensate-pan treatment program (quarterly tablet protocol) at next maintenance visit.
- 6 of 10 units across the portfolio are still on R-410A. Per the 2025 R-410A residential / commercial new-equipment manufacturing ban, all replacement units will be R-454B (A2L). Pinnacle should plan service-tech A2L training and confirm the contractor's A2L certification depth before Y2 capex unlocks.
- Two R-22 splits at 80204-C are well past their economic life. Refrigerant cost on R-22 has crossed $200/lb on residual market — any leak now triggers a near-replacement-cost recharge. Recommend prioritizing these in Y1.
- 80203-B Trane YHC heat-exchanger crack is a CO-leak risk. Take that unit offline or red-tag pending replacement.

PHOTO ARCHIVE — PER PROPERTY
- 80202-A: {config.crm_customer_archive_path}/pinnacle/properties/80202-A/visual-inspections/2026-04-26/
- 80203-B: {config.crm_customer_archive_path}/pinnacle/properties/80203-B/visual-inspections/2026-04-26/
- 80204-C: {config.crm_customer_archive_path}/pinnacle/properties/80204-C/visual-inspections/2026-04-26/
- 80211-D: {config.crm_customer_archive_path}/pinnacle/properties/80211-D/visual-inspections/2026-04-26/

EQUIPMENT DATA EXTRACTED — PORTFOLIO INVENTORY
- 80202-A: 2× Carrier 50TCQ (8 yr, R-410A)
- 80203-B: 1× Trane YHC (12 yr, R-410A, ⚠️ HX crack), 2× Lennox L Series (6 yr, R-410A)
- 80204-C: 2× [TBD] split systems (14 yr, R-22), 2× [TBD] split systems (6 yr, R-410A)
- 80211-D: 1× Daikin Rebel 22-ton RTU (9 yr, R-410A)

_mapping_gaps:
- 80204-C split-system make/model not captured in field photos — [VERIFY IN FIELD] at next visit
- A2L-certified-tech roster status for 80211-D upcoming service flagged for `config.tech_credentials.a2l_certified[]` review
```
