# Skill Evaluation Summary — 2026-04-12

## Overview

Evaluated 8 HVAC skills across 6 dimensions (clarity, specificity, industry_fit, output_quality, personalization, efficiency). This is the **first evaluation run** — no prior results exist for regression comparison.

## Score Rankings (Pre-Improvement)

| Rank | Skill | Category | Score | Status |
|------|-------|----------|-------|--------|
| 1 | service-call-diagnosis-brief | operations | 9.20 | PASS |
| 2 | predictive-maintenance-summary | operations | 8.90 | PASS |
| 3 | field-report-dictation | operations | 8.80 | PASS |
| 4 | warranty-claim-drafter | admin | 4.40 | FAIL |
| 5 | energy-savings-report | customer-service | 4.00 | FAIL |
| 5 | load-calculation-assistant | operations | 4.00 | FAIL |
| 5 | maintenance-agreement-writer | sales | 4.00 | FAIL |
| 5 | proposal-generator | sales | 4.00 | FAIL |

**Passing threshold:** 7.0 | **Improvement threshold:** 6.0

**Pass rate:** 3/8 (37.5%)

## Key Findings

### Pattern: Boilerplate vs. Bespoke
The repo has a stark quality divide. Three operations skills (field-report-dictation, predictive-maintenance-summary, service-call-diagnosis-brief) were written with deep HVAC domain expertise — specific output formats, real terminology, example outputs, and practical workflows. The other five skills use a generic boilerplate template with identical instructions that could apply to any industry.

### Cross-Skill Dimension Weaknesses
- **Specificity** is the weakest dimension across failing skills (avg 3.0). None of the boilerplate skills define an output format or provide example outputs.
- **Industry fit** averages 4.0 across failing skills — they mention HVAC concepts in the purpose line but revert to generic instructions.
- **Output quality** averages 3.2 across failing skills — without output formats or examples, the AI has no anchor for quality.

### Category Health
- **Operations:** Strong. 3 of 4 skills pass. Only load-calculation-assistant needs work (now improved).
- **Sales:** Both skills failed. Critical gap since proposals and maintenance agreements are core revenue activities.
- **Customer Service:** Only skill failed. Energy savings reports are a key sales tool.
- **Admin:** Only skill failed. Warranty claims are a significant time sink for office staff.

## Improvements Made

Three skills were rewritten from scratch, targeting the bottom of the rankings:

| Skill | Before | After | Change | Key Improvements |
|-------|--------|-------|--------|------------------|
| energy-savings-report | 4.00 | 8.80 | +4.80 | Added SEER/AFUE calculation methodology, age-based efficiency estimation, payback period formula, incentive integration, detailed output format with assumptions |
| load-calculation-assistant | 4.00 | 9.10 | +5.10 | Added full Manual J methodology with U-values, infiltration rates, duct losses, internal gains, 6-point sizing mistake checklist, altitude adjustment |
| proposal-generator | 4.00 | 8.90 | +4.90 | Added good-better-best tier structure with SEER2 ranges, value justification framework, pricing presentation with incremental cost bridging, side-by-side comparison, incentive integration |

**Post-improvement pass rate:** 6/8 (75%)

## Still Failing (Next Priority)

| Skill | Score | Priority Fix |
|-------|-------|-------------|
| warranty-claim-drafter | 4.40 | Needs manufacturer claim form structure, RMA process, required documentation checklist, example claim |
| maintenance-agreement-writer | 4.00 | Needs coverage tier definitions, visit frequency, parts/labor coverage, exclusions, renewal terms, equipment-specific checklists |

## Recommendations

1. **Immediate:** Rewrite warranty-claim-drafter and maintenance-agreement-writer using the same depth as the improved skills.
2. **Cross-linking:** The improved skills now reference each other (proposal-generator ↔ energy-savings-report ↔ load-calculation-assistant). Continue building these connections.
3. **Examples:** Every skill should have a concrete example output — this is the single biggest quality driver.
4. **Personalization:** All skills could better leverage config values. The top skills reference config for tone and details but don't deeply integrate pricing, service area, or preferred equipment brands into the workflow.
