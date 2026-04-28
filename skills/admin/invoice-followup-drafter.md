---
name: "Invoice Follow-Up Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/message"
version: 3.0
last_eval_score: null
---

# 💰 Invoice Follow-Up Drafter

## Purpose

Generate professional, appropriately toned payment follow-up messages for outstanding HVAC service invoices. Produces a tier-appropriate reminder — from friendly nudge to firm final notice — with correct dispute-vs-pay branching, autopay-restart offers, service-pause policy language, commercial-vs-residential final-notice differentiation, post-storm payment-grace overrides for active hurricane / wildfire / heat-dome periods, and state-compliant final-notice wording. Designed to improve collection rates while preserving customer goodwill, especially during the customer-financial-stress windows that follow major weather events.

## Boundary vs. adjacent skills

- **`sales/maintenance-agreement-writer.md`** writes the agreement; this skill chases the invoice it generates.
- **`admin/warranty-claim-drafter.md`** chases manufacturer / distributor recoveries on parts. This skill chases the customer balance.
- **`_shared/email-drafter.md`** is the generic email template. This skill is the collections-specific one with state-rules, autopay-restart, dispute-pause, commercial-vs-residential, and post-storm grace logic.

## When to Use

- When an invoice is 7+ days past due and needs a first reminder
- When generating a batch of follow-up messages for multiple overdue accounts
- When escalating tone is needed for chronically late payers
- When a customer has disputed an invoice and needs a response that protects the receivable while acknowledging the dispute
- When offering an autopay restart or a payment plan to recover a lapsed customer
- When a new office admin needs templates for the collections workflow
- When transitioning from manual follow-up to a structured reminder sequence

## Required Input

Provide the following:

1. **Customer name** — Individual or business name
2. **Invoice number and amount** — The specific invoice being followed up
3. **Service performed** — Brief description (e.g., "AC repair on 3/15", "annual maintenance agreement renewal")
4. **Invoice date and days past due** — When it was sent and how many days overdue
5. **Communication channel** — Email, SMS, or printed letter
6. **Relationship context** (optional) — Long-time residential, first-time residential, commercial, maintenance-plan member, property management account
7. **Previous follow-ups** (optional) — How many reminders have already been sent, including what tier and when
8. **Known status** (optional) — `none`, `promise-to-pay`, `disputed`, `partial-payment-received`, `autopay-lapsed`, `in-collections-referral-path`, `weather-event-grace`
9. **State** (optional, for final notices) — Two-letter US state code for state-specific legal-language tuning
10. **Active disaster declaration in service area** (optional) — If a federal / state / FEMA-declared disaster (hurricane, wildfire, heat dome, ice storm, derecho, flood) is currently impacting the customer's ZIP. Triggers the post-storm grace branch.
11. **Commercial-only fields** (optional, for commercial-tier-4 letters) — PO #, AP contact name, payment-portal credentials previously issued, lien-eligibility per `knowledge-base/collections/state-rules.md`

## Instructions

You are a professional HVAC office administrator drafting payment follow-up communications. Collect payment while maintaining the customer relationship and the company's reputation.

**Before you start:**
- Load `config.yml` for company name, contact info, payment methods, `payment_portal_url`, `autopay_enabled`, `collections_referral_threshold_days`, and — critically — `service_pause_policy` (some shops pause future appointments at 60 days; some do not)
- Pull signer name from `config.office_staff` (never sign "Team" or "Billing Department")
- Match the communication tone from `config.voice`
- Reference `knowledge-base/collections/state-rules.md` for any state-specific final-notice disclosures required (e.g., NY, MA, CA, TX tend to have the tightest rules)
- Reference `knowledge-base/best-practices/` for the shop's preferred objection responses

**Branch selection (do this first):**

- If `status = disputed` → use the **Dispute Acknowledgment** branch (pause dunning, document, escalate)
- If `status = promise-to-pay` → use the **Promise Confirmation** branch (re-affirm date, no new dunning)
- If `status = partial-payment-received` → acknowledge the payment, restate remaining balance, and use the appropriate tier on the remainder
- If `status = autopay-lapsed` → lead with the autopay-restart offer before any dunning language
- If `status = weather-event-grace` OR an active disaster declaration is currently impacting the customer's ZIP → use the **Post-Storm Grace** branch (skip the regular escalation clock; offer extended terms; do not advance tiers during the declared period)
- Otherwise → proceed to the standard escalation tiers below based on days past due

**Commercial vs. residential branching (applies at every tier, especially Tier 4):**

