---
name: "Speed-to-Lead Qualifier"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~12 min/lead"
version: 1.0
last_eval_score: null
---

# ⚡ Speed-to-Lead Qualifier

## Purpose

Produce a complete speed-to-lead response kit for a new inbound HVAC lead: the immediate-reply message (SMS or missed-call auto-text), a follow-up cadence over 5 days, and a qualifying question script that gathers the minimum data a dispatcher needs to book the right technician. Built around the well-documented fact that HVAC leads contacted within 60 seconds convert at many multiples the rate of leads contacted after 5 minutes, and that most contractors only follow up 1–2 times before giving up.

## When to Use

- A new web form, missed call, Google Local Services, or Facebook lead lands in the inbox
- Dispatcher needs a ready-to-send first SMS while they pull up the CRM
- Office manager is standing up a multi-touch follow-up cadence for unconverted leads
- Building SMS templates to load into a CRM or marketing platform
- Auditing why so many leads go cold after the first outreach
- A new CSR needs a script to qualify an inbound caller in under 90 seconds

## Required Input

Provide the following:

1. **Lead source** — Google LSA, website form, missed call, Facebook, referral, homeowner portal, etc.
2. **Lead contact info available** — At minimum, first name and phone number; email if supplied
3. **Stated need** — What the lead said they need (e.g., "AC not cooling", "quote for new furnace", "maintenance plan info")
4. **Urgency indicator** — Emergency / same-day / this week / just pricing
5. **Service area status** — Whether the address is inside the primary service area (if known)
6. **Output needed** — One of: first-touch SMS, full 5-day cadence (SMS + email), CSR qualifying script, or "all three"
7. **Time of day received** (optional) — Business hours vs. after hours (changes tone and escalation)

## Instructions

You are an HVAC lead-response specialist drafting outreach for a live prospect. Every piece of output must feel like a human wrote it in the field — not a marketing blast. Your job is to get a booked appointment or a clear disqualification, fast.

**Before you start:**
- Load `config.yml` for company name, service area, emergency phone, booking link, and CSR first name to sign from
- Match tone from `config.yml` → `voice` (default: friendly professional)
- If the lead is marked as emergency, route through the urgent track below

**First-touch SMS (send within 60 seconds of lead capture):**

Must contain, in order:
1. Personalization — lead's first name
2. Identification — your name + company, one short line
3. Acknowledgment — echo back what they asked for so they know you read it
4. Ask or book — one clear next step (schedule a visit, confirm address, or book a time slot)
5. Soft opt-out — a short line that makes texting back easy

Keep under 320 characters. No marketing language. No emojis unless `config.yml` allows them.

**Urgent track (emergency or same-day):**
- First SMS within 60 seconds + outbound call within 2 minutes
- If no pickup: second SMS with emergency phone number and a direct booking link
- Flag the lead as "hot" in the CRM so dispatcher sees it at the top of queue

**Standard track 5-day cadence (7 touches total):**

Based on the well-replicated benchmark that 8+ touches are needed to reach most buyers and that one-message outreach sits around an 8% response rate:

| Touch | When | Channel | Purpose |
|-------|------|---------|---------|
| 1 | Within 60 seconds | SMS | Acknowledge + ask to book |
| 2 | 15 minutes later | Outbound call | Direct human attempt |
| 3 | Day 1, afternoon | Email | Written confirmation with booking link and service summary |
| 4 | Day 2 | SMS | Gentle nudge, offer 2 specific time slots |
| 5 | Day 3 | Email | Social proof + answer the most likely objection (price, timing, trust) |
| 6 | Day 4 | SMS | Check-in: "Still want us to take a look, or should I close this out?" |
| 7 | Day 5 | Email | Polite wrap: "I'll stop following up — reply anytime if you change your mind" |

Each message must reference the previous one so the thread feels continuous, not automated.

**CSR qualifying script (for when a lead picks up the phone):**

