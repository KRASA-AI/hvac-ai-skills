# AI Search Visibility for HVAC Contractors (2026) — Reference Notes

Quick-reference facts for any HVAC AI skill that touches website content, location pages, Google Business Profile, FAQ content, or anything that shapes whether a contractor appears as a recommendation inside a homeowner's ChatGPT, Perplexity, Google AI Overview, Gemini, or Angi-in-ChatGPT query. Use as ground truth when drafting on-site copy, schema markup guidance, or competitive-positioning language.

## Why this anchor exists

The way homeowners discover HVAC contractors changed materially in late-2025 and Q1 2026. A monitor-log watch item carried since 2026-04-20 ("Google LSA + GBP auto-edit — watching for consolidation into a `best-practices/` entry if sources keep crystallizing") was tripped this cycle by three converging events: the Google March 2026 core update completed 2026-04-08 with templated location pages as its named target, the Angi app integration into ChatGPT launched 2026-03-04, and the consolidation of the "AI is now ~45% of local discovery" datapoint across Marchex, Metricus, MarketingCode, and Contractor Commerce coverage. Together these reshape what content earns a citation and what content gets ignored.

This document is the single source of truth for AI-search-visibility numbers, tactics, and platform behaviors used inside `_shared/local-page-builder.md`, future `_shared/review-responder.md` updates, and any future `gbp-hygiene.md` companion. Skills must not fabricate visibility numbers — pull from this file or flag as estimate.

## The 2026 visibility landscape — anchor numbers

- **AI is the third most-used local-discovery channel.** Roughly 45% of homeowners now use ChatGPT, Perplexity, Google AI Mode, Gemini, or a similar tool when looking for a local service contractor — up from approximately 6% one year prior. Behind only Google traditional search and Facebook, ahead of Yelp and Nextdoor.
- **AI Overviews appear in ~40% of local-business queries.** Google's generative answer block now serves an estimated two billion monthly users and consumes the result page real estate that "best HVAC near me" used to surface organic listings into.
- **ChatGPT cites a tiny fraction of local businesses.** Industry estimates put ChatGPT's local-business recommendation rate at around 1.2% of available locations in any given metro. The contractors who get cited are not the ones with the most reviews — they are the ones with structured, AI-legible website content and a consistent presence inside the data sources ChatGPT pulls from (Angi, Yelp, BBB, Google Reviews, manufacturer dealer locators, news mentions).
- **Different AIs recommend different contractors.** Perplexity tends to cite a wider pool (often 10+ sources per query) and rewards specific, well-built websites. ChatGPT leans heavily on review platforms and tends to favor national franchises over local independents. Google AI Overviews lean on its existing local-pack signals plus structured data. Gemini follows AI Overview behavior with deeper integration into Google Maps and reviews.
- **Click-through is collapsing.** Industry coverage estimates roughly 51% of searches now end without a click — the user gets the answer they need from the AI summary, the local pack, or the GBP card. Half of "discovery" no longer involves a website visit at all, which makes Google Business Profile content the de-facto storefront.

## What got hit by the Google March 2026 core update

- Update window: rolled out across late-March 2026; completion confirmed by Google on 2026-04-08.
- The single biggest target was templated location pages — pages with the same four to five paragraphs of body copy and a single `{city}` swap. Common patterns hit hard: "HVAC Repair in [City]", "AC Replacement in [City]", "Furnace Service [City]" with otherwise identical content across 30, 50, or 100 cities.
- Home services, legal, and healthcare were the three verticals with the largest ranking shifts. HVAC contractor sites that built city pages by template (often through SEO-vendor packages) saw declines reported in the 30–60% organic traffic range.
- What survived and gained: pages with verifiable local proof — a real job address or street block, named local utility, named local permit office, photos taken in that geography, neighborhood references, real customer quotes attributed to that area.

## What AI engines reward (so skills can produce it)

