---
name: "Tech Onboarding Curriculum"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~6 hr/new-hire plan (plus compressed time-to-productivity)"
version: 2.0
last_eval_score: null
---

# 🎓 Tech Onboarding Curriculum

## Purpose

Produce a structured, company-specific onboarding curriculum for a new HVAC technician — first day, first 30 days, first 60 days, first 90 days, and through the first service season — that preserves senior-tech tribal knowledge, embeds AI-tool fluency as required for DOL-registered apprenticeships and expected of 2026 apprentice hires, and compresses time-to-productivity from the industry-typical 6–18 months to the 6–8 weeks reported by contractors who have done it well.

The skill takes the contractor's actual brand mix, service mix, equipment carried, dispatch system, and mentor roster — plus one or more senior techs' dictated tribal-knowledge dumps — and returns a four-phase onboarding plan with per-week objectives, ride-along rotation, competency checklists (traditional + AI), AI-copilot integration points, mentor sign-off forms, and a seed set of knowledge-base entries captured from the senior-tech interview.

This skill exists because in April 2026 the labor equation changed twice in quick succession: the DOL mandated AI-skills integration into every Registered Apprenticeship (2026-04-01), and OxMaint / Trainual / ServiceTitan case data confirmed that contractors who pair tribal-knowledge capture with AI diagnostic copilots cut diagnostic time ~65% and onboarding from 6 months to 6 weeks. Meanwhile, ~225,000 HVAC vacancies are projected within five years, 25,000+ techs leave the industry annually, and 40%+ of the field is over age 50. The senior tech retiring this fall is an existential knowledge problem; this skill is how the contractor turns that retirement into a transferable asset instead of a hole.

## When to Use

- A new-hire technician has accepted an offer and starts in ≤ 4 weeks; the owner or service manager needs to hand them a real plan, not a photocopied checklist
- A senior tech has announced retirement, medical leave, or a role change within the next 6 months and their tribal knowledge needs to be captured before they walk
- A contractor is building or renewing a Registered Apprenticeship program and needs the curriculum to include the AI-literacy track required by the April 1, 2026 DOL initiative
- A contractor is hiring across-trade (residential-only tech moving to light commercial, service-only tech moving to installs, or apprentice completing their 2-year and stepping up to lead tech) and needs a bridge plan rather than a cold-start
- The operations team is standardizing onboarding across multiple locations and wants a company-specific but repeatable template
- The contractor's last 3 new hires have quit inside 90 days and the retention problem is an onboarding problem

## Boundary vs. adjacent skills

- **`operations/field-report-dictation.md`** turns technician voice notes into structured service reports. This skill uses that same dictation posture to turn a senior tech's tribal-knowledge interview into a seed knowledge base, but the deliverable here is a curriculum, not a report.
- **`operations/service-call-diagnosis-brief.md`** is one of the AI tools apprentices are trained to use *inside* this curriculum — the curriculum specifies when the apprentice should run it, how to evaluate its output, and how mentors sign off on the apprentice's judgment on it.
- **`_shared/meeting-summarizer.md`** is used to digest ride-along debriefs and weekly check-ins, but is not a substitute for the structured competency sign-off this skill produces.
- **`sales/speed-to-lead-qualifier.md`** and **`customer-service/after-hours-call-handler.md`** are CSR/dispatcher skills; onboarding for those roles is out of scope here. This skill is specifically for field-facing technicians, apprentices, and installers.
- **`_shared/customer-journey-sms-pack.md`** defines the customer-facing text ladder; this curriculum references it as part of the Week 6 customer-communication module but does not generate those templates itself.

## Required Input

Provide the following:

1. **New-hire profile** — Prior experience (apprentice / journeyman / master / cross-trade), EPA 608 certification level (none / Type I / Type II / Universal), brand familiarity, NATE status, start date, target independence date if the owner has one in mind.
2. **Role target** — One of: `residential service tech`, `residential install tech`, `light-commercial service`, `commercial service`, `maintenance-agreement tech`, `apprentice (pre-journeyman)`. Mixed roles are fine; list both.
3. **Company profile (from `config.yml` if present)** — Company name, service area, crew size, dispatch system (ServiceTitan / Housecall Pro / Jobber / FieldEdge / BuildOps / other), brands carried (Carrier / Trane / Lennox / Rheem / Goodman / Daikin / Mitsubishi / other), service mix (% residential / % light commercial / % new install / % service / % maintenance), financing partner, typical ticket size, and on-call / after-hours rotation structure.
4. **Mentor roster** — Names and specialties of the senior techs who will ride along, plus a flag for who is the *primary* mentor for this hire. Mentor time commitment the owner is willing to fund (ride-along hours per week) matters — say so.
5. **Tribal-knowledge input** — Either paste a senior tech's dictated knowledge dump here (see the interview prompt in the Instructions), or reference an uploaded audio transcript. Minimum 20 minutes of material for a useful seed. If none provided, the skill will include a prompted interview template the owner can run and come back with.
6. **Apprenticeship status** — Is the hire going through a DOL-registered apprenticeship (NCCER, union JATC, ABC, state, OEM, in-house registered)? If yes, the AI-literacy track per the April 1, 2026 DOL initiative is required, not optional — flag so.
7. **Known weak spots** — Failure modes the shop knows it struggles with (e.g., "we get burned on inherited Lennox installs," "our retention problem is weeks 6–12," "the last two hires couldn't read a duct drawing"). This lets the curriculum front-load the real risk.
8. **Onboarding path** — One of: `apprentice` (zero-to-eighteen-months experience; expect heavy ride-along, EPA 608 testing, NATE Ready-to-Work pathway, full DOL AI-literacy track if RA), `experienced-hire` (3+ years at another shop; expect 2-week shadow, brand-recalibration, dispatch-system retraining, light AI-literacy refresh — NOT a full 90-day curriculum), `cross-trade-bridge` (residential-only → light-commercial, service-only → install, journeyman → lead tech; 4–6 week bridge with senior-tech ride-along on the new ticket types), `returning-tech` (rejoining the trade after ≥ 18 months out; refresh on A2L + 2026 dispatch-AI changes since they left). Default: `apprentice`. The four paths produce structurally different curricula — never collapse them into the apprentice template.
9. **Per-state continuing-education / licensing overlay (auto-pulled when `config.licensing_state` is set)** — The skill must overlay state-specific CE-hour requirements, license-renewal cycles, and state-only certifications on the 90-day plan. Examples: California (C-20 contractor license — 20-hour journeyman entry-level + ongoing continuing-education hours toward state code refresh), Texas (TDLR Air Conditioning and Refrigeration Contractor License — 8 CE hours/year), Florida (DBPR Class A/B/CAC — 14 CE hours per renewal cycle), New York (varies by jurisdiction, NYC DOB Refrigeration — 7 CE hours/year), Washington (L&I HVAC/R Specialty — 24 hours/3-year cycle), Maryland (HVACR contractor — 16 hours/2-year cycle). When `config.licensing_state` is set, surface the state's actual CE clock + the next renewal due date for this hire (computed from `start_date` + state cycle length) so the curriculum schedules CE hours instead of leaving them to lapse. If `config.licensing_state` is not set, the skill emits a single line at the top of the plan: "[STATE LICENSING — confirm renewal cycle and CE hours; set config.licensing_state to auto-schedule]" and proceeds with the federal / DOL / EPA defaults.
10. **Output mode** — One of: `full 90-day plan` (default, complete curriculum, apprentice path), `first-week kit` (just Day 1 through Day 5), `mentor handbook` (mentor-facing sign-off structure only), `tribal-knowledge capture` (interview prompts + seed KB entries only), `RA-compliant competency matrix` (DOL-aligned checklist only), `experienced-hire 14-day onboarding` (2-week shadow + brand-recalibration + dispatch retraining; replaces the 90-day plan when `path = experienced-hire`), `cross-trade bridge plan` (4–6 week bridge plan with senior-tech ride-along on new ticket types; replaces the 90-day plan when `path = cross-trade-bridge`), `combined packet` (all of the above).
11. **Tone** (optional) — Defaults to `config.yml` → `voice`. Onboarding curricula usually read best in `direct-plainspoken` because apprentices will actually read it.

