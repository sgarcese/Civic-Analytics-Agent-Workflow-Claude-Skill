---
name: worked-example-311-response-equity
description: "End-to-end worked example demonstrating all four phases applied to: Are 311 response times equitable across Boston neighborhoods, and how does Boston compare to Pittsburgh? Uses real Boston Open Data field names and query patterns."
---

# Worked Example: 311 Response Time Equity Analysis + Cross-City Benchmark

This walks through the complete FRAME → ANALYZE → BENCHMARK → COMMUNICATE workflow.

---

## PHASE 1: FRAME (Bloomberg Methodology)

### The Triggering Question

A City Councilor asks: *"My constituents in Mattapan feel like the city takes forever to fix things compared to other neighborhoods. Is that true? And is Boston even doing well compared to other cities?"*

### Reframe Before Querying

**Don't jump to:** "Let me pull the 311 data and compare averages."

**Instead, apply problem framing:**
- "Fix things" — which service types? All 311 categories or specific ones?
- "Takes forever" — total time to close? Time to first response?
- "Compared to other neighborhoods" — which comparison is fair?
- "Other cities" — Pittsburgh? San Jose? What metrics are comparable?
- What would explain differences? Volume? Complexity? Staffing? Geography?

### Stakeholder Map

| Stakeholder | Perspective |
|-------------|------------|
| Mattapan residents | Direct experience; may have stopped reporting |
| 311 operators | See the full queue; know bottlenecks |
| PWD / ISD field crews | Know operational constraints |
| Councilor | Needs defensible evidence to advocate |
| Mayor's office | Needs to know if there's a systemic issue |

### Problem Statement

> Residents in Mattapan report experiencing longer wait times for 311 resolution compared to other Boston neighborhoods. Available 311 data from 2024-2025 includes case open/close dates, on-time indicators, service types, and neighborhood — allowing us to assess whether measurable response time disparities exist. We will examine on-time performance across neighborhoods, disaggregated by service type. Key limitations: 311 data measures reported cases (subject to reporting bias), date fields measure system closure (which may not match physical work completion), and the on-time field depends on SLA definitions that may vary by request type.

---

## PHASE 2: ANALYZE (J-PAL Methodology)

### Query Sequence

```
# Step 1: Confirm 2024 schema
get_datastore_schema("dff4d804-5031-443a-8409-8344efd0e5c8")
→ Legacy schema: open_dt, closed_dt, on_time, type, department, neighborhood

# Step 2: Mattapan volume
query_datastore("dff4d804-...", filters={"neighborhood": "Mattapan"}, limit=5)
→ Note: total_count returned in response

# Step 3: On-time vs. overdue for Mattapan
query_datastore("dff4d804-...", filters={"neighborhood": "Mattapan", "on_time": "ONTIME"}, limit=5)
query_datastore("dff4d804-...", filters={"neighborhood": "Mattapan", "on_time": "OVERDUE"}, limit=5)

# Step 4: Repeat for comparison neighborhoods
[Back Bay, Dorchester, Jamaica Plain, Roxbury — same queries]

# Step 5: Service type breakdown for Mattapan
query_datastore("dff4d804-...", filters={"neighborhood": "Mattapan"}, limit=200)
→ Tally most common types and their on-time rates

# Step 6: Population for per-capita rates
query_datastore("b0543358-d03f-4682-bf0c-658ea4573d6f", limit=30)
→ Get population for each neighborhood
```

### Sample Results Table

| Neighborhood | Total Requests | On-Time | Overdue | On-Time Rate | Per-Capita Rate |
|-------------|---------------|---------|---------|-------------|----------------|
| Mattapan | [N] | [N] | [N] | [X%] | [X/10K] |
| Back Bay | [N] | [N] | [N] | [Y%] | [Y/10K] |
| Dorchester | [N] | [N] | [N] | [Z%] | [Z/10K] |
| Roxbury | [N] | [N] | [N] | [W%] | [W/10K] |
| Citywide | [N] | [N] | [N] | [V%] | [V/10K] |

### Claim Strength Labels

| Finding | Claim Level | Language |
|---------|------------|----------|
| Mattapan's on-time rate is X% vs. citywide Y% | **Descriptive** | "The data shows Mattapan's on-time rate is X percentage points below citywide average" |
| Gap is largest for street repair requests | **Descriptive** | "The disparity is most pronounced for [type] requests" |
| Service mix explains part of the gap | **Correlational** | "Differences in request types account for approximately Z% of the gap" |
| Staffing allocation drives the gap | **Hypothetical** | "One possible explanation is that crew allocation doesn't proportionally match request volume; this hypothesis cannot be tested with available data" |

### Equity Analysis

Geographic equity check:
- Group all 24 neighborhoods by on-time rate
- Cross-reference with population demographics
- Question: Do neighborhoods with higher proportions of residents of color or lower incomes show systematically lower on-time rates?

Access equity check:
- Per-capita 311 request rates by neighborhood
- Cross-reference with `2024-digital-equity-survey`
- If lower digital access = lower per-capita reporting → measured disparity may understate actual service gap

**Required caveat:**
> "This analysis uses neighborhood-level data as a proxy for demographic analysis. Individual-level demographic data is not available in the 311 dataset. Patterns observed at the neighborhood level may not apply to all individuals within those neighborhoods (ecological fallacy). However, neighborhood-level patterns are directly relevant for resource allocation decisions."

---

## PHASE 4: BENCHMARK (Cross-City Comparison)

