# Utility Rate Table — Directional Fallback Reference

Quick-reference fallback for any HVAC AI skill that needs an electricity ($/kWh) or natural-gas
($/therm) rate when the customer's actual bill is not provided (primarily
`customer-service/energy-savings-report.md`, plus any savings or payback math in
`repair-vs-replace-advisor` and `proposal-generator`).

**Read this first — these are DIRECTIONAL averages, not quotes.** Whenever a skill uses a number
from this file instead of the customer's own bill or `config.utility_rates[zip]`, it MUST label the
result **"directional — confirm with customer's bill."** Real rates vary widely by utility, rate
schedule, season, tier, and time-of-use window. Resolution order a skill should follow:

1. Customer's actual bill (best).
2. `config.utility_rates` keyed by ZIP (set at init for the shop's service area).
3. The regional average in this file (last resort; flag as directional).

**Currency:** Reviewed 2026-06-29. Refresh cadence: **quarterly** (next refresh 2026-09-29).
Verify against EIA (eia.gov electricity & natural-gas data) or the local utility's posted tariff
before relying on any figure for a contractual savings claim.

---

## Residential electricity — directional averages (all-in, ¢/kWh)

| Region (Census division, representative states) | Directional residential rate | Notes |
|---|---|---|
| Pacific (CA, OR, WA) | 28–42 ¢/kWh | CA highest; heavy TOU; PG&E/SCE/SDG&E peak rates can exceed 45¢ |
| Mountain (CO, AZ, NV, UT, NM) | 12–17 ¢/kWh | AZ/NV summer-peak TOU common |
| West South Central (TX, OK, LA) | 13–16 ¢/kWh | ERCOT retail-choice spread is wide |
| Midwest (IL, OH, MI, MN, WI) | 14–19 ¢/kWh | |
| Northeast (NY, MA, CT, NJ, PA) | 22–34 ¢/kWh | New England + downstate NY highest |
| South Atlantic (FL, GA, NC, VA) | 13–17 ¢/kWh | FL summer cooling-dominated |
| National rough midpoint | ~17 ¢/kWh | Use only when region is unknown; flag low-confidence |

## Residential natural gas — directional averages ($/therm, delivered)

| Region | Directional residential rate | Notes |
|---|---|---|
| Pacific / CA | $1.80–$2.60/therm | CA climate-credit + delivery charges push high |
| Mountain | $0.90–$1.40/therm | |
| Midwest | $0.90–$1.30/therm | Winter-peak; cheapest gas region |
| Northeast | $1.50–$2.30/therm | |
| South | $1.20–$1.80/therm | |
| National rough midpoint | ~$1.40/therm | Use only when region is unknown; flag low-confidence |

## Time-of-use (TOU) default shape

When a customer is on TOU but has not provided their schedule, assume a 3-period shape and label
every derived number directional:

- **On-peak** ≈ 1.6–2.2× the flat rate (late-afternoon/evening summer window, e.g., 4–9 p.m.).
- **Mid-peak** ≈ 1.0–1.2× the flat rate.
- **Off-peak** ≈ 0.5–0.7× the flat rate (overnight).

Heat-pump pre-cool / pre-heat scheduling savings should be modeled against the **off-peak**
multiplier, and the assumption stated explicitly in the report.

## Demand charges (commercial only)

Light-commercial and commercial-portfolio work frequently carries a **demand charge** ($/kW of
monthly peak) on top of energy ($/kWh). Directional range **$8–$25/kW-month**; this is often the
single largest lever in a commercial energy-savings story and must be pulled from the actual
utility tariff or account, never from this table, before it appears in a proposal.

---

**Bottom line:** this file exists so a skill never dead-ends when a bill is missing — but every
number here is a placeholder to be replaced by the customer's real rate or `config.utility_rates`,
and any output built on it must say so.