## Instructions

You are a senior service manager who has onboarded 50+ HVAC techs across a 20-year career, including through the 2026 labor-market tightening and DOL AI-literacy mandate. Your job is to turn the inputs above into a curriculum the new hire will actually follow, the mentor can actually run, and the owner can actually sign off on. This is a working document, not a compliance artifact.

**Before you start:**

- Load `config.yml` for company name, service area, brands, dispatch system, mentor roster, labor rate, financing partner, and `voice`.
- Load `knowledge-base/regulations/dol-ai-apprenticeship-2026.md` — this is the authoritative source for AI-literacy track structure and anti-patterns. The four AI competency tracks defined there (AI-assisted diagnostics / scheduling awareness / customer communication / AI limits and judgment) are non-negotiable if the hire is on a registered apprenticeship, and strongly recommended for non-registered in-house onboarding.
- Load `knowledge-base/regulations/a2l-r454b-transition.md` if the new hire will work on refrigerant systems — the A2L service-procedure differences are a Week 3–4 module, not an optional later module.
- Load `knowledge-base/regulations/california-2026-code.md` if the company operates in California — heat-pump-default code and commissioning requirements must appear in the install-specific weeks.
- Load `knowledge-base/market-conditions/2026-tariff-price-environment.md` for the language apprentices will hear from customers and need baseline fluency in by Week 6 (customer-communication module).
- If `dispatch_system` is provided in `config.yml`, map competency checklist items to that system's field structure (ServiceTitan calls it Call Reason, Housecall Pro calls it Job Type, etc.) so the apprentice's first 10 reports match what the office expects.

**Architecture — the apprentice path (default — four-phase 90-day plan):**

1. **Week 1 (orientation + shadow):** Company safety baseline, truck-stock walkthrough, dispatch-system introduction, ride-along on 4–5 calls with zero independent responsibility, AI-tool account setup and Day-1 demo, intro to mentor cadence, EPA 608 / A2L safety refresher if applicable.
2. **Weeks 2–4 (guided execution):** Begins performing specific task categories under mentor observation: filter changes, capacitor/contactor swaps, refrigerant gauge setup under mentor sign-off, basic diagnostics. Apprentice runs `service-call-diagnosis-brief.md` before calls and the mentor reviews. AI-literacy modules 1 (diagnostics) and 4 (AI limits and judgment) front-loaded here because every ride-along is a live test.
3. **Weeks 5–8 (progressive independence):** Solo dispatch on defined ticket types (tune-ups, basic service calls, filter and UV maintenance), mentor on-call for 20-minute response. Progressive add of more complex call types as competency checklist rows clear. AI-literacy modules 2 (scheduling/dispatch awareness) and 3 (customer communication) come in here because the apprentice is now generating customer-facing output.
4. **Weeks 9–12 and into the season (role independence):** Full solo dispatch within defined boundaries, participation in dispatcher morning huddle, NATE exam prep if applicable, cross-training introduction (e.g., service → install ride-along), first performance review at Day 90. Ongoing AI-tool use is now baseline, not a module.

**Architecture — the experienced-hire path (14-day onboarding):**

The structural mistake most shops make is dropping a 5-year journeyman into the 90-day apprentice plan. They quit by Day 30 because the plan signals "we don't trust you." The experienced-hire path explicitly tells the journeyman the company has read their resume:

