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

### HEEHRA / HEEHR / HEAR — High-Efficiency Electric Home Rebate (point-of-sale)
- IRA-funded, **state-administered** (rollout varies by state; some live, some still standing up).
- Income-tested, point-of-sale rebate:
  - **< 80% AMI:** up to **$8,000** for a qualifying heat pump (covers up to 100% of project cost).
  - **80–150% AMI:** up to **$8,000** but capped at **50%** of project cost.
  - **> 150% AMI:** not eligible.
- Household cap across all electrification measures is **$14,000**. Other line items (panel, wiring,
  heat-pump water heater) apply against it. Confirm the customer's state program is **live** before
  promising point-of-sale.

> 🚨 **2026 DOE guidance rewrite — fuel switching is out of HEEHR.** DOE resumed the paused $8.8B
> Home Energy Rebates program under revised guidance (Program Notices issued mid-2026; reported by
> ACHR NEWS 2026-06-25). The change contractors must internalize: **HEEHR rebates no longer cover
> fuel-switching.** The program now funds *electric-to-more-efficient-electric* HVAC upgrades. What
> remains allowable: electric HVAC in **new construction**, and heat-pump installs at homes that have
> a fossil-fuel system **provided the fossil system is retained** (i.e., the heat pump does not have
> to become the primary heating source). What is no longer covered: ripping out gas/propane/oil
> equipment and replacing it with a heat pump under HEEHR.
>
> Practical consequences for skills:
> - **Do not tell a gas-furnace homeowner that HEEHR will pay to replace it with a heat pump.** That
>   is now the single highest-risk incentive statement a skill can make.
> - **Dual-fuel / hybrid gets more attractive**, not less — the retained fossil system is compatible
>   with the new rules and is often the most cost-effective design anyway. `sales/proposal-generator.md`
>   and `customer-service/rebate-and-tax-credit-navigator.md` should treat dual-fuel as a first-class
>   option for fossil-heated homes, not a fallback.
> - **ENERGY STAR requirements are now optional** rather than prescribed at the federal level; states
>   may still impose them.
> - **Rebate spending power broadened:** incremental claims (insulation, air sealing, ventilation,
>   electrical wiring done over time) are more flexible, and grantees may let rebate funds cover state/
>   local tax, warranties, and necessary accessories — *if their state adopts the flexibility*.
> - **Post-install inspection / commissioning verification requirements were loosened.** ACCA objects,
>   citing that up to 90% of residential HVAC installs carry significant faults costing 30–50% of rated
>   efficiency. Quality-install language remains a legitimate differentiator in proposals.
> - **States must revise their programs under the new guidance (roughly a three-month window from the
>   guidance release), and implementation varies by state.** Any HEEHR dollar figure a skill produces
>   must carry a "confirm with your state energy office — program is being revised" caveat.

### HOMES — Home Efficiency Rebates (performance-based)
- IRA-funded, state-administered, **modeled or measured energy-savings** based (not income-gated the
  same way). Up to **$8,000** based on modeled savings, with a **20% minimum modeled savings** floor.
  Stackability with HEEHRA is restricted — generally cannot double-dip on the same measure.
- 2026 guidance removed the **40% reserved allocation for disadvantaged communities** and the
  associated mapping/outreach mandates; a **$200-per-dwelling-unit** payment to the contractor or
  aggregator for completed installs in a disadvantaged community remains, with the grantee defining
  the boundary.

---

## Commercial federal incentives (hand to `rebate-and-tax-credit-navigator.md` for CPA-facing detail)

- **179D — Energy Efficient Commercial Buildings Deduction.** Sliding-scale $/sqft deduction;
  base rate vs. an enhanced rate that requires **prevailing-wage + apprenticeship (PW/A)**, which
  applies a roughly **5×** multiplier to the base. Modeled against the **ASHRAE 90.1** baseline in
  effect (90.1-2019 for current projects). Note: OBBBA set a **termination of 179D for property whose
  construction begins after a 2026 cutoff** — confirm the current begin-construction date rule before
  promising it on a future project.

  > ⚠️ **Which band is which — the repo's most-repeated commercial error.** The **base** deduction is
  > the *low* band (order of **$0.50–$1.00/sqft**, sliding with modeled savings from 25% up). The
  > **$2.50–$5.00/sqft** band is the **PW/A-ENHANCED** rate — it is *already* the multiplied number.
  > Do **not** describe $2.50/sqft as "base" and then apply a 5× (or any) multiplier on top of it:
  > that double-counts by roughly 5×, and $2.50 → $5.00 is 2×, not 5×, which makes the error
  > self-evident to any CPA reading the worksheet. The ~5× relationship is base → enhanced
  > (≈$0.50 → ≈$2.50), not enhanced → enhanced.
  >
  > Two further guardrails skills must carry:
  > - **179D is a deduction, not a credit.** Its cash value is the deduction × the entity's marginal
  >   tax rate (≈21% for a C-corp). Never subtract the face deduction from CapEx as if it were cash.
  > - **179D is capped at the cost of the qualifying property installed.** A deduction larger than the
  >   project itself is not possible; cap it at project cost before computing cash value.

- **Section 48 / 48E — Investment Tax Credit (ITC).** Applies to geothermal (ground-source) heat
  pumps, CHP, fuel cells and other listed energy property: **base 6% / enhanced 30%** (PW/A), plus
  **energy-community** and **domestic-content** adders. Technology-neutral 48E governs property
  placed in service after 2024.

  > ⚠️ **Section 48 does NOT cover packaged rooftop AC units or ordinary air-source heat pumps.** A
  > Carrier/Trane/Lennox RTU changeout — however high its IEER — is not §48 energy property. Claiming
  > a 6%/30% ITC on an air-source RTU portfolio is a hard error that a CPA will strike, and it is the
  > single fastest way to lose credibility on a commercial worksheet. §48 enters an HVAC job only when
  > the equipment is **ground-source/geothermal** (or CHP). For air-source commercial work the federal
  > lever is **179D**, plus utility/manufacturer cash.
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
4. **Fuel-test HEEHR before quoting it at all.** If the home currently heats with gas, propane, or oil
   and the customer intends to *remove* that system, HEEHR does not fund the swap under 2026 guidance.
   Route to dual-fuel (retain the fossil system) or to the state/utility stack instead.
5. **Utility rebates usually need pre-approval** — say so.
6. **Commercial → defer to the navigator** for 179D/Section 48/§6417; do not compute those inline.
7. **Always label dollar figures "estimate — confirm current program terms."**

---

*Maintained by the skill-evaluator / landscape-monitor jobs. If a federal figure here conflicts with
current IRS or DOE guidance, the live guidance wins and this file should be updated.*