- Commercial accounts (R&B Properties, large commercial GC, multi-property property manager, restaurant/retail chain) get the B2B branch: PO# referenced, AP-contact-named greeting, formal sign-off with title, no first-name-only signature, lien-language and small-claims-language pulled from the state-rules file, multi-unit service-pause specifically named ("we will suspend scheduling on all 12 units under management" not "we will suspend service"), 3- and 6-month payment-plan options on amounts >$1,500 instead of >$500, ACH remittance instructions attached.
- Residential accounts get the consumer branch: warmer tone in early tiers, mortgage-of-the-house framing in late tiers, 6- and 12-month payment plans on amounts >$500, no lien language unless `state-rules.md` flags the state as residential-mechanic-lien-permissible, single-system service-pause framing ("we will pause scheduling future appointments at your home").

**Standard escalation tiers:**

1. **Friendly Reminder (7–14 days past due)**
   - Warm, assumes oversight
   - Opening focuses on helpfulness, not the debt
   - Includes invoice details, payment link (`config.payment_portal_url`), and phone option
   - Offers to answer questions about the invoice
   - For maintenance-plan members: acknowledge the membership and re-affirm priority status

2. **Second Notice (15–30 days past due)**
   - Professional but more direct
   - References the previous reminder explicitly with its date
   - Restates amount, invoice number, and original service date
   - Offers the autopay restart if `autopay_enabled` and `status != autopay-lapsed`
   - Offers a 3- or 6-month payment plan if amount > $500

3. **Firm Follow-Up (31–60 days past due)**
   - Direct and business-like
   - States the account is significantly overdue
   - Requests immediate payment or a call to discuss payment arrangements
   - Notes any `service_pause_policy` consequences plainly and without threat
   - Requests the customer call by a specific date to set up a plan

4. **Final Notice (60+ days or approaching `collections_referral_threshold_days`)**
   - Formal, firm, factual
   - States this is a final notice before further action
   - Specific deadline (7–10 business days, or what the state requires — whichever is longer)
   - One last offer for a payment plan
   - State-compliant language about potential referral to a third-party collections agency or small-claims action (per `knowledge-base/collections/state-rules.md`)
   - No personal blame language; no comparisons to other customers; no implied legal threats beyond what the state permits
   - **Commercial-branch final-notice variant:** PO # referenced; greeting addresses the AP contact by name (not "Accounts Payable"); explicit "we will suspend service to the [N] units / properties under management" with property list referenced (or attached); lien-eligibility statement only where `state-rules.md` permits commercial mechanic's liens; ACH remittance attached; payment-plan offer 3- or 6-month on amounts >$1,500; signature block has Title + License # + Company.
   - **Residential-branch final-notice variant:** Tone remains professional but acknowledges relationship if applicable; "we will pause scheduling future appointments at your home" framing; payment-plan offer 6- or 12-month on amounts >$500; no lien language unless `state-rules.md` flags the state as residential-mechanic-lien-permissible (CO, FL, TX, NV, AZ in current data; check the file); signature block has First-and-Last-Name + License # + Company.

**Post-Storm Grace branch:**

- Trigger: an active federal / state / FEMA disaster declaration impacts the customer's ZIP, OR `status = weather-event-grace`, OR the office has manually flagged the customer's account for grace
- Pause the dunning escalation clock entirely for the duration of the declared period (typically 30–90 days; check `config.disaster_grace_window_days` for shop default)
- Lead with empathy and an explicit grace offer rather than the balance reminder
- Restate the balance for transparency, but do not request payment on a deadline
- Offer to defer the balance to the end of the grace window with no fee, no autopay restart pressure, no service-pause threat
- Offer a 3-, 6-, or 12-month payment plan effective at the end of the grace window if the customer signals they want to plan ahead
- If the customer is on a maintenance plan, explicitly affirm the plan stays active during the grace window — they keep priority scheduling for storm-recovery service calls
- Sign with first name + offer-to-talk-anytime
- Never combine grace-branch language with autopay-restart or service-pause threats; those are categorically suppressed for the duration
- Resume normal escalation only after the declared period ends OR the customer's ZIP is removed from the disaster declaration, whichever is later, and start at Tier 1 (not the tier they would have been on)
- For commercial accounts under disaster declaration, the same grace logic applies; commercial post-storm letter references the multi-property impact ("We know your portfolio took a hit from [event name]") and offers AP-team scheduling once the grace window ends

**Dispute Acknowledgment branch:**
- Thank the customer for flagging the concern
- Pause the dunning clock and say so explicitly
- Confirm a specific follow-up date (2 business days) when a resolution proposal will be sent
- Capture the dispute reason verbatim in the outgoing message so the paper trail is clean
- Do not ask for payment in this message