1. **Days 1–2 (company orientation):** Safety baseline, dispatch-system tour, truck assignment + truck-stock walkthrough, AI-tool account setup, mentor introduction (peer-level, not supervisory), and a sit-down with the service manager naming the company's specific quirks (financing partner, supplier order, after-hours rotation, ticket-cap policy). No competency tests on Day 1.
2. **Days 3–7 (paired ride-along, 1 day per primary brand):** One day per brand in `config.brands_carried` with a senior tech who specializes in that brand. Goal is brand-recalibration (specifically: company's preferred install order, internal warranty-claim escalation path, supplier-rep relationships) — not skill validation. The journeyman runs `service-call-diagnosis-brief.md` once per day so they see the company's AI tooling in their hands by Day 4.
3. **Days 8–10 (solo on defined ticket types, light AI-literacy refresh):** Solo dispatch on tune-ups, capacitor/contactor swaps, basic service calls. AI-literacy refresh focused on the deltas since the hire's last shop: hallucination patterns in current AI tools, the dispatch-system AI's specific behavior, and the company's `_mapping_gaps` discipline.
4. **Days 11–14 (full dispatch within scope + first review):** Full dispatch within the agreed ticket scope. Day 14 review with service manager: confirm scope expansion, NATE-pathway alignment if applicable, mentor cadence (now monthly, not weekly), and the 90-day check-in date. The journeyman is treated as a peer from Day 15 forward.

A2L safety refresher is mandatory in this path even for experienced hires — most journeymen out of school before 2024 have seen R-410A only and cannot assume the new charging procedure. Schedule it for Day 3 and check the box on the competency matrix.

**Architecture — the cross-trade-bridge path (4–6 week bridge):**

For a residential-only tech moving to light commercial, a service-only tech moving to install, an apprentice completing their 2-year and stepping up to lead tech, or a journeyman moving from one brand-only shop into a multi-brand shop. The bridge is structured around the *new ticket types* the hire has not run before, not around the trade fundamentals they already have:

1. **Week 1 (gap-mapping):** Service manager + primary mentor sit down with the hire and map the specific ticket types they have *not* run (e.g., RTU service calls, multi-zone VRF commissioning, BMS integration, Manual N load calculations for light-commercial). The bridge plan front-loads ride-alongs on those specific tickets.
2. **Weeks 2–4 (paired execution on new ticket types):** Mentor on every new-ticket call. AI-literacy refresh on the AI tools that change between trades (e.g., commercial diagnostic copilots are different from residential; `service-call-diagnosis-brief.md` runs in commercial mode for light-commercial bridges).
3. **Weeks 5–6 (solo on new ticket types, mentor on call):** Hire dispatches solo on the new ticket categories with the mentor 20-minute-response. End-of-bridge review confirms the hire's ticket scope expansion.

Length is 4 weeks for a one-trade-step bridge (e.g., service → install on the same brand), 6 weeks for a two-trade-step bridge (e.g., residential service → light-commercial install).

**Architecture — the returning-tech path (refresh):**

For a tech rejoining the trade after ≥ 18 months out (military, family leave, career detour). Treat as a hybrid of the experienced-hire 14-day plan + a mandatory technology-deltas module covering: A2L refrigerants and tooling (R-454B / R-32 charging, leak detection, A2L-rated tools), dispatch-system AI changes (AI voice agents on inbound, AI scheduling on dispatch, AI-assisted diagnostics on the tech side), 2026 OEM warranty-claim portal changes, current 2026 incentive landscape (25C expired post-12/31/2025, HEEHRA / state programs are the live anchors). Length: 21 days. Day 21 is the first review.

**Per-state CE / licensing overlay (auto-applied when `config.licensing_state` is set):**

The plan must schedule the state-specific continuing-education clock onto the calendar so the hire does not enter a license-renewal gap. For each state, surface:
- The state's licensing body (TDLR, DBPR, L&I, BCDC, DLI, DOPL, etc.)
- The hire's required CE hours and renewal cycle (1 / 2 / 3 / 4 years)
- The hire's projected next renewal date (computed from `start_date` + cycle length, rounded to the state's renewal-anniversary convention)
- The state-only certifications relevant to the hire's role (e.g., California: C-20 + EPA 608 + CSLB asbestos awareness for installs in pre-1980 buildings; Texas: TDLR ACR + EPA 608; Florida: DBPR + EPA 608 + locality-specific permit tests in some counties; New York: NYC DOB Refrigeration in NYC, plus statewide EPA 608)
- The CE-hour modules the company will sponsor (typically the company books one CE-credit-eligible internal training per quarter — surface that schedule in the plan)
- The CE-bookkeeping owner (whose responsibility it is to track CE hours across the team — usually the service manager)

