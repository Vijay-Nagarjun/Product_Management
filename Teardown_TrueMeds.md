**TrueMeds — Product Teardown**

_E-pharmacy platform — two problems selected for deep product-level analysis._

| Company | TrueMeds (Intellihealth Solutions Pvt. Ltd.) |
| --- | --- |
| Author | Vijay Nagarjun Savvasere |
| Version | 1.0 |
| Sequencing | Inventory Visibility ships first (RICE 12) — Auto-Refill second (RICE 9), on dependency, not score margin. |

# 1\. Problems & Evidence

*   **Problem 1 — No auto-refill:** Chronic-condition patients (diabetes, hypertension, thyroid) must manually reorder every month — no subscription, unlike PharmEasy/Netmeds. Chronic patients are TrueMeds' highest-value segment (₹1,000–3,000/month spend); India's online pharmacy market reached ~$3.18B in 2024 (IMARC).
*   **Problem 2 — Promised vs. actual delivery gap:** TrueMeds promises 3–4 days at checkout; actual delivery often takes 7–20 days or never arrives. Late/undelivered orders tied with unresolved support as the two largest themes in 702 analysed Play Store reviews (~120 of ~300 substantive reviews each). ~25–30 reviews describe delivery attempts falsely logged as failed, one citing CCTV evidence. Corroborated by Trustpilot 1.6/5, JustDial 2.7/5.
*   **Competitive gap:** Apollo 24/7's ~19–29 min delivery comes from checking live stock before accepting an order — a routing discipline, not only real estate (it also has 7,000+ outlets TrueMeds lacks).

# 2\. Root Cause

TrueMeds already runs 7–8 warehouses plus a partner-pharmacy network across 19,000+ pin codes — so this isn't a shortage-of-nodes problem. Orders are most likely accepted and assigned to a node without confirming stock is there first; when it isn't, the order silently re-routes, producing the exact review pattern of multi-day gaps with no visible status change. Node density (TrueMeds vs. Tata 1mg's 28 fulfilment centres) is secondary context — the core mechanism is visibility and routing logic, which is also why the fix below adds no new warehouses.

# 3\. Proposed Fixes (Must-have)

*   **Fix 2 — Real-Time Inventory Visibility & Route-Before-Accept:** Central live stock ledger across existing warehouses and partner pharmacies; checkout routes to the nearest confirmed-stock node, reserves it, and shows a node-specific ETA before payment — replacing the flat 3–4-day promise. Auto order-splitting when no single node holds the full cart. Reservations auto-release on cancel/fail/timeout.
*   **Fix 1 — Subscribe & Save (Auto-Refill):** One-tap subscription setup; 4–5 days before each renewal, a check-in asks if the prescription is unchanged. "Yes" → auto-reorder via Fix 2's routing; "No" → new prescription required. Pause/skip/cancel anytime. Gated to launch only in cities where Fix 2 is live.

# 4\. RICE Scores

| Problem / Fix | RICE Score | Why |
| --- | --- | --- |
| Inventory Visibility (Fix 2) | 12 | Reach 4, Impact 4, Confidence 3, Effort 4 — largest named theme, directly reused by Apollo/Tata 1mg as their winning mechanism. |
| Auto-Refill (Fix 1) | 9 | Reach 3, Impact 4, Confidence 3, Effort 4 — real retention play, but a smaller and more speculative gain than fixing delivery. |

# 5\. Top Risks

*   **Data quality is the single point of failure** for Fix 2 — one under-reporting node recreates the exact gap this closes. Mitigated by staleness thresholds and phased city rollout.
*   **Honest ETAs likely cost conversion short-term** — a real tension with TrueMeds' acquisition-led priorities, tracked as a named guardrail rather than hidden.
*   **Shipping Auto-Refill before Fix 2 is live** turns a one-time missed-dose risk into a recurring one — hence the hard city-level launch gate.
*   **Apollo comparison is necessary, not sufficient** — stock-aware routing closes most of the gap, but Apollo's speed also rests on 7,000+ outlets TrueMeds doesn't have.

# 6\. Prioritization Conclusion

Fix 2 scores 12 against Fix 1's 9 — a lead too small for an ordinal 1-5 RICE scale to rest a decision on. The real reason to sequence Fix 2 first is dependency, which RICE doesn't capture: an Auto-Refill subscription automates the existing delivery experience, and if that experience is still 7–20 days and unreliable, the subscription just automates a bad outcome into a recurring one. Delivery is also the more frequently-cited complaint, reaching far more customers than the chronic-patient segment alone, and it's the one competitor have already made table stakes. Once fast, predictable delivery is live, Auto-Refill becomes a genuinely valuable retention play on top of it — not a subscription to a broken experience.
