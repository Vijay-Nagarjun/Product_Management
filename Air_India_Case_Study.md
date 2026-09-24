**Air India: Fixing Baggage, Refund & Disruption Communication**

| Company | Air India (Tata Group) |
| --- | --- |
| Case Type | Product / Feature Case Study |
| Author | Vijay Nagarjun Savvasere |
| Version | 1.0 |

# Context

Air India's Tata-led turnaround (2022–present) has modernized inputs — a 570-aircraft order, cabin retrofits, a full rebrand — but hasn't moved the two outputs that matter most: the airline posted a consolidated net loss of ₹10,859 crore in FY25, and domestic market share has stayed essentially flat near 26–27% while IndiGo's has risen past 64–66%.

DGCA's June 2026 complaint-category data shows baggage handling as the single largest complaint category (27.8%), ahead of flight-related issues (24.3%) and refunds (19.7%). A separate Quality Council of India passenger survey identifies "lack of information during cancellations/delays" and "refusal of ticket refunds" as contributing grievance drivers — a hypothesis this case treats as worth designing around, not a proven causal link, since the two data sources measure different things.

# Problem & Scope

The broader question — "how does Air India fix its turnaround" — spans fleet economics, labour costs, and safety investigations outside product's remit. This case scopes to one ownable problem: when a passenger's bag is mishandled, their flight is delayed or cancelled, or they're owed a refund, they currently have no reliable way to know what's happening without calling support or visiting a counter.

This maps to a pattern Japan Airlines diagnosed in itself before its 2010 turnaround — a systemic information and visibility gap, not primarily a financial one. This case applies that same insight to the one place product can act on directly: what the passenger sees on their screen during and after a disruption.

# Target User

**Primary persona — "The Silent-Disruption Flyer" (Ananya, 29):** Marketing professional, flies Air India domestic 4–5 times a year for work. Books on price and schedule, holds no loyalty tier, experiences the airline almost entirely through the app and website. Her worst experiences aren't the delay itself — they're not knowing: standing at baggage claim with no update, or waiting days for refund status with zero visibility into where the request stands.

# Key User Stories

*   As a passenger whose checked bag hasn't arrived, I want to see its last known status and location without calling support, so I know what's happening without waiting on hold.
*   As a passenger on a delayed or cancelled flight, I want to be proactively notified with rebooking options, so I don't have to discover the disruption at the gate or on a stale app screen.
*   As a passenger owed a refund, I want a visible status tracker with an expected resolution date, so I'm not left wondering whether my request was even received.
*   As a passenger, I want baggage, refund, and disruption status all in one place, so I'm not checking three different channels for one trip.

# Proposed Solution: "Digital Trust & Recovery"

A unified, real-time status layer inside the Air India app and website, covering the three DGCA-named top complaint categories, built around three connected components:

*   Baggage tracker — checkpoint-level status pushed to app/SMS (checked in, loaded, offloaded/mishandled, located, out for redelivery).
*   Disruption & rebooking assistant — proactive delay/cancellation alerts with real-time rebooking options and connection-risk warnings.
*   Refund status tracker — a visible status with an SLA countdown, replacing today's opaque refund experience.

# Core Requirements (Must-have)

*   Display real-time baggage status in the app and via SMS for any passenger with a checked bag on an active or recent journey.
*   Automatically open a trackable case with a reference number when a bag is flagged mishandled, without requiring a support call.
*   Send a proactive push/SMS notification within 15 minutes of a confirmed delay or cancellation.
*   Show available rebooking options directly in the notification or one tap away.
*   Calculate and surface connection risk for passengers with a booked onward connection.
*   Display a visible refund-status tracker with an estimated resolution date.
*   Make all three status types — baggage, disruption, refund — accessible from a single "Trust Center" screen per booking, rather than three separate flows.
*   Deliver notifications via app push and SMS at minimum, with email as a fallback for passengers without the app installed.

# Trade-offs & Risks

*   A visibility layer doesn't fix the underlying operational problem — this should be positioned internally as a first step, not the whole fix.
*   Post-AI171 sensitivity: any feature touching delay or disruption communication needs careful tone calibration, given the ongoing investigation and the documented ~20% booking decline after the crash — a reason for careful rollout, not a reason to avoid the feature, since silence was named as a top complaint driver.
*   Compensation-eligibility logic carries real risk if wrong in either direction, which is why it's scoped as a Should, not a Must, and defaults to human review in ambiguous cases.

# If Forced to Ship One Component First

The baggage tracker. It addresses the single largest named complaint category (27.8%) and is the component that most directly reuses an operational data stream — checkpoint scans — that already exists, making it the lowest-effort, highest-named-impact starting point.

# Success Metrics & Guardrails

*   Primary: baggage-related DGCA complaint rate (target: reduce from the current 27.8% share); refund-category complaint rate and average resolution time; % of disruption cases where the passenger got a proactive notification before contacting support.
*   Guardrails: customer support contact rate per disputed case (should fall, not just shift channels); notification opt-out/mute rate (a rise signals over-notification); compensation-eligibility accuracy, spot-audited.

# What I'd Validate Next

*   Whether baggage/refund complaints correlate with a measurable NPS or repeat-booking drop — the single most important number this case doesn't have.
*   What proportion of current baggage/refund complaints are actually visibility-driven versus resolution-driven, since this case assumes a meaningful share is visibility-driven but hasn't validated the exact split.
*   Current customer-support contact volume for these three categories, to establish a baseline the guardrail metrics can be measured against.
