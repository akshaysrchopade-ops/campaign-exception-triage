# Weeks 1–2 Workbook
### Discovery interviews and synthetic data build for Campaign Exception Triage

**Status: CONCEPT.** Use this alongside the product plan. Everything here is a working instruction, not a result.

---

## Part A. Discovery interviews (week 1)

### A1. Goal
Test hypotheses H1–H5 in the plan with 5–8 conversations. You are trying to learn what really happens, not to get people to like your idea.

### A2. Who to talk to
- Media planners, campaign managers, traders, ad-ops and analytics people who run paid campaigns.
- Choose people **outside your current employer and its clients**. This keeps confidentiality clean and avoids signalling a job search inside your workplace.
- Possible sources: alumni networks, course peers, communities you belong to, and LinkedIn connections of connections.
- If you can only reach friendly contacts, say so in the case study. They tend to tell you what you want to hear.

### A3. Outreach message (adapt it)
> Hi [name], I'm working on a small independent project on how paid-media teams catch campaign problems early, such as pacing issues, cost spikes and broken tracking. I'm not selling anything. Could I ask for 20 minutes to hear about the last time something went wrong for you? I won't ask about any client names or confidential details.

### A4. Ground rules
- Ask for permission before taking notes or recording.
- Do not ask for or accept client data, names or screenshots.
- Do not pitch the idea until the last five minutes.
- Ask about **past behaviour**, not opinions about the future.

### A5. Interview script (about 20 minutes)

**Opening (2 minutes).** Thank them, explain the topic, confirm no confidential detail is needed.

**Questions**
1. What kind of campaigns do you work on, and roughly how many at a time?
2. Tell me about the last time a campaign went wrong and you found out later than you wanted. What happened?
3. How did you find out? How long after it started?
4. What did it cost you (money, time, trust)?
5. What do you check each morning? What gets skipped when you're busy?
6. Which alerts do you receive today? Which do you ignore, and why?
7. What have you tried to fix this? What worked and what didn't?
8. When something is flagged, what do you need to know to act on it quickly?
9. Which type of problem hurts most: pacing, cost spikes, or tracking breaks? Why?

**Probes to use anywhere:** "Can you walk me through that?", "What happened next?", "How often does that happen?", "How did you decide?"

**Closing (3 minutes).** Describe the idea in two sentences and ask what is wrong with it. Then ask, "Who else should I talk to?"

### A6. Note template (one per interview)

| Field | Notes |
|---|---|
| Date, role, years of experience | |
| Types of campaigns and tools | |
| Last incident: what, when, how found, delay, cost | |
| Current daily or weekly checks | |
| Alerts ignored and why | |
| Fixes tried | |
| What they need in an alert | |
| Costliest problem type | |
| Verbatim quotes (exact words) | |
| Surprises | |

### A7. Synthesis template

| Hypothesis | Supports | Contradicts | Unclear | Evidence (short quotes or notes) |
|---|---|---|---|---|
| H1 Pacing issues often found more than a day late | | | | |
| H2 Alert fatigue is why alerts are ignored | | | | |
| H3 Tracking breaks are the costliest | | | | |
| H4 Users want explanation and next step | | | | |
| H5 One set of thresholds can work broadly | | | | |

**Apply the kill rule from the plan:** if H1 and H2 are both contradicted by most interviews, stop building and write up the finding.

**Also record:** what they told you about thresholds they use, because these replace the starting assumptions in the plan.

---

## Part B. Synthetic data build (week 2)

### B1. Design
- 30 campaigns, 60 days of daily data = 1,800 rows.
- All flights run 90 days from 1 Jan 2026 (day 60 falls partway through the flight).
- 25 anomalies injected into campaigns C001–C025, one each, so none overlap.
- C026–C030 are **clean control campaigns** with no anomaly. Any alert on them is a false positive.
- Use fictional names only. No real brands or client data.

### B2. Sheet 1: `campaigns` (rows 2–31)

