---
name: city-performance-management
description: "Use this skill when the user needs to analyze city service efficiency through the lens of budget, expenditures, and staffing. ALWAYS use when the user asks about performance management, results for money invested, cost per outcome, workload per FTE, staffing efficiency, budget vs. performance trends, or wants to connect spending and staffing data to service delivery outcomes. Inspired by Results for America and PerformanceStat (CitiStat). Triggers include: 'performance management', 'results for money', 'budget vs. performance', 'cost per outcome', 'workload per FTE', 'staffing efficiency', 'are we getting results', 'spending vs. outcomes', 'PerformanceStat', '311 efficiency', 'permit processing time', 'code violations performance', 'public safety performance', 'how much does it cost to', 'how many cases per employee', 'is the department understaffed', 'overtime analysis'."
---

# City Performance Management — Results for America / PerformanceStat Methodology

## Core Commitment
**Public investment must produce measurable results.** Connect what the city spends and who it employs to what it actually delivers — then ask whether that relationship is improving over time.

---

## Step 0 — Identify the Service Domain

Before pulling any data, determine which city service the user is asking about. Select the **primary operational data source** from the table below. **Never default to CityScore if a direct operational dataset exists.**

| Service Domain | Primary Performance Data | Key Performance Fields |
|---------------|-------------------------|----------------------|
| Basic services (trash, recycling, streetlights, rodents, potholes, graffiti, abandoned vehicles) | **311** — filter by `type` (legacy) or `service_name` (new system) | `on_time`, resolution time (`close_date − open_date`), volume by `neighborhood` and `department` |
| Code enforcement | **Code violations** — `search_datasets("code violations")` to confirm resource IDs | Open/closed violations, days-to-resolve, address/neighborhood |
| Permitting | **Building permits** — `approved-building-permits` | Permit volume, days-to-issue, project type, declared value |
| Public safety | **911 dispatch** (`911-daily-dispatch-count-by-agency`) + **Crime incidents** (`crime-incident-reports`) | Daily dispatch volume by agency, incident counts by type and district |
| All other services (parks, social services, etc.) | **CityScore** (fallback only) — resource ID `dd657c02-3443-4c00-8b29-56a40cfe7ee4` | Score numerator/denominator by metric name and timestamp |

> **311 schema reminder:** Legacy system (pre-Oct 2025) uses `type`, `open_dt`, `closed_dt`, `department`. New system (Oct 2025+) uses `service_name`, `open_date`, `close_date`, `assigned_department`. Always confirm schema first.

---

## Pre-Analysis Checklist

Before querying anything:
- [ ] Have I identified the service domain and selected the correct operational dataset?
- [ ] Do I have the fiscal year(s) the user wants to analyze? (Boston FY = July 1 – June 30)
- [ ] Have I confirmed field names via `get_datastore_schema()`? **Never guess field names.**
- [ ] Do I know which department name to filter on? (Names vary across datasets — verify before filtering)
- [ ] Am I clear on whether the user wants *planned* budget (Operating Budget) or *actual* spend (Checkbook Explorer)?

---

## Module 1 — Cost per Outcome

*"How much does it cost to deliver one unit of this service?"*

### Data Sources
- **Actual expenditures:** Checkbook Explorer (`d22fdd5c-7e4c-41b7-a3eb-dfc57a87b245`) — filter by `dept_name`
- **Planned budget:** Operating Budget (`8f2971f0-7a0d-401d-8376-0289e3b810ba`) — filter by `Department`
- **Outcome volume:** Domain-specific operational dataset (see Step 0)

### MCP Workflow
```
1. get_datastore_schema("d22fdd5c-7e4c-41b7-a3eb-dfc57a87b245")
   → confirm exact dept field name in Checkbook

2. query_datastore("d22fdd5c-7e4c-41b7-a3eb-dfc57a87b245",
     filters={"dept_name": "[Department Name]"},
     date_range={"field": "entered_date", "start_date": "YYYY-07-01", "end_date": "YYYY+1-06-30"})
   → total actual expenditure for fiscal year

3. get_datastore_schema([operational_resource_id])
   → confirm service type, date, and on_time field names

4. query_datastore([operational_resource_id],
     filters={"department": "[Department Name]", "on_time": "ONTIME"},
     date_range={"field": "[date_field]", "start_date": "YYYY-07-01", "end_date": "YYYY+1-06-30"})
   → count of successfully resolved outcomes

5. Compute: cost_per_outcome = total_expenditure / outcome_count
```

### Interpretation Flags
| Signal | What It Means |
|--------|--------------|
| Spending ↑ + outcomes flat or ↓ | Efficiency loss — investigate cause before drawing conclusions |
| Spending flat + outcomes ↑ | Efficiency gain — examine whether workload or complexity changed |
| Spending ↑ + outcomes ↑ proportionally | Scaling up — cost per outcome stable |
| Spending ↓ + outcomes ↓ | Potential disinvestment — check staffing and volume context |

