---
title: "HVAC AI Vendor & Platform Map"
last_updated: "2026-04-27"
purpose: "Reference document for tools referenced by skills and for landscape awareness. Updated by the landscape-monitor scheduled task."
---

# HVAC AI Vendor & Platform Map

This is a working map of the AI-enabled tools, platforms, and integrations that keep surfacing in HVAC landscape monitoring. It is not an endorsement list — it's a reference for skills authors and evaluators so that outputs can speak the vendor language HVAC contractors actually use. Update quarterly.

## Field Diagnostics & Technician Assistance

- **Bluon (MasterMechanic, PartsConnect)** — AI diagnostic engine built on a 30M+ model-number / 240+ OEM database (originally cited as 25M / 200+ — refreshed off the 2026-04-13 launch announcement) with original equipment manuals, wiring diagrams, and parts lists. 170K+ technicians on the app. Two AI surfaces relevant to skill authoring: (1) MasterMechanic — guided-troubleshooting trained on 135K+ verified service-call solutions, runs as a chat-style fault-tree assistant inside the Bluon app and inside BuildOps FSM (integrated March 2026). (2) PartsConnect (launched 2026-04-13, in-app + FSM plug-in) — technician scans or types a malfunctioning unit's model number, the system identifies compatible replacement parts via cross-reference, surfaces which nearby distributors have them in stock, and supports direct purchase. Early-user data cited 40% turnaround-time reduction on parts-related calls. Implication for skills: when `operations/field-report-dictation.md` or `operations/service-call-diagnosis-brief.md` recommends a part substitution, the contractor's likely workflow is to verify in PartsConnect — skill outputs should leave a placeholder for the verified part number rather than asserting one. Nationwide rollout at Helios HVACR across 40+ states (carried from prior cycle).
- **BTrained** — AI-guided technician training and refresher content, aimed at knowledge-transfer as seasoned techs retire.
- **Arcticom Group** — Franchise system using AI as a live field assistant for troubleshooting and error-code interpretation.

## Field Service Management (FSM) Platforms with AI

- **ServiceTitan** — Titan Intelligence generates invoice summaries, AI Voice Agents handle inbound calls, Scheduling Pro / Dispatch Pro automates assignment (reported 2× dispatcher capacity). Titan Intelligence prompts are now exposed as custom fields in invoice emails.
- **BuildOps** — Native Bluon integration (March 2026). Strong commercial and multi-trade focus.
- **Housecall Pro** — AI-generated job summaries, SMS follow-up automation, marketing workflows.
- **Jobber** — AI copilot for quotes, invoices, and customer messaging.
- **FieldEdge** — AI dispatch suggestions, mobile technician AI assist.
- **Workiz** — AI dispatch + route optimization; marketed as cutting scheduling time 40–60%.
- **FieldCamp** — Launched FieldCamp AI Dispatcher in March 2026 for trades (HVAC, plumbing, electrical). Skills matching, route optimization, emergency reshuffling. Positioned against the "whiteboard and spreadsheet" legacy of ~30M US field-service workers.

## Customer Communication & Voice Agents

