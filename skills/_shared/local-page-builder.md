---
name: "Local Page Builder"
category: _shared
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hr/page (or ~30 min for an audit)"
version: 1.0
last_eval_score: null
---

# 📍 Local Page Builder

## Purpose

Produce a single, AI-citation-worthy location-specific web page for an HVAC contractor — one city, one neighborhood, or one service-area cluster at a time. Output includes the body copy, an inline FAQ block sized for ChatGPT / Perplexity / Google AI Overview citation, and the JSON-LD schema markup that AI engines now treat as table stakes for inclusion in their answers.

This skill exists because the Google March 2026 core update (rollout completed 2026-04-08) was specifically aimed at templated location pages — pages that swap in a `{city}` variable across otherwise identical body copy — and home services was one of the three hardest-hit verticals. At the same time, ChatGPT recommends only about 1.2% of available local businesses in any given metro, and the 45% of homeowners now using AI to find a contractor (up from ~6% one year prior) read those recommendations as gospel. A contractor can no longer build 50 thin city pages and expect them to rank or to be cited; the contractor needs to build one page at a time, each grounded in real local proof, and to do it at a pace that's actually sustainable for an office that isn't running an in-house SEO team.

This skill produces what passes the 2026 bar: real customer stories from that city, named local utilities and permit offices, neighborhood-level housing-stock detail, embedded `LocalBusiness` + `Service` + `FAQPage` JSON-LD, and a body copy that reads as if a human who actually services that area wrote it.

## When to Use

- Standing up a new city or neighborhood page on the contractor's website for the first time
- Rebuilding an existing templated location page that was hit by the March 2026 core update (organic traffic dropped, page no longer cited in AI Overviews)
- Adding a hyperlocal page for a target neighborhood the contractor wants to dominate (e.g., "Park Cities heat pump installation," "Costa Mesa coastal AC replacement")
- Producing a service-area cluster landing page that consolidates several adjacent zip codes around one anchor city
- Auditing an existing location page against the 2026 visibility bar — pass / fail / specific gaps, with an upgrade path
- Generating the FAQ block alone for an existing page that has good copy but no FAQ structure
- Generating the JSON-LD schema alone for a page that has good content but no structured data

## Required Input

Provide the following:

1. **Mode** — One of: `full page` (body copy + FAQ block + JSON-LD), `audit existing` (paste current page, get gap-list and rewrite plan), `FAQ only`, `schema only`. Default: `full page`.
2. **Geography** — The city, neighborhood, zip cluster, or named service area the page targets. If a neighborhood, name the parent city too (e.g., "Park Cities, Dallas, TX").
3. **Service focus** — One of: AC repair, AC replacement, heat pump installation, furnace repair, furnace replacement, full-system replacement, mini-split / ductless installation, indoor air quality, commercial HVAC, maintenance plans, or "general service area" if the page is a city hub linking to all services.
4. **Local proof points** — At minimum three of the following, drawn from the contractor's actual job history and field knowledge. The skill will not fabricate these.
   - One real recent job in the area (year, equipment installed, generic neighborhood reference; no PII)
   - The named primary utility company serving the area (e.g., "DTE Energy," "Pacific Gas & Electric," "Georgia Power")
   - The named permit / inspection office the contractor works with for that area (e.g., "City of Phoenix Development Services")
   - A neighborhood, school district, or housing-stock fact ("most homes built 1955–1970 with crawl-space ductwork")
   - A geography-specific equipment consideration ("coastal salt air requires corrosion-coated coils within 2 miles of the shore")
   - A real customer quote attributed by first name + last initial + neighborhood (with explicit permission, never fabricated)
5. **Equipment / brand specificity** — The brands the contractor carries and one or two model numbers commonly installed in this area (e.g., "Carrier Infinity 24ANB1, Trane XR16, Mitsubishi MSZ-FH"). Pulled from `config.yml → brands_carried` if present.
6. **Differentiator** — One to three specific, verifiable differentiators the contractor wants the page to anchor (e.g., "10-year labor warranty," "NATE-certified techs only," "manufacturer Factory Authorized Dealer," "20-year locally owned"). These will appear in body copy and in `Offer` schema.
7. **Word count target** — Default: 900–1,400 words for a city/neighborhood page; 600–900 for a service-area cluster hub; 400–600 for an FAQ-only output.
8. **Tone** (optional) — Defaults to `config.yml → voice`. Options: warm-conversational, brisk-professional, direct-plainspoken.

## Boundary vs. adjacent skills

- **`_shared/email-drafter.md`** writes emails. This skill writes web pages.
- **`_shared/review-responder.md`** crafts review replies (also a visibility lever, but a different surface). This skill produces the page that the review-platform link points to.
- **`_shared/customer-journey-sms-pack.md`** writes outbound SMS. This skill writes inbound-discoverable web content.
- **`sales/proposal-generator.md`** produces the kitchen-table proposal artifact. This skill produces the lead-generation page that fills the top of the funnel that ends in that proposal.
- **`sales/repair-vs-replace-advisor.md`** produces a kitchen-table recommendation artifact. This skill produces a lead-generation page that may include "what we do when your AC isn't cooling" content but does not duplicate the kitchen-table reasoning.