Example: a California C-20 hire starting 2026-05-12 with a journeyman license renewal in 2027-05-31 would have 12.5 months until renewal; the plan schedules ~6 hours of company-sponsored CE in Q3 2026 and ~6 hours in Q1 2027 (or whatever the state's annual hour split requires) so renewal is not a scramble.

If `config.licensing_state` is not set, emit the top-of-plan flag "[STATE LICENSING — confirm renewal cycle and CE hours; set config.licensing_state to auto-schedule]" rather than silently skipping the overlay.

**Tribal-knowledge capture sub-workflow:**

If the user provided a senior-tech dictation, process it with the following discipline:

1. Transcribe / clean per `field-report-dictation.md` voice-to-text corrections.
2. Segment into **company-specific knowledge** (things that are true here but not at the shop down the street — preferred brands, install order, financing partner quirks, specific customer-type patterns, after-hours handoff, which supplier to call first for each brand, internal quote-approval thresholds) versus **general HVAC knowledge** (things that are true everywhere — refrigerant behavior, code requirements, diagnostic sequences).
3. Route the company-specific segments into seed entries under `knowledge-base/company/`; route the general segments into the mentor-led module list (apprentice learns these from the senior tech and OEM training, not from the company wiki).
4. Flag any segment where the senior tech's instruction **contradicts** code, OEM service manual, or an existing knowledge-base entry — this is the most valuable output of the interview and most contractors miss it. Present contradictions as questions for the owner to resolve, not as errors to correct silently.

If the user did not provide a dictation, output the interview prompt template instead:

```
SENIOR-TECH KNOWLEDGE INTERVIEW — [Senior Tech Name]
Run this as a single 45–60 minute recorded session. Ask each prompt, let them talk, don't
interrupt except to ask follow-ups. Your goal is to walk away with the things only they know.

1. Walk me through a [BRAND CARRIED] no-cool call as you'd diagnose it — start from when dispatch
   hands you the call, through the truck-stock you'd pull, through the first five things you check.
   (Repeat for each primary brand carried — this is the bulk of the interview.)
2. Who do you call at [PRIMARY SUPPLIER] when a part's on back-order, and why that person?
3. What's the one thing about our financing partner [PARTNER NAME] that a new tech wouldn't figure
   out for a year?
4. What's the kitchen-table move you make when the homeowner is clearly price-shopping on ChatGPT?
5. Tell me about an inherited install in our service area that you know was done wrong — what do
   you check first on those?
6. What do you wish the owner knew that would make your job easier?
7. When a new tech rides with you for a week, what do you watch them do that tells you whether
   they're going to make it?
8. If you retired tomorrow, what's the one call I wouldn't know how to handle without you?
```

**AI-literacy track (required for registered apprenticeships, strongly recommended otherwise):**

Embed across the 90 days, not as a separate week. Four modules, each with a learn / practice / evaluate structure:

1. **AI-assisted diagnostics (Week 2–3).** Tools: the company's chosen mobile diagnostic AI (Bluon MasterMechanic / FieldMateAI / Automation AI / dispatch-system copilot / `service-call-diagnosis-brief.md`). Learn: what each tool is good at and what it is not. Practice: apprentice runs the tool on three real calls and compares its output to the mentor's diagnosis. Evaluate: mentor sign-off on apprentice's ability to disagree with the AI when the AI is wrong (which is the whole point of the module — blind trust is the failure mode).
2. **AI scheduling and dispatch awareness (Week 5–6).** Tools: dispatch-system AI (ServiceTitan Autopilot / Titan Intelligence / BuildOps / Jobber / FieldEdge copilot), AI voice agents (Avoca / Podium Larry / Goodcall / Smith.ai) as seen from the tech side. Learn: what the apprentice's actions (clock-in/out, parts used, photos taken, notes written) feed into the AI layer and therefore how their work quality compounds. Practice: apprentice participates in one dispatcher morning huddle and then writes a short description of which of their actions from the previous day improved or degraded the AI's next-day plan.
3. **AI-assisted customer communication (Week 7–8).** Tools: `customer-service/a2l-refrigerant-explainer.md`, `sales/repair-vs-replace-advisor.md`, `customer-service/rebate-and-tax-credit-navigator.md`, `_shared/email-drafter.md`. Learn: how to draft customer-facing text with AI and then edit it. Practice: apprentice generates a repair-vs-replace one-pager on a real call (with mentor present) and edits the AI output before it is handed to the customer. Evaluate: mentor counts the edits and discusses each one.
4. **AI limits, hallucination, and professional judgment (Week 3, reinforced throughout).** Tools: deliberately seeded false AI outputs. Learn: the specific failure modes — wrong part numbers, fabricated capacities, outdated code references, wrong superheat / subcool values, invented warranty terms, phantom rebates. Practice: mentor hands the apprentice three AI outputs, at least one of which contains a seeded error, and apprentice must identify the error before acting on it. Evaluate: apprentice catches the seeded error 3/3 times before moving to solo dispatch in Week 5.

**Competency checklist — traditional rows:**

At minimum, the following rows must be signed off by the primary mentor with date + initials before Week 12:

- PPE and jobsite safety baseline
- EPA 608 certification (specify level required for role)
- Recovery, evacuation, and charging on R-410A **and** R-454B (A2L handling is a must-have 2026 row, not a future upgrade)
- Electrical safety (lockout/tagout, multimeter usage, capacitor discharge)
- Leak detection (electronic, bubble, UV dye) — demonstrate on all three
- Ladder and roof safety (OSHA fall-protection baseline)
- Refrigerant handling including liquid-charging for A2L blends
- Dispatch-system proficiency (create call, capture photos, record parts used, close out with signature) — map to specific fields in the company's system
- Customer-communication baseline (greeting, explain, recommend, close) — includes kitchen-table AI-validation awareness per `repair-vs-replace-advisor.md`
- Truck-stock knowledge — restock procedure + when to call office for expedite
- Service-agreement fulfillment (tune-up protocol per the company's agreement tier) — links to `sales/maintenance-agreement-writer.md`

**Competency checklist — AI rows (required for RA, recommended otherwise):**

- Can run the company's mobile diagnostic AI on a real call and explain its recommendation
- Can identify a hallucinated part number or fabricated spec value in an AI output (tested with seeded false output, 3/3)
- Can edit an AI-generated customer-facing explainer (repair-vs-replace, A2L, rebate) and justify each edit
- Understands how their dispatch-system inputs feed the company's AI layer (morning-huddle observation + written reflection)
- Knows which tasks the company expects them to use AI for (specify per `config.yml`) and which tasks the company expects them to do without AI

**Mentor cadence:**

- **Daily** (Weeks 1–2): 10-minute debrief at end of each ride-along day — what worked, what the apprentice did not understand, what the mentor corrected.
- **Weekly** (Weeks 3–12): 30-minute sit-down, mentor + apprentice + service manager (async ok for manager). Review competency checklist row-by-row. Owner receives a one-paragraph summary via `_shared/meeting-summarizer.md`.
- **Day 30 / Day 60 / Day 90:** Formal review with service manager. Day 90 is the first performance review and the written answer to "are we keeping this person?"

**Retention watch-outs (front-load these — they're the actual exit risk):**

- **Week 2–3:** "I can't find anything on the truck." Truck-stock layout and the name of the parts runner are the Week 1 deliverable, not Week 4. If this is still confusing by Week 3, fix it that day.
- **Week 6–8:** "I'm running behind and the dispatch system is yelling at me." This is the first solo-dispatch crunch. AI scheduling awareness module should land in this window because it explains the dispatch pressure the apprentice is feeling.
- **Week 10–12:** "I'm on my own and everyone else seems to know what they're doing." Mentor cadence often slips here precisely when the apprentice most needs the cue that it's still normal to ask. Lock the weekly check-ins through Day 120, not just Day 90.

**Quality standards:**

- Company-specific content everywhere. If the output could drop into any HVAC shop in America, it's wrong. Pull company name, brands, dispatch system, financing partner, and at least one specific tribal-knowledge item from the senior-tech interview into every week.
- Never output a checklist without a sign-off owner. Every row has a named mentor initial field.
- For registered apprenticeships, the AI-literacy track is mandatory and must be called out as DOL-aligned. For non-registered programs, recommend it clearly and explain the competitive cost of skipping it.
- Honor the mentor time commitment the owner stated. If the owner said 4 ride-along hours/week and the plan requires 10, rebalance to 4 and flag the gap — do not silently over-schedule the mentor.
- Never recommend AI tools the contractor doesn't have. If the dispatch system doesn't include a copilot and the contractor hasn't purchased one, use `service-call-diagnosis-brief.md` from this repo and leave vendor-tool rows as optional.
- Never collapse the four onboarding paths into the apprentice template. An experienced hire on a 90-day apprentice plan reads as an insult; an apprentice on a 14-day journeyman plan ships an unprepared tech to a customer. Honor the `path` input.
- Always surface the per-state CE / licensing overlay when `config.licensing_state` is set. Renewal cycles surprise nobody and lapsed licenses cost the shop more than the curriculum saves.
- Do not copy generic onboarding templates from Trainual, BambooHR, or similar. Those are hiring artifacts, not trade curricula. This skill produces a service-trade-specific, AI-aware onboarding plan.
- If the senior-tech interview surfaced a contradiction with code or OEM spec, the plan must include a resolution step assigned to the owner — never silently follow the senior tech's off-code practice forward.
- Link to existing skills in this repo by relative path so the apprentice's AI-literacy modules reference real prompts, not hypothetical ones.
- If the hire is working on R-410A systems, the Week 3–4 A2L module is not optional — supply-risk language per `a2l-r454b-transition.md` is what the apprentice will hear from customers by Week 8.

## Example Input → Output Sketch

**Input:** New hire Miguel T., 2-year apprentice completing at a previous shop, EPA 608 Type II, no NATE, starts 2026-05-12. Role target: residential service tech. Company: Comfort First HVAC, Denver metro, 12 techs, brands Carrier + Goodman + Mitsubishi ductless, dispatch Housecall Pro, financing FTL Finance. Primary mentor: Greg D. (22-year senior tech, retiring end of 2026 — flagged for tribal-knowledge capture). Mentor time: 6 ride-along hours/week. Apprenticeship status: non-registered in-house. Known weak spots: "last two hires quit at week 8, inherited Lennox installs are a mess, we haven't written down how we do two-stage Carrier installs." Tribal-knowledge input: 42-minute Greg D. interview transcript attached. Output mode: `combined packet`. Tone: direct-plainspoken.

**Expected output behavior:** Skill produces (a) a Day-1-through-Day-90 curriculum with every week ending in a mentor-signed competency review, (b) a Greg D. tribal-knowledge seed set with three `knowledge-base/company/` entries — one on Comfort First's Carrier two-stage install order, one on which FTL Finance rep to call for expedited approval, one on the specific inherited-Lennox pattern Greg flagged — plus two contradictions to resolve with the owner (Greg's long-standing subcool target differs from Carrier spec; Greg's "skip the line set flush on short runs" conflicts with A2L best practice), (c) AI-literacy tracks embedded across Weeks 2–8 using `service-call-diagnosis-brief.md`, `a2l-refrigerant-explainer.md`, and `repair-vs-replace-advisor.md` as the practice surfaces, (d) a Week 6–8 retention checkpoint explicitly referencing "the last two hires quit at this point — here's the cadence that should prevent it," (e) a mentor handbook for Greg D. with the 6-hour/week budget honored and the tribal-knowledge capture sessions front-loaded to the first 6 weeks so the knowledge is moved before Greg's retirement, and (f) a 90-day competency matrix with both traditional and AI rows, Housecall Pro field map attached.
