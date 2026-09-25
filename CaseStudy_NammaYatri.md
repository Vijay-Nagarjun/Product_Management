**Namma Yatri: Fixing Ride Reliability & Fare Trust**

| Company | Namma Yatri (MovingTech Innovations, built on ONDC/Beckn Protocol) |
| --- | --- |
| Case Type | Product / Feature Case Study |
| Author | Vijay Nagarjun Savvasere |
| Version | 1.0 |

# Context

Namma Yatri is India's zero-commission, open-network ride-hailing app built on the ONDC/Beckn protocol — drivers keep 100% of the fare, and the platform charges no commission and applies no surge markup of its own. As of its public operating-statistics dashboard (verified 9 August 2026), it has 1.63 crore registered users, 7.71 lakh enabled drivers, and 15.62 crore completed trips across seven cities.

This zero-commission model is also the platform's core structural trade-off: unlike Uber or Ola's closed dispatch, where a driver's income is directly tied to platform standing, Namma Yatri has comparatively weaker mechanisms to enforce behavior at the point of a ride. Analysis of 331 recent user reviews shows this trade-off surfacing directly in two of the five most common complaint clusters.

# Problem & Root Cause

Five complaint clusters emerged from the review analysis: driver cancellation/non-acceptance (~17%), poor customer support (~12%), fare overcharging (~11%), long wait times (~11%), and app bugs/crashes (~11%). This case study focuses on two, deliberately set aside from the other three: long wait times is folded into Problem 1 rather than treated separately, since matching latency is largely a downstream symptom of non-acceptance and counting it twice would double-count roughly a third of total complaint volume. App bugs/crashes is real but a QA/engineering execution issue with limited room to demonstrate product trade-off reasoning. Poor customer support is set aside because it functions as a downstream metric of this case rather than a third independent problem — if Problems 1 and 2 are fixed well, disputed cancellations and fare disputes shouldn't need to escalate to a human agent in the first place. The two problems kept trace directly to Namma Yatri's zero-commission business model, not generic ride-hailing UX gaps.

**Problem 1 — Driver Cancellation & Non-Acceptance (~28% combined with correlated wait-time complaints):** On closed-dispatch platforms, a driver's acceptance and cancellation rate directly affects their platform standing via commission-based leverage. Namma Yatri's zero-commission model removes that lever — drivers keep the full fare regardless of standing, so there is a materially weaker economic incentive tying long-term access to good behavior at the point of accepting or honoring a ride.

**Problem 2 — Fare Overcharging / Off-App Cash Demands (~11%):** Because Namma Yatri is a peer-to-peer, open-network model rather than a closed dispatch system with mandatory in-app payment capture, the platform has comparatively less enforcement leverage at the point of the actual cash/UPI transaction. A meaningful subset of these complaints is also avoidable rather than adversarial: tolls are paid by the driver out of pocket, so a route that includes one gives the driver a legitimate reason to ask for more — but from the rider's side it is indistinguishable from opportunistic overcharging. Separating the two is a prerequisite for fair enforcement.

# Target User

**Primary persona — "The Reliability-First Commuter":** Chose Namma Yatri specifically for lower fares and its driver-friendly ethos, but is increasingly unsure whether a booking will actually result in a ride, and now budgets extra time as a hedge against cancellation.

# Key User Stories

*   As a rider, I want a driver's acceptance to be a reliable commitment, so I'm not left waiting and then forced to rebook after a cancellation.
*   As a rider booking a short or low-fare trip, I want my ride matched with the same reliability as any other trip, so I'm not effectively deprioritized for booking a less lucrative ride.
*   As a rider, I want to see the exact final fare confirmed before the ride starts, so a driver cannot ask me for more once I'm in the vehicle.
*   As a rider taking a route with a toll, I want the toll included in the fare I'm quoted upfront, so I'm not surprised by an extra demand and left unsure whether it's legitimate.
*   As a rider who paid in cash and was overcharged, I want my complaint to still count toward holding that driver accountable even though I can't prove the specific incident.

# Proposed Solution

**7a. "Binding Acceptance" Framework (Problem 1):** Introduce real consequences tied specifically to post-acceptance cancellation, enforced through matching visibility and priority — not monetary incentives. Namma Yatri earns no margin on rides, so it has no revenue base to fund driver-side payouts the way a commission-based platform could; the only lever genuinely available is who gets shown rides, not who gets paid more.

**7b. Fare Lock, Upfront Tolls & Pattern-Based Dispute Handling (Problem 2):** Lock the quoted fare at trip start as the reference fare shown throughout the ride, with a one-tap "report fare mismatch" flow. Fold known tolls into that quoted fare upfront, removing the single most common legitimate reason for a mid-ride cash demand. Because cash-paid disputes can never be individually proven, handle them at the pattern level — weighting complaint frequency against the time window in which complaints occur.

# Core Requirements (Must-have)

*   Track a per-driver post-acceptance cancellation rate over a rolling 30-day window.
*   Reduce a driver's ride visibility/priority in matching once that rate exceeds a defined threshold.
*   Immediately re-queue a rider with priority after a post-acceptance cancellation, and log it against the driver's rate.
*   Keep the quoted fare visible and unchanged as the reference fare throughout the ride, on both rider and driver screens.
*   Itemize known tolls in the upfront quoted fare rather than leaving drivers to recover them in cash.
*   Let a rider report a fare mismatch with one tap, during the ride or within a defined window after, without contacting support first.
*   For UPI-paid rides, let the rider attach a transaction screenshot or reference ID to a report, so it can be verified against the driver's fare-dispute record.
*   Evaluate unprovable, cash-paid fare-mismatch reports at the pattern level — distinct complainants within a time window — rather than the incident level.

# Trade-offs & Risks

*   Enforcement mechanisms sit in tension with the platform's founding ethos — "we take nothing from you." Visibility penalties need careful framing or risk alienating the driver base the zero-commission model was built to attract.
*   Cash-paid disputes can never be individually proven, so pattern-based enforcement is a deliberate response to that limit, not a solution to it — genuine one-off cash overcharges remain a real, accepted gap.
*   Quoting tolls upfront makes Namma Yatri responsible for toll-data accuracy, and raises the headline quoted fare — which may dent the price-competitiveness perception that draws riders, even though the rider's true total cost is unchanged.

# If Forced to Ship One First

Cancellation tracking + priority re-queue. It targets the single largest complaint cluster (17%, ~28% combined with correlated wait-time complaints) and needs no new user-facing reporting flow or manual verification step — just matching-logic and data-tracking changes to systems that likely already exist.

# Success Metrics & Guardrails

*   Primary: post-acceptance cancellation rate (platform-wide); fare-mismatch report rate as a % of completed rides; repeat-booking rate among riders who experienced a resolved cancellation or fare dispute.
*   Guardrails: driver-side complaint rate about unfair penalization (a rise signals thresholds are misfiring); overall driver acceptance rate should not drop as a side effect of enforcement friction; share of pattern-based actions later reversed on appeal.

# What I'd Validate Next

*   The actual platform-wide post-acceptance cancellation rate and fare-dispute rate — this case only has review-based estimates.
*   What share of fare disputes are toll-attributable, which determines how much of Problem 2 the upfront-toll fix alone resolves.
*   The real distribution of complaints per driver per week — the only sound basis for setting pattern-detection thresholds rather than guessing them.
*   Driver-side sentiment on enforcement mechanisms, since that trade-off remains the single biggest open risk in this proposal.
