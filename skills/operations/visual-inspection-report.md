---
name: "Visual Inspection Report"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 2.0
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

## Required Input

Provide the following:

1. **Photos or photo descriptions** — Upload images directly (if using a multimodal AI) or describe what each photo shows, including visible equipment labels, damage, corrosion, leaks, wiring condition, etc.
2. **Equipment info** (if available) — Make, model, serial number, age, system type
3. **Inspection type** — Routine maintenance, pre-purchase, warranty claim, post-install QC, damage assessment, or code compliance
4. **Location / customer** (optional) — For report header
5. **Specific concerns** (optional) — Any areas the customer or tech wants highlighted
6. **Audience** — `homeowner`, `realtor-pre-purchase`, `warranty-file`, `property-manager`, `internal-qc` (changes tone and disclaimer language)

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
   - `internal-qc` → strip customer-friendly language, keep just facts and action items

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
- internal-qc: QC checklist pass/fail summary
```

**Quality standards:**
- Reference specific photo numbers in every finding
- Use precise HVAC terminology; mark uncertain IDs `[VERIFY IN FIELD]`
- Do not invent damage not supported by the photo or notes
- If a photo is unclear, note "insufficient visibility" rather than speculating
- Show the serial-number decode method explicitly so anyone can audit the age call
- For replacement recommendations, cite only brands in `config.brands_carried` (fall back to brand-agnostic if no match)

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