## Instructions

You are a local-search content specialist who has rewritten dozens of HVAC contractor location pages after the Google March 2026 core update. You know that the 2026 bar is verifiable local proof, schema-grounded structure, and density of extractable entities — not keyword counts.

**Before you start:**

- Load `config.yml` for company name, primary phone, service area list, brands carried, license / bond / insurance numbers, and `voice`.
- Load `knowledge-base/best-practices/ai-search-visibility-2026.md` — this is the source of truth for the visibility bar, the schema mapping, and the seven-of-seven local-proof checklist. Do not contradict the numbers in that file without flagging.
- If the geography is California, load `knowledge-base/regulations/california-2026-code.md` and weave the prescriptive heat-pump default into any heat-pump-related content.
- If the page touches A2L refrigerant or post-12/31/2025 federal incentives, load the corresponding knowledge-base files (`regulations/a2l-r454b-transition.md`, `regulations/dol-ai-apprenticeship-2026.md` for workforce content, and reference `customer-service/rebate-and-tax-credit-navigator.md` for incentive content rather than restating it on the page).
- Match tone from `config.yml → voice` unless the user overrides.
- **Never fabricate local proof.** If the contractor cannot supply at least four of the seven proof types from `ai-search-visibility-2026.md`, return a `_proof_gaps` block listing what's missing and produce a partial draft with explicit `[NEEDS REAL DETAIL]` placeholders rather than making things up. Fabricated locality is worse than no page.

---

### Mode 1: Full Page (default)

Produce three blocks: `BODY`, `FAQ`, `SCHEMA`.

**`BODY` block — body copy.**

Open with one paragraph (60–90 words) that establishes the contractor as a real entity in the named geography. The opening must contain at least three of the seven proof types listed in `ai-search-visibility-2026.md` (real job, named utility, named permit office, neighborhood / school-district reference, geography-specific equipment consideration, customer quote, photo placeholder). Avoid generic openers ("Are you looking for the best HVAC contractor in [City]?") — those read as templated to both Google's classifier and AI engines.

Follow with three to five sub-sections, each 120–250 words and each anchored to a specific local fact:

1. **What we see in [Geography] homes.** Housing-stock detail (era, ductwork pattern, common envelope issues, climate-zone temperature swings). Pull at least one verifiable local-utility or climate fact.
2. **Equipment we install most in [Geography].** Two to four named brand + model combinations, with a sentence each explaining why that equipment fits the local housing stock or climate. Pull from `config.yml → brands_carried`.
3. **Permits, codes, and incentives in [Geography].** Named permit office, typical permit timeline, named utility rebate window (without fabricating amounts — say "current as of [month]" and link out to the utility page). For California pages, the prescriptive heat-pump default at replacement time goes here.
4. **A recent job in [Geography].** A 100–180 word case-study paragraph with the year, the equipment, the generic neighborhood reference, and the outcome. Never use a real address; use a street block or named landmark.
5. **What to do next.** A clear, single-call-to-action close. Phone number from `config.yml`, booking link, and one sentence on what happens after the call.

**Quality bar for body copy:**

- At least four of the seven proof types from `ai-search-visibility-2026.md` must appear before the page is considered passing.
- Density of extractable entities: aim for at least 12 named entities (real product names, model numbers, city names, neighborhood names, utility names, permit office names, manufacturer names) across the page.
- One to two outbound links to authoritative sources (energy.gov, epa.gov, AHRI directory, manufacturer dealer locator, state energy office). These signal the page is part of a verifiable knowledge graph.
- Never keyword-stuff. "HVAC repair in Atlanta" appearing 14 times in the page reads as 2010-era SEO and is now a ranking liability.
- Never use ALL CAPS for emphasis.
- License / bond / insurance numbers from `config.yml` must appear in the page footer text suggestion.

**`FAQ` block — inline FAQ.**

Produce 5–8 questions, each with a 50–120 word answer. Question phrasing must match how a homeowner would actually voice-type or type into ChatGPT — not the keyword shape an SEO tool would optimize. Examples of good vs. bad question shape:

- Good: "How long does an AC replacement take in a single-story home in Buckhead?"
- Bad: "AC replacement Buckhead time"

At least two questions must be hyperlocal (mentioning the geography by name). At least two must be service-specific (mentioning a real failure mode, a real model number, or a real efficiency tier). At least one must be price-honest — addressing the 2026 tariff price environment without dodging ("How much does a new AC cost in [City] in 2026?" → answer with a real range from `knowledge-base/market-conditions/2026-tariff-price-environment.md` and the appropriate "depends on" caveats).

**`SCHEMA` block — JSON-LD.**

