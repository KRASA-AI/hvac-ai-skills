# Collections & Final-Notice State Rules — Reference Notes

Ground-truth reference for any HVAC AI skill that drafts a payment reminder, final notice, lien
statement, or collections letter — primarily `admin/invoice-followup-drafter.md` (Tier 4 is
**gated** on this file). Skills should **defer to this file** rather than hardcoding legal
language inline.

**Currency:** Reviewed 2026-07-20. Debt-collection and mechanic's-lien rules vary by state, change
by legislature, and turn on facts (consumer vs. commercial, residential vs. non-residential
property, whether a preliminary notice was served on time). **Every entry below carries a "confirm
current statute and route to office/counsel before quoting exact legal text" posture.** This file
tells a skill *whether a category of language is permissible and roughly how to frame it* — it does
**not** substitute for the current statutory disclosure text or for legal advice.

**Fail-safe rule:** when in doubt, **omit** lien/legal-threat language and route the letter to
office staff for review. A softer letter that collects a day later is always better than an
overreaching one that draws a state-UDAP or FDCPA-style complaint. A creditor collecting its own
debt in its own name is generally outside federal FDCPA (which governs third-party collectors), but
**state mini-FDCPA / UDAP statutes frequently apply to first-party creditors** — so the tone rules
below are not optional.

---

## How skills should use this file

1. **Disclosure text (Tier 4 / final notice):** For any state flagged "disclosure required" below,
   the letter must carry the state-required notice. **Do not paraphrase or invent statutory text.**
   Insert the placeholder `[STATE-REQUIRED DISCLOSURE — <STATE> — confirm current text with office/counsel]`
   and route to staff. Ship the placeholder, never a fabricated statute quote.
2. **Lien language:** Include a mechanic's-lien statement **only** where the table below marks the
   relevant category (residential vs. commercial) permissible, and only reference the statute by
   citation as a pointer — never assert that a lien *will* be filed or that filing deadlines have
   been met (they usually turn on a preliminary/pre-lien notice served much earlier).
3. **Legal-threat ceiling (all states):** No "we will sue you," no implied criminal consequence, no
   personal-blame language, no comparison to other customers, no threat of action the company does
   not actually intend or is not legally entitled to take. This ceiling applies everywhere and
   overrides any per-state note.

---

## Universal tone / conduct rules (apply in every state)

- Contact only at reasonable hours; honor any written request to cease a specific contact channel.
- State the amount, invoice number, and service date accurately; correct any known error before dunning.
- Offer a dispute path in every final notice ("respond in writing if any portion is disputed").
- Never threaten collections referral, lien, or service-pause unless the company will actually do it.
- For anything touching a lien, small-claims filing, or a state with "disclosure required" below:
  route to office staff / counsel review before send.

---

## Per-state quick table

Legend — **Disclosure:** does a first-party collections/final notice trigger a state-specific
required disclosure? (**confirm exact text locally — placeholder only**). **Res. lien / Com. lien:**
is a mechanic's/construction lien category generally available to an HVAC service contractor
(subject to notice + deadline requirements verified per job)? **Tone flag:** states with
aggressive UDAP/mini-FDCPA enforcement where extra restraint is warranted.

| State | Disclosure | Res. lien | Com. lien | Notes (verify per matter) |
|-------|-----------|-----------|-----------|---------------------------|
| CA | Required (Rosenthal Act mini-FDCPA reaches many first-party creditors) | Generally available w/ 20-day preliminary notice | Available w/ preliminary notice | Tightest tone state; strict notice-timing. Route Tier 4 to review. |
| NY | Required (state + NYC consumer-collection rules) | Available, notice-dependent | Available | Tight; NYC has additional rules. Placeholder + review. |
| MA | Required (93A UDAP; debt-collection regs) | Available, notice-dependent | Available | 93A treble-damage exposure — restraint on tone. Placeholder + review. |
| TX | Required (Finance Code Ch. 392 debt collection) | **Available** (constitutional/statutory; homestead rules apply) | Available | Res. lien permissible but homestead + notice rules are strict. |
| CO | Standard | **Available** | **Available** (C.R.S. § 38-22-101 et seq.) | Cite statute as a pointer only; never assert a filing is perfected. |
| FL | Standard | **Available** (Ch. 713) | **Available** | 45-day NTO and other notice deadlines usually control. |
| NV | Standard | **Available** (NRS 108) | **Available** | Pre-lien notice deadlines strict. |
| AZ | Standard | **Available** (Title 33) | **Available** | 20-day preliminary notice typical. |
| Most other states | Standard tone rules apply | Commercial lien commonly available; residential availability **varies — verify** | Commonly available | Default to omit-and-review when unconfirmed. |

**Residential mechanic's-lien "generally permissible" shortlist used by the skill:** CA, TX, CO,
FL, NV, AZ (each still subject to preliminary-notice + deadline verification per job). **Commercial
mechanic's liens** have broader availability across states, but availability ≠ perfected — timing
and notice always control. **When a state is not confirmed on this list, omit lien language.**

---

## Disclosure-text handling (the part Tier 4 depends on)

For CA / NY / MA / TX (and any future "Required" row), the final notice must include the state's
mandated collection disclosure. Because that text is statutory, versioned, and occasionally
jurisdiction-specific (e.g., NYC vs. NY State), this file **intentionally does not reproduce it**.
The skill inserts:

```
[STATE-REQUIRED DISCLOSURE — <STATE> — confirm current text with office/counsel before send]
```

and flags the letter for staff review. This is the fail-safe: a placeholder that a human fills from
the current statute is correct; a paraphrased or model-generated "disclosure" is a compliance risk
and is prohibited.

---

## Change log

- 2026-07-20 — File created to resolve the standing gap where `invoice-followup-drafter.md` Tier 4
  referenced this path (9 references) but the file did not exist, causing every Tier 4 letter to
  emit an unfilled placeholder in legally-sensitive collections language. Structure + permissibility
  table + fail-safe placeholder discipline established; exact statutory disclosure text deliberately
  left to counsel/office confirmation.