- **Schema markup is now table stakes, not advanced.** Both Google and Microsoft confirmed in March 2026 that they use structured data for generative AI features. Pages with appropriate schema get cited 2–3x more often in AI answers than pages without. The schemas that matter most for HVAC contractor pages: `LocalBusiness`, `HVACBusiness` (a more-specific subtype), `Service`, `FAQPage`, `Review`, `Offer`, `Place`, and `BreadcrumbList`. JSON-LD is the format every major engine reads — never use Microdata or RDFa for new builds.
- **FAQ blocks earn ChatGPT citations disproportionately.** ChatGPT prefers conversational Q&A structure for citation. A page with an inline FAQ block (4–8 specific questions, each with a 50–120 word factual answer) tends to outperform a page with the same total word count but no FAQ structure.
- **Specific, verifiable, geographic detail beats general copy.** "Serving Orange County" loses to "We installed 47 systems in Costa Mesa, Newport Beach, and Huntington Beach over the past 12 months, mostly Carrier Infinity 24ANB1 and Trane XR16 replacements on 1980s-era ductwork." AI engines extract entities and prefer pages dense with extractable, verifiable entities (real product names, real model numbers, real city names, real permit offices, real utility companies).
- **Authoritative outbound citation works in your favor.** Pages that link out to authoritative sources (DOE energy.gov, EPA epa.gov, AHRI directory, manufacturer pages, state energy office) score better with AI engines because they signal that the contractor's content is part of a verifiable knowledge graph.
- **Reviews still matter — but for different reasons.** ChatGPT and Angi-in-ChatGPT lean heavily on review platforms. Google AI Overviews use the GBP rating + review count. The number itself matters less than the volume of recent (last 90 days), specific, verbal-detailed reviews. A 4.6-star contractor with 240 reviews and frequent 100+ word recent reviews tends to outperform a 4.9-star contractor with 60 short reviews.

## Angi-in-ChatGPT and what it means for non-Angi contractors

- On 2026-03-04, Angi launched the Angi app inside ChatGPT, allowing homeowners to move from a question ("my AC isn't cooling, who do I call in Tampa") to a quote-request inside the chat.
- For contractors who participate in Angi, this is incremental visibility — the Angi profile becomes ChatGPT-discoverable.
- For contractors who do not participate in Angi, this raises the importance of the other paths into ChatGPT: structured website content with schema, a strong Yelp / BBB / Google Reviews presence, and inclusion in manufacturer dealer locators (Carrier Factory Authorized, Trane Comfort Specialist, Lennox Premier, Bryant Factory Authorized, Mitsubishi Diamond, Daikin Comfort Pro).
- The contractor-side decision is not "join Angi" or "don't join Angi" — it's "which paths into ChatGPT do we own?" Generally one or two paths is enough; zero paths is a 2026 visibility liability.

## Templated location pages — fix patterns that recover ranking

Skills that produce location-specific content for an HVAC contractor must follow these rules to avoid the Google March 2026 penalty pattern. A page passes when at least four of the following are present:

1. **A real address or street-block reference** within the city (e.g., "We installed a 3-ton Carrier Infinity at a 1972 ranch on the 4400 block of Riviera Drive last August").
2. **The named local utility company** that serves the city, plus its current rebate window or rate environment for HVAC ("DTE Energy is running a $400 heat-pump rebate through 2026-12-31").
3. **The named permit / inspection office** the contractor works with most, plus a procedural detail that proves familiarity ("permits in Phoenix go through Development Services at 200 W. Washington — typical residential AC permit clears in 3 business days").
4. **A photo (or photo placeholder) taken in the city.** A photo of a real truck in a real driveway in that city beats a stock equipment photo every time.
5. **A neighborhood, school district, or hyperlocal reference** ("most homes in Park Cities Highland Park were built between 1940 and 1965 with crawl-space ductwork that loses 25–30% of treated air").
6. **A specific equipment / install detail** for the housing stock common to that area ("Costa Mesa's coastal salt-air environment is hard on outdoor coils — we install Carrier with corrosion-coated coils as standard for homes within 2 miles of the coast").
7. **A real customer quote** attributed by first name and last initial and a neighborhood ("'Joe and his crew were in and out in one day.' — Maria S., Buckhead").

Three or fewer of these and the page reads as boilerplate to both Google's classifier and AI engines.

## What NOT to do

- Don't use AI to write 50 location pages with the same structure swapping in city names. The March 2026 core update specifically targeted this pattern. AI-generated content per se is not penalized — generic, unverifiable AI-generated content is.
- Don't fabricate local detail. Don't claim to have installed in a neighborhood you've never serviced. Both AI engines and competitors increasingly cross-check, and a fabricated address is reputation poison if a real customer in that city calls it out.
- Don't keyword-stuff. "HVAC repair Atlanta best HVAC company Atlanta affordable HVAC service Atlanta" reads as 2010-era SEO and gets penalized.
- Don't rely on a single visibility path. If the contractor's only inbound channel is Google organic, the next core update can take 30%+ of leads in a week. Build at least two independent paths (organic, GBP, Angi, manufacturer locator, Yelp, BBB).
- Don't cargo-cult schema. Adding `LocalBusiness` schema to a page that doesn't have a real local business identity (NAP consistent, hours filled, real photos) is worse than no schema — engines interpret schema-content mismatch as a quality signal.

