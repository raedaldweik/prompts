> **Sample document for the bootcamp.** A fictionalised policy written for the demo — replace with your real data governance policy.

# Data Governance Policy (Internal)

## §1 Purpose and scope
Governs how employees classify, access, use, and share data held in the
entity's systems, including procurement records on Etimad and analytical
copies in SAS Viya.

## §2 Classification
- **Public** — already published (e.g. published award results).
- **Internal** — operational data not intended for publication.
- **Confidential** — supplier bids, evaluation records, pre-award pricing,
  personal data. Default class for procurement transaction records.
- **Restricted** — investigation and audit case files; access by named
  authorization only.

## §3 Access
- §3.1 Access follows least privilege and is granted by the data owner.
- §3.2 Analytical environments receive Confidential data only where scoped
  to a defined use case and approved by the data owner.
- §3.3 Access rights are reviewed quarterly.

## §4 Use and sharing
- §4.1 Confidential data **must not** be shared with third parties without a
  signed agreement and data-owner approval.
- §4.2 Pre-award information (bids, estimates, evaluations) **must not** be
  disclosed to any bidder or to staff outside the evaluation committee before
  the award is announced.
- §4.3 Aggregated or anonymised statistics may be shared internally provided
  no individual supplier's confidential terms can be re-identified.
- §4.4 Personal data handling follows the national data protection framework.

## §5 Analytics and AI
- §5.1 Models and assistants operating on procurement data run inside the
  approved environment; they access only the tables scoped to their use case.
- §5.2 Risk scores and flags are decision support, not decisions; adverse
  actions require human review.
- §5.3 Synthetic data is preferred for training, demos, and testing.

## §6 Incidents
Suspected data leakage or misuse is reported to the data office within
24 hours of discovery.
