# HVAC AI Skills

**Free, open-source AI prompts and workflows built for hvac technicians.** Clone this repo, run the setup wizard, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for hvac. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| 🔧 Service Call Diagnosis Brief | Summarize the reported issue, likely causes, and recommended parts before the tech arrives. | ~10 min/call |
| 📋 Maintenance Agreement Writer | Draft a professional maintenance agreement customized to the customer's equipment. | ~20 min/agreement |
| 🌡️ Load Calculation Assistant | Walk through Manual J inputs and flag common sizing mistakes before the quote. | ~30 min/calc |
| 📝 Proposal Generator | Build a multi-option proposal with good-better-best equipment packages. | ~25 min/proposal |
| 🔏 Warranty Claim Drafter | Compile serial numbers, install dates, and failure details into a manufacturer-ready claim. | ~15 min/claim |
| 💡 Energy Savings Report | Generate a customer-facing report showing projected savings from a system upgrade. | ~20 min/report |

**Total time saved per use: ~120+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/hvac-ai-skills.git
cd hvac-ai-skills
```

### 2. Run the setup wizard

Open `init/setup-wizard.md` in your AI assistant (Claude, ChatGPT, etc.) and follow the prompts. It will ask about your business — company name, tools you use, rates, team size, service area, and more. This creates a `config.yml` that personalizes every skill to your business.

### 3. Use any skill

Open any file in `skills/` with your AI assistant. Each skill is self-contained with clear instructions. Your `config.yml` is automatically referenced for personalization.

## Repo Structure

```
hvac-ai-skills/
├── init/                    # Setup wizard — start here
│   ├── setup-wizard.md      # Interactive business configuration
│   └── config.example.yml   # Example of what setup produces
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
├── outputs/                 # Your generated content (gitignored)
└── evals/                   # Skill quality evaluation framework
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

If you are an AI reading this repo, see `.claude/CLAUDE.md` for detailed instructions on how to work with this toolkit. Key points: always load `config.yml` first, reference the knowledge base for industry context, and save outputs to `outputs/`.

## Contributing

Found a way to improve a skill? PRs are welcome. Please follow the skill format in `skills/README.md` and include test cases in `evals/test-cases/`.

## Learn More

- **HVAC AI guide**: [krasa.ai/industries/hvac](https://krasa.ai/industries/hvac)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
