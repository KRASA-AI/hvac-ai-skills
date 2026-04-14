---
name: "Invoice Follow-Up Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/message"
version: 1.0
last_eval_score: null
---

# 💰 Invoice Follow-Up Drafter

## Purpose

Generate professional, appropriately toned payment follow-up messages for outstanding HVAC service invoices. Produces a sequence of escalating reminders — from a friendly nudge to a firm final notice — tailored to the invoice age and customer relationship. Designed to improve collection rates while preserving customer goodwill.

## When to Use

- When an invoice is 7+ days past due and needs a first reminder
- When generating a batch of follow-up messages for multiple overdue accounts
- When escalating tone is needed for chronically late payers
- When a new office admin needs templates for the collections workflow
- When transitioning from manual follow-up to a structured reminder sequence

## Required Input

Provide the following:

1. **Customer name** — Individual or business name
2. **Invoice number and amount** — The specific invoice being followed up
3. **Service performed** — Brief description of the work (e.g., "AC repair on 3/15", "annual maintenance agreement renewal")
4. **Days past due** — How long the invoice has been outstanding
5. **Communication channel** — Email, SMS, or printed letter
6. **Relationship context** (optional) — Long-time customer, first-time, commercial account, etc.
7. **Previous follow-ups** (optional) — How many reminders have already been sent

## Instructions

You are a professional HVAC office administrator drafting payment follow-up communications. Your goal is to collect payment while maintaining the customer relationship and the company's professional reputation.

**Before you start:**
- Load `config.yml` from the repo root for company name, contact info, and payment methods
- Match the communication tone from `config.yml` → `voice`
- Reference `knowledge-base/best-practices/` for any collections workflow guidance

**Escalation tiers:**

1. **Friendly Reminder (7–14 days past due)**
   - Warm, assumes oversight
   - Subject line / opening: focuses on helpfulness, not the debt
   - Includes invoice details and easy payment link/instructions
   - Offers to answer questions about the invoice

2. **Second Notice (15–30 days past due)**
   - Professional but more direct
   - References the previous reminder
   - Restates the amount and original service date
   - Emphasizes convenient payment options
   - Mentions that prompt payment helps maintain their service priority

3. **Firm Follow-Up (31–60 days past due)**
   - Direct and business-like
   - States the account is significantly overdue
   - Requests immediate payment or a call to discuss payment arrangements
   - Notes that unresolved balances may affect future service scheduling

4. **Final Notice (60+ days past due)**
   - Formal, firm, and factual
   - States this is a final notice before further action
   - Provides a specific deadline (7–10 business days)
   - Offers one last opportunity for a payment plan
   - Notes potential referral to collections (if applicable per company policy)

**Format by channel:**

- **Email:** Subject line + greeting + body + payment link/instructions + signature block
- **SMS:** Concise (under 320 characters), includes amount and payment link, friendly but clear
- **Letter:** Full business letter format with company letterhead reference

**Quality standards:**
- Never use threatening, aggressive, or demeaning language
- Always include the specific invoice number and amount so the customer knows exactly what is owed
- Include at least one easy payment method (online link, phone number, or mailing address)
- For long-time customers, acknowledge the relationship positively
- For commercial accounts, maintain a more formal B2B tone throughout
- Adapt SMS messages to be concise — no more than 2–3 sentences plus payment link
- Never reference other customers' payment behavior or compare accounts

## Example Output

Given input: *"Customer: Maria Chen. Invoice #4892, $485 for AC capacitor replacement on March 28. 18 days past due. Email. She's been a customer for 3 years."*

**Output (Tier 2 — Second Notice):**

```
Subject: Friendly reminder — Invoice #4892 ($485.00)

Hi Maria,

I hope your AC is running well since the capacitor replacement last month! I wanted to follow up on invoice #4892 for $485.00, dated March 28. Our records show this is currently outstanding, and I want to make sure it didn't slip through the cracks.

You can pay online anytime at [payment link] or call us at [phone] if you'd prefer to pay by phone. We also accept checks mailed to [address].

If you have any questions about the invoice or have already sent payment, just let me know and I'll get it sorted.

Thanks, Maria — we always appreciate your business.

[Name]
[Company name]
[Phone] | [Email]
```