**Claim strength: correlational.** Label as: *"The data is consistent with [interpretation], though spending-outcome correlations do not establish causation."*

---

## Module 2 — Workload per FTE

*"How much work is each employee carrying, and are there signs of staffing stress?"*

### Data Sources
- **Staffing proxy:** Employee Earnings Report (`29b3544f-752a-4cb1-a6af-a1de153d20a0`)
  - Count unique employee entries per department = headcount proxy (not authorized FTE count)
  - Sum `OVERTIME` pay and `TOTAL_GROSS` per department
- **Workload volume:** Total cases/requests/incidents from operational dataset (do not filter on_time — use all volume)

### MCP Workflow
```
1. get_datastore_schema("29b3544f-752a-4cb1-a6af-a1de153d20a0")
   → confirm field names: DEPARTMENT_NAME, OVERTIME, TOTAL_GROSS, etc.

2. query_datastore("29b3544f-752a-4cb1-a6af-a1de153d20a0",
     filters={"DEPARTMENT_NAME": "[Department Name]"},
     limit=500)
   → count rows = employee headcount proxy
   → sum OVERTIME / sum TOTAL_GROSS = overtime rate

3. query_datastore([operational_resource_id],
     filters={"department": "[Department Name]"},
     date_range={"field": "[date_field]", "start_date": "YYYY-07-01", "end_date": "YYYY+1-06-30"})
   → total volume (total_count from response, not just returned rows)

4. Compute:
   workload_per_employee = total_volume / employee_count
   overtime_rate = total_overtime / total_gross
```

### Staffing Stress Indicators
- **Overtime rate > 15%** of gross compensation → flag for further review
- **Workload per employee trending up YoY** without resolution time improving → potential capacity gap
- **High overtime + declining on-time rate** → strongest combined signal of staffing stress

**Caveat:** Employee Earnings headcount ≠ authorized FTE positions. It counts payroll entries in a given year. Always state: *"Employee count is a proxy derived from payroll records, not official FTE authorization data."*

---

## Module 3 — Budget vs. Performance Trend (Multi-Year)

*"Is performance improving in proportion to investment over time?"*

### Data Sources
- **Budget trend:** Operating Budget — fields `FY23 Actual Expense`, `FY24 Actual Expense`, `FY25 Appropriation`, `FY26 Budget`
- **Performance trend:** Repeat Module 1 outcome count query for each fiscal year

### MCP Workflow
```
1. get_datastore_schema("8f2971f0-7a0d-401d-8376-0289e3b810ba")
   → confirm multi-year expense field names

2. query_datastore("8f2971f0-7a0d-401d-8376-0289e3b810ba",
     filters={"Department": "[Department Name]"})
   → retrieve FY23, FY24, FY25, FY26 budget figures in one query

3. Run outcome count queries for each fiscal year (adjust date_range per FY):
   FY23: July 1, 2022 – June 30, 2023
   FY24: July 1, 2023 – June 30, 2024
   FY25: July 1, 2024 – June 30, 2025

4. Build YoY table:
   | FY | Actual Spend | Outcome Count | Cost per Outcome | % Spend Change | % Outcome Change |
```

### Trend Signals
- Calculate % change year-over-year for both spending and outcomes
- If both move together proportionally → stable efficiency
- If spend growth outpaces outcome growth → declining efficiency
- If outcome growth outpaces spend growth → improving efficiency or demand-driven volume

**Claim strength: descriptive/correlational.** Always list plausible alternative explanations: inflation, demand changes, policy changes, data collection changes, staffing vacancies.

---

## Module 4 — Overtime & Staffing Efficiency

*"Which departments are showing staffing stress signals?"*

### MCP Workflow
```
1. query_datastore("29b3544f-752a-4cb1-a6af-a1de153d20a0",
     filters={"DEPARTMENT_NAME": "[Department Name]"},
     limit=500)
   → aggregate: overtime_total / gross_total per department

2. Flag: overtime_rate > 0.15 (15%) = staffing stress signal

3. Cross-reference with operational data:
   → Are high-overtime departments also showing longer resolution times?
   → Are they showing declining on-time rates?
```

### What Overtime Signals (and Doesn't Signal)
- **Does signal:** Potential capacity gap relative to current workload at current staffing levels
- **Does NOT prove:** That more FTEs are needed (could reflect scheduling, vacancy management, or demand spikes)
- **Use for:** Budget advocacy data point, not standalone staffing conclusion

---

## Evidence Synthesis Template (Results for America Framing)

Use this output structure for every performance management analysis:

```markdown
## Performance Summary: [Department / Service Domain] — FY[XX]

### Investment
- Budget allocated: $[X] (source: Operating Budget, FY[XX] Appropriation)
- Actual spend: $[X] (source: Checkbook Explorer, July [YYYY]–June [YYYY])
- Employee headcount (proxy): [N] (source: Employee Earnings Report)
- Overtime as % of gross: [X]% [flag if >15%: ⚠️ Staffing stress indicator]

### Outcomes
- Total service volume: [N] [cases / permits / incidents / violations]
  (source: [311 / approved-building-permits / code violations / 911 dispatch])
- On-time / resolved rate: [X]%
- Average resolution time: [X] days
- [Domain-specific metric if applicable]

### Efficiency Ratios
- Cost per [outcome unit]: $[X]
- Workload per employee: [N] [units] per person per year

### Year-over-Year Trend
| FY | Actual Spend | Outcome Count | Cost per Outcome | % Spend Δ | % Outcome Δ |
|----|-------------|---------------|-----------------|-----------|-------------|
| FY23 | | | | baseline | baseline |
| FY24 | | | | | |
| FY25 | | | | | |

Trend signal: [improving efficiency / declining efficiency / scaling proportionally / mixed / insufficient data]

### What We Don't Know
- Headcount is a payroll proxy, not authorized FTE count
- Checkbook captures recorded transactions; may not reflect all cost centers
- [Operational data source] volume reflects reported activity, not underlying need
- [List any data gaps or coverage issues encountered]

### Implications for Decision-Makers
[2–4 action-oriented bullets. Label each with claim strength: Descriptive / Correlational / Suggestive]
- [Finding 1] (Descriptive): ...
- [Finding 2] (Correlational): ...
- [Open question]: To determine whether [X], additional data on [Y] would be needed.
```

---

## Key Caveats — Always Include

| Caveat | Reason |
|--------|--------|
| Employee Earnings headcount ≠ authorized FTE positions | Payroll records count individuals paid in a year, including part-year, part-time, and transitional staff |
| Checkbook actual spend ≠ Operating Budget appropriation | Appropriations are authorized maximums; actuals reflect what was drawn down |
| Boston FY = July 1 – June 30 | FY26 = July 1, 2025 – June 30, 2026 — date ranges must reflect this |
| 311 volume = reported requests, not actual service need | Reporting rates vary by neighborhood, language access, digital access |
| Spending-outcome correlation ≠ causation | Many factors change simultaneously; label all trend findings as correlational |
| Department name inconsistency across datasets | "Public Works" in one dataset may appear as "PWD" or "Public Works Department" in another — verify before filtering |
| CityScore is output/activity data | CityScore metrics measure volume and timeliness, not impact or resident outcomes |

---

## Full MCP Query Sequence

```
Step 1: Identify service domain (Step 0 table) → select operational dataset
Step 2: search_datasets("[service domain]") → confirm dataset and resource IDs
Step 3: get_datastore_schema([operational_resource_id]) → exact field names
Step 4: query_datastore([operational_resource_id],
          filters={"[dept_field]": "[dept_name]"},
          date_range={"field": "[date_field]", "start_date": "YYYY-07-01", "end_date": "YYYY+1-06-30"})
        → total volume and outcome metrics
Step 5: get_datastore_schema("d22fdd5c-7e4c-41b7-a3eb-dfc57a87b245") → Checkbook fields
Step 6: query_datastore("d22fdd5c-7e4c-41b7-a3eb-dfc57a87b245",
          filters={"[dept_field]": "[dept_name]"},
          date_range=...) → actual expenditure
Step 7: get_datastore_schema("8f2971f0-7a0d-401d-8376-0289e3b810ba") → Budget fields
Step 8: query_datastore("8f2971f0-7a0d-401d-8376-0289e3b810ba",
          filters={"Department": "[dept_name]"}) → multi-year budget figures
Step 9: get_datastore_schema("29b3544f-752a-4cb1-a6af-a1de153d20a0") → Earnings fields
Step 10: query_datastore("29b3544f-752a-4cb1-a6af-a1de153d20a0",
           filters={"DEPARTMENT_NAME": "[dept_name]"}, limit=500) → headcount + overtime
Step 11: Compute ratios → apply Evidence Synthesis Template
```

---

## Handoff

Pass to other phases when relevant:
- **→ `Analytical_Skill.md`** if equity analysis is needed (do outcomes vary by neighborhood relative to spending?)
- **→ `Benchmarking_Skill.md`** if the user wants to compare Boston's efficiency ratios to San Francisco, Seattle, or DC (see Performance Management Benchmarking module in that skill)
- **→ `Communication_Skill.md`** when converting findings into a memo, brief, or dashboard

---

## PerformanceStat Principles Applied

| Principle | Application in This Skill |
|-----------|--------------------------|
| **Regular data review** | Structure findings as a standing template; update each fiscal year |
| **Root cause orientation** | Don't stop at "costs are up" — examine workload, overtime, and volume together |
| **Accountability** | Name the department; name the metric; name the fiscal year |
| **Action orientation** | Every synthesis ends with implications for decision-makers |
| **Honest accounting** | State every caveat; label every claim; show what we don't know |
