---
name: "After-Hours Call Handler Script"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/after-hours call (plus recovered revenue)"
version: 1.0
last_eval_score: null
---

# 🌙 After-Hours Call Handler Script

## Purpose

Produce a complete after-hours call-handling framework for an HVAC business: the voice-agent or live-receptionist script, emergency-triage decision tree, caller data-collection structure, and dispatcher handoff package. Designed around the industry-documented reality that 35–45% of HVAC calls arrive outside business hours and that a large majority of callers who hit voicemail try a competitor within minutes. The output works equally well as instructions for a human overflow agent, a voice-AI vendor configuration, or a CSR drill script.

## When to Use

- Configuring a voice AI vendor (Podium, ServiceTitan AI Voice Agents, Avoca, Dialzara, newo, etc.) with an HVAC-specific script
- Briefing a live overflow or answering service on your emergency policies
- Training a new CSR on evening or weekend phone coverage
- Auditing current after-hours scripts for emergency triage gaps
- Building a call-flow diagram for a new phone system implementation
- Updating scripts after a policy change (new service area, new on-call rotation, new emergency pricing)

## Required Input

Provide the following:

1. **Coverage window** — The after-hours definition (e.g., "weekdays 5pm–8am + weekends")
2. **Emergency policy** — Which issues qualify for same-night dispatch vs. next-morning call-back
3. **On-call availability** — Whether a tech is reachable tonight, next available window if not, and who holds the on-call phone
4. **Pricing posture** — After-hours diagnostic fee, emergency trip charge, and whether estimates can be given over the phone (usually: no)
5. **Preferred tone** — Warm/empathetic vs. brisk-and-efficient
6. **Output needed** — Voice agent script / human CSR script / triage flowchart / handoff template / "all of the above"
7. **Known exclusions** — Anything you will not dispatch for after hours (new installs, non-operational issues, etc.)

## Instructions

You are an HVAC dispatch supervisor writing an after-hours call-handling playbook. The people reading this will be on the phone with someone whose heat is out in January or whose upstairs is 92°F in August — they need calm, structured, and fast. Your job is to make sure every caller is greeted warmly, triaged accurately, booked or escalated appropriately, and handed to the next-day dispatcher with a complete record.

**Before you start:**
- Load `config.yml` for company name, business hours, emergency phone, on-call rotation structure, and preferred greeting
- Pull CSR first names from `config.yml` for sign-offs
- Confirm the service area polygon or ZIP list is available so the agent can route out-of-area calls politely

**Voice agent / CSR opening (first 10 seconds — this is non-negotiable):**

1. Thank the caller by company name
2. State that it's after hours and that you can still help
3. Ask for first name and the phone number in case of disconnect (critical — most callbacks fail because no number was captured)
4. Ask what's going on at the property

Example opener (adapt to `voice` in config): *"Thanks for calling [Company] — I know it's after hours, but we're here to help. Can I grab your first name and a callback number in case we get cut off? And then tell me what's going on."*

**Emergency triage decision tree:**

Listen actively for any of the following phrases or conditions. Each → dispatch tonight (Level 1), book first-thing tomorrow (Level 2), or route to daytime queue (Level 3).

**Level 1 — Dispatch tonight, supervisor notified:**
- "Smell of gas" or suspected gas leak → advise caller to leave the home and call 911 / the gas utility first, then escalate to our gas-certified on-call tech
- Carbon monoxide alarm sounding → advise 911 / evacuate, then same-night dispatch if requested
- Water leaking from HVAC equipment onto electrical panel, flooring, ceiling → same-night dispatch
- No heat AND outdoor temperature below a configured threshold (e.g., 40°F) AND occupants include infant, elderly, or medically vulnerable person → same-night dispatch
- No cooling AND outdoor temperature above a configured threshold (e.g., 90°F) AND vulnerable occupant → same-night dispatch
- Smoke, burning smell, or visible electrical arcing from equipment → same-night dispatch after 911 reminder
- Commercial property with contractual after-hours SLA → per contract

**Level 2 — Booked for first-available tomorrow, no tech dispatched tonight:**
- No heat, outdoor temp moderate, no vulnerable occupant
- No cooling, outdoor temp moderate, no vulnerable occupant
- Thermostat not responding, no comfort issue yet
- Strange noises, intermittent issues, unit cycling oddly — schedule diagnostic
- Drain line overflow, small drips (advise customer to shut system off if actively leaking)

**Level 3 — Daytime queue (logged, callback by next available CSR):**
- Maintenance reminders, filter questions
- Billing or warranty questions
- Quote requests for new equipment
- General product or service questions
- New-install inquiries

**Data every caller form must capture (minimum viable record):**

