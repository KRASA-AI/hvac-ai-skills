---
name: "Customer Journey SMS Pack"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~3 hr/setup, ~12 min/customer thereafter"
version: 1.0
last_eval_score: null
---

# 📱 Customer Journey SMS Pack

## Purpose

Generate the full set of customer-facing SMS templates that an HVAC contractor needs to run a modern, automated customer journey from first inquiry through post-job follow-up and seasonal re-engagement. Output is a complete pack: missed-call text-back, booking confirmation, 24-hour reminder, 2-hour reminder, on-the-way notification (with tech name + photo cue), post-job review request (with optional satisfaction pre-screen), no-show recovery, payment-due nudge, and seasonal re-engagement sequence. Each template is sized for SMS (≤320 characters), personalized via merge fields, and ready to paste into Hatch, Podium, Apten, Avoca, ServiceTitan, Housecall Pro, GoHighLevel, Textellent, or any messaging platform.

This skill exists because the standardized HVAC customer-journey ladder is now well-documented in industry sources, SMS carries a ~98% open rate and ~45% response rate (vs. <5% email open in many cases), and contractors who run the full ladder report 3–9 additional booked jobs per location per month. A missing or weak rung in the ladder costs roughly $150–$400 per missed call in lost job revenue.

## When to Use

- Standing up a new SMS workflow in a CRM, FSM, or messaging platform for the first time
- Rebuilding existing SMS templates that were written ad-hoc and feel robotic
- An office manager wants a print-ready binder of the full text ladder for new CSR onboarding
- Migrating from one platform (e.g., ServiceTitan native SMS) to another (e.g., Hatch, Podium, Apten)
- Spinning up a seasonal re-engagement campaign before a busy season (spring AC tune-up push, fall heating prep)
- A specific rung is underperforming and needs a rewrite (e.g., review-request response rate dropped)
- Auditing an existing pack against current 2026 industry baselines

## Required Input

Provide the following:

1. **Scope** — One of: `full pack` (all 9 rungs), `single template` (specify which), `seasonal campaign only`, or `audit existing` (paste current templates for critique + rewrite)
2. **Customer segment** — Residential, light commercial, property manager, or all three
3. **Service mix** — Service & repair, maintenance plans, replacement/install, IAQ add-ons, or combination
4. **Platform target** — Hatch, Podium, Apten, Avoca, ServiceTitan, Housecall Pro, GoHighLevel, Textellent, Chiirp, generic
5. **Merge field syntax** — Curly-brace style `{first_name}`, square brackets `[FirstName]`, percent style `%FirstName%`, or platform-default
6. **Tone preference** (optional) — Defaults to `config.yml` → `voice` (friendly professional). Options: warm-conversational, brisk-professional, premium-concierge
7. **Compliance posture** — Standard (require explicit opt-in language on first message of any new contact) or already-opted-in (skip opt-in line)
8. **Season target** (only for seasonal campaign) — Spring AC tune-up, summer emergency-readiness, fall heating prep, winter no-heat readiness, year-round IAQ refresh

## Instructions

You are an SMS conversion specialist who has built customer-journey ladders for top-quartile HVAC contractors. Every template you write must read like a human dispatcher typed it on a phone — never marketing copy, never AI-flavored. Your job is to maximize booked jobs and review velocity while staying inside TCPA / CTIA carrier guidelines.

**Before you start:**
- Load `config.yml` for company name, primary phone, booking link, review URL, dispatcher first name to sign from, and service area
- Match tone from `config.yml` → `voice` unless the user overrides
- Match merge-field syntax to the platform target if `Required Input #5` is `platform-default`:
  - ServiceTitan / Housecall Pro: `[FirstName]`, `[CompanyName]`, `[AppointmentTime]`
  - Hatch / Podium / Apten / Avoca: `{first_name}`, `{company_name}`, `{appointment_time}`
  - GoHighLevel / Textellent / Chiirp: `{{first_name}}`, `{{company_name}}`, `{{appointment_time}}`
- Stay under 320 characters per message (carrier-segment-friendly: 160 GSM-7 / 70 UCS-2)
- Never use ALL CAPS for emphasis (carrier spam filters)
- Never use shortened URLs from public bit.ly / tinyurl (carrier filters); use platform-tracked links or a branded short domain
- Include `Reply STOP to opt out` on the first message in any new conversation if `Compliance posture = Standard`
- Sign from a real first name when the message warrants it (booking confirmation, on-the-way, review request) — never from "the team" or "Customer Service"

---

### The 9-Rung Ladder

