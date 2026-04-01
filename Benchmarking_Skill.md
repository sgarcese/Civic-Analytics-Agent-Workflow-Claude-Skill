---
name: city-benchmarking
description: "Use this skill whenever the user wants to compare Boston's performance to peer cities, identify best practices from other municipalities, understand how Boston ranks on any metric, or find evidence that a policy approach has worked elsewhere. ALWAYS use when the user mentions 'other cities', 'how does Boston compare', 'best practices', 'peer cities', 'what works', 'benchmarks', 'lessons from elsewhere', or asks whether Boston is doing better or worse than comparable municipalities. This skill uses Boston Open Data MCP (primary), San Francisco Open Data MCP (Socrata), Seattle Open Data MCP (Socrata), and DC Open Data MCP (ArcGIS) for rigorous cross-city comparisons against cities of comparable scale and complexity. Applies J-PAL caution to comparative claims and Bloomberg framing to translate benchmarks into actionable city policy recommendations. Includes a dedicated Performance Management Benchmarking module — pair with Performance_Management_Skill.md to compare cost-per-outcome, workload-per-FTE, and budget-efficiency ratios across all four cities."
---

# City Benchmarking — Cross-City Comparative Analysis

## Purpose and Power

Benchmarking against peer cities is one of the most persuasive forms of evidence in city policy. It answers the question that every skeptic asks: *"Is this actually a problem, or is every city like this?"* Used carefully, cross-city comparison can:
- Validate that a problem is real and solvable (not just inherent to city life)
- Identify specific practices from peer cities worth adapting
- Set realistic performance targets grounded in what's achievable
- Build the case for investment by showing what's possible
- Compare efficiency ratios (cost-per-outcome, workload-per-FTE) against cities with similar service obligations

**Used carelessly, it misleads.** Cities differ in geography, demographics, data systems, and reporting practices. Apply J-PAL rigor to every comparative claim.

---

## Why San Francisco, Seattle, and DC

These three cities are Boston's primary benchmarking peers because they share a constellation of structural features that make comparison meaningful:

| Feature | Boston | San Francisco | Seattle | Washington DC |
|---------|--------|---------------|---------|---------------|
| Population | ~675K | ~870K | ~750K | ~690K |
| Urban density | High | Very high | High | High |
| Tech sector | Significant | Very high | Very high | Moderate |
| Union workforce | Yes | Yes | Yes | Yes |
| Strong open data | Yes | Yes | Yes | Yes |
| 311 / service system | Yes | Yes | Yes | Yes |
| Similar service obligations | Yes | Yes | Yes | Yes |
| Data platform | CKAN/Socrata | Socrata | Socrata | ArcGIS |

Population proximity and shared urban complexity make comparison substantive. All four cities operate strong open data programs, enabling real cross-city queries rather than report-based comparisons.

---

## Available Data Sources

### Primary: Boston Open Data
- MCP: `Boston Open Data MCP`
- Portal: https://data.boston.gov
- Use for: Boston's baseline metrics across all domains

### Peer City 1: San Francisco, CA
- MCP: `San Francisco Open Data`
- Portal: https://data.sfgov.org
- City profile: ~870K residents; dense urban; tech-sector economy; Socrata platform; strong civic data infrastructure; comparable service obligations (311, permits, public safety, budget, payroll)
- **Comparability strengths:** Comparable population scale and urban density; Socrata platform (familiar field patterns); strong union workforce; similar innovation economy
- **Comparability cautions:** Higher cost of living inflates all dollar figures; California regulatory environment; different pension and benefit structures inflate per-employee costs relative to Boston

### Peer City 2: Seattle, WA
- MCP: `Seattle Open Data`
- Portal: https://data.seattle.gov
- City profile: ~750K residents; tech-driven economy; Socrata platform; Pacific Northwest; active 311 and permitting systems; strong data culture
- **Comparability strengths:** Very similar population; Socrata platform; tech-sector city comparable to Boston's innovation economy; strong service request data; active performance management tradition
- **Comparability cautions:** Different climate affects infrastructure demands; Washington state regulations differ from Massachusetts