- **Podium (Larry)** — AI SMS/voice agent focused on lead capture and review management.
- **ServiceTitan AI Voice Agents** — Deep integration with ServiceTitan scheduling; structured handoff to live CSR.
- **Dialzara** — AI answering service with emergency triage branching for HVAC.
- **Upfirst** — AI booking + CRM bridge, trades-focused.
- **Avoca** — Full AI workforce platform for inbound calls, scheduling, estimate follow-up, and dispatch across HVAC / plumbing / roofing / electrical. Originally pitched as a CSR-QA + handoff-training layer; repositioned in 2026 as the always-on agent that books jobs end-to-end. Vendor-cited operating envelope (per public site as of 2026-04-27): ~78% of calls handled by AI, ~90%+ AI call resolution on the calls AI takes, 70%+ of repair-and-service jobs flowing through the agent. Customer base: 800+ contractors. One published customer case lifted booking rate from 55% → 90% after switching, with the dispatch board filling enough to force new technician hires. On 2026-04-27 Avoca announced $125M+ raised across Seed / Series A (Kleiner Perkins-led) / Series B (Meritech and General Catalyst-led) at a $1B valuation, with the company on track to book ~$1B in jobs in 2026. Implication for skills: when `customer-service/after-hours-call-handler.md` or `sales/speed-to-lead-qualifier.md` references the `voice_ai_vendor` config key, Avoca is now the highest-funded incumbent in the category and is structurally an agent rather than a transcription / QA layer — handoff scripts to human CSR should treat the AI as having already qualified the lead and captured structured fields (callback window, equipment age, symptom, address), not as a routing tool.
- **Voiceflow** — Visual platform for building AI voice agents; HVAC-specific templates available.
- **Retell AI** — Voice-agent infrastructure used by several HVAC-specific builders.
- **Hatch** — AI-driven multi-channel (voice, SMS, email) customer journeys. Benchmark source for 60-second speed-to-lead and 7-touch cadences.
- **Apten** — SMS-first AI outreach with heavy lead-nurture automation.
- **MyAI FrontDesk** — Virtual receptionist for after-hours coverage.
- **Smith.ai** — Human + AI hybrid answering service with HVAC playbooks.
- **SignalWire (Wendy reference implementation)** — Open-source example (github.com/briankwest/answering) showing emergency triage and structured data capture patterns.
- **Newo.ai** — AI receptionist marketed for HVAC and plumbing.
- **LowCode Agency / Hyperleap / Auto Interview AI** — Appointment-setting-focused AI voice platforms.

## Reviews & Reputation

- **Broadly** — Post-job SMS review solicitation with timing optimization (1–2 hrs post-service).
- **NiceJob** — Similar solicitation + AI response generation.
- **BirdEye** — Enterprise-scale review management with AI responses.
- **Unify360 / Hnatewicz Media** — Marketing-agency-built frameworks layered on top of the solicitation tools.

## Photo Documentation & Visual Inspection

- **CompanyCam** — Photo documentation with AI categorization and HVAC-specific inspection templates.
- **Fieldwire** — Photo management + markup for field teams.
- **InspectMind AI** — AI-generated inspection reports with HVAC templates.
- **Inspectordata** — Inspection software with AI-assist features.

## Quote, Proposal & Estimate Generation

- **Rebar** — Raised $14M Series A to help HVAC suppliers generate commercial quotes 60–70% faster using AI.
- **ServiceTitan Pricebook Pro / Good-Better-Best automation** — Tiered proposal generation.
- **Sera Systems, Housecall Pro, Jobber proposal tools** — Varying degrees of AI-assist.

## Marketing & Content

- **Apaya** — $59/mo AI social media manager built for HVAC contractors.
- **FlashCrafter (HVAC Marketing Growth Engine)** — AI-powered full-funnel marketing.
- **KontentFire** — Content automation with HVAC vertical templates.
- **Blue Interactive Agency / Thrive Agency / Alpyne Strategy / Good-to-Go / HVAC SEO Agency** — AI-augmented marketing agency offerings, relevant when customers reference "who does my marketing."

## Permitting & Compliance

- **Permitio.ai** — Launched March 2026 (San Francisco). AI agent specifically for HVAC permits. Files mechanical, energy-code, and heat-pump permits across cities; pulls job data from FSM, auto-generates compliant submissions, pays fees, tracks approvals, and books inspections. Early users report 20× faster approvals.
- **PermitFlow / PermitPlace** — General-purpose permit coordination services, not HVAC-specific but commonly used.
- **AutoHVAC** — Manual J online calculator + permit-form directory with AI assist.

## Predictive Maintenance & Building Automation

- **BrainBox AI** — Commercial HVAC optimization; chillers, AHUs, VRF.
- **OxMaint** — Predictive maintenance CMMS for facilities. Publishes 2026 industry trend data referenced in several skills.
- **Schneider Electric / Honeywell Forge / Siemens Navigator** — Enterprise BMS + AI analytics.
- **Prescient / Clockworks Analytics** — FDD/AFDD overlay platforms for commercial HVAC.