## Quick lookup — schemas to use by page type

| Page type | Required schemas |
|---|---|
| Home page | `LocalBusiness` or `HVACBusiness`, `Organization`, `BreadcrumbList` |
| City / location page | `LocalBusiness` (with `areaServed` set to the city), `Service`, `FAQPage`, `BreadcrumbList` |
| Service page (e.g., "AC repair") | `Service` (with `areaServed`, `provider`, `offers`), `FAQPage`, `BreadcrumbList` |
| Equipment page (e.g., "Carrier Infinity") | `Product` (or `Service` if framed as installed service), `Offer`, `Review` |
| Project / case-study page | `Article`, `ImageObject`, `Place` (for the install location), `Review` |
| Review / testimonial page | `Review`, `AggregateRating` (sitewide on `LocalBusiness`) |
| FAQ / knowledge page | `FAQPage`, `Article` |

Always use JSON-LD. Never inline schema in body copy.

## Sources

- ACHR News — "ChatGPT Is Forcing HVAC Contractors to Post Prices Online" (2026); "Crawl, Walk, Run: How HVAC Contractors Are Successfully Adopting AI in 2026" (166041); "Survey: 25% of Residential Contractors Using AI to Boost Business" (166074); "AI and HVAC: Techs are Safe, but Office Roles Face High Risk" (165979); April 20 2026 Issue (Edition 2337).
- MarketingCode — "Google March 2026 Core Update: What Contractors Must Do Now"; "45% Of Consumers Use AI Search For Local Services — Is Your Business Visible?"; "ChatGPT Recommends Only 1.2% Of Local Businesses — Google Rankings Won't Save You"; "38% Of Contractors Use AI In 2026"; "Angi Is Inside ChatGPT Now. Google Clicks Are Down 42%. Wake Up."; "Gemini Replacing Google Assistant 2026 — What Contractors Must Do Now"; "Every AI Recommends Different Contractors. And Siri Is Next."; "Google Is Auto-Editing Contractor Business Profiles"; "70% Of Contractors Are On LSAs Now"; "TikTok Local Feed: The AI Search Engine Most Contractors Don't Know Exists"; "Hottest March Ever + 110,000 Missing HVAC Techs = Summer 2026 Crisis."
- Search Engine Land — "Google March 2026 core update rollout is now complete" (2026-04-08); "How schema markup fits into AI search — without the hype"; "Schema and AI Overviews: Does structured data improve visibility?"
- Search Engine Journal — "Google Confirms March 2026 Core Update Is Complete."
- Search Engine Roundtable — "Google March 2026 Core Update Rolling Out."
- Schema App — "What 2025 Revealed About AI Search and the Future of Schema Markup."
- Stackmatix, Wellows, WolfPack Advising, Writesonic, Digidop — independent 2026 schema-and-AI-search tactical guides.
- Scorpion — "Google's March 2026 Core Update: What Local Service Businesses Need to Know."
- ALM Corp — "Google March 2026 Core Update: Confirmed Timeline, SEO Impact, and What Site Owners Should Do Next"; "SEO for HVAC Companies: Advanced Tactics for 2026."
- Angi (ANGI) press release / Investor Relations — "Angi Launches the Angi App in ChatGPT" (2026-03-04); Cheddar coverage; Contractor Magazine "Angi Expands AI Strategy with ChatGPT App Integration"; Homepros.news "Angi launches app in ChatGPT."
- Marchex — "ChatGPT Recommended You Guys."
- Metricus — "Why Doesn't AI Mention My Contracting Business?"; "Why Your HVAC Business Is Getting Fewer Calls in 2026 (It's Not Your Reviews)."
- Housecall Pro / GlobeNewswire — "Housecall Pro Launches AI Accelerator Week to Help Home Service Pros Put AI to Work in Their Businesses" (announced 2026-04-24, event runs 2026-04-27 through 2026-05-01) — featured Claude, NotebookLM, ChatGPT, Perplexity integration into FSM workflows.

## Refresh cadence

Quarterly. Next refresh: 2026-07-25. Trigger an off-cadence refresh if (a) Google issues another core update with named local-service impact, (b) a major AI engine adds or removes a citation source (e.g., OpenAI partners with another marketplace beyond Angi, or a manufacturer launches an in-AI dealer locator), (c) Google or OpenAI publishes new guidance on schema requirements for citation, or (d) a state attorney general / FTC issues guidance on AI-generated local content.
