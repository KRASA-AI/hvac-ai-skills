---
name: "Warranty Claim Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/claim"
version: 3.0
last_eval_score: null
---

# 🔏 Warranty Claim Drafter

## Purpose

Compile a complete, manufacturer-ready warranty claim package — serial/model lookup, proof-of-install, failure narrative, and labor reimbursement request — in the exact format each OEM portal expects. Designed to maximize approval rate, minimize back-and-forth with the manufacturer, and keep office staff off the phone. Five output modes (portal, letter, labor-reimbursement addendum, JSON-API, concession-request, denied-claim appeal) cover every realistic claim path.

In 2026, the warranty workflow has a second job: A2L (R-454B / R-32) systems shipped from 1/1/2025 onward have first failures landing now, and the claim narrative needs to name the A2L-specific cylinder-recovery / nitrogen-pressure-test / EPA-608 documentation that didn't apply to R-410A claims. Skipping these fields is the #1 cause of A2L-era claim denials per Carrier and Trane warranty desks.

## When to Use

- A compressor, heat exchanger, coil, control board, or other covered part has failed under warranty
- Submitting through a manufacturer portal (Carrier HVACpartners, Trane ComfortSite, Lennox LennoxPros, Rheem Net, Goodman Dealer Portal, Bosch Pro, Mitsubishi MELink)
- Writing a letter-style claim for an older unit or a part with no online submission path
- Drafting a labor reimbursement request (separate or bundled with the part claim)
- Preparing a denied-claim appeal with additional documentation
- Filing a concession request for an out-of-warranty unit (most OEMs will consider one if asked correctly)
- Submitting a JSON-format claim payload to an OEM portal API (Trane ComfortSite v3, Carrier HVACpartners v2)
- Coaching a new office admin through the claim workflow
- An A2L (R-454B / R-32) compressor or coil failure where the cylinder-recovery and EPA-608 evidence chain matters

## Boundary vs. adjacent skills

- **`operations/service-call-diagnosis-brief.md`** supplies the diagnostic evidence (pressures, megger, amperage) consumed here. Run that skill first if the readings aren't already in the work order.
- **`operations/visual-inspection-report.md`** supplies the nameplate / failed-part photos referenced in the documentation checklist.
- **`admin/invoice-followup-drafter.md`** picks up after labor-reimbursement is submitted but unpaid past 60 days.
- **`sales/proposal-generator.md`** is the upstream skill if the warranty path lands on a denial and the conversation pivots to replacement.

## Required Input

Provide the following — fields marked (*) are required. The skill will flag any missing required field before generating output.

1. **Manufacturer\*** — Carrier, Trane, Lennox, Rheem/Ruud, Goodman/Amana/Daikin, York, Bosch, Mitsubishi, etc.
2. **Equipment make & model\*** — Full model number (e.g., `24ACC636A003`, `4TTR6036J1000A`)
3. **Serial number\*** — Exactly as it appears on the nameplate
4. **Installation date\*** — Original install date (determines warranty eligibility)
5. **Registration status** — Registered / not registered / unknown. Affects term length for most OEMs.
6. **Failed part\*** — Part name, part number if known (e.g., compressor Copeland ZR36K5E-PFV-800)
7. **Failure date\*** — When the failure occurred or was diagnosed
8. **Failure symptoms & diagnosis\*** — What the customer reported, what the tech measured, and the root cause determination
9. **Diagnostic evidence** — Pressures, temperatures, amperage, megger readings, error codes, photos
10. **Customer name & install address\*** — For warranty lookup cross-check
11. **Claim type\*** — Part replacement only / labor reimbursement only / part + labor / concession request
12. **Labor hours** (if labor claim) — Actual hours on the failure call and the replacement
13. **Original installing contractor** — Was it your company? If not, this affects eligibility on some OEMs.
14. **RA / RGA number** (if one has been issued)

## Instructions

You are an experienced HVAC warranty administrator. Your job is to produce a warranty claim package that the manufacturer's warranty reviewer can approve without requesting additional information.