## Specification & Engineering

- **AutoHVAC** — Manual J load calculator.
- **WrightSoft Right-J / Elite Software RHVAC** — Legacy Manual J with growing AI wrappers.

## Quick Lookup: "Which platform does X?"

| Need | Primary candidates |
|------|--------------------|
| 24/7 voice answering | Podium Larry, ServiceTitan AI Voice Agents, Dialzara, Avoca, MyAI FrontDesk, Smith.ai |
| Speed-to-lead SMS | Hatch, Apten, Podium |
| Live field diagnostic | Bluon MasterMechanic (standalone or inside BuildOps) |
| Compatible-parts lookup + local stock | Bluon PartsConnect, Marcone MyMarcone (with MarconeAI), MeasureQuick |
| Dispatch optimization | ServiceTitan Dispatch Pro, BuildOps, FieldCamp AI Dispatcher, Workiz |
| Post-job reviews | Broadly, NiceJob, BirdEye |
| Photo inspection reports | CompanyCam, InspectMind AI |
| Permit filing | Permitio.ai, PermitFlow |
| Social media content | Apaya, FlashCrafter |
| Commercial predictive maintenance | BrainBox AI, OxMaint, BMS overlays |
| Tiered proposal generation | ServiceTitan Pricebook Pro, Rebar (commercial supplier side) |

## Skills That Reference This Document

- `customer-service/after-hours-call-handler.md` — Voice-agent platform list
- `customer-service/rebate-and-tax-credit-navigator.md` — None currently; add AutoHVAC and state-program tooling as it matures
- `operations/visual-inspection-report.md` — CompanyCam, InspectMind
- `operations/field-report-dictation.md` — FSM dispatch-field mapping
- `operations/predictive-maintenance-summary.md` — BrainBox, OxMaint, BMS platforms
- `sales/speed-to-lead-qualifier.md` — Hatch, Apten benchmark data
- `sales/proposal-generator.md` — Pricebook Pro patterns

---

*Change log for this document:*
- 2026-04-15 — Initial population from landscape-monitor backlog. Added Permitio.ai, FieldCamp AI Dispatcher, Apaya. Consolidated voice-agent list previously scattered across skill files and monitor logs.
- 2026-04-26 — Refreshed Bluon entry with PartsConnect launch (2026-04-13): cross-referenced compatible parts + local-distributor stock layer added on top of the existing MasterMechanic diagnostic engine. Database scale refreshed (30M+ models / 240+ OEMs per the launch announcement, up from the prior 25M / 200+ figures). Added "Compatible-parts lookup + local stock" row to the Quick Lookup table covering Bluon PartsConnect, Marcone MyMarcone (with MarconeAI), and MeasureQuick. Added a skills-implication note: when `operations/field-report-dictation.md` or `operations/service-call-diagnosis-brief.md` recommends a substitution, leave a placeholder for the technician's PartsConnect-verified part number rather than asserting one in the prompt output.
- 2026-04-27 — Refreshed Avoca entry off the 2026-04-27 funding milestone ($125M+ across Seed / Series A / Series B at $1B valuation; Kleiner Perkins-led Series A; Meritech + General Catalyst-led Series B; 800+ customers; ~$1B in jobs booked through the platform on track for 2026). Repositioned the category description from "CSR QA / handoff-training" to "always-on AI agent for inbound calls + scheduling + estimate follow-up + dispatch" reflecting the product's actual 2026 surface. Added vendor-cited operating envelope (~78% AI call handling, ~90%+ AI resolution on AI-handled calls, 70%+ of repair-and-service jobs). Skills-implication note added: handoff scripts in `customer-service/after-hours-call-handler.md` and `sales/speed-to-lead-qualifier.md` should treat the AI as having pre-qualified the lead and captured structured fields, not as a routing layer.