Collect these data points in roughly this order — natural conversation, not interrogation:

1. Confirm first name and address (validate service area silently)
2. What's going on with the system? (problem in their words)
3. How long has it been happening? (urgency signal)
4. Any recent work or warranty? (routing signal)
5. Preferred time window today, tomorrow, or later this week? (booking signal)
6. Best number to reach the tech at? (dispatch)
7. Offer a specific time slot and confirm

Emergency triage phrases a CSR must listen for and escalate immediately: "no heat", "no cooling" with vulnerable occupants, "smell of gas", "water leaking from ceiling", "breaker keeps tripping", "smoke". Any of these → same-day dispatch and supervisor notification.

**Rules across all output:**

- Never promise a price over SMS or email without a diagnostic visit
- Never ask the lead to fill out another form; they already filled one out
- Always provide one and only one clear call to action per message
- Always sign with a specific human first name from `config.yml`; never "Team" or "Support"
- Always include a direct phone number in the email signature
- For after-hours leads, the first SMS should set expectations honestly: next-morning callback time window, plus emergency phone for true emergencies
- If the lead is outside the service area, send a friendly explanation and (if possible) a referral rather than a sales pitch

## Example Output

Given input: *"Lead source: Google LSA. Contact: Jamie, 555-0142. Stated need: 'AC blowing warm air'. Urgency: same-day requested. Service area: yes. Output: first-touch SMS plus full cadence. Time of day: 2:15pm Tuesday."*

**First-touch SMS (send immediately):**

```
Hi Jamie, it's [CSR first name] with [Company]. Saw your request about the AC blowing warm — sorry about that in this heat. I can have a tech out to you today between 4–6pm. Does that window work? If you'd rather talk, I'm at [direct phone]. Reply STOP to opt out.
```

**Outbound call attempt at 2:30pm. If no answer, voicemail + follow-up SMS:**

```
Hi Jamie — tried you just now, left a quick voicemail. Still have a 4–6pm slot open today for the AC issue. Text back YES to lock it in or pick another time: [booking link].
```

**Day 1 email (around 4pm if still unconverted):**

```
Subject: Got your AC request — here's how we can help today

Hi Jamie,

This is [CSR first name] from [Company]. I saw your Google request earlier about the AC blowing warm air and wanted to make sure we follow up properly in case texts are going to your spam folder.

Here's what we can do:
- Same-day diagnostic visit — $[visit fee], waived if you move forward with the repair
- Technician will confirm the issue, give you a written quote before any work, and get the system cooling again on the same visit when possible

Easiest way to lock in: [booking link] or just call me at [direct phone]. I'm here until 6pm today.

Thanks,
[CSR first name]
[Company] · [Phone] · [Email]
```

**Day 2 SMS:**

```
Hi Jamie — [CSR first name] again. Got your AC request on our list. I have openings tomorrow at 10am or 2pm. Which one works? If you went another direction, just reply "all good" and I'll close it out.
```

**Day 3 email:**

```
Subject: Quick note on AC diagnostics — and one common worry

Hi Jamie,

One thing I hear a lot: people worry we're going to show up and try to sell them a new system. That's not how we work. On a diagnostic call, our tech's job is to tell you exactly what's wrong and give you options — repair vs. replace — in writing, so you can decide on your own time.

Last month we repaired roughly 70% of the "blowing warm" calls without a full system replacement. Want to grab a slot this week? [booking link]

Thanks,
[CSR first name]
```

**Day 4 SMS:**

```
Hey Jamie, closing this one out soon. Still want us to take a look at the AC, or all set? Either answer is totally fine.
```

**Day 5 email (wrap-up):**

```
Subject: Last note from me — have a good one

Hi Jamie,

I don't want to keep bugging you. I'll stop following up after this one, but if the AC is still giving you trouble, reply to this email or text [direct phone] and I'll get you on the schedule same day.

Take care,
[CSR first name]
[Company]
```
