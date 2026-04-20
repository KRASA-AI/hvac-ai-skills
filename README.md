# HVAC AI Skills

**Free, open-source AI prompts and workflows built for hvac technicians.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for hvac. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Field Report Dictation | Transform raw, spoken-style technician notes into a structured, professional service report that drops cleanly into the shop's dispatch system (ServiceTitan, Housecall Pro, Jobber, FieldEdge, BuildOps, or plain PDF). | ~15 min/report |
| Load Calculation Assistant | Walk through Manual J residential load calculation inputs step-by-step, flag common sizing mistakes, and produce a preliminary load estimate with equipment sizing recommendations — before the formal quote. | ~30 min/calc |
| Predictive Maintenance Summary | Analyze equipment sensor data, maintenance history, and performance trends to generate a predictive maintenance report. | ~20 min/report |
| Service Call Diagnosis Brief | Summarize the reported issue, likely causes, and recommended parts before the tech arrives — and optionally guide live troubleshooting in the field with interactive, step-by-step diagnostic assistance. | ~10 min/call |
| Visual Inspection Report | Transform technician field photos and accompanying notes into a structured visual inspection report. | ~20 min/report |
| Maintenance Agreement Writer | Produce a complete, signature-ready residential or light-commercial HVAC maintenance agreement in one of three clearly-defined coverage tiers (Silver / Gold / Platinum). | ~30 min/agreement |
| Proposal Generator | Build a professional, multi-option replacement or installation proposal with good-better-best equipment packages. | ~25 min/proposal |
| Repair vs. Replace Advisor | Produce a defensible, kitchen-table-ready recommendation on whether a homeowner should repair their existing HVAC system or replace it — at 2026 tariff-adjusted prices, with financing math, available incentives, and explicit AI-validation framing so the homeowner doesn't hand the decision over to a naïve ChatGPT check after the technician leaves. | ~25 min/conversation |
| Speed-to-Lead Qualifier | Produce a complete speed-to-lead response kit for a new inbound HVAC lead: the immediate-reply message (SMS or missed-call auto-text), a follow-up cadence over 5 days, and a qualifying question script that gathers the minimum data a dispatcher needs to book the right technician. | ~12 min/lead |
| A2L / R-454B Refrigerant Transition Explainer | Produce clear, honest, non-alarming customer-facing explanations of the 2026 A2L refrigerant transition — most commonly R-454B replacing R-410A — tailored to a specific homeowner or light-commercial situation. | ~15 min/customer conversation |
| After-Hours Call Handler Script | Produce a complete after-hours call-handling framework for an HVAC business: the voice-agent or live-receptionist script, emergency-triage decision tree, caller data-collection structure, and dispatcher handoff package. | ~20 min/after-hours call (plus recovered revenue) |
| Energy Savings Report | Generate a customer-facing report showing projected energy and cost savings from upgrading their HVAC system — with real numbers, payback timelines, and available rebates. | ~20 min/report |
| Rebate & Tax Credit Navigator | Produce a clear, state-aware, customer-facing answer to the question "what money is still on the table for my HVAC or heat pump project?" The federal 25C Energy Efficient Home Improvement Credit for heat pumps expired for installations completed after December 31, 2025, and the landscape has shifted abruptly to a patchwork of state HEEHRA/HOMES rebates, utility programs, manufacturer promotions, and contractor-financing incentives. | ~20 min/customer conversation |
| Invoice Follow-Up Drafter | Generate professional, appropriately toned payment follow-up messages for outstanding HVAC service invoices. | ~10 min/message |
| Warranty Claim Drafter | Compile a complete, manufacturer-ready warranty claim package — serial/model lookup, proof-of-install, failure narrative, and labor reimbursement request — in the exact format each OEM portal expects. | ~25 min/claim |
| Customer Journey SMS Pack | Generate the full set of customer-facing SMS templates that an HVAC contractor needs to run a modern, automated customer journey from first inquiry through post-job follow-up and seasonal re-engagement. | ~3 hr/setup, ~12 min/customer thereafter |
| Email Drafter | Turn rough notes into a professional email matching your company's voice and tone. | ~10 min/use |
| Meeting Summarizer | Summarize meeting notes into action items, decisions, and follow-ups. | ~10 min/use |
| Review Responder | Craft professional, context-aware responses to online reviews — both positive and negative — with built-in local SEO optimization. | ~10 min/use |

**Total time saved per use: ~339+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/hvac-ai-skills.git
cd hvac-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
hvac-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **HVAC AI guide**: [krasa.ai/industries/hvac](https://krasa.ai/industries/hvac)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