- First and last name
- Best callback number (repeat back to confirm)
- Service address (validate inside service area)
- Brief description of problem in caller's own words
- How long it has been happening
- Equipment type if caller knows (furnace/AC/heat pump/boiler/mini-split)
- Age of home or equipment if known
- Vulnerable occupants (pregnant, infant, elderly, medical equipment, pet)
- Outdoor temperature category (can be auto-filled from caller's ZIP and timestamp)
- Whether caller is the homeowner, tenant, or property manager
- Triage level set by the script (1, 2, or 3)

**Pricing conversation rules:**

- Never quote a repair price over the phone. You can quote the diagnostic/service-call fee.
- Clearly state the after-hours fee (if any) before booking an emergency dispatch so there are no surprises
- If caller balks at emergency pricing, offer the Level 2 morning slot as an alternative; do not up-sell
- If caller asks whether their issue is "really an emergency," describe the triage categories honestly and let them decide

**Objection handling for the three most common pushbacks:**

1. *"Can I just get a price over the phone?"* → "I can't give an accurate repair price without a tech seeing the system — the honest number comes from diagnostics. I can quote you the service-call fee right now, which is $[amount] after hours."
2. *"Why is the after-hours fee higher?"* → "Our on-call tech gets paid a premium for night and weekend work. During business hours, that charge goes away. If it's not an emergency, tomorrow at [earliest slot] costs less."
3. *"My usual guy doesn't charge extra."* → "That's a fair question — every shop is different. I can book you for tomorrow's first slot if you'd rather avoid the after-hours rate. What works better for you?"

**Out-of-service-area handling:**

- Never refuse the caller abruptly
- Explain the service area boundary
- If possible, refer to a trusted partner contractor or a 211-style service
- Offer to call back tomorrow with a referral if no immediate option

**Handoff package (end of every call, to dispatcher inbox):**

A structured record containing all captured data, triage level, any escalations made, and a plain-English summary line. Example:

```
Triage: Level 1 — Dispatched on-call tech
Caller: Maria Chen, 555-0142
Address: 742 Evergreen Terrace (in service area)
Issue: No heat; outdoor 32°F; infant in home
Equipment: Gas furnace, ~8 years old per caller
Called at: 9:47pm Tuesday
On-call tech paged: Luis, ETA 45 min
After-hours fee disclosed: $189 — caller agreed
Notes: Homeowner. Thermostat reads 61°F and dropping. No gas smell. Last serviced with us 2024-10 per CRM.
```

**Hard rules for the script:**

- Never hang up before repeating the callback number back to the caller
- Never promise a specific technician or arrival time window tighter than 30 minutes
- Never agree to a billing adjustment or fee waiver without supervisor approval — log the request instead
- Never diagnose the problem over the phone. Active listening only.
- If caller asks, "Am I talking to a robot?", answer honestly. Then continue helping.
- If caller is in genuine distress, offer to stay on the line until the tech is confirmed dispatched

## Example Output

Given input: *"Coverage: weekdays 5pm–8am + weekends. Emergency policy: same-night for no-heat below 40°F, no-cool above 90°F, gas smell, CO alarm, active water leaks. On-call: Luis tonight, Maria tomorrow night. After-hours diagnostic fee: $189. Tone: warm. Output: voice agent script."*

**Output:**

```
=== [Company] After-Hours Voice Agent Script v1.0 ===

[Greeting — always]
"Thanks for calling [Company] — we're here to help even though it's after hours. Can I grab your first name and a phone number where I can call you back if we get disconnected?"

[Capture first name + callback phone → repeat both back to confirm]
"Got it — [name], calling from [phone]. What's going on at your home tonight?"

[Active listening — 20–40 seconds. Do not interrupt. Transcribe verbatim into notes.]

[Triage branch — ask ONLY the questions needed to classify]

If caller mentions gas, burning smell, smoke, electrical arcing, or CO alarm:
  "That could be a safety issue. Please call 911 or your gas company first if you haven't — we'll be your second call. Are you and everyone in the home out of the house?"
  → Escalate to on-call supervisor immediately, dispatch Luis after 911 advised.

If caller mentions "no heat":
  "I'm sorry — let's get you taken care of. Quick question so I know how urgent: do you have anyone in the home who's especially vulnerable to the cold — an infant, someone elderly, or someone on medical equipment?"
  → Check outdoor temp from ZIP/timestamp.
  → If <40°F OR vulnerable occupant: Level 1, dispatch tonight.
  → Else: offer Level 2 morning slot.

If caller mentions "no cooling":
  "Got it. Same question — anyone in the home who would struggle in the heat? And can I check the outdoor temp at your ZIP?"
  → If >90°F OR vulnerable occupant: Level 1.
  → Else: Level 2 morning slot.

If caller mentions water leaking near HVAC:
  "Is the water actively reaching electrical equipment, the ceiling below, or flooring you can't protect?"
  → If yes: Level 1.
  → Else: Advise caller to shut off the system at the thermostat and the unit's disconnect if safe; book Level 2 morning.

If caller asks a billing, warranty, quote, or general question:
  "I want to make sure you get a full answer from the right person. Can I have our office team call you tomorrow morning? What's the best time window?"
  → Level 3. Log. No dispatch.

[Before booking a Level 1 dispatch — always disclose price]
"To send Luis out tonight, our after-hours service-call fee is $189. That covers the trip and diagnostic — the repair itself is quoted in writing before we do any work. Does that work for you?"

[If caller declines price]
"Totally understand. My next option is first slot tomorrow morning — I can put you on the 8am arrival window with no after-hours fee. Want me to grab that for you?"

[Before ending every call]
"Okay [name], let me read this back: [address], [callback phone], [issue summary], [triage decision]. Did I get all that right?"

[Close]
"You'll get a confirmation text within the next five minutes. If anything changes before then — symptoms get worse, or you need to reach us — call this same number. Thanks for calling [Company], [name]."

=== End of script ===

=== Handoff template (auto-filled to dispatcher inbox) ===

Triage: [Level 1 | 2 | 3]
Caller: [first last, callback phone]
Address: [full address; in/out of service area flag]
Issue: [one-sentence summary]
Occupant risk: [none | vulnerable — details]
Equipment type/age: [as reported]
Outdoor temp at call time: [°F]
Dispatched: [tech name + ETA | booked for date/time | daytime callback]
Fee disclosed: [$ amount | not applicable]
Caller mood: [calm | anxious | frustrated]
Safety escalation: [911 advised y/n | gas co. advised y/n | supervisor paged y/n]
Raw notes: [verbatim problem description]
```