| Col | Field | How to fill |
|---|---|---|
| A | campaign_id | C001 to C030 |
| B | campaign_name | Fictional, e.g. Brand A UK Search Q1 |
| C | market | Pick from UK, DE, IN, AU, US |
| D | platform | Pick from Programmatic, Search, Paid Social |
| E | budget_total | `=ROUND(RANDBETWEEN(30000,150000),-3)` |
| F | start_date | 2026-01-01 |
| G | end_date | 2026-03-31 |
| H | status | live |
| I | base_daily_spend | `=ROUND(E2/(G2-F2+1),2)` |
| J | base_cpm | `=ROUND(5+RAND()*10,2)` |
| K | base_ctr | `=ROUND(0.008+RAND()*0.012,4)` |
| L | base_cvr | `=ROUND(0.02+RAND()*0.03,4)` |

**Important:** `RAND()` and `RANDBETWEEN()` change every time the sheet recalculates. After filling columns E, J, K and L, select them, copy, then use Edit > Paste special > Values only. Do this immediately or your data will keep changing.

### B3. Sheet 2: `anomaly_plan` (also your ground-truth table)

Columns: A anomaly_id, B campaign_id, C type, D start_date, E end_date, F spend_factor, G conv_factor, H start_day, I end_day.

Formulas for rows 2–26:
- `D2: =DATE(2026,1,1)+H2-1`
- `E2: =DATE(2026,1,1)+I2-1`

Enter these inputs:

| ID | Campaign | Type | Start day | End day | Spend factor | Conv factor |
|---|---|---|---|---|---|---|
| A01 | C001 | pacing_under | 20 | 29 | 0.5 | 1 |
| A02 | C002 | pacing_under | 25 | 34 | 0.5 | 1 |
| A03 | C003 | pacing_under | 31 | 40 | 0.5 | 1 |
| A04 | C004 | pacing_under | 37 | 46 | 0.5 | 1 |
| A05 | C005 | pacing_under | 43 | 52 | 0.5 | 1 |
| A06 | C006 | pacing_under | 49 | 58 | 0.5 | 1 |
| A07 | C007 | pacing_over | 22 | 28 | 1.5 | 1 |
| A08 | C008 | pacing_over | 30 | 36 | 1.5 | 1 |
| A09 | C009 | pacing_over | 38 | 44 | 1.5 | 1 |
| A10 | C010 | pacing_over | 46 | 52 | 1.5 | 1 |
| A11 | C011 | cpa_spike | 18 | 22 | 1 | 0.4 |
| A12 | C012 | cpa_spike | 24 | 28 | 1 | 0.4 |
| A13 | C013 | cpa_spike | 30 | 34 | 1 | 0.4 |
| A14 | C014 | cpa_spike | 36 | 40 | 1 | 0.4 |
| A15 | C015 | cpa_spike | 42 | 46 | 1 | 0.4 |
| A16 | C016 | cpa_spike | 48 | 52 | 1 | 0.4 |
| A17 | C017 | cpa_spike | 54 | 58 | 1 | 0.4 |
| A18 | C018 | cpa_spike | 20 | 24 | 1 | 0.4 |
| A19 | C019 | tracking_break | 21 | 24 | 1 | 0 |
| A20 | C020 | tracking_break | 27 | 30 | 1 | 0 |
| A21 | C021 | tracking_break | 33 | 36 | 1 | 0 |
| A22 | C022 | tracking_break | 39 | 42 | 1 | 0 |
| A23 | C023 | tracking_break | 45 | 48 | 1 | 0 |
| A24 | C024 | tracking_break | 51 | 54 | 1 | 0 |
| A25 | C025 | tracking_break | 56 | 59 | 1 | 0 |

### B4. Sheet 3: `daily_base` (the clean data, rows 2–1801)

Columns: A campaign_id, B report_date, C spend, D impressions, E clicks, F conversions, G spend_factor, H conv_factor.

Put these formulas in row 2, then fill them down to row 1801:

```
A2: ="C"&TEXT(INT((ROW()-2)/60)+1,"000")
B2: =INDEX(campaigns!$F$2:$F$31,INT((ROW()-2)/60)+1)+MOD(ROW()-2,60)
C2: =ROUND(INDEX(campaigns!$I$2:$I$31,INT((ROW()-2)/60)+1)*(0.9+RAND()*0.2),2)
D2: =ROUND(C2/INDEX(campaigns!$J$2:$J$31,INT((ROW()-2)/60)+1)*1000*(0.9+RAND()*0.2),0)
E2: =ROUND(D2*INDEX(campaigns!$K$2:$K$31,INT((ROW()-2)/60)+1)*(0.85+RAND()*0.3),0)
F2: =ROUND(E2*INDEX(campaigns!$L$2:$L$31,INT((ROW()-2)/60)+1)*(0.8+RAND()*0.4),0)
G2: =IFERROR(INDEX(FILTER(anomaly_plan!$F$2:$F$26,anomaly_plan!$B$2:$B$26=A2,anomaly_plan!$D$2:$D$26<=B2,anomaly_plan!$E$2:$E$26>=B2),1),1)
H2: =IFERROR(INDEX(FILTER(anomaly_plan!$G$2:$G$26,anomaly_plan!$B$2:$B$26=A2,anomaly_plan!$D$2:$D$26<=B2,anomaly_plan!$E$2:$E$26>=B2),1),1)
```

Then **paste C2:F1801 as values only**, as in B2, so the clean data stops changing.

### B5. Sheet 4: `daily_performance` (the data with anomalies applied)

Columns: A campaign_id, B report_date, C spend, D impressions, E clicks, F conversions. Row 2 formulas, filled down to row 1801:

```
A2: =daily_base!A2
B2: =daily_base!B2
C2: =ROUND(daily_base!C2*daily_base!G2,2)
D2: =ROUND(daily_base!D2*daily_base!G2,0)
E2: =ROUND(daily_base!E2*daily_base!G2,0)
F2: =ROUND(daily_base!F2*daily_base!G2*daily_base!H2,0)
```

This is the dataset the rules will run on. It scales volume for pacing anomalies, lowers conversions for cost spikes, and zeroes conversions for tracking breaks.

### B6. Sanity checks (record the results in your test log)
- [ ] 1,800 rows, 30 campaigns, 60 days each.
- [ ] No blank or negative values.
- [ ] Pick three anomalous campaigns and confirm the values change only inside the anomaly window.
- [ ] Control campaigns C026–C030 match `daily_base` exactly.
- [ ] Count how many campaigns average at least 200 clicks and 5 conversions a day. Campaigns below these levels **cannot trigger** the tracking or cost rules because of the minimum-volume guards. That is a deliberate trade-off. Write down how many fall below, and whether the guards are too strict.
- [ ] Check that each pacing anomaly can actually be caught. For example, a 7-day overspend around day 40 moves the cumulative ratio by under 10%, which is why the plan adds a run-rate check (decision D7).

### B7. Export and publish
1. Open `daily_performance` and choose File > Download > Comma-separated values. Save as `daily_performance.csv`.
2. Do the same for `campaigns` and `anomaly_plan`.
3. Upload all three to a `data/` folder in your repo.
4. Add a short `data/README.md` stating that the data is synthetic, how it was generated, the date you generated it, and that the anomaly table is the ground truth.

---

## Part C. Weeks 1–2 checklist

**Week 1**
- [ ] 5–8 interviews attempted, notes saved
- [ ] Synthesis table completed honestly, including contradicted hypotheses
- [ ] Thresholds updated if interviewees gave real ones
- [ ] One-page PRD v1 revised
- [ ] 60 minutes of SQL practice on most weekdays

**Week 2**
- [ ] Four sheets built and sanity-checked
- [ ] CSVs committed to the repo
- [ ] Three SQL rules run on the data (pacing, tracking, cost spike) plus the run-rate check
- [ ] Each rule tested against `anomaly_plan` and results recorded
- [ ] README progress table updated

**Be ready to explain in an interview:** how the data was generated, why the anomalies were injected the way they were, what the control campaigns are for, and what the synthetic data cannot tell you.