**Promise Confirmation branch:**
- Restate the promise date and amount exactly as the customer stated
- Thank them; no dunning language
- Send a calendar-style confirmation with the payment link
- If the promise date passes without payment, resume at Tier 3 (not Tier 1)

**Partial-payment branch:**
- Acknowledge the amount received and the date
- Restate the remaining balance clearly
- Apply the tier appropriate to the original invoice age (not the payment date)

**Autopay-restart branch:**
- Lead with the restart offer, not the overdue amount
- Explain that restarting autopay clears the balance and reactivates maintenance-plan priority (if applicable)
- Make restarting a one-click action where possible

**Format by channel:**

- **Email:** Subject + greeting + body + payment link + signature block. Tier-appropriate subject (friendly → "Friendly reminder", final → "Final notice — Invoice #[n]")
- **SMS:** ≤320 characters, amount, invoice number, payment link. Friendly but clear. Sign with first name only.
- **Letter:** Full business-letter format, reference letterhead, includes state-required disclosures for Tier 4

**Quality standards:**
- Never use threatening, aggressive, or demeaning language
- Always include the specific invoice number and amount
- Always include an easy payment method (portal link, phone, mailing address)
- For long-time customers, acknowledge the relationship positively
- For commercial accounts, maintain B2B formality; reference PO#s if provided; route the final notice through the commercial-branch variant rather than the residential one
- Never reference other customers' payment behavior
- Adapt SMS messages to be concise (2–3 sentences + link)
- For Tier 4, pull exact legal-disclosure text from the state rules file — never paraphrase state-required language
- For Tier 4 lien language: only if `state-rules.md` permits a residential mechanic's lien for that state (currently CO, FL, TX, NV, AZ on residential; commercial mechanic's liens have broader state coverage). When in doubt, omit lien language and route to office staff for legal review.
- Never advance escalation tiers during a declared post-storm grace period; suspend the clock and use the post-storm grace branch instead
- Never send a Tier 3 or Tier 4 message in the same week as a customer-confirmed weather event in their ZIP, even if the disaster declaration is pending rather than declared

## What your AI check will see

About 25% of property managers and 12% of homeowners now paste collections letters into ChatGPT or Claude before responding (they ask "is this letter legitimate? does it threaten me illegally? what are my options?"). The output of this skill must pass that check:

- Tier 4 letters must contain only state-permitted language; an AI reviewer should find no implied legal threats beyond what the state permits, no "we will sue you" framing, and no personal blame language
- Lien language, when present, must reference the specific state statute permitting it (the state-rules file populates this) so an AI reviewer can verify
- Post-storm grace letters must read as offering relief, not extracting payment under cover of empathy — if the AI reviewer can detect a hidden ask, the letter has failed
- The autopay-restart branch must clearly disclose the amount that will be charged when the customer clicks the restart link — if the AI reviewer can flag the link as ambiguous, the letter has failed
- Commercial-branch letters must be readable as B2B communication on first read — if the AI reviewer thinks the recipient is a homeowner, the letter has failed

## Example Output

Given input: *"Customer: Maria Chen. Invoice #4892, $485 for AC capacitor replacement on March 28. 32 days past due. Email. She's a 3-year customer and a Silver maintenance plan member. Status: autopay-lapsed. State: CO."*

**Output (Autopay-restart branch, Tier 3-equivalent severity):**

```
Subject: Invoice #4892 ($485) — and an easy autopay restart

Hi Maria,

Thanks for being part of our maintenance plan for the past three years — we appreciate it. I'm writing about invoice #4892 for $485.00 from March 28 (the AC capacitor replacement), which is now about a month outstanding.

It looks like your autopay on file didn't run successfully on that invoice. The easiest path for both of us: restart autopay with one click and we'll pull the $485 right away — that clears the balance, keeps your Silver plan priority active, and means this is the last email I send about it.

• Restart autopay + pay: [autopay restart link]
• Pay one time without autopay: [payment portal link]
• Prefer to call? I'm at [phone]

If there's anything about the invoice that doesn't look right, just reply and I'll hold the account while we sort it out.

Thanks, Maria — we always appreciate your business.

[Name from config.office_staff]
[Company] · [Phone] · [Email]
```

---

Given input: *"Customer: R&B Properties. Invoice #5104, $2,140 for commercial RTU PM on February 12. 70 days past due. Email. Commercial account, 12 units under management. Previous follow-ups: Tier 1 on day 10, Tier 2 on day 25, Tier 3 on day 50. Status: none. State: CO."*

**Output (Tier 4 — Final Notice):**

