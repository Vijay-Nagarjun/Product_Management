**PRD: Real-Time Inventory Visibility & Route-Before-Accept Fulfilment**

_Stock-aware order routing so an order is accepted only once a node confirms it has the item — no new warehouses._

| Product | TrueMeds — E-Pharmacy Platform |
| --- | --- |
| Feature | Fix 2 (Teardown Problem 2) |
| Author | Vijay Nagarjun Savvasere |
| Version | 1.0 |
| Status | Draft for Review |
| Sequencing | Ships first — hard prerequisite for Subscribe & Save's city-by-city launch gate. |

# 1\. Problem & Evidence

TrueMeds promises delivery in 3–4 days at checkout; in practice orders often take 7–20 days, or are cancelled/never delivered. TrueMeds already runs 7–8 warehouses and a partner-pharmacy network across 19,000+ pin codes, so this isn't a shortage-of-nodes problem — orders are accepted and assigned to a node without first confirming stock is there, so the order silently re-routes while status keeps showing progress that hasn't happened.

*   **Scale:** Late/undelivered orders were tied with unresolved support as the two largest themes in 702 analysed Play Store reviews (~120 of ~300 substantive reviews each, ~17% of all reviews).
*   **Pattern:** ~25–30 reviews describe delivery attempts falsely logged as failed/attempted, one citing CCTV evidence — the exact failure this fix targets, not just slowness.
*   **Corroboration:** Trustpilot 1.6/5, JustDial 2.7/5 list delivery and accuracy as top complaints.
*   **Competitive gap:** Apollo 24/7's ~19–29 min delivery comes from checking live stock before accepting an order — a routing discipline, not only real estate (it also has 7,000+ outlets TrueMeds lacks).
*   **RICE score: 12** (Reach 4, Impact 4, Confidence 3, Effort 4) — sequenced ahead of Subscribe & Save (RICE 9) on dependency, not score margin.

# 2\. Goals & Non-Goals

*   **Goal —** Replace the flat 3–4 day promise with an honest, per-node delivery estimate.
*   **Goal —** Cut delivery failures and false progress-logging caused by stock gaps.
*   **Goal —** Use existing infrastructure only — no new owned warehouses.
*   **Goal —** Unblock Subscribe & Save's launch gate with a stock-confirmed ETA.
*   **Non-goal —** Not targeting Apollo-level (~19–29 min) speed; not fixing support/refunds; no paid express tier in v1; not live in all 19,000+ pin codes at once (phased by city).

# 3\. Target User & Key Stories

*   **Primary — Anjali Verma** (38, caregiver, Lucknow): needs delivery that reliably arrives in 1–2 days because the order is routed to a node confirmed to have stock — not silently assigned to one that doesn't.
*   As a customer, I want a delivery estimate at checkout that reflects real nearby stock, not a platform-wide promise.
*   As a customer with a multi-item order, I want in-stock items to ship on their own timeline, not wait on one unavailable SKU.
*   As a customer, I want to be told immediately — not days later — if my order's fulfillment changes.
*   As TrueMeds ops, I want to pause routing to a node whose stock data is stale or unreliable.

# 4\. Key Requirements (Must-have)

*   Central stock ledger (on-hand + available qty per SKU per node), synced via warehouse WMS, partner API, or manual count; every entry timestamped.
*   Stale nodes are automatically excluded from routing until they re-sync.
*   At checkout: query the ledger, route to the nearest confirmed-stock node, reserve stock (with a TTL), and show a node-specific ETA before payment.
*   No stock nearby → an honest "unavailable" message, never a silently-accepted order.
*   Reservations auto-release on cancel, payment failure, or timeout; reserve/release is race-safe (no overselling).
*   Split shipment when no single node holds the full cart — at no extra delivery fee, each shipment tracked and refunded independently.
*   A background job re-checks reservations against the live ledger; any discrepancy triggers an immediate customer notification and re-route attempt.
*   Partner pharmacies are onboarded only once license-verified and their stock-reporting is proven working.
*   Ops dashboard: promised-vs-actual delivery by node/city, staleness alerts, full routing audit log.

# 5\. Success Metrics

| Metric | Target |
| --- | --- |
| Promised-vs-actual delivery gap | Materially narrower than today's 3–4 vs 7–20 day gap |
| Orders delivered within (honest) node ETA | ≥ 90% |
| Falsely-logged delivery attempts | Materially below current ~25–30 review-flagged rate |
| Guardrail: checkout conversion | Expect a temporary dip on longer honest ETAs — tracked, not hidden |
| Guardrail: phantom stock-outs / overselling | Near-zero / zero |

# 6\. Top Risks

*   **Data quality is the single point of failure** — one under-reporting node recreates the exact silent-gap problem this fix closes. Mitigated by staleness thresholds, reconciliation audits, and a phased city rollout.
*   **Honest ETAs likely cost conversion short-term** — a direct tension with TrueMeds' acquisition-led priorities; tracked as a named, temporary guardrail metric.
*   **Reservation holds can create their own phantom stock-outs** if release logic lags — TTL/release is built as a first-class requirement, not an afterthought.
*   **This fix alone won't reach Apollo-level speed** — stock-aware routing is necessary but not sufficient; Apollo's speed also rests on 7,000+ outlets TrueMeds doesn't have.

# 7\. Open Questions

*   Does a breakdown of real order timelines confirm the stock-visibility hypothesis, or point somewhere else? (Phase 0)
*   What integration method is realistic per warehouse and per partner pharmacy (API, DB, manual)?
*   What reservation TTL best balances checkout completion against phantom-stock-out risk?
*   Should the paid express-delivery tier raised by Anjali's persona be scoped as a follow-on, and when?_._