### Peer City 3: Washington, DC
- MCP: `DC Open Data`
- Portal: https://opendata.dc.gov
- City profile: ~690K residents; dense urban; unique governance structure (city + state functions within federal enclave); ArcGIS platform; sophisticated data infrastructure; active 311 system (DC311/DCii)
- **Comparability strengths:** Most similar population to Boston; very high urban density; union workforce; sophisticated performance management culture (CitiStat model — Boston's CitiStat is directly modeled on DC's); comparable governance complexity
- **Comparability cautions:** City-state hybrid government handles functions other cities delegate to counties or states; federal government presence distorts some metrics; ArcGIS platform requires different query syntax from Boston and other Socrata cities

---

## MCP Tool Reference — All Four Cities

### Boston (Boston Open Data MCP)
```
search_datasets(query)                      → Find datasets by topic
get_dataset_info(dataset_id)                → Get resource IDs and metadata
get_datastore_schema(resource_id)           → Get exact field names
query_datastore(resource_id, filters={},    → Retrieve records
  sort="", limit=100, date_range={})
```

### San Francisco (San Francisco Open Data — Socrata)
```
San Francisco Open Data:socrata__search_datasets(query)
San Francisco Open Data:socrata__get_dataset(dataset_id)
San Francisco Open Data:socrata__get_schema(resource_id)
San Francisco Open Data:socrata__query_dataset(resource_id, ...)
San Francisco Open Data:socrata__execute_sql(soql_query)
San Francisco Open Data:socrata__list_categories()
```

### Seattle (Seattle Open Data — Socrata)
```
Seattle Open Data:socrata__search_datasets(query)
Seattle Open Data:socrata__get_dataset(dataset_id)
Seattle Open Data:socrata__get_schema(resource_id)
Seattle Open Data:socrata__query_dataset(resource_id, ...)
Seattle Open Data:socrata__execute_sql(soql_query)
Seattle Open Data:socrata__list_categories()
```

### Washington DC (DC Open Data — ArcGIS)
```
DC Open Data:arcgis__search_datasets(query)
DC Open Data:arcgis__get_dataset(dataset_id)
DC Open Data:arcgis__query_data(dataset_id, ...)
DC Open Data:arcgis__get_aggregations(dataset_id, ...)
```

**⚠️ ALWAYS call get_schema/get_datastore_schema before any query. Field names differ across cities and across datasets within the same city. DC uses ArcGIS — syntax differs from Socrata for SF and Seattle.**

---

## Benchmarking Workflow

### Phase 1: Establish the Comparison Question

Before querying any city's data, define:

1. **What metric** are we comparing? (Be precise — response time? Per-capita complaints? Permit approval rate?)
2. **Are these metrics comparable?** Do all cities measure and report the same thing?
3. **What denominator** makes the comparison fair? (Per capita? Per lane-mile? Per housing unit?)
4. **What time period** aligns across cities?
5. **What structural differences** must we account for?

**Comparability Assessment Template:**
```
Metric being compared: [X]
Boston measures it as: [field name, definition]
San Francisco measures it as: [field name, definition]
Seattle measures it as: [field name, definition]
DC measures it as: [field name, definition]
Are definitions equivalent? [Yes / Approximately / No — explain]
Recommended denominator: [why]
Known structural differences: [list]
Confidence in comparison: [High / Moderate / Low]
```

---

### Phase 2: Data Discovery Across Cities

Run parallel searches across all four cities:

```
# Boston
search_datasets("[topic]")

# San Francisco
San Francisco Open Data:socrata__search_datasets("[topic]")

# Seattle
Seattle Open Data:socrata__search_datasets("[topic]")

# Washington DC
DC Open Data:arcgis__search_datasets("[topic]")
```

For each city that has relevant data:
1. Identify dataset IDs and resource IDs
2. Inspect schema to find the equivalent fields
3. Note data quality indicators (how current? how complete?)

---

### Phase 3: Parallel Data Collection

Collect metrics for each city using consistent parameters:

```
# Example: 311/service request volume per city

# Boston 2024 data
query_datastore("[resource_id]", limit=5)  → note total_count
# cross-reference with Boston population for per-capita rate

# San Francisco — search for equivalent 311/service request data
San Francisco Open Data:socrata__search_datasets("311 service requests")
San Francisco Open Data:socrata__get_schema(resource_id)
San Francisco Open Data:socrata__query_dataset(resource_id, limit=5)  → note equivalent metrics

# Seattle
Seattle Open Data:socrata__search_datasets("service requests")
Seattle Open Data:socrata__get_schema(resource_id)
Seattle Open Data:socrata__query_dataset(resource_id, limit=5)  → note equivalent metrics

# DC
DC Open Data:arcgis__search_datasets("311 service requests")
DC Open Data:arcgis__get_dataset(dataset_id)
DC Open Data:arcgis__query_data(dataset_id, limit=5)  → note equivalent metrics
```

---

### Phase 4: Normalize and Compare

**Standard Normalization Approaches:**

| Metric Type | Denominator | Rationale |
|-------------|-------------|-----------|
| Service requests, complaints | Per 10,000 residents | Population-adjusted comparison |
| Response times | Median (not mean) | Resistant to outliers from long-tail cases |
| Permit approvals | Per 1,000 housing units | Accounts for housing stock size |
| Infrastructure issues | Per lane-mile or per acre | Accounts for city footprint |
| Budget allocations | Per resident | Standard public finance comparison |
| Cost per outcome | $ per resolved case | Service efficiency comparison |
| Workload per staff | Cases per employee per year | Staffing efficiency comparison |

**City Population Reference:**
- Boston: ~675,000
- San Francisco: ~870,000
- Seattle: ~750,000
- Washington DC: ~690,000

---

### Phase 5: Interpret with J-PAL Rigor

Cross-city comparisons require special care. Apply these claim strength rules:

| Finding | Appropriate Language |
|---------|---------------------|
| Boston's metric is higher/lower than SF/Seattle/DC | "Boston's [metric] is [X%] higher than [city]'s [metric] in [year]" |
| The difference may reflect a real performance gap | "This suggests Boston may have room to improve [metric]" |
| A peer city policy may explain their performance | "[City] implemented [policy] in [year]; their performance improved; this is consistent with — but does not prove — that the policy worked" |
| Boston should adopt a peer city's approach | "[City]'s experience provides preliminary evidence for [approach], though adaptation to Boston's context would be needed" |

**Never say:** "[City]'s policy caused their better performance" without quasi-experimental evidence.

---

### Phase 6: Benchmark Report Template

```markdown
## Cross-City Benchmark: [Metric] — Boston, San Francisco, Seattle, DC

### Summary Table

| City | [Metric] | Per Capita | Time Period | Data Source |
|------|----------|------------|-------------|-------------|
| Boston | [value] | [value/10K] | [period] | [dataset ID] |
| San Francisco | [value] | [value/10K] | [period] | [dataset ID] |
| Seattle | [value] | [value/10K] | [period] | [dataset ID] |
| Washington DC | [value] | [value/10K] | [period] | [dataset ID] |

### How Boston Compares
[1-2 sentences: Is Boston above, below, or in line with peers?
 Use hedged language appropriate to comparison confidence level.]

### What Might Explain Differences
[Structural factors, policy differences, data collection differences, confidence level for each]

### What Boston Can Learn
[Specific practices from peer cities worth investigating — labeled as "worth exploring," not "proven solutions"]

### Important Caveats
[Definition alignment, data quality, structural differences, time period alignment]

### Recommended Next Steps
[What additional investigation is needed before acting on these benchmarks]
```

---

## Performance Management Benchmarking

> **This module requires familiarity with `Performance_Management_Skill.md`.** Run that skill first to establish Boston's internal efficiency ratios, then use this module to compare against peer cities.

The most powerful benchmarking extends beyond service volume and response times to **efficiency ratios**: How much does it cost to deliver one unit of service? How much work is each employee carrying? Are peers getting better outcomes from the same investment?

### Why This Matters

A city can have fast response times *because* it's understaffing other services, or *because* it has invested more — or *because* it's measuring differently. Performance management benchmarking surfaces these distinctions and gives decision-makers evidence to defend or challenge investment levels.

### Step 1: Establish Boston's Baseline First

Before comparing across cities, complete a Boston-only analysis using `Performance_Management_Skill.md`:
- **Module 1:** Cost per outcome for the service domain
- **Module 2:** Workload per FTE
- **Module 3:** Budget vs. performance trend (multi-year)

