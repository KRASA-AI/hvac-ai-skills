# HVAC Incentives Landscape (Federal / State / Utility) — Reference Notes

Quick-reference ground truth for any HVAC AI skill that quotes a rebate or tax credit.
Skills should **defer to this file** rather than hardcoding incentive figures inline, the
same way refrigerant facts defer to `a2l-r454b-transition.md`. This is the file that keeps
time-sensitive tax facts from going stale and undetected inside individual prompts.

**Currency:** Reviewed 2026-06-22. Tax law changed materially in mid-2025 (OBBBA, below), so
**every federal credit below carries a "confirm current IRS guidance before quoting" posture.**
The goal is fail-safe: a skill should refuse to assert an unverified credit rather than ship a
stale one. Dollar figures are program parameters, not promises — always pair with "estimate —
confirm current program terms."

---

## Federal tax credits — the 2026 reality

### 25C — Energy Efficient Home Improvement Credit (heat pumps, AC, furnaces)
- **Status: EXPIRED for property placed in service after 12/31/2025.** The One Big Beautiful
  Bill Act (OBBBA, signed July 2025) accelerated the termination of 25C. Installs **completed on
  or before 12/31/2025** could claim it on the 2025 return (Form 5695).
- Pre-expiration parameters (for 2025-and-earlier installs only): up to **$2,000** for qualifying
  heat pumps; up to **$600** for qualifying AC / furnace; **$3,200** annual aggregate ceiling; no
  federal income limit; customer keeps the manufacturer Qualifying Certificate (and, for 2025, the
  PIN/QM product-identifier requirement applied).
- **Do not quote 25C on any 2026 install.** If a skill is modeling a 2026 job, 25C = $0.

### 25D — Residential Clean Energy Credit (geothermal, solar)
- **Status: TERMINATED for expenditures made after 12/31/2025** (OBBBA). Previously scheduled to
  run at 30% through 2032 with a step-down; OBBBA pulled that forward.
- Applies (at 30% of project cost, no cap) **only to systems paid for and placed in service by
  12/31/2025.**
- 25D was always limited to **geothermal heat pumps and solar** — it **never** applied to
  air-source heat pumps. A skill that attaches 25D to an air-source HP is wrong on two counts.
- **Do not quote 25D on any 2026 install**, geothermal included. Pivot 2026 geothermal customers
  to state/utility incentives and, for commercial, the Section 48 ITC via the navigator skill.

> ⚠️ The 25C/25D terminations were established by web verification in mid-2026 (postdates some model
> training cutoffs). If a skill's model "remembers" 25D running through 2032, that memory is stale —
> this file overrides it. Still, instruct the output to "verify current IRS guidance before quoting."

### HEEHRA / HEAR — High-Efficiency Electric Home Rebate (point-of-sale)
- IRA-funded, **state-administered** (rollout varies by state; some live, some still standing up).
- Income-tested, point-of-sale rebate:
  - **< 80% AMI:** up to **$8,000** for a qualifying heat pump (covers up to 100% of project cost).
  - **80–150% AMI:** up to **$8,000** but capped at **50%** of project cost.
  - **> 150% AMI:** not eligible.
- Per-household lifetime caps and other electrification line items (panel, wiring, heat-pump water
  heater) apply. Confirm the customer's state program is **live** before promising point-of-sale.

### HOMES — Home Efficiency Rebates (performance-based)
- IRA-funded, state-administered, **modeled or measured energy-savings** based (not income-gated the
  same way). Stackability with HEEHRA is restricted — generally cannot double-dip on the same measure.

---

## Commercial federal incentives (hand to `rebate-and-tax-credit-navigator.md` for CPA-facing detail)

- **179D — Energy Efficient Commercial Buildings Deduction.** Sliding-scale $/sqft deduction;
  base rate vs. an enhanced rate that requires **prevailing-wage + apprenticeship (PW/A)**, which
  applies a roughly **5×** multiplier to the base. Modeled against the **ASHRAE 90.1** baseline in
  effect (90.1-2019 for current projects). Note: OBBBA set a **termination of 179D for property whose
  construction begins after a 2026 cutoff** — confirm the current begin-construction date rule before
  promising it on a future project.
- **Section 48 / 48E — Investment Tax Credit (ITC).** Applies to geothermal and certain other
  property: **base 6% / enhanced 30%** (PW/A), plus **energy-community** and **domestic-content**
  adders. Technology-neutral 48E governs property placed in service after 2024.
- **§6417 — Elective pay ("direct pay").** Lets tax-exempt entities (municipal, non-profit, school
  districts, some co-ops) monetize Section 48/48E as a cash payment. Drives the filing-entity question
  on commercial-portfolio jobs.

---

## State + utility layer (the durable savings in 2026)

With 25C/25D gone for new installs, the **utility + state + manufacturer** stack is the realistic
incentive story for most 2026 residential jobs:

- **Utility rebates:** commonly **$300–$2,000** per qualifying heat pump, gated on **SEER2/HSPF2
  thresholds** and an **AHRI-matched certificate**. Often require **pre-approval before install** —
  flag this; submitting after the fact can void the rebate. Pull program names from `config.state_programs`.
- **State programs:** TECH Clean California, Mass Save, NYSERDA, Efficiency Maine, Xcel/CO programs,
  etc. — names and dollar levels vary; treat `config.state_programs` + the utility site as authoritative.
- **Manufacturer promos:** filter by `config.brands_carried`; seasonal, with expiration dates — always
  print the expiry.
- **Demand-response / TOU:** some utilities pay for enrolling a smart-thermostat / variable-speed system
  in a demand-response program; relevant to the TOU overlay in `energy-savings-report.md`.

---

## Rules of thumb for skills quoting incentives

1. **Gate on install timing.** Pre-2026 installs can carry 25C/25D; 2026+ installs cannot.
2. **Never quote a federal credit on a 2026 install without the verify caveat.** Default to the
   utility/state/manufacturer stack.
3. **Income-test HEEHRA** before quoting a dollar figure; if AMI tier is unknown, give the range and
   flag it.
4. **Utility rebates usually need pre-approval** — say so.
5. **Commercial → defer to the navigator** for 179D/Section 48/§6417; do not compute those inline.
6. **Always label dollar figures "estimate — confirm current program terms."**

---

*Maintained by the skill-evaluator / landscape-monitor jobs. If a federal figure here conflicts with
current IRS or DOE guidance, the live guidance wins and this file should be updated.*
