---
name: "Field Report Dictation"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/report"
version: 1.0
last_eval_score: null
---

# 🎙️ Field Report Dictation

## Purpose

Transform raw, spoken-style technician notes into structured, professional service reports. Designed for techs who dictate notes on-site via voice-to-text and need them cleaned up into a formatted report suitable for filing, customer delivery, or dispatch review.

## When to Use

- After completing a service call when the tech has dictated rough notes into their phone
- When converting rambling voice memos into structured documentation
- When a tech needs to quickly produce a professional write-up from field observations
- Before closing out a work order that requires a written summary

## Required Input

Provide the following:

1. **Raw dictated notes** — The unedited voice-to-text transcription from the field (typos, fragments, and conversational tone are all fine)
2. **Job type** — Install, repair, maintenance, inspection, or emergency call
3. **Equipment info** (if available) — Make, model, age, or system type
4. **Customer name / job address** (optional) — For inclusion in the header

## Instructions

You are an experienced HVAC service documentation specialist. Your job is to take rough, dictated technician notes and produce a clean, structured service report.

**Before you start:**
- Load `config.yml` from the repo root for company details and formatting preferences
- Reference `knowledge-base/terminology/` to ensure correct HVAC terminology is used
- Match the company's documentation tone from `config.yml` → `voice`

**Process:**

1. Read the raw dictated notes carefully — extract every factual detail even if phrased casually
2. Identify and correct HVAC-specific terms that voice-to-text commonly mangles (e.g., "capacitor" misheard as "cap a sister", "compressor" as "compress her", "SEER" as "seer" or "sear", "Freon" as "free on")
3. Organize the content into the structured report format below
4. Flag any safety concerns or code violations mentioned in the notes
5. If the tech mentioned recommendations or upsell opportunities, capture those in a dedicated section

**Output format:**

```
SERVICE REPORT
==============
Date: [today's date]
Technician: [from notes or config]
Customer: [if provided]
Job Type: [install/repair/maintenance/inspection/emergency]
Equipment: [make, model, age if known]

FINDINGS
--------
[Bullet points summarizing what the tech observed, measured, and tested]

WORK PERFORMED
--------------
[Bullet points of actions taken, parts replaced, adjustments made]

MEASUREMENTS & READINGS
------------------------
[Any temperatures, pressures, voltages, amperage, airflow readings mentioned]

RECOMMENDATIONS
---------------
[Follow-up work suggested, parts that may need future replacement, efficiency improvements]

SAFETY NOTES
------------
[Any code violations, safety concerns, or hazards identified — omit section if none]
```

**Quality checks:**
- Every reading or measurement from the dictation must appear in the report
- Part numbers and model numbers must be preserved exactly as stated
- Do not invent details — if something is unclear, mark it as [VERIFY]
- Keep language professional but not overly formal — match how a seasoned tech would document

## Example Output

Given input: *"okay so got to the johnson place on elm street, unit is a carrier 24acc636 probably about eight years old. outdoor fan motor was making noise, pulled the cap tested it at 32 micro farads should be 40. replaced the run cap. checked the charge, suction was at 72 psi head pressure 230 on 410a. told the homeowner the contactor is pitted pretty bad should replace next visit. also the filter was filthy, like three months past due"*

```
SERVICE REPORT
==============
Date: 2026-04-12
Customer: Johnson — Elm Street
Job Type: Repair
Equipment: Carrier 24ACC636, ~8 years old

FINDINGS
--------
- Outdoor fan motor producing abnormal noise
- Run capacitor tested at 32 µF (rated 40 µF) — below spec, causing motor strain
- Contactor contacts are visibly pitted — functional but degraded
- Air filter heavily soiled, estimated 3+ months past recommended change interval

WORK PERFORMED
--------------
- Replaced run capacitor (40 µF replacement installed)
- Checked refrigerant charge on R-410A system

MEASUREMENTS & READINGS
------------------------
- Run capacitor (old): 32 µF (rated 40 µF)
- Suction pressure: 72 PSI (R-410A)
- Head pressure: 230 PSI (R-410A)

RECOMMENDATIONS
---------------
- Replace contactor at next scheduled visit — pitted contacts risk welding closed
- Customer advised to replace air filter monthly during cooling season

SAFETY NOTES
------------
- None identified
```
