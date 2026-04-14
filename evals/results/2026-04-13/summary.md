# Skill Evaluation Summary — 2026-04-13

## Overview

Evaluated 9 HVAC skills across 6 dimensions (clarity, specificity, industry_fit, output_quality, personalization, efficiency). Two new skills added since the last cycle (visual-inspection-report, invoice-followup-drafter) received their first evaluation. The two skills still failing after 2026-04-12 (warranty-claim-drafter, maintenance-agreement-writer) were rewritten and re-evaluated.

## Score Rankings (Pre-Improvement)

| Rank | Skill | Category | Score | Δ vs 2026-04-12 | Status |
|------|-------|----------|-------|------------------|--------|
| 1 | service-call-diagnosis-brief | operations | 9.20 | 0.00 | PASS |
| 2 | load-calculation-assistant | operations | 9.10 | 0.00 | PASS |
| 3 | predictive-maintenance-summary | operations | 8.90 | 0.00 | PASS |
| 3 | proposal-generator | sales | 8.90 | 0.00 | PASS |
| 5 | field-report-dictation | operations | 8.80 | 0.00 | PASS |
| 5 | energy-savings-report | customer-service | 8.80 | 0.00 | PASS |
| 5 | visual-inspection-report | operations | 8.80 | — (new) | PASS |
| 5 | invoice-followup-drafter | admin | 8.80 | — (new) | PASS |
| 9 | warranty-claim-drafter | admin | 4.40 | 0.00 | FAIL |
| 10 | maintenance-agreement-writer | sales | 4.00 | 0.00 | FAIL |

**Passing threshold:** 7.0 | **Improvement threshold:** 6.0

**Pre-improvement pass rate:** 8/10 (80%)

## Regression Check

No regressions detected. All previously-evaluated skills held their scores. The two skills flagged as next-priority in the 2026-04-12 summary remained at their prior failing scores — they had not been touched between cycles.

## New Skills This Cycle

- **visual-inspection-report** (operations, v1.0) — scored 8.80 on first evaluation. Strong clarity, specificity, industry fit, and output quality. Personalization the lowest at 7.
- **invoice-followup-drafter** (admin, v1.0) — scored 8.80 on first evaluation. Strong tier escalation logic, channel-specific formatting, and compliant final-notice language.

Both new skills were introduced by the landscape monitor on 2026-04-13 and launched above passing threshold — a quality-at-creation improvement over the initial 2026-04-11 skill batch.

## Cross-Skill Dimension Weaknesses

- **Personalization** is now the weakest dimension across the repo, averaging 7.6. Strong skills still score 7–8 here. The top three skills (service-call-diagnosis-brief, field-report-dictation, predictive-maintenance-summary) have personalization at 7 and would benefit from deeper config integration.
- The two failing skills continued to score lowest on **specificity** (3) and **output_quality** (4) — the exact pattern identified in the last cycle.

## Improvements Made This Cycle

Two skills were rewritten from generic boilerplate to full HVAC-specific implementations:

| Skill | Before | After | Change | Key Improvements |
|-------|--------|-------|--------|------------------|
| warranty-claim-drafter | 4.40 | 9.00 | +4.60 | Added per-OEM portal references (HVACpartners, ComfortSite, LennoxPros, Rheem Net, Goodman Dealer, MELink), registration grace-window rules, three output formats (portal/letter/labor addendum), documentation checklist, compressor burn-out evidence requirements, and a complete Carrier example with megger diagnostics |
| maintenance-agreement-writer | 4.00 | 9.00 | +5.00 | Added three-tier framework (Silver/Gold/Platinum) with visit counts, discount %, inclusions, and pricing bands; equipment-specific PM checklists (AC vs heat pump vs furnace vs boiler vs mini-split vs package unit); named exclusions; term/renewal/transferability clauses; and a full signature-ready Trane-equipped example |

**Post-improvement pass rate:** 10/10 (100%)

## Repo Health at End of Cycle

- 10 skills total (excluding _shared), all passing
- Mean weighted score: 8.54 (up from 7.73 at end of 2026-04-12 cycle, driven by the two rewrites and two above-threshold new skills)
- All skills now have concrete example outputs tied to HVAC-specific scenarios
- All skills now define an explicit output format

## Recommendations for Next Cycle

1. **Personalization push** — Target the 7-scoring skills with deeper config integration. Specifically: pre-fill dispatch system field mappings in field-report-dictation, equipment-brand preferences in predictive-maintenance-summary and service-call-diagnosis-brief.
2. **Cross-linking** — The rewritten skills now reference visual-inspection-report, invoice-followup-drafter, and the v2 sales/customer-service skills. Continue building these connections so the repo operates as a pipeline, not a set of isolated tools.
3. **Commercial variant** — maintenance-agreement-writer has a residential bias; the tiers are shaped for homes. A commercial variant (quarterly PMs, SLA language, multi-site schedules, net-30 billing) would fill the largest remaining gap.
4. **Knowledge-base seeding** — Several skills reference `knowledge-base/manufacturers/` and `knowledge-base/maintenance-checklists/`. Populating those will move personalization scores from 7–8 up to 9–10 across the repo.