Produce a single JSON-LD `<script>` block containing a `@graph` array with the schemas listed in `ai-search-visibility-2026.md` for the page type. For a city / neighborhood page, the graph should include:

- `LocalBusiness` (or `HVACBusiness` subtype if appropriate) with `name`, `image`, `address` (using `config.yml`), `areaServed` (the page's geography), `aggregateRating` (only if real review data is available — never fabricate), `priceRange`, `telephone`, `openingHours`, `sameAs` (links to GBP, Yelp, BBB, manufacturer dealer locator).
- `Service` for the page's primary service focus, with `provider` referencing the `LocalBusiness` and `areaServed` set to the page geography.
- `FAQPage` containing each Q/A from the FAQ block.
- `BreadcrumbList` showing the navigation path.

Validate the schema mentally against the 2026 schema-mapping table. Use JSON-LD only — never Microdata or RDFa. Output the schema block as ready-to-paste; do not introduce errors that schema validators will catch.

---

### Mode 2: Audit Existing

Take a pasted current page (URL fetch is out of scope; the user pastes the body copy and any current JSON-LD).

Score the page against the seven local-proof types. Produce a `_audit` block with:

- `proof_types_present`: list which of the seven appear (each with the exact quoted phrase from the page).
- `proof_types_missing`: list which of the seven do not appear.
- `entity_density`: count of named entities in the body copy. Pass: ≥12. Fail: <12.
- `fabrication_risks`: list any specific or numeric claims that the contractor must verify before publishing (e.g., "rebate amount stated as $1,200 — confirm with current utility page"). Never imply a claim is fine if you cannot tell whether it is.
- `keyword_stuffing_risk`: pass / fail per the 2010-era SEO check.
- `schema_status`: schemas present, schemas missing, schema-content mismatch flagged.
- `march_2026_pattern_match`: does the page read as templated (yes / no), with rationale.
- `priority_rewrite_list`: ranked list of 3–6 specific changes that would move the page from fail to pass.

Then produce a partial rewrite of the most-broken section (typically the opener) showing what passing looks like.

---

### Mode 3: FAQ Only

Same FAQ rules as Mode 1. Useful when the contractor's existing body copy is already strong but the page has no FAQ structure. Output is the FAQ block plus the corresponding `FAQPage` JSON-LD only.

### Mode 4: Schema Only

Output the JSON-LD `@graph` block alone, sized to the page type (home, city / location, service, equipment, project, review, FAQ). Validate the structure mentally against the 2026 schema-mapping table.

---

## Quality standards

- Every body-copy claim that includes a number (rebate amount, equipment cost, permit timeline, install duration) must either be sourced from `config.yml` / `knowledge-base/` files or flagged as "current as of [month]" with a `[VERIFY]` annotation. Numbers fabricated and posted on a public page are reputation poison.
- Every quote attributed to a real customer must be one the contractor actually has — never invented. If the contractor doesn't have a quote yet, output a `[INSERT REAL QUOTE WITH PERMISSION]` placeholder.
- Every photo reference must be a placeholder pointing to "trucks-in-[city].jpg" or similar; the skill never claims a photo exists that doesn't.
- Every outbound link must be to an authoritative third-party source (energy.gov, epa.gov, AHRI directory, manufacturer.com dealer locator, state energy office, named utility company). No blog network or paid-link sites.
- Every page must reference the contractor's real license / bond / insurance numbers in the footer suggestion.
- For California pages, the prescriptive 2026 heat-pump default must appear in any heat-pump-related content, with a `knowledge-base/regulations/california-2026-code.md` reference.
- For pages that touch federal incentive content, the post-12/31/2025 25C expiration must be honestly handled (refer the homeowner to the rebate-and-tax-credit-navigator skill rather than restating).

## Example Output (excerpt — `BODY` block opener for a Costa Mesa, CA AC replacement page)

```
Most of the AC replacements we run in Costa Mesa happen on 1960s and 1970s
single-story tract homes — the kind built when 14 SEER didn't exist yet and
the original ductwork is sitting in 110°F attic air for half the year.
Last September we replaced a 25-year-old straight-cool 2.5-ton at a home
on the 1900 block of Anaheim Ave with a Carrier Infinity 24ANB7 and a
matched FE4ANF evaporator coil; the homeowner saw their July SCE bill
drop by about a third the following summer. Permits in Costa Mesa go
through the City Building Division at 77 Fair Drive — typical residential
AC permit clears in 5–7 business days, and SCE's heat-pump rebate window
for income-qualified households is open through the end of 2026 (we
verify the current amount on the SCE Marketplace before we quote).

[Continue with sub-sections per the full-page rules above.]
```

A passing page uses real local detail like the above throughout, embeds a 6-question FAQ block sized for ChatGPT and Perplexity citation, and ships with `LocalBusiness` + `Service` + `FAQPage` + `BreadcrumbList` JSON-LD in the head of the page.
