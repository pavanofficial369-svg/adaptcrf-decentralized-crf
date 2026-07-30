# AdaptCRF — Adaptive Decentralized Case Report Form Engine

**Study DCT-HF-501** · A 32-form, industry-structured CRF binder built as an interactive, single-page prototype — not a static template.

[Live demo](https://pavanofficial369-svg.github.io/adaptcrf-decentralized-crf/)
---

## The problem

Most CDM portfolio projects — including my first four — are backend and analytics builds: SQL, Python, Excel, dashboards. They're valuable, but they don't show what a CRF actually *feels* like to fill out, and they don't demonstrate how query logic, eligibility gating, or e-signature sign-off actually behave inside an EDC.

AdaptCRF is an attempt to build that missing piece: a fully clickable CRF binder for a hypothetical decentralized heart-failure trial, structured the way a real study build in Medidata Rave or Veeva Vault CDMS would be — folders, forms, live edit-checks, and an audit trail — rather than a flat PDF or Word form.

## What it is

A single HTML file (no backend, no build step) containing:

- **32 forms**, grouped into 5 folders that mirror a real protocol structure
- **Live edit-checks** that fire the instant a value is entered — not on a separate QC pass
- **Wearable/decentralized data capture**, with source provenance (device-synced vs. manually entered)
- **Adaptive branching**, where fields only appear once they're relevant
- **Cross-form derived logic** — some forms (Visit Assessments, Safety Assessments) don't ask for input at all; they read live off other forms
- **An eligibility-gated randomization step** and an **e-signature sign-off** that's blocked while any query is open anywhere in the binder

## Form structure

| Folder | Forms |
|---|---|
| Screening & Enrollment | Study Information, Informed Consent, Subject Registration, Demographics, Inclusion Criteria, Exclusion Criteria, Medical History, Surgical History, Family History, Social History, Pregnancy Test |
| Baseline & Diagnostics | Physical Examination, Vital Signs, Laboratory Tests, ECG/Diagnostic Tests, Randomization/Allocation |
| Treatment & Visit Forms | Study Treatment Administration, Drug Accountability, Concomitant Medications, Study Procedures, Visit Assessments, Efficacy Assessments |
| Safety | Safety Assessments, Adverse Events, Serious Adverse Events, Protocol Deviations, Missed/Unscheduled Visit |
| Study Closeout | Withdrawal/Discontinuation, End of Treatment, Follow-up Visit, End of Study, Investigator Sign-off |

## Key design decisions

**1. Live edit-checks instead of batch queries.**
In most CDM workflows, query generation happens on a scheduled review pass after data entry. Here, range checks on vitals, labs, ECG QTc, drug accountability compliance, and weight-gain-vs-baseline all fire the moment a value is typed, and resolve automatically once it's back in range. This is closer to how a rules-based edit-check engine (e.g., Rave Architect edit checks) behaves, just visualized in real time.

**2. Provenance tagging on vitals.**
Each vital sign is tagged *Device* or *Manual* depending on whether it came from the simulated home-monitoring sync or was typed in — a decentralized-trial requirement, since regulators care about data source when the subject isn't in a supervised site setting.

**3. Cross-field and cross-form logic.**
A serious + related AE with no documented action opens a query on that specific field. A fatal SAE outcome flags for expedited reporting. Drug accountability computes compliance % live from dispensed/returned/expected units. Visit Assessments and Safety Assessments don't collect any new data — they're read-only summaries derived from the rest of the binder, which is closer to how a "visit completeness" dashboard works in a real EDC than how a CRF page normally behaves.

**4. Sign-off as a binder-wide gate, not a single checkbox.**
The Investigator Sign-off form checks every open query across all 32 forms and both eligibility checklists before allowing a signature — the same kind of gate a 21 CFR Part 11–style e-signature enforces in production systems.

## Honest limitations

- This is a **single visit instance**, not a repeating-folder design. A production EDC would reuse the same form definitions (Vitals, Labs, AE, etc.) across Baseline, Week 4, Week 8, Week 12 as separate folder instances with their own data. I kept it to one visit to keep the prototype navigable — happy to walk through how a repeating-folder version would be structured if asked.
- Randomization uses a simple 50/50 client-side draw, not a real block/stratified randomization algorithm.
- There's no persistence — refreshing the page clears all entered data, since this is a UI/logic demo, not a production data store.

## Tech stack

Vanilla HTML/CSS/JS — no frameworks, no dependencies. Built this way deliberately so it opens and works identically on GitHub Pages, a local double-click, or inside any browser, with nothing to install.

## How to view it

Open `adaptcrf.html` in any browser, or visit the [live demo](#) if deployed via GitHub Pages.

Try this sequence to see the logic work end to end:
1. Go to **Demographics**, fill DOB/sex — age computes automatically
2. Go to **Inclusion/Exclusion Criteria**, check all inclusion boxes, leave exclusion boxes unchecked
3. Go to **Randomization** — the button unlocks and assigns an arm
4. Go to **Vital Signs**, click **Sync device** twice — the second sync returns an out-of-range weight and opens a live query
5. Go to **Investigator Sign-off** — try signing with the query still open (it blocks you), then resolve it and sign again

## Author

Pavankumar — PG Diploma in Clinical Research & Bioinformatics (CLRI), B.Sc. Renal Dialysis Technology (RGUHS). Built as the fifth project in a CDM/PV-focused GitHub portfolio, alongside a FAERS signal-detection tool, a synthetic oncology CDM query pipeline, a PV case coding dashboard, and a Medidata Rave-style EDC oversight dashboard (ClinTrack).

[LinkedIn](https://www.linkedin.com/in/pavankumarofficial369) · [GitHub](https://github.com/pavanofficial369-svg)