Document and lock these numbers before searching peer city data:
- Boston's cost-per-resolved-case: $[X]
- Boston's workload per employee: [N] cases/year
- Boston's on-time rate: [X]%
- Fiscal year(s) used: FY[XX] (July 1 – June 30)

### Step 2: Search for Equivalent Financial Data in Peer Cities

Each city organizes budget and staffing data differently. Use this discovery sequence:

```
# San Francisco budget and payroll data
San Francisco Open Data:socrata__search_datasets("budget expenditure")
San Francisco Open Data:socrata__search_datasets("employee compensation payroll")
San Francisco Open Data:socrata__search_datasets("department salaries")

# Seattle budget and payroll data
Seattle Open Data:socrata__search_datasets("budget expenditure")
Seattle Open Data:socrata__search_datasets("payroll employee wages")
Seattle Open Data:socrata__search_datasets("city employee compensation")

# DC budget and payroll data
DC Open Data:arcgis__search_datasets("budget expenditure")
DC Open Data:arcgis__search_datasets("payroll compensation")
DC Open Data:arcgis__search_datasets("employee salaries")
```

Once datasets are found: confirm schema, identify the department/agency field, identify the spend/compensation fields, and align to the same calendar period used for Boston.

### Step 3: Search for Equivalent Service Outcome Data

```
# For 311/basic services benchmarking
San Francisco Open Data:socrata__search_datasets("311 cases service requests")
Seattle Open Data:socrata__search_datasets("service requests customer service 311")
DC Open Data:arcgis__search_datasets("311 service requests")
```

Confirm: does the peer city have an equivalent "on-time" or "resolved" field? If not, use total volume and median resolution time as proxies.

### Step 4: Compute Normalized Efficiency Ratios

For each city where both budget and outcome data are available:

```
cost_per_outcome         = department_annual_spend / resolved_cases_same_period
workload_per_employee    = total_service_volume / employee_headcount_proxy
overtime_rate            = total_overtime / total_gross_compensation
```

Apply the same fiscal-year alignment and service-scope boundaries used for Boston. Do not mix fiscal years across cities.

### Step 5: Performance Benchmark Template

```markdown
## Performance Benchmark: [Service Domain] — Boston vs. Peer Cities

### Efficiency Ratio Comparison

| City | Annual Spend | Resolved Cases | Cost/Case | Headcount Proxy | Workload/Employee | On-Time Rate |
|------|-------------|----------------|-----------|-----------------|-------------------|-------------|
| Boston | $[X] | [N] | $[X] | [N] | [N] | [X]% |
| San Francisco | $[X] | [N] | $[X] | [N] | [N] | [X]% |
| Seattle | $[X] | [N] | $[X] | [N] | [N] | [X]% |
| Washington DC | $[X] | [N] | $[X] | [N] | [N] | [X]% |

### Interpretation (with J-PAL Claim Strength)
[Label each finding: Descriptive / Correlational / Suggestive]

### Key Caveats Across Cities
- Budget definitions differ: some cities include fringe/benefits in department budgets; others budget separately
- Headcount proxies from payroll records ≠ authorized FTE positions in any city
- Service volumes reflect reported/recorded cases only — reporting rates differ across cities
- SF and Seattle labor costs are significantly higher than Boston; raw dollar comparisons without cost-of-living adjustment favor lower-cost cities
- DC handles city + state functions; its service scope may be broader than Boston's equivalent department
- Fiscal year definitions vary: Boston = July–June; verify each peer city's fiscal calendar before aligning

### What Boston Can Investigate Further
[Specific efficiency gaps worth deeper investigation — not conclusions]
```

### Performance Benchmarking Anti-Patterns

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| Raw cost comparison | SF/Seattle labor costs are higher; raw $/case favors Boston unfairly | Acknowledge cost-of-living gap; focus on trend direction, not absolute dollar comparison |
| Ignoring scope differences | DC handles city + state functions that Boston delegates to the Commonwealth | Note scope differences before comparing headcounts or budgets |
| Matching fiscal years carelessly | Misaligned FY periods mix different service volumes | Verify each city's fiscal year definition before computing ratios |
| Using headcount as proof | Payroll-based proxy ≠ authorized FTE in any city | Always caveat as headcount proxy |
| Single-year conclusions | One year of efficiency ratios may reflect anomaly (COVID, crisis, vacancy spike) | Use at least 2–3 years for trend claims |

