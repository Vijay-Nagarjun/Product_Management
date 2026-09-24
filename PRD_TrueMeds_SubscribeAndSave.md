**PRD: Subscribe & Save — Auto-Refill Subscription**

_Confirm-before-refill subscriptions for chronic-medicine reorders — launched only where stock-confirmed fulfillment (Fix 2) is live._

| Product | TrueMeds — E-Pharmacy Platform |
| --- | --- |
| Feature | Fix 1 (Teardown Problem 1) |
| Author | Vijay Nagarjun Savvasere |
| Version | 1.0 |
| Status | Draft for Review |

# 1\. Problem & Evidence

Chronic-condition patients (diabetes, hypertension, thyroid) must manually re-upload a prescription and reorder every month — TrueMeds has no auto-refill, unlike PharmEasy and Netmeds. This drives forgotten reorders and lost retention. But shipping a subscription before delivery is reliable turns a one-time missed-dose risk into a recurring one — which is why this feature is gated on Fix 2 being live in a given city, not launched nationally on day one.

*   **Recurring friction:** manual reordering hassle recurs across reviews, with reviewers comparing TrueMeds unfavorably to PharmEasy and Tata 1mg on this exact gap.
*   **Segment value:** chronic patients are TrueMeds' highest-value, highest-LTV segment (₹1,000–3,000/month spend); auto-refill users show materially higher retention on competitor data.
*   **RICE score: 9** — lower than Fix 2's 12, but the real sequencing driver is dependency: a subscription automates whatever delivery experience sits under it, and today that experience is unreliable.

# 2\. Goals & Non-Goals

*   **Goal —** Keep chronic patients continuously supplied (raise Proportion of Days Covered).
*   **Goal —** Improve retention, measured against a randomized holdout — not a self-selected non-subscriber comparison.
*   **Goal —** Cut the monthly reorder to one or two taps when the prescription hasn't changed.
*   **Goal —** Reach feature parity with PharmEasy/Netmeds on auto-refill; test, not assume, that the unit economics work.
*   **Non-goal —** No fully-silent auto-refill in v1 (confirm-before-refill only, pending compliance review); doesn't fix delivery reliability itself (that's Fix 2); no express delivery, multi-address split, or dosage increase without a fresh prescription; excludes restricted-schedule drugs, cold-chain items, and COD-only customers in v1.

# 3\. Target Users & Key Stories

*   **Primary — Rajesh Gupta** (32, Bangalore): orders his father's diabetes/BP medicines; wants to set up once as a safety net for months he forgets, with control to pause, skip, or change.
*   **Secondary — Anjali Verma** (38, Lucknow): wants an honest ETA and a named order owner — no surprises on high-risk medicines.
*   As a subscriber, I want a check-in well before my refill is due, so I have time to confirm or make changes.
*   As a subscriber whose prescription is unchanged, I want to confirm with one tap and see when it'll arrive.
*   As a subscriber, I want to be warned early if delivery will land after my supply runs out, so I can act.
*   As a subscriber, I want to pause, skip, or cancel at any time — as easily as I subscribed.

# 4\. Key Requirements (Must-have)

*   Subscription requires a pharmacist-verified prescription linked to it; excludes restricted-schedule and cold-chain drugs.
*   Available only in pin codes where Fix 2's stock-confirmed routing is live — other pin codes see a waitlist, not a false promise.
*   Cycle is scheduled backwards from the supply-end date; check-in sent 5 days before the order deadline via push + WhatsApp/SMS (generic copy, no medicine names in previews).
*   "Yes, unchanged" → one-tap reorder through Fix 2's route-before-accept flow. "No, changed" → blocked until a new prescription is pharmacist-verified.
*   No response → reminders at 3 and 2 days, then no auto-order at cutoff; customer is told plainly and given a one-tap "Order now."
*   If the expected delivery date falls after the supply-end date, the customer gets a refill-at-risk alert — never silent.
*   V1 payment is customer-initiated, one-tap at "Yes" (no auto-debit mandate yet — evaluated separately for a later phase).
*   Auto-placed order failures trigger an automatic same-day refund; support may not default to "cancel and place a fresh order."
*   Every subscription order has a named support owner; "report a problem" automatically holds the next cycle until resolved.

# 5\. Success Metrics

| Metric | Target |
| --- | --- |
| Proportion of Days Covered (PDC) | Positive uplift vs randomized Control |
| 3- and 6-month chronic retention | Positive uplift vs Control (not vs self-selected non-subscribers) |
| Refills delivered before supply-end | ≥ 90% |
| Guardrail: invalid-prescription dispensing | Zero confirmed cases |
| Guardrail: cycles skipped for no response | ≤ 15% |

# 6\. Top Risks

*   **Shipping before Fix 2 is live** turns a one-time missed-dose risk into a recurring one — mitigated by a hard city-level launch gate tied to Fix 2's rollout.
*   **Silence becomes a missed dose** if a forgetful caregiver also misses the check-in — mitigated by an escalating reminder ladder and refill-at-risk alerts.
*   **Self-selection inflates results** — loyal patients are the ones who subscribe, so this is measured against a randomized holdout, never a raw subscriber-vs-non-subscriber comparison.
*   **The discount may not pay for itself** — tested as a separate experiment arm against explicit break-even math (Section 14 of the full PRD), not assumed.

# 7\. Open Questions

*   Does existing engineering infrastructure actually support the reuse this effort estimate assumes? (Phase 0 scoping)
*   What payment/mandate approach clears Payments & Legal for an eventual auto-debit phase?
*   Should COD-only customers be supported in a later phase, and how?
*   What subscriber discount, if any, clears the break-even bar in Section 14 of the full PRD?