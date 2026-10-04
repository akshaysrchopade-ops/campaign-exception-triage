# Campaign Exception Triage

A product case study and build plan for an exception queue that helps paid-media teams catch budget pacing problems, cost spikes and broken conversion tracking early.

> **Status: Concept (v0.1).** Nothing here is built, tested or validated with real users yet. All data in this repo is synthetic. No employer or client data is used.

## The problem

Teams running many campaigns across markets often learn about pacing issues, cost spikes or tracking breaks late. This project explores whether a daily, ranked exception queue with a short factual explanation does a better job than dashboards and platform-native alerts.

This is a hypothesis to test, not an established fact. The plan lists the evidence that would disprove it.

## What this project is (and is not)

- **Is:** a product exercise covering discovery, prioritisation, a PRD, metrics, experimentation, a technical design and a launch plan.
- **Is not:** a production tool, a reporting dashboard, or a claim of real-world results.

## Tools

n8n (workflow automation), SQL (rules and evaluation), Google Sheets (synthetic data), Gemini via Google AI Studio (short explanations of alerts).

## Contents

| Path | What it is |
|---|---|
| `docs/campaign-exception-triage-product-plan.md` | Problem framing, PRD, metrics, prioritisation, technical design, delivery plan |
| `docs/weeks-1-2-workbook.md` | Discovery interview guide and synthetic data build instructions |
| `data/` | Synthetic datasets (added in week 2) |
| `sql/` | Detection rules and evaluation queries (added in weeks 2-3) |
| `n8n/` | Exported workflow with credentials removed (added in weeks 3-4) |
| `docs/test-log.md` | Test results (added in week 5) |

## Progress

| Stage | Status |
|---|---|
| Problem framing and PRD | Drafted |
| Discovery interviews | Not started |
| Synthetic data and SQL rules | Not started |
| n8n workflow | Not started |
| Gemini explanations and evaluation | Not started |
| Case study and demo | Not started |

Update this table as work happens. Mark results as **observed**, **simulated** or **hypothetical**.

## Limitations

- No real users or real demand are evidenced yet.
- Results come from synthetic data and will not capture real-world data messiness.
- Free-tier limits of the tools used may change.

## Licence

MIT for code. Documents are shared for portfolio and learning purposes.