---

## Domain-Specific Benchmarking Guides

### 311 / City Services
- **Boston:** `311-service-requests` — `on_time` field, `neighborhood` field
- **San Francisco:** `San Francisco Open Data:socrata__search_datasets("311 cases")` — SF311 case data
- **Seattle:** `Seattle Open Data:socrata__search_datasets("customer service requests")` — Seattle has active service request data
- **DC:** `DC Open Data:arcgis__search_datasets("311 service requests")` — DC311 (DCii) data
- **Key metrics:** On-time rate, median resolution time, per-capita volume
- **Key caveat:** SLA definitions differ by city and by service type. "On-time" means different things when SLA targets differ.

### Building Permits / Housing Development
- **Boston:** `approved-building-permits`
- **San Francisco:** `San Francisco Open Data:socrata__search_datasets("building permits")`
- **Seattle:** `Seattle Open Data:socrata__search_datasets("building permits")`
- **DC:** `DC Open Data:arcgis__search_datasets("building permits")`
- **Key metrics:** Permits issued per 1,000 housing units, median approval time, residential vs. commercial mix
- **Key caveat:** Permitting processes differ significantly; some cities require more permits for the same scope of work. SF's permitting complexity is among the highest in the US.

### Public Safety
- **Boston:** `crime-incident-reports`
- **San Francisco:** `San Francisco Open Data:socrata__search_datasets("police incidents crime")`
- **Seattle:** `Seattle Open Data:socrata__search_datasets("crime data police reports")`
- **DC:** `DC Open Data:arcgis__search_datasets("crime incidents")`
- **Key metrics:** Incident rate per 10,000 residents by category
- **Key caveat:** Reporting practices and offense classifications vary significantly across cities. Among the least comparable metrics. UCR vs. NIBRS transitions affect year-over-year comparability within cities.

### Transportation / Mobility
- **Boston:** `blue-bikes-system-data`
- **San Francisco:** `San Francisco Open Data:socrata__search_datasets("bay wheels bike share")`
- **Seattle:** `Seattle Open Data:socrata__search_datasets("bike share")`
- **DC:** `DC Open Data:arcgis__search_datasets("capital bikeshare")`

---

## Contextual Enrichment: Beyond the Data

Data comparison is stronger when paired with qualitative context. For any benchmarking finding:

1. **Search for what the peer city did** — Use web search to find news, policy documents, or case studies explaining the peer city's performance
2. **Check What Works Cities** (Bloomberg Philanthropies) and **Results for America** for documented case studies
3. **Look for cost data** — Performance differences may reflect investment differences, not just approach differences
4. **Contact peer city practitioners** — Data benchmarks are a starting point for peer learning, not a conclusion

---

## Benchmarking Anti-Patterns

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| **Metric shopping** | Picking comparisons that make Boston look good or bad | Define metrics before looking at results |
| **Definition mismatch** | Comparing metrics that sound the same but measure differently | Always document what each city actually measures |
| **Scale blindness** | Comparing without adjusting for population or cost of living | Normalize by population; note labor cost differences for financial comparisons |
| **Attribution error** | "[City] is better because of policy X" | Label as correlation; require additional investigation for causation |
| **Cherry-picked peers** | Only choosing cities that support a preferred narrative | SF, Seattle, and DC were chosen for structural comparability, not outcomes |
| **Data quality gap** | One city's data is much more complete than another's | Note data quality differences; they may explain apparent performance gaps |
| **Cost-of-living blindness** | Comparing raw cost-per-outcome without adjusting for labor costs | Flag that SF/Seattle labor costs are significantly higher than Boston |

---

## J-PAL Core Values Applied to Benchmarking
1. **Comparability first** — Ensure you're measuring the same thing before comparing
2. **Structural humility** — Cities differ; differences in outcomes don't automatically mean differences in effort or competence
3. **Peer learning framing** — Benchmarks should inspire and inform, not shame
4. **Action orientation** — Every benchmark finding should connect to a specific investigable hypothesis for Boston
5. **Transparency** — Show which city's data you used, how you normalized, and what you couldn't account for