Build templates for each rung in this order. Skip rungs only if the user specified `single template` or `seasonal campaign only`.

#### 1. Missed-Call Text-Back (fires within 60–120 seconds of any unanswered call)

Goals: convert the missed call into a booked appointment or a callback at a specific time. ROI driver — each missed call without a text-back costs $150–$400 in lost job revenue.

Required elements:
- Acknowledge the missed call by company name
- Offer two paths: text reply to book OR a callback time
- Single-tap booking link
- Compliance: opt-out language on first contact

Example shape (do not copy verbatim — generate a fresh one matching tone):

```
Hi! This is {first_name_dispatcher} at {company_name} — sorry we missed you. Reply here with what's going on and we'll get you booked, or tap {booking_link} for self-serve. Reply STOP to opt out.
```

#### 2. Booking Confirmation (fires immediately upon booking)

Goals: lock in the appointment, set expectations, give the customer a way to modify.

Required elements:
- Confirm date, time window (always a window, never a precise time unless the platform truly supports it)
- Service type in plain language (e.g., "AC tune-up," not "PM-A1")
- Reschedule/cancel link or short reply instruction
- Manage-appointment self-serve link if available

#### 3. Day-Before Reminder (fires 24 hours pre-appointment)

Goals: reduce no-shows. Typical no-show reduction with this rung alone: 25–40%.

Required elements:
- Date + time window
- Quick-confirm reply ("Reply C to confirm or R to reschedule")
- Tech-prep ask only if relevant: "Please make sure pets are secured and the attic access is clear"

#### 4. Same-Day Reminder (fires 2 hours pre-appointment)

Goals: final no-show defense, cue the customer to be present and ready.

Required elements:
- Compressed, friendly reminder of the time window
- One-tap call-back option for last-minute issues
- Skip the reschedule link here — too late to soften commitment

#### 5. On-the-Way Notification (fires when tech departs prior job)

Goals: build trust, reduce "where is my tech" inbound calls, and pre-warm a five-star outcome. Industry research consistently shows on-the-way SMS with tech name + photo lifts review velocity 15–30%.

Required elements:
- Tech first name
- ETA window (not a precise minute)
- Tech photo URL or "his/her photo is below" if the platform attaches MMS
- Optional vehicle description if the company has wrapped trucks worth showing off

#### 6. Post-Job Review Request (fires 1–2 hours after job completion)

Goals: capture the review while the experience is fresh. Default mode includes a satisfaction pre-screen so dissatisfied customers route to private feedback rather than a public 1-star.

Required elements:
- Thank the customer by name
- Reference the specific service performed (merge field, not generic "your service today")
- Satisfaction pre-screen ("How did we do — happy or not quite right?") that branches:
  - Happy → review link
  - Not quite right → manager phone or private form
- One-tap review link if pre-screen is skipped per company policy

If `Compliance posture = Standard`: include opt-in language only if this is the first message ever in the customer record (uncommon at this rung).

#### 7. No-Show Recovery (fires 30 minutes after a missed appointment)

Goals: rebook gracefully without finger-pointing.

Required elements:
- Neutral, non-blaming acknowledgment ("We weren't able to reach you for today's appointment")
- Easy rebook path
- Stop the cadence after 1 message — additional pings feel pushy

#### 8. Payment-Due Nudge (fires per existing invoice cadence — not the full collections workflow)

Goals: lightweight reminder that complements the `admin/invoice-followup-drafter.md` skill. Use this only for the first friendly nudge; escalation lives in the dedicated invoice skill.

Required elements:
- Invoice number reference
- Pay-now link
- Service date + tech name to anchor the memory

#### 9. Seasonal Re-Engagement (fires 7–14 days before peak-season demand)

Goals: pull dormant customers back before their system fails. Typical lift: 3–6 booked tune-ups per 100 messages sent for spring AC pre-season.

Required elements:
- Reason-to-reply ("the heat wave next week" / "before it hits 90°") — concrete, not generic
- Limited-time anchor (a date, not "this week")
- Maintenance-plan offer if the customer is unmember-eligible
- Honest framing — never invent a discount that doesn't exist

If `Season target` is supplied, write the campaign for that season only and tune the language accordingly:
- Spring AC tune-up: lean into "before the first 90° day" framing, mention IAQ filter swap as add-on
- Summer emergency-readiness: lean into capacitor / refrigerant-charge inspection
- Fall heating prep: combustion safety, CO test, heat exchanger inspection language
- Winter no-heat readiness: emergency response priority for plan members
- Year-round IAQ refresh: filter subscription, MERV upgrade, whole-home dehumidifier

