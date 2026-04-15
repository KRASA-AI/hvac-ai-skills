---
title: "HVAC AI Vendor & Platform Map"
last_updated: "2026-04-15"
purpose: "Reference document for tools referenced by skills and for landscape awareness. Updated by the landscape-monitor scheduled task."
---

# HVAC AI Vendor & Platform Map

This is a working map of the AI-enabled tools, platforms, and integrations that keep surfacing in HVAC landscape monitoring. It is not an endorsement list — it's a reference for skills authors and evaluators so that outputs can speak the vendor language HVAC contractors actually use. Update quarterly.

## Field Diagnostics & Technician Assistance

- **Bluon (MasterMechanic)** — AI diagnostic engine built on a 25M+ unit database, parts lookup across 200+ OEMs, and original equipment manuals. 170K+ technicians on the app. Integrated inside BuildOps FSM as of March 2026. Nationwide rollout at Helios HVACR across 40+ states.
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
- **Avoca** — AI call analysis and QA for CSR teams, strong for training the human-handoff moment.
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
| Live field diagnostic | Bluon (standalone or inside BuildOps) |
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
