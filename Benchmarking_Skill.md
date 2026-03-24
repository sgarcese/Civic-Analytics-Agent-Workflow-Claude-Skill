---
name: city-benchmarking
description: "Use this skill whenever the user wants to compare Boston's performance to peer cities, identify best practices from other municipalities, understand how Boston ranks on any metric, or find evidence that a policy approach has worked elsewhere. ALWAYS use when the user mentions 'other cities', 'how does Boston compare', 'best practices', 'peer cities', 'what works', 'benchmarks', 'lessons from elsewhere', or asks whether Boston is doing better or worse than comparable municipalities. This skill uses Boston Open Data MCP (primary), Pittsburgh Open Data MCP, and San Jose Open Data MCP to conduct rigorous cross-city comparisons. Applies J-PAL caution to comparative claims and Bloomberg framing to translate benchmarks into actionable city policy recommendations."
---

# City Benchmarking — Cross-City Comparative Analysis

## Purpose and Power

Benchmarking against peer cities is one of the most persuasive forms of evidence in city policy. It answers the question that every skeptic asks: *"Is this actually a problem, or is every city like this?"* Used carefully, cross-city comparison can:
- Validate that a problem is real and solvable (not just inherent to city life)
- Identify specific practices from peer cities worth adapting
- Set realistic performance targets grounded in what's achievable
- Build the case for investment by showing what's possible

**Used carelessly, it misleads.** Cities differ in geography, demographics, data systems, and reporting practices. Apply J-PAL rigor to every comparative claim.

---

## Available Data Sources

### Primary: Boston Open Data
- MCP: `Boston Open Data MCP`
- Portal: https://data.boston.gov
- Use for: Boston's baseline metrics across all domains

### Peer City 1: Pittsburgh, PA
- MCP: `PGH Open Data MCP`
- Portal: https://data.wprdc.org
- City profile: ~300K residents; older Rust Belt city; strong university sector; significant neighborhood equity challenges; comparable housing stock
- **Comparability strengths:** Similar scale, legacy infrastructure, equity challenges, mid-Atlantic climate
- **Comparability cautions:** Different political structure (strong mayor vs. weak mayor), different union contracts, different demographic mix

### Peer City 2: San Jose, CA
- MCP: `San Jose MCP`
- Portal: https://data.sanjoseca.gov
- City profile: ~1M residents; tech-driven economy; Silicon Valley context; significant immigrant populations; very different housing market
- **Comparability strengths:** Large diverse city with strong data infrastructure; often a leader in civic tech and innovation
- **Comparability cautions:** Very different cost structure, tech-sector wealth effects, California regulatory environment, much larger scale

---

## MCP Tool Reference — All Three Cities

### Boston (boston-open-data-mcp)
```
search_datasets(query)                      → Find datasets by topic
get_dataset_info(dataset_id)                → Get resource IDs and metadata
get_datastore_schema(resource_id)           → Get exact field names
query_datastore(resource_id, filters={},    → Retrieve records
  sort="", limit=100, date_range={})
```

### Pittsburgh (PGH Open Data MCP)
```
PGH Open Data MCP:ckan__search_datasets(query)
PGH Open Data MCP:ckan__get_dataset(dataset_id)
PGH Open Data MCP:ckan__get_schema(resource_id)
PGH Open Data MCP:ckan__query_data(resource_id, ...)
PGH Open Data MCP:ckan__aggregate_data(resource_id, ...)
```

### San Jose (San Jose MCP)
```
San Jose MCP:ckan__search_datasets(query)
San Jose MCP:ckan__get_dataset(dataset_id)
San Jose MCP:ckan__get_schema(resource_id)
San Jose MCP:ckan__query_data(resource_id, ...)
San Jose MCP:ckan__aggregate_data(resource_id, ...)
```

**⚠️ ALWAYS call get_schema/get_datastore_schema before any query. Field names differ across cities and across datasets within the same city.**

---

## Benchmarking Workflow

### Phase 1: Establish the Comparison Question

Before querying any city's data, define:

1. **What metric** are we comparing? (Be precise — response time? Per-capita complaints? Permit approval rate?)
2. **Are these metrics comparable?** Do both cities measure and report the same thing?
3. **What denominator** makes the comparison fair? (Per capita? Per lane-mile? Per housing unit?)
4. **What time period** aligns across cities?
5. **What structural differences** must we account for?

**Comparability Assessment Template:**
```
Metric being compared: [X]
Boston measures it as: [field name, definition]
Pittsburgh measures it as: [field name, definition]
San Jose measures it as: [field name, definition]
Are definitions equivalent? [Yes / Approximately / No — explain]
Recommended denominator: [why]
Known structural differences: [list]
Confidence in comparison: [High / Moderate / Low]
```

---

### Phase 2: Data Discovery Across Cities

Run parallel searches across all three cities:

```
# Boston
search_datasets("[topic]")  →  Boston Open Data

# Pittsburgh
PGH Open Data MCP:ckan__search_datasets("[topic]")

# San Jose
San Jose MCP:ckan__search_datasets("[topic]")
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
query_datastore("dff4d804-...", limit=5)  → note total_count
# [cross-reference with Boston population for per-capita rate]

# Pittsburgh — search for equivalent 311/service request data
PGH Open Data MCP:ckan__search_datasets("311 service requests")
PGH Open Data MCP:ckan__get_schema(resource_id)
PGH Open Data MCP:ckan__query_data(resource_id, limit=5)  → note equivalent metrics

# San Jose — search for equivalent data
San Jose MCP:ckan__search_datasets("service requests")
San Jose MCP:ckan__get_schema(resource_id)
San Jose MCP:ckan__query_data(resource_id, limit=5)  → note equivalent metrics
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

**City Population Reference:**
- Boston: ~675,000 (2025 Planning Department estimates)
- Pittsburgh: ~300,000
- San Jose: ~1,000,000

---

### Phase 5: Interpret with J-PAL Rigor

Cross-city comparisons require special care. Apply these claim strength rules:

| Finding | Appropriate Language |
|---------|---------------------|
| Boston's metric is higher/lower than Pittsburgh's | "Boston's [metric] is [X%] higher than Pittsburgh's [metric] in [year]" |
| The difference may reflect a real performance gap | "This suggests Boston may have room to improve [metric]" |
| A Pittsburgh policy may explain their performance | "Pittsburgh implemented [policy] in [year]; their performance improved; this is consistent with — but does not prove — that the policy worked" |
| Boston should adopt Pittsburgh's approach | "Pittsburgh's experience provides preliminary evidence for [approach], though adaptation to Boston's context would be needed" |

**Never say:** "Pittsburgh's policy caused their better performance" without quasi-experimental evidence.

---

### Phase 6: Benchmark Report Template

```markdown
## Cross-City Benchmark: [Metric] — Boston, Pittsburgh, San Jose

### Summary Table

| City | [Metric] | Per Capita | Time Period | Data Source |
|------|----------|------------|-------------|-------------|
| Boston | [value] | [value/10K] | [period] | [dataset ID] |
| Pittsburgh | [value] | [value/10K] | [period] | [dataset ID] |
| San Jose | [value] | [value/10K] | [period] | [dataset ID] |

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

## Domain-Specific Benchmarking Guides

### 311 / City Services
- **Boston:** `311-service-requests` — on_time field, neighborhood field
- **Pittsburgh:** Search `ckan__search_datasets("service requests")` or `311`
- **San Jose:** Search `ckan__search_datasets("service requests")` or `constituent services`
- **Key metrics:** On-time rate, median resolution time, per-capita volume
- **Key caveat:** SLA definitions differ by city. On-time means different things if SLA targets differ.

### Building Permits / Housing Development
- **Boston:** `approved-building-permits`
- **Pittsburgh:** Search `permits` or `building permits`
- **San Jose:** Search `permits` — San Jose has active building permit data
- **Key metrics:** Permits issued per 1,000 housing units, approval time, residential vs. commercial mix
- **Key caveat:** Permitting processes differ significantly; some cities require more permits for the same work

### Public Safety
- **Boston:** `crime-incident-reports`
- **Pittsburgh:** Search `crime` or `police incidents`
- **San Jose:** Search `crime` or `police reports`
- **Key metrics:** Incident rate per 10,000 residents by category, clearance rates
- **Key caveat:** Reporting practices vary significantly. UCR vs. NIBRS differences. These are among the least comparable metrics across cities.

### Transportation / Mobility
- **Boston:** `blue-bikes-system-data`
- **Pittsburgh:** Search `bike share` or `transit`
- **San Jose:** Search `bikeshare` or `transportation`

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
| **Scale blindness** | Comparing Boston (675K) to San Jose (1M) without adjusting | Normalize by population or relevant denominator |
| **Attribution error** | "San Jose is better because of policy X" | Label as correlation; require additional investigation for causation |
| **Cherry-picked peers** | Only choosing cities that support a preferred narrative | Choose comparison cities based on structural similarity, not outcomes |
| **Data quality gap** | One city's data is much more complete than another's | Note data quality differences; they may explain apparent performance gaps |

---

## J-PAL Core Values Applied to Benchmarking
1. **Comparability first** — Ensure you're measuring the same thing before comparing
2. **Structural humility** — Cities differ; differences in outcomes don't automatically mean differences in effort or competence
3. **Peer learning framing** — Benchmarks should inspire and inform, not shame
4. **Action orientation** — Every benchmark finding should connect to a specific investigable hypothesis for Boston
5. **Transparency** — Show which city's data you used, how you normalized, and what you couldn't account for
