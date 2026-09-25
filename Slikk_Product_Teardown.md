**Slikk — Product Teardown**

_Quick-commerce fashion, "Try & Buy" model — two problems identified via secondary research (Play Store + Reddit)._

| **Company**     | Slikk                                                                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Author**      | Vijay Nagarjun Savvasere                                                                                                                          |
| **Version**     | 1.0                                                                                                                                               |
| **Methodology** | Slikk has no in-app review system — both problems validated entirely through Play Store reviews (n=378) and Reddit threads, not a personal order. |

# **1\. Problems & Evidence**

- **Problem 1 — Items arrive used/damaged/defective, returns rejected by blaming the customer:** rusted/stained fabric, loose stitching, tags detached inside sealed packaging. ~106 of n=378 Play Store reviews (~28%) — the single largest theme. Several reviewers reported photographic proof of the defect that support didn't accept.
- **Problem 2 — Support unresponsive, no continuity, defaults to blame:** unreachable/slow support, context lost across transfers, agents accusing customers of damaging or swapping items rather than investigating. ~53–72 of 378 reviews (~14–19%), plus 14 Reddit threads. Slikk's own developer response acknowledged a disconnected-call failure as real, not a one-off.

# **2\. Root Cause**

- **Problem 1:** Fast-fashion + 60-minute delivery likely means upfront QC is deprioritized (manufacturing defects); separately, returned items are likely re-shelved without re-inspection (previously-worn items sold as new).
- **Problem 2:** The Try & Buy model exposes Slikk to genuine return fraud with no evidence trail — so agents default to blaming the customer as a low-cost fraud-protection heuristic. Support may also be structurally understaffed (50–200 total headcount), and there's no shared ticketing system, so every transfer resets the conversation to zero.

# **3\. Proposed Fixes (Must-have)**

- **Problem 1 — Evidence-based return adjudication:** timestamped photo at delivery/pack-time; a disputed return is checked against that photo instead of an agent's judgment call. Restocking gate: returned items failing a visual check against their own return-time photo are pulled from sellable stock, not re-shelved as new.
- **Problem 1 — Vendor accountability score:** each vendor's damaged-item return rate becomes a visible internal score, feeding de-prioritization in search — sequenced to ship after adjudication, since today's return-rejection data would rank vendors on corrupted numbers.
- **Problem 2 — Callback queue with an SLA:** replaces a dead end with a queued, SLA-backed callback. **Shared ticketing system:** every interaction creates a ticket with full history, so a transfer doesn't reset context. **Priority tiering:** routes critical issues (wrong item, payment deducted, damage) ahead of routine queries.

# **4\. RICE Scores**

| **Problem**                          | **RICE Score** | **Why**                                                                                                                                                                   |
| ------------------------------------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 — Defective items / unfair returns | 9              | Reach 3, Impact 4, Confidence 3, Effort 4 — largest named theme; fixes remove most, not all, of the harm (a defective item can still leave the vendor).                   |
| 2 — Support unresponsiveness         | 9              | Reach 3, Impact 3, Confidence 3, Effort 3 — fixes remove the "unreachable"/"re-explain" harm, but not the accusation pattern itself (a separate, unvalidated hypothesis). |

# **5\. Top Risks**

- **The two problems tie at RICE 9** — a genuine tie, not a scoring gap to paper over. Sequencing rests on individual fix cost and dependency, not the RICE score.
- **Bundling multiple fixes into one Effort score understates what can ship fast** — the callback queue alone is the cheapest fix in the whole set and doesn't need to wait on ticketing or tiering.
- **Auto-approving evidence-backed returns adds refund/fraud exposure** — a real cost against adjudication's benefit, not a free win.
- **Root cause for the accusation pattern (Hypothesis ②) is unvalidated** — plausible drivers (fraud-prevention KPIs, scripted objection-handling, no approval authority) are flagged for validation with Slikk's support team, not asserted as confirmed.

# **6\. Prioritization Conclusion**

RICE cannot rank these two problems — they tie at 9 once each score is checked against the rubric. My actual recommendation: ship evidence-based return adjudication (Problem 1) and the callback/SLA queue (Problem 2) first — both are comparatively low effort and each removes a distinct source of the "this app is a scam" reaction. Vendor accountability scoring follows once adjudication is live, not before, since shipping it earlier would rank vendors on corrupted data. Run the customer-photo fix in parallel with shared ticketing, since neither depends on the other; treat priority tiering as the final piece once ticketing exists to route against.