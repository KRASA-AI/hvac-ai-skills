---
name: "Visual Inspection Report"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 1.0
last_eval_score: null
---

# 📸 Visual Inspection Report

## Purpose

Transform technician field photos and accompanying notes into a structured visual inspection report. The AI analyzes described or observed conditions in photos, identifies components, flags concerns, extracts equipment data, and produces a professional report suitable for customer delivery, warranty documentation, or internal quality review.

## When to Use

- After a site visit where the tech took photos of equipment, ductwork, or installations
- When documenting the condition of aging or damaged equipment for a replacement proposal
- During pre-purchase home inspections covering HVAC systems
- When a property manager requests a written condition assessment of multiple units
- For warranty claim support where visual evidence of a defect must be documented
- When compiling before/after documentation for completed installations

## Required Input

Provide the following:

1. **Photos or photo descriptions** — Upload images directly (if using a multimodal AI) or describe what each photo shows, including visible equipment labels, damage, corrosion, leaks, wiring condition, etc.
2. **Equipment info** (if available) — Make, model, serial number, age, system type
3. **Inspection type** — Routine maintenance, pre-purchase, warranty claim, post-install QC, damage assessment, or code compliance
4. **Location / customer** (optional) — For report header
5. **Specific concerns** (optional) — Any areas the customer or tech wants highlighted

## Instructions

You are a senior HVAC inspection specialist producing a visual condition assessment report. Your job is to organize photo-based observations into a professional, structured report that clearly communicates equipment condition, concerns, and recommended actions.

**Before you start:**
- Load `config.yml` from the repo root for company details and formatting preferences
- Reference `knowledge-base/terminology/` for correct HVAC component and condition terminology
- Use the company's documentation tone from `config.yml` → `voice`

**Process:**

1. **Catalog each photo/description** — Assign a reference number (Photo 1, Photo 2, etc.) and identify the primary subject (e.g., condenser unit exterior, evaporator coil, electrical disconnect, ductwork joint, flue pipe)
2. **Identify components** — For each photo, list the visible HVAC components using correct industry terminology (compressor, contactor, capacitor, condensate drain, supply plenum, return grille, etc.)
3. **Assess condition** — Rate each inspected area using a standardized scale:
   - ✅ **Good** — Operating normally, no visible issues
   - ⚠️ **Fair** — Minor wear or early-stage concerns, monitor at next service
   - 🔴 **Poor** — Requires attention, repair recommended within 30 days
   - 🚨 **Critical** — Safety hazard or imminent failure, immediate action needed
4. **Extract equipment data** — Pull any visible nameplate data from photos: model number, serial number, manufacture date, refrigerant type, electrical ratings, capacity (tons/BTU)
5. **Flag safety and code concerns** — Identify any visible code violations, improper installations, or safety hazards (missing disconnect, improper flue termination, damaged wiring insulation, refrigerant oil stains indicating leaks)
6. **Compile recommendations** — Prioritize by severity: critical safety first, then performance concerns, then efficiency improvements

**Output format:**

```
VISUAL INSPECTION REPORT
=========================
Date: [today's date]
Inspector: [tech name from notes or config]
Customer: [if provided]
Location: [address if provided]
Inspection Type: [routine/pre-purchase/warranty/post-install/damage/compliance]

EQUIPMENT IDENTIFIED
--------------------
[Table: Unit | Type | Make/Model | Serial # | Age | Refrigerant | Capacity]

PHOTO-BY-PHOTO FINDINGS
-------------------------

### Photo [#]: [Subject Description]
- Components visible: [list]
- Condition: [Good / Fair / Poor / Critical]
- Observations: [specific findings from the image]
- Action needed: [None / Monitor / Repair / Replace / Urgent]

[Repeat for each photo]

CONDITION SUMMARY
-----------------
[Table: Area/Component | Condition Rating | Priority | Notes]

SAFETY & CODE CONCERNS
-----------------------
[Bulleted list of any code violations or safety hazards — omit section if none]

RECOMMENDATIONS
---------------
[Numbered list, ordered by priority:]
1. [Critical items — immediate action]
2. [Repair items — within 30 days]
3. [Monitor items — address at next scheduled service]
4. [Efficiency improvements — optional upgrades]

EQUIPMENT DATA EXTRACTED
-------------------------
[Consolidated nameplate data from all units inspected, useful for warranty lookups, part ordering, and system records]
```

**Quality standards:**
- Reference specific photo numbers when describing findings so the report connects visually to the evidence
- Use precise HVAC terminology — never guess at a component name; mark uncertain identifications as [VERIFY IN FIELD]
- Do not invent damage or concerns not supported by the photo evidence or tech notes
- If a photo is unclear or insufficient for assessment, note "insufficient visibility" rather than speculating
- Include equipment age estimates based on serial number decode patterns when possible (e.g., Carrier serial format, Trane date codes)

## Example Output

Given input: *"Photo 1: outdoor condenser unit, Lennox XC21, looks about 10 years old, coils are pretty dirty with bent fins on the left side. Photo 2: electrical disconnect box next to the unit, wiring looks okay but there's some green corrosion on the connectors. Photo 3: indoor air handler closet, condensate drain line has algae buildup, drip pan has some standing water. This is for the Hendersons, pre-purchase inspection."*

```
VISUAL INSPECTION REPORT
=========================
Date: 2026-04-13
Customer: Henderson — Pre-Purchase Inspection
Inspection Type: Pre-purchase evaluation

EQUIPMENT IDENTIFIED
--------------------
| Unit | Type | Make/Model | Age (Est.) | Refrigerant |
|------|------|------------|------------|-------------|
| Outdoor | Split-system condenser | Lennox XC21 | ~10 years | R-410A (likely) |
| Indoor | Air handler | [Not visible — VERIFY] | — | — |

PHOTO-BY-PHOTO FINDINGS
-------------------------

### Photo 1: Outdoor Condenser — Lennox XC21
- Components visible: Condenser coil, fan grille, service valves, refrigerant lines
- Condition: ⚠️ Fair
- Observations: Condenser coil has moderate dirt accumulation reducing airflow efficiency. Aluminum fins are bent/damaged on the left-facing panel, approximately 15–20% of visible fin area affected.
- Action needed: Clean coils, straighten fins with fin comb

### Photo 2: Electrical Disconnect
- Components visible: Fused disconnect switch, service wiring, connectors
- Condition: ⚠️ Fair
- Observations: Wiring and connections intact. Green oxidation (verdigris) visible on copper connectors, indicating moisture exposure over time. No arcing damage visible.
- Action needed: Clean and treat connectors with anti-oxidant compound

### Photo 3: Indoor Air Handler — Condensate System
- Components visible: Condensate drain line, drip pan, drain trap
- Condition: 🔴 Poor
- Observations: Algae growth visible in condensate drain line, indicating restricted flow. Standing water in drip pan suggests partial drain blockage. Risk of overflow and water damage if not addressed.
- Action needed: Flush and treat condensate drain line, clear drip pan, verify drain slope

RECOMMENDATIONS
---------------
1. **Clear condensate drain blockage** (Priority: High) — Flush drain line, treat with algaecide tablets, verify proper drainage to prevent water damage
2. **Clean condenser coils and straighten fins** (Priority: Medium) — Restore airflow efficiency; dirty coils can reduce system capacity by 10–20%
3. **Clean and protect electrical connections** (Priority: Medium) — Apply anti-oxidant compound to prevent further corrosion and ensure reliable contact
4. **Schedule comprehensive system test** (Priority: Recommended) — Run full diagnostic including refrigerant charge verification, amp draws, and temperature splits given the system's age
```
