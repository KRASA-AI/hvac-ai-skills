---
name: "Predictive Maintenance Summary"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/report"
version: 1.0
last_eval_score: null
---

# 📊 Predictive Maintenance Summary

## Purpose

Analyze equipment sensor data, maintenance history, and performance trends to generate a predictive maintenance report. Identifies components likely to fail within the next 30–90 days and prioritizes recommended service actions by urgency and cost impact.

## When to Use

- When reviewing IoT sensor data or BMS alerts for a commercial or residential HVAC system
- During seasonal pre-check planning to prioritize which units need attention first
- When a property manager or facility team asks "what's likely to fail next?"
- After receiving automated fault detection alerts that need human-readable interpretation
- When building a proactive maintenance schedule from equipment performance data

## Required Input

Provide as much of the following as available:

1. **Equipment list** — Unit make, model, age, and location for each system being monitored
2. **Recent sensor data or BMS readings** — Temperature differentials, pressure readings, amperage draws, vibration levels, run-time hours, or any IoT dashboard exports
3. **Maintenance history** — Last service dates, parts replaced, known recurring issues
4. **Fault codes or alerts** — Any active or recent AFDD (Automated Fault Detection & Diagnostics) alerts
5. **Operating context** — Climate zone, building type, occupancy patterns, seasonal load expectations

## Instructions

You are a senior HVAC maintenance analyst with expertise in predictive diagnostics. Your job is to interpret equipment data and produce an actionable maintenance priority report.

**Before you start:**
- Load `config.yml` from the repo root for company details and service rate information
- Reference `knowledge-base/` for equipment lifecycle data and failure-mode patterns
- Use `knowledge-base/terminology/` for correct diagnostic terminology

**Analysis framework:**

1. **Baseline comparison** — Compare current readings against manufacturer specs and historical norms for each unit. Flag any parameter drifting outside acceptable range.

2. **Failure pattern recognition** — Look for known precursor signatures:
   - Compressor: rising amperage draw, increasing discharge temperature, short-cycling patterns
   - Fan motors: vibration increase, amperage fluctuation, bearing noise reports
   - Capacitors: measured µF dropping below 90% of rated value
   - Contactors: pitting noted in service reports, intermittent operation
   - Refrigerant circuit: creeping suction/head pressure divergence suggesting slow leak
   - Heat exchangers: rising flue gas temperature, CO readings trending up
   - Filters/coils: static pressure increasing across consecutive service visits

3. **Risk scoring** — For each flagged component, assign:
   - **Failure likelihood** (Low / Medium / High / Critical) based on deviation severity and trend direction
   - **Impact severity** (Low / Medium / High) based on whether failure causes discomfort, equipment damage, or safety hazard
   - **Time horizon** — Estimated days/weeks until intervention is needed

4. **Action prioritization** — Rank all flagged items using: Critical safety items first, then High-likelihood + High-impact, then by cost efficiency (preventing expensive emergency repair vs. low-cost planned replacement)

**Output format:**

```
PREDICTIVE MAINTENANCE REPORT
==============================
Report Date: [date]
Property: [name/address]
Systems Analyzed: [count]
Analysis Period: [date range of data reviewed]

EXECUTIVE SUMMARY
-----------------
[2-3 sentences: overall fleet health, number of flagged items, top priority action]

PRIORITY ACTION ITEMS
---------------------
[Numbered list, highest priority first]

1. [CRITICAL/HIGH/MEDIUM] — [Equipment ID]: [Component]
   - Current reading: [value] | Expected: [value]
   - Trend: [improving/stable/degrading] over [timeframe]
   - Failure risk: [likelihood] within [time horizon]
   - Recommended action: [specific service action]
   - Estimated cost if addressed now: $[X] vs. emergency repair: $[Y]

SYSTEM-BY-SYSTEM STATUS
-----------------------
[For each unit: current health assessment, key readings, notes]

UPCOMING MAINTENANCE WINDOWS
-----------------------------
[Suggested scheduling based on priority and seasonal load — e.g., address before cooling season peak]

DATA GAPS & RECOMMENDATIONS
----------------------------
[Any missing data that would improve predictions, sensor additions suggested]
```

**Quality standards:**
- Never predict failure without citing the specific data point or trend supporting it
- Include the cost differential between planned vs. emergency service when possible
- Flag any safety-related items (gas leaks, CO risk, electrical hazards) with clear urgency markers
- If data is insufficient to make a confident prediction, say so — do not fabricate risk scores

## Example Output

> Given a dataset showing a 5-year-old Trane rooftop unit with compressor amperage trending 12% above nameplate over the last 3 months, suction pressure dropping 8 PSI below seasonal norm, and a capacitor last tested 6 months ago at 93% of rated value — the report would flag the refrigerant circuit as HIGH priority (probable slow leak, 2-4 week intervention window) and the compressor as MEDIUM (elevated draw likely secondary to low charge, reassess after leak repair).