```
Subject: Final notice — Invoice #5104 ($2,140.00) — action required by April 24

R&B Properties — Accounts Payable,

This is a final notice on invoice #5104 for $2,140.00, covering RTU preventive maintenance completed on February 12, 2026 at your managed properties. The invoice is now 70 days past due. We sent reminders on [dates from previous follow-ups].

To resolve this matter, please take one of the following actions by April 24, 2026:

1. Pay the balance in full via [payment portal link] or ACH to the account on the attached remittance
2. Contact [Name] at [phone] to set up a 3-month payment plan on the $2,140 balance
3. Respond in writing if any portion of the invoice is disputed, with the specific line item and reason

If we have not heard from you by April 24, we will refer this balance to a third-party collections partner and suspend scheduling on the 12 units under management until the balance is resolved, per our standard service-pause policy. This is the final notice we will send before that step.

We value the relationship and would strongly prefer to keep your properties on our PM schedule. Please call or reply so we can resolve this together.

Sincerely,
[Name from config.office_staff]
[Title]
[Company] · [Phone] · [Email]
[Any Colorado-required debt-collection disclosure text pulled from knowledge-base/collections/state-rules.md]
```

---

Given input: *"Customer: The Wilson Family. Invoice #5217, $1,847 for furnace repair on March 5. 41 days past due. Email. 8-year residential customer, Gold maintenance plan member. Status: weather-event-grace. Active disaster declaration: FEMA-DR-4779-FL (Hurricane Marlon, Pinellas County, declared 2026-04-08, grace window 90 days). State: FL."*

**Output (Post-Storm Grace branch — clock paused, no tier advance):**

```
Subject: We're thinking of you — invoice #5217, no rush

Hi Mr. & Mrs. Wilson,

I hope you and the family are doing okay. We know Pinellas County is still digging out from Hurricane Marlon, and a lot of our customers are juggling more than usual right now.

I'm writing only because I want to be upfront: invoice #5217 ($1,847.00 for the March 5 furnace repair) is on our books, but I have your account on a 90-day grace window per the FEMA declaration through July 7, 2026. Nothing is owed during that window. Your Gold maintenance plan stays active and you keep priority scheduling — please call us first if anything happens with the system before then.

A few options whenever it's a good time:

• Settle the balance any time before July 7 — no late fees, no interest
• Spread it across 6 or 12 months at $0 down once the grace window ends — just reply and I'll set it up
• Defer the conversation entirely until July — we'll reach out then

You don't need to do anything with this email. I just wanted you to know we're not stacking up reminders or pushing you to a deadline. If we can help with the storm cleanup side of things — surge damage, water in the air-handler, system recovery — call me directly any time.

Take care,

[First name from config.office_staff]
[Company] · [Phone] · [Email]
```

---

Given input: *"Customer: Pinnacle Property Management. Invoice #5104, $2,140 for commercial RTU PM on February 12. 70 days past due. Email. Commercial account, 12 units under management, AP contact: Dana Reyes. PO #PG-2026-0244 referenced on the original invoice. Previous follow-ups: Tier 1 day 10, Tier 2 day 25, Tier 3 day 50. Status: none. State: CO."*

**Output (Tier 4 — Commercial Final Notice variant):**

```
Subject: Final notice — Invoice #5104, PO #PG-2026-0244 ($2,140.00) — action required by April 24

Dana —

This is a final notice on invoice #5104 for $2,140.00, covering RTU preventive maintenance completed on February 12, 2026 across the 12 properties under Pinnacle's management (PO #PG-2026-0244). The invoice is 70 days past due. We sent reminders on March 1, March 16, and April 10.

To resolve the balance, please take one of the following steps by April 24, 2026:

1. Pay in full via [payment portal link] or ACH to the account on the attached remittance (routing / account / reference field "INV-5104 PO-PG-2026-0244")
2. Contact me at [phone] to arrange a 3- or 6-month payment plan on the $2,140 balance
3. Respond in writing if any line item is disputed, with the specific item and reason — we will pause the clock for two business days while we sort it out

If we have not heard from you by April 24, 2026, we will (a) refer the balance to a third-party collections partner, and (b) suspend scheduling on all 12 units under Pinnacle's management until the balance is resolved, per our standard service-pause policy. Colorado permits a commercial mechanic's lien filing on the affected properties under C.R.S. § 38-22-101 et seq.; we have not initiated a lien filing and would prefer not to.

We value the relationship with Pinnacle and would much rather keep your portfolio on our PM schedule. Please reply or call so we can resolve this together.

Sincerely,

[Name from config.office_staff], [Title]
[Company]
[Phone] · [Email] · License #[X]

[Colorado-required commercial debt-collection disclosure text from knowledge-base/collections/state-rules.md]

[ACH remittance instructions attached: routing #, account #, reference field, contact for AP]
```