### Discover Pittsburgh's Equivalent Data
```
PGH Open Data MCP:ckan__search_datasets("service requests")
PGH Open Data MCP:ckan__search_datasets("311")
→ Identify Pittsburgh's equivalent dataset

PGH Open Data MCP:ckan__get_schema(resource_id)
→ Find equivalent fields for: case open date, case close date,
  on-time indicator or resolution time, request type, neighborhood/district
```

### Discover San Jose's Equivalent Data
```
San Jose MCP:ckan__search_datasets("service requests")
San Jose MCP:ckan__search_datasets("constituent")
→ Identify San Jose's equivalent dataset

San Jose MCP:ckan__get_schema(resource_id)
→ Find equivalent fields
```

### Cross-City Comparability Assessment

| Dimension | Boston | Pittsburgh | San Jose |
|-----------|--------|-----------|---------|
| Metric | on_time field (ONTIME/OVERDUE) | [check schema] | [check schema] |
| Population | ~675K | ~300K | ~1M |
| SLA definitions | Vary by request type | [verify] | [verify] |
| Comparable? | — | Approximately | Partially |

### Benchmark Summary

```
CROSS-CITY BENCHMARK: 311 On-Time Performance

City        On-Time Rate    Per 10K Requests    Period    Source
──────────────────────────────────────────────────────────────
Boston      [X%]            [normalized]         2024      data.boston.gov
Pittsburgh  [Y%]            [normalized]         [period]  data.wprdc.org
San Jose    [Z%]            [normalized]         [period]  data.sanjoseca.gov

Comparability: Moderate — SLA definitions and request category definitions 
differ across cities. Use as directional, not precise.
```

**Appropriate claim:** "Boston's on-time rate is [X] percentage points [above/below] Pittsburgh's, though differences in SLA definitions mean this comparison is directional rather than precise."

---

## PHASE 3: COMMUNICATE (GovLab / InnovateUS)

### Three Audience-Specific Outputs

**Output A: Executive Memo for Mayor's Office**
```
TO:   [Chief of Staff]
FROM: [Analytics Team]
DATE: [Date]
RE:   311 Response Time Disparities — Mattapan and Peer City Comparison

BOTTOM LINE: Analysis of 2024 311 data confirms that Mattapan residents 
experience on-time completion rates X percentage points below the citywide 
average. This pattern extends to [Y other neighborhoods] and likely reflects 
systemic resource allocation, not neighborhood-specific factors. Boston's 
on-time rate appears [above/below/comparable to] Pittsburgh's, though 
definitional differences limit direct comparison.

[Continue with Key Findings, Equity Note, Recommendation, Requested Decision]
```

**Output B: Policy Brief for City Council (3-5 pages)**
Uses Policy Brief template from TEMPLATES.md. Include "What Other Cities Have Done" section with Pittsburgh/San Jose benchmark findings.

**Output C: Community Fact Sheet for Mattapan Residents**
Uses Community Fact Sheet template. Plain language. Specific to Mattapan experience. Include feedback mechanism.

---

## Engagement Design (GovLab)

1. Share draft findings with Mattapan community organizations before finalizing
2. Ask specific questions: "Does this match your experience? What are we missing?"
3. Build a neighborhood comparison dashboard for public exploration
4. Commit to reporting back on what changed based on findings
5. Publish methodology so others can verify or extend the analysis

---

## MCP Tool Call Summary

| Step | City | Tool | Purpose |
|------|------|------|---------|
| 1 | Boston | `search_datasets("311")` | Find dataset |
| 2 | Boston | `get_dataset_info("311-service-requests")` | Get resource IDs |
| 3 | Boston | `get_datastore_schema(2024_resource_id)` | Confirm field names |
| 4 | Boston | `query_datastore(…, filters={"neighborhood":"Mattapan"})` | Mattapan volume |
| 5 | Boston | `query_datastore(…, on_time="ONTIME")` | On-time count |
| 6 | Boston | `query_datastore(…, on_time="OVERDUE")` | Overdue count |
| 7 | Boston | Repeat 4-6 for each comparison neighborhood | Comparison data |
| 8 | Boston | `query_datastore(population_resource_id)` | Per-capita denominator |
| 9 | Pittsburgh | `ckan__search_datasets("311 service requests")` | Find Pittsburgh data |
| 10 | Pittsburgh | `ckan__get_schema(resource_id)` | Field names |
| 11 | Pittsburgh | `ckan__query_data(resource_id, …)` | Pittsburgh metrics |
| 12 | San Jose | `ckan__search_datasets("service requests")` | Find SJ data |
| 13 | San Jose | `ckan__get_schema(resource_id)` | Field names |
| 14 | San Jose | `ckan__query_data(resource_id, …)` | San Jose metrics |

---

## Key Methodology Lessons

**Problem Framing (Bloomberg):** The Councilor's question needed reframing before querying. Stakeholder mapping revealed frontline knowledge the data can't capture. Problem statement explicitly listed what the data could and couldn't answer.

**Analysis (J-PAL):** Started descriptive before diagnostic. Checked whether service mix explains the gap before concluding systemic discrimination. Used explicit claim-strength language throughout. Included honest limitations.

**Benchmarking:** Discovered Pittsburgh and San Jose data before comparing. Documented metric definitions to assess comparability. Reported differences as directional, not precise. Connected benchmarks to a specific investigable hypothesis for Boston.

**Communication (GovLab):** Three outputs for three audiences from the same analysis. Executive memo leads with bottom line. Community fact sheet uses plain language and invites participation. All cite data sources and acknowledge limitations. Engagement hooks built in.