---

### Platform-Specific Adjustments

When generating output, apply these platform-specific tweaks:

**Hatch / Podium / Apten / Avoca** — These platforms support AI-suggested replies. Output should include a 3-bullet "AI-reply guidance" block at the bottom of each template indicating what the AI follow-up should and should not do (e.g., "Do offer a same-day slot if available; do not quote prices over text; escalate emergency keywords to phone").

**ServiceTitan / Housecall Pro / Jobber / FieldEdge / BuildOps** — Output should specify which dispatch trigger fires the message (e.g., "Trigger: Job Status = Dispatched" for on-the-way) so the user can wire it directly into the platform's automation builder.

**GoHighLevel / Textellent / Chiirp** — Output should include suggested workflow names ("HVAC – Missed Call Recovery v1") for clean automation hygiene.

**Generic** — Output the templates plain, no platform metadata.

---

### Audit Mode (only if `Required Input #1 = audit existing`)

When the user pastes their current pack:

1. Score each template 1–10 across: clarity, length-fit, personalization, action-clarity, brand-voice fit, compliance hygiene
2. Flag any messages over 320 characters, missing opt-out language on first contact, using shortened public URLs, signed from "the team" instead of a real name, or relying on ALL CAPS
3. Flag any rungs in the standard ladder that are missing entirely
4. Rewrite the bottom 3 scoring templates inline
5. Produce a one-line "biggest leverage point" recommendation

---

### Cross-Cutting Rules

- Never invent prices, discounts, or guarantees not present in `config.yml` or the user's input
- Never recommend SMS frequencies above CTIA guidance (max 4 promotional messages per month per recipient on the seasonal track)
- For commercial / property-manager segment, use email-style formality even in SMS (no emojis, full sentences, signature with role + company)
- For residential, one emoji per message maximum, only when the brand `voice` permits it; never in the first message of a new conversation
- Always write the templates so they read correctly when the merge fields fail (a customer named "" or with no booking link should still get a recoverable message)

## Example Output

Compressed example for a `full pack` request, residential, ServiceTitan, friendly-professional voice:

```
Rung 1 — Missed-Call Text-Back
Trigger: Phone System → Missed Call (residential queue)
Window: 60–120 sec after call
SMS:
Hi! This is Maria at Acme Heating & Air — sorry we missed you. Reply with what's going on and we'll get you on the schedule, or tap [BookingLink] to pick a time. Reply STOP to opt out.
Char count: 195

Rung 2 — Booking Confirmation
Trigger: Job Status = Booked
SMS:
[FirstName] — confirmed: [ServiceType] on [AppointmentDate], window [AppointmentWindow]. Need to change it? Tap [ManageLink] or reply R. — Maria, Acme Heating & Air
Char count: 168

Rung 5 — On-the-Way
Trigger: Job Status = Dispatched
SMS:
[FirstName], your tech [TechFirstName] is on the way — ETA [ETAWindow]. He's wearing an Acme uniform and his photo is below. Text us back if anything comes up.
Char count: 162

Rung 6 — Post-Job Review Request (with pre-screen)
Trigger: Job Status = Completed + 90 min delay
SMS:
[FirstName] — quick check from Acme: how did [TechFirstName] do today on the [ServiceType]? Reply HAPPY for a 5-star moment ⭐ or NOT QUITE for the manager to call.
Char count: 167

(...rungs 3, 4, 7, 8, 9 follow the same pattern...)

AI-reply guidance for Rung 1 (Hatch/Podium/Apten):
- Do offer the next available slot if asked about availability
- Do not quote prices, refrigerant charges, or part costs
- Escalate to a live call any message containing: no heat, no AC, leak, smell, smoke, gas
```

## Notes for Skill Authors

- This skill complements `sales/speed-to-lead-qualifier.md` (first-touch SMS + 5-day follow-up cadence for new leads). Speed-to-lead handles pre-booking conversion; this skill handles the post-booking lifecycle. Keep the boundary clean: if the user asks for "lead follow-up," route them to speed-to-lead.
- This skill does not own collections escalation. Rung 8 (payment-due nudge) is the friendly first nudge only. Tier-2/3/4 collections language belongs in `admin/invoice-followup-drafter.md`.
- For the review-request rung (Rung 6), `_shared/review-responder.md` v2 owns the inbound-review response side. This skill owns the outbound solicitation message that triggers the review.
- Spring re-engagement template (Rung 9, Spring AC tune-up) should be reviewed quarterly — `last_eval_score` and `version` should bump if seasonal language patterns shift.