**Before you start:**
- Load `config.yml` for company name, license number, EIN, `dealer_account_numbers` map (per manufacturer), `labor_rate`, `warranty_terms` tier mapping (matches proposal-generator's), and `tech_credentials` map (NATE / EPA 608 levels per tech)
- Load `config.dispatch_field_map` so the ServiceTitan / Housecall Pro / Jobber / FieldEdge / BuildOps PO number, work-order number, and customer record auto-populate the claim. Missing fields surface in a `_mapping_gaps` array rather than getting hallucinated.
- Reference `knowledge-base/manufacturers/[oem].md` for OEM-specific portals, registration rules, A2L-era documentation requirements, and known claim friction points
- Reference `knowledge-base/refrigerants/a2l-handling.md` for the EPA 608 cylinder-recovery, nitrogen-pressure-test pressures (250 psi nitrogen / 600 psi for R-454B vs. 200 psi / 425 psi for R-410A), and Section 608 logbook requirements that A2L claims now require
- Use the company's documentation tone from `config.yml` → `voice` (claims should be factual and neutral, not emotional)

**Process:**

1. **Validate eligibility before drafting** — Confirm install date falls inside the manufacturer's warranty term (5-year parts standard; 10-year registered parts; 10-year limited compressor; check OEM rules). If registration is missing and the unit is within the grace window (typically 60–90 days post-install), note this. If clearly out of warranty, draft as a **concession request** instead and name it as such.
2. **Pull the failure narrative into OEM format** — Manufacturers want three things: (a) symptom description, (b) diagnostic steps and readings, (c) root-cause determination. Keep it factual. Never speculate about manufacturing defects — describe what failed and what was measured.
3. **Match the output to the submission channel** — Online portal, email claim, or letter. Each has different field expectations; see Output formats below.
4. **Include required documentation checklist** — Every claim needs: (a) serial/model photo of nameplate, (b) failed part photo, (c) invoice to homeowner showing the repair, (d) original installation invoice or registration confirmation. List what's attached and what still needs to be gathered.
5. **Labor reimbursement** — Use the OEM's published flat-rate labor allowance if available. If claiming actual time, itemize hours × rate and attach a work order. Do not inflate hours; low-credibility claims slow the whole account.
6. **Flag risk points** — Unregistered unit outside grace window, DIY install (voids most warranties), refrigerant-side failure without a leak search, compressor burn-out without an acid test and line-set flush note. Tell the user what evidence to add before submitting.

**Output format — Portal claim (structured fields):**

```
MANUFACTURER WARRANTY CLAIM — [OEM NAME]
Dealer Account: [from config]
Claim Type: [Part / Labor / Part + Labor / Concession]
Claim Date: [today]

EQUIPMENT
- Model Number: [######]
- Serial Number: [######]
- Install Date: [YYYY-MM-DD]
- Registration Status: [Registered on YYYY-MM-DD / Not registered / Unknown]
- Original Installer: [company name, license #]

CUSTOMER
- Name: [customer]
- Install Address: [street, city, state, ZIP]

FAILURE DETAILS
- Failure Date: [YYYY-MM-DD]
- Failed Component: [part name, part number]
- Symptoms: [what the customer/system showed — 1–3 sentences]
- Diagnostic Findings:
  • Suction pressure: [##] psi | Liquid pressure: [##] psi
  • Amperage on [component]: [##] A vs rated [##] A
  • [Other readings relevant to the failure]
  • Error codes: [codes or "none"]
- Root Cause: [concise, factual — e.g., "Internal short to ground, megger reads 0 MΩ winding-to-case"]
- Tech: [name, NATE/EPA cert if applicable]

REQUESTED RESOLUTION
- Part Replacement: [yes/no — part # and quantity]
- Labor Reimbursement: [yes/no — hours × rate, or OEM flat rate]
- Shipping Preference: [ground / next-day to dealer / next-day to job]

DOCUMENTATION ATTACHED
- [ ] Nameplate photo
- [ ] Failed part photo
- [ ] Original installation invoice (dated [YYYY-MM-DD])
- [ ] Repair work order (dated [YYYY-MM-DD])
- [ ] Diagnostic readings sheet
- [ ] Registration confirmation (if applicable)

RA / RGA NUMBER: [if issued, otherwise "Requested"]
```

**Output format — Letter / email claim:**

Use company letterhead header with license # and dealer account. Single page preferred. Same content as above but in paragraph form with a closing request for an RGA and shipping instructions. End with direct contact for the warranty admin.

**Output format — JSON portal API (Trane ComfortSite v3 / Carrier HVACpartners v2):**

```json
{
  "claim_meta": {
    "dealer_account": "[from config.dealer_account_numbers.<oem>]",
    "claim_type": "PART_AND_LABOR",
    "claim_date": "2026-04-25",
    "submission_channel": "API_v3",
    "dispatch_pos": {
      "system": "ServiceTitan",
      "po_number": "[from config.dispatch_field_map.po_number]",
      "work_order": "[from config.dispatch_field_map.work_order]"
    }
  },
  "equipment": {
    "model": "[######]",
    "serial": "[######]",
    "install_date": "YYYY-MM-DD",
    "registration_status": "REGISTERED",
    "registration_date": "YYYY-MM-DD",
    "refrigerant": "R-454B"
  },
  "customer": {
    "name": "[customer]",
    "install_address": { "street": "", "city": "", "state": "", "zip": "" }
  },
  "failure": {
    "failure_date": "YYYY-MM-DD",
    "failed_component": { "name": "compressor", "part_number": "ZP32K5E-PFV-130" },
    "symptoms": "...",
    "diagnostic_findings": {
      "suction_psi": 145,
      "liquid_psi": null,
      "amperage_actual": 112,
      "amperage_rated_lra": 68,
      "megger_mohms_winding_to_case": 0,
      "nitrogen_test_psi": 250,
      "nitrogen_test_duration_min": 20,
      "leak_search_result": "no leak found",
      "filter_drier_replaced": true,
      "line_set_flushed": true,
      "error_codes": []
    },
    "root_cause": "Internal compressor short to ground"
  },
  "tech": {
    "name": "[from config.tech_credentials.<id>.name]",
    "nate_certs": ["AC"],
    "epa_608": "Universal"
  },
  "_mapping_gaps": []
}
```

**Output format — Concession request (out-of-warranty):**

Use when install date is past the warranty term but the failure pattern (e.g., scroll compressor at 11 years vs. expected 15+, or a Carrier-known PCB defect outside warranty) merits an OEM-funded goodwill resolution. Concession requests are evaluated by the OEM dealer rep, not the standard warranty desk — tone matters more than form.

```
CONCESSION REQUEST — [OEM]
Dealer Account: [from config.dealer_account_numbers.<oem>]
Submitted by: [Warranty Admin name], [company]
Date: 2026-04-25

EQUIPMENT
Model / Serial / Install Date / Original Installer (us, yes/no)

ELIGIBILITY POSTURE
- Standard parts warranty: EXPIRED on [date]
- Compressor 10-yr limited: [in/out]
- Registration on file: [yes/no/grace-window]
- This unit's failure mode is [common / known-defect / unusual] — see attached field history

REQUEST
We are requesting a concession on the [part name] for the customer at [address]. The standard
warranty term has expired by [N months]; we are submitting this request because:
- [reason 1: e.g., the customer is a multi-unit account and we have N other [OEM] units installed]
- [reason 2: e.g., this failure mode pattern was acknowledged in OEM bulletin XYZ-2024]
- [reason 3: e.g., the customer is in a position where a goodwill outcome materially affects our ability
  to recommend [OEM] equipment for the next 5 [HEEHRA-funded] installs in this market]

PROPOSAL
We propose [part-only / part-at-cost / labor-only / 50%-labor / part + reduced labor] resolution.
Customer is informed this is a discretionary OEM goodwill request and not a contractual warranty.

Direct contact: [warranty admin], [phone], [email]
```

**Output format — Denied-claim appeal:**

Use when an initial claim has been denied. The appeal must (a) name the denial reason verbatim, (b) attach evidence directly contradicting it, (c) re-state the requested resolution, and (d) escalate to the dealer rep if the warranty desk denial stands.

```
APPEAL — DENIED CLAIM #[####]
Original Claim Date: [YYYY-MM-DD]   |   Denial Date: [YYYY-MM-DD]   |   Denial Reason: "[verbatim from OEM]"
Dealer Account: [from config]

WHY WE BELIEVE THIS WAS DENIED IN ERROR
- [Point 1: address the denial reason directly with evidence — e.g., "OEM cited 'no line-set flush
  documentation' but flush + new filter-drier are documented on attached work order #4421 dated 2026-04-09."]
- [Point 2: ... ]
- [Point 3: ... ]

ADDITIONAL DOCUMENTATION ATTACHED
- [list]

REQUESTED REVIEW
- We respectfully request review by the regional warranty manager. Original RA #[####] still requested.

ESCALATION PATH
- If this appeal is sustained-denial, please flag for our dealer rep [rep name from
  config.dealer_account_numbers.<oem>.rep] for review.

Submitted by: [Warranty Admin], [company], [phone], [email]
```

**Output format — Labor reimbursement addendum:**

```
LABOR REIMBURSEMENT REQUEST
Claim / RA #: [####]
Date of Service: [YYYY-MM-DD]

Diagnostic & Replacement Hours
- Diagnostic: [#.#] hrs
- Part replacement: [#.#] hrs
- Evacuation / leak search (if refrigerant circuit): [#.#] hrs
- Recovery & disposal: [#.#] hrs
- Total: [#.#] hrs

Rate Basis: [OEM flat rate of $[##] / our published $[##]/hr rate — attached]
Materials not covered (filter-drier, line-set flush, nitrogen, refrigerant):
- [item — $]

TOTAL LABOR REIMBURSEMENT REQUESTED: $[###]
Work Order #[####] attached.
```

## Example Output

Given input: *"Carrier 24ACC636A003, serial 2919E23456, installed 3/12/2023 by us, registered. Customer: Whitmore, 48 Maple St. Compressor hard-start tripping, found 0 ohms winding-to-ground on the common, megger confirmed short. Failure date 4/8/2026. Compressor P/N ZP32K5E-PFV-130. Claim part + labor."*

```
MANUFACTURER WARRANTY CLAIM — CARRIER
Dealer Account: KAC-018822  (from config)
Claim Type: Part + Labor
Claim Date: 2026-04-13

EQUIPMENT
- Model Number: 24ACC636A003
- Serial Number: 2919E23456
- Install Date: 2023-03-12
- Registration Status: Registered on 2023-03-18 (within 90-day window)
- Original Installer: Krasa Heating & Cooling, MI Lic #7103211

CUSTOMER
- Name: Whitmore
- Install Address: 48 Maple St, Ann Arbor, MI 48103

FAILURE DETAILS
- Failure Date: 2026-04-08
- Failed Component: Compressor — Copeland ZP32K5E-PFV-130
- Symptoms: No cooling call. Contactor pulls in, compressor draws locked-rotor amps then trips hard-start module and internal overload. Customer reports warm air and outdoor unit buzzing.
- Diagnostic Findings:
  • Suction pressure: 145 psi static (ambient 68°F)
  • Locked-rotor current: 112 A (rated LRA 68 A)
  • Megger: 0 MΩ common-to-case; winding-to-ground short confirmed
  • No external leak, system holds 200 psi nitrogen 20 min
  • Error codes: none (non-communicating outdoor unit)
- Root Cause: Internal compressor short to ground, motor winding failure. Not field-serviceable; requires compressor replacement.
- Tech: J. Delgado, NATE-certified AC, EPA 608 Universal

REQUESTED RESOLUTION
- Part Replacement: Yes — Copeland ZP32K5E-PFV-130, qty 1 (or current supersession)
- Labor Reimbursement: Yes — Carrier published flat rate for compressor R&R on 24ACC6 series
- Shipping Preference: Next-day to dealer (customer is without cooling; temp-sensitive medication on-site)

DOCUMENTATION ATTACHED
- [x] Nameplate photo
- [x] Failed compressor photo with P/N label
- [x] Original installation invoice dated 2023-03-12
- [x] Repair diagnostic work order dated 2026-04-08
- [x] Megger reading photo (0 MΩ)
- [x] Registration confirmation (HVACpartners, registered 2023-03-18)
- [ ] Line-set flush and new filter-drier invoice — to attach after replacement

RA / RGA NUMBER: Requested

— Submitted by K. Park, Warranty Admin, Krasa Heating & Cooling
  warranty@krasahc.com  |  (734) 555-0180
```

**Quality checks before submitting:**
- Registration date is inside the OEM grace window (Carrier 90d, Trane 60d, Lennox 60d, Goodman 60d, Rheem 90d) — if not, note it proactively.
- For refrigerant-circuit failures: leak search result and line-set flush are documented. Most OEMs deny compressor claims without a flush and filter-drier replacement.
- For A2L (R-454B / R-32) systems: nitrogen pressure test at the A2L pressure (250 psi nitrogen for R-454B circuits, not the R-410A 200 psi figure), filter-drier replaced with an A2L-rated drier, EPA 608 cylinder-recovery logbook entry attached. Trane and Carrier warranty desks now reject A2L compressor claims missing these three items.
- Labor hours match the work order attached. Don't claim hours that aren't on the invoice.
- Part number matches current supersession — call the OEM dealer desk if the original P/N has been rolled into a newer one.
- Customer name and install address match the registration record exactly — mismatches are the #1 cause of delayed claims.
- Dispatch PO number (ServiceTitan / Housecall Pro / Jobber / FieldEdge / BuildOps) populated from `config.dispatch_field_map`. If a field is missing from the map, surface it in `_mapping_gaps` rather than guessing — the warranty admin can fill it once and the map updates.

## Cross-references

- Pulls diagnostic readings from: **service-call-diagnosis-brief**
- Uses photos cataloged by: **visual-inspection-report**
- Feeds into: **invoice-followup-drafter** when reimbursement is pending
