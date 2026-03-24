# Boston Open Data MCP — Evaluation Plan

## Purpose

This eval tests whether the Boston Open Data MCP server returns accurate, complete, and structurally correct data across multiple domains. The assertions are designed to be **durable** — grounded in historical facts, structural constants, or slow-changing civic data that should remain true for years.

The eval is organized into **four levels of increasing rigor**, from simple structural checks to complex multi-step analytical queries that require joining data across datasets.

---

## Level 1: Structural & Schema Assertions

These test whether the MCP can discover datasets, retrieve schemas, and return correctly shaped data. They require no domain knowledge — just that the plumbing works.

| ID | Prompt | Tool Sequence | Assertions |
|----|--------|---------------|------------|
| 1.1 | "Search for 311 datasets in Boston" | `search_datasets("311")` | Returns ≥4 results; results include dataset IDs for 2022, 2023, 2024 |
| 1.2 | "Get the schema for Boston 311 2023 data" | `get_schema("e6013a93-1321-4f2a-bf91-8d8a02f1e62f")` | Returns ≥20 fields; includes fields: `case_enquiry_id`, `open_dt`, `type`, `neighborhood`, `on_time`, `source`, `department` |
| 1.3 | "Search for crime incident data in Boston" | `search_datasets("crime incident reports")` | Returns ≥1 result; includes dataset ID `6220d948-eae2-4e4b-8723-2dc8e67722a3` |
| 1.4 | "Get the schema for Boston crime reports 2023-present" | `get_schema("b973d8cb-eeb2-4e7e-99da-c92938efc9c0")` | Returns fields including: `INCIDENT_NUMBER`, `OFFENSE_DESCRIPTION`, `DISTRICT`, `SHOOTING`, `YEAR`, `DAY_OF_WEEK` |
| 1.5 | "Search for population data in Boston" | `search_datasets("population")` | Returns ≥1 result; includes neighborhood-level population dataset |
| 1.6 | "Get the schema for Blue Bike Stations" | `get_schema("44c931aa-4577-4caa-a8f2-ed1e38263e70")` | Returns fields including: `Name`, `Latitude`, `Longitude`, `District`, `Total_docks` |
| 1.7 | "Search for building permits in Boston" | `search_datasets("building permits")` | Returns ≥1 result; result includes "Approved Building Permits" dataset |
| 1.8 | "Get the schema for approved building permits" | `get_schema("6ddcd912-32a0-43df-9908-63574f8c7e77")` | Returns fields including: `permitnumber`, `worktype`, `permittypedescr`, `issued_date`, `status`, `address`, `zip` |

---

## Level 2: Single-Dataset Fact Retrieval

These test whether the MCP returns correct counts, distinct values, and rankings from individual datasets. Ground truth was collected directly from the portal on March 30, 2026.

### 2A: Neighborhoods & Population (very stable — changes only with Census updates)

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.1 | "How many neighborhoods does Boston have in its population dataset?" | **24** (including Harbor Islands) | Exact | Boston's neighborhood boundaries are defined by city policy; unchanged since 2010+ |
| 2.2 | "What is the most populous neighborhood in Boston?" | **Dorchester** (~123,056) | Exact on name; ±5% on population | Dorchester has been Boston's largest neighborhood for decades |
| 2.3 | "What is the least populous neighborhood in Boston (excluding Harbor Islands)?" | **Longwood** (~5,553) | Exact on name; ±10% on population | Longwood is a medical/academic campus with minimal residential |
| 2.4 | "What is the total population of Boston according to the neighborhood population dataset?" | **~716,313** (sum of all 23 non-Harbor Islands neighborhoods) | ±3% | Based on 2025 city estimates; will shift slowly with ACS updates |
| 2.5 | "What are the 5 most populous neighborhoods in Boston?" | **Dorchester, Roxbury, Brighton, East Boston, Jamaica Plain** (in roughly that order) | Top 3 must be exact; 4-5 may swap | Dorchester and Roxbury have been #1-2 for decades |

### 2B: 311 Service Requests — 2023 (historical, immutable)

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.6 | "How many total 311 cases were filed in Boston in 2023?" | **266,438** | Exact (±0) | Historical dataset; records are final |
| 2.7 | "How many Missed Trash cases were there in Boston 311 in 2023?" | **12,587** | Exact | Filter: `type = 'Missed Trash/Recycling/Yard Waste/Bulk Item'` |
| 2.8 | "What was the most common 311 request type in Boston in 2023?" | **Parking Enforcement** (56,941 cases) | Exact on type name; ±1% on count | Parking Enforcement has been the top category for years |
| 2.9 | "Which neighborhood had the most 311 cases in Boston in 2023?" | **Dorchester** (36,244 cases) | Exact on name; ±1% on count | Dorchester is largest by pop and area |
| 2.10 | "How many distinct 311 request types exist in the 2023 dataset?" | **172** | Exact | `COUNT(DISTINCT type)` |
| 2.11 | "How many distinct departments handled 311 cases in 2023?" | **18** | Exact | `COUNT(DISTINCT department)` |
| 2.12 | "What percentage of Boston 311 cases in 2023 were resolved on time?" | **~77.3%** (206,054 ONTIME out of 266,438) | ±1 percentage point | 206,054 ONTIME / 266,438 total |
| 2.13 | "How many 311 source channels exist in the 2023 data?" | **6** | Exact | Citizens Connect App, Constituent Call, City Worker App, Self Service, Employee Generated, Maximo Integration |
| 2.14 | "What was the most common 311 source channel in 2023?" | **Citizens Connect App** (141,420 cases) | Exact on name | App has been top source since its launch |
| 2.15 | "Which department handled the most 311 cases in 2023?" | **PWDx** (127,398 cases) | Exact on name; ±1% on count | Public Works is consistently #1 |

### 2C: 311 Service Requests — 2024 (historical, immutable)

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.16 | "How many total 311 cases were there in Boston in 2024?" | **282,836** | Exact | Historical dataset; records are final |

### 2D: Crime Incident Reports — 2023 (historical, immutable)

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.17 | "How many crime incidents were reported in Boston in 2023?" | **78,046** | Exact | Historical year; records are final |
| 2.18 | "How many shooting incidents were reported in Boston in 2023?" | **636** | Exact | `SHOOTING = '1'` and `YEAR = '2023'` |
| 2.19 | "How many BPD police districts reported crime in 2023?" | **14** (excluding blank) | Exact | A1, A15, A7, B2, B3, C11, C6, D4, D14, E5, E13, E18, External |
| 2.20 | "Which police district had the most crime incidents in 2023?" | **D4** (10,408) | Exact on district; ±2% on count | D4 (South End/Back Bay) is consistently high |
| 2.21 | "What was the most common offense type in 2023 crime data?" | **INVESTIGATE PERSON** (7,004) | Exact on name; ±2% on count | |

### 2E: Crime Incident Reports — 2024

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.22 | "How many shooting incidents were reported in Boston in 2024?" | **485** | Exact | Historical, immutable year |

### 2F: Blue Bike Stations (slowly changing — updated nightly but count is stable)

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.23 | "How many Blue Bike stations are in Boston proper?" | **324** (District = 'Boston') | ±10% | Stations grow slowly; 324 as of March 2026 |
| 2.24 | "How many total Blue Bike stations are in the system?" | **572** | ±10% | Includes Cambridge, Somerville, etc. |
| 2.25 | "How many municipalities have Blue Bike stations?" | **13** | ±2 | Slow expansion into neighboring towns |
| 2.26 | "How many total docks are at Boston Blue Bike stations?" | **~5,945** | ±15% | Docks fluctuate more than station count |

### 2G: Building Permits — 2023 (historical, immutable)

| ID | Prompt | Expected Answer | Tolerance | Rationale |
|----|--------|-----------------|-----------|-----------|
| 2.27 | "How many building permits were issued in Boston in 2023?" | **42,075** | Exact | Historical year |
| 2.28 | "How many distinct permit types exist in the Boston building permits dataset?" | **13** | Exact | Fixed by ISD policy |
| 2.29 | "What was the most common permit type issued in Boston in 2023?" | **Short Form Bldg Permit** (12,362) | Exact on name; ±2% on count | Short Form has been #1 for years |

---

## Level 3: Multi-Step & Cross-Field Queries

These require the MCP user to chain multiple operations — filtering, grouping, sorting, and interpreting results.

| ID | Prompt | Required Steps | Expected Answer | Tolerance |
|----|--------|----------------|-----------------|-----------|
| 3.1 | "What were the top 5 311 request types in Boston in 2023 and how many cases did each have?" | Aggregate by `type`, order DESC, limit 5 | 1. Parking Enforcement: 56,941 — 2. Requests for Street Cleaning: 22,193 — 3. Improper Storage of Trash: 19,479 — 4. CE Collection: 16,188 — 5. Missed Trash: 12,587 | Names exact; counts ±1% |
| 3.2 | "Which 5 neighborhoods had the most 311 cases in 2023?" | Aggregate by `neighborhood`, order DESC, limit 5 | 1. Dorchester: 36,244 — 2. Roxbury: 23,186 — 3. South Boston / South Boston Waterfront: 22,385 — 4. Allston / Brighton: 19,870 — 5. East Boston: 19,065 | Names exact; counts ±1% |
| 3.3 | "How many 311 cases by source channel in 2023, ranked?" | Aggregate by `source`, order DESC | Citizens Connect App: 141,420 — Constituent Call: 87,146 — City Worker App: 30,854 — Self Service: 3,569 — Employee Generated: 3,447 — Maximo Integration: 2 | Ranking and names exact |
| 3.4 | "Which BPD districts had the most crime in 2023? Top 5." | Aggregate by `DISTRICT`, year filter, order DESC | D4: 10,408 — B2: 10,336 — A1: 9,555 — C11: 9,073 — B3: 7,903 | Names exact; counts ±2% |
| 3.5 | "How many building permits of each type were issued in 2023?" | Filter by year, aggregate by `permittypedescr` | 13 types returned; top is Short Form Bldg Permit at 12,362 | All 13 types present; top 3 names exact |

---

## Level 4: Cross-Dataset & Analytical Queries

These require navigating multiple datasets, joining concepts, and performing calculations that stress the MCP's tool-chaining capabilities.

| ID | Prompt | Required Steps | Expected Answer | Tolerance |
|----|--------|----------------|-----------------|-----------|
| 4.1 | "What is Dorchester's population and how many 311 cases did it have in 2023? What's the per-capita rate?" | Query population dataset for Dorchester (~123,056); query 311 2023 for Dorchester (36,244); calculate rate | ~294.5 cases per 1,000 residents | ±10% on rate |
| 4.2 | "Compare 311 volume between 2023 and 2024 — did it go up or down?" | Aggregate total from both datasets | 2023: 266,438 vs 2024: 282,836 → **Up by ~6.2%** | Direction must be correct; percentage ±1pp |
| 4.3 | "How did shooting incidents change from 2023 to 2024?" | Query crime data for both years | 2023: 636 → 2024: 485 → **Down by ~23.7%** | Direction must be correct; percentage ±2pp |
| 4.4 | "What are the top 3 neighborhoods by population, and which police districts cover them?" | Query population for top 3 (Dorchester, Roxbury, Brighton); query crime data to identify districts covering those areas | Dorchester → C11/B3; Roxbury → B2; Brighton → D14 | Neighborhood names exact; district mapping approximately correct |
| 4.5 | "For Dorchester, what was the 311 on-time rate in 2023 and how does it compare to the citywide average?" | Filter 311 2023 for Dorchester, calculate on-time%; compare to citywide 77.3% | Dorchester on-time % should be calculable; comparison to 77.3% citywide | Citywide rate ±1pp; Dorchester ±3pp |

---

## Ground Truth Reference Table

All values collected from Boston Open Data portal on **March 30, 2026**.

| Fact | Value | Source Resource ID | Stability |
|------|-------|-------------------|-----------|
| Number of neighborhoods | 24 | `b0543358-...` | Permanent |
| Most populous neighborhood | Dorchester (~123K) | `b0543358-...` | Very stable |
| Least populous (excl. Harbor Islands) | Longwood (~5.5K) | `b0543358-...` | Very stable |
| Total city population (neighborhood sum) | ~716,313 | `b0543358-...` | Stable (updates with Census/ACS) |
| 311 total cases 2023 | 266,438 | `e6013a93-...` | Immutable |
| 311 total cases 2024 | 282,836 | `dff4d804-...` | Immutable |
| 311 Missed Trash cases 2023 | 12,587 | `e6013a93-...` | Immutable |
| 311 top type 2023 | Parking Enforcement (56,941) | `e6013a93-...` | Immutable |
| 311 top neighborhood 2023 | Dorchester (36,244) | `e6013a93-...` | Immutable |
| 311 distinct types 2023 | 172 | `e6013a93-...` | Immutable |
| 311 distinct departments 2023 | 18 | `e6013a93-...` | Immutable |
| 311 on-time rate 2023 | 77.3% (206,054 / 266,438) | `e6013a93-...` | Immutable |
| 311 source channels 2023 | 6 | `e6013a93-...` | Immutable |
| 311 top source 2023 | Citizens Connect App (141,420) | `e6013a93-...` | Immutable |
| 311 top department 2023 | PWDx (127,398) | `e6013a93-...` | Immutable |
| Crime incidents 2023 | 78,046 | `b973d8cb-...` | Immutable |
| Shooting incidents 2023 | 636 | `b973d8cb-...` | Immutable |
| Shooting incidents 2024 | 485 | `b973d8cb-...` | Immutable |
| BPD districts (non-blank) 2023 | 14 | `b973d8cb-...` | Immutable |
| Top crime district 2023 | D4 (10,408) | `b973d8cb-...` | Immutable |
| Top offense 2023 | INVESTIGATE PERSON (7,004) | `b973d8cb-...` | Immutable |
| Blue Bike stations (Boston) | 324 | `44c931aa-...` | Slowly growing |
| Blue Bike stations (all) | 572 | `44c931aa-...` | Slowly growing |
| Blue Bike municipalities | 13 | `44c931aa-...` | Slowly growing |
| Blue Bike docks (Boston) | ~5,945 | `44c931aa-...` | Slowly growing |
| Building permits 2023 | 42,075 | `6ddcd912-...` | Immutable |
| Permit types | 13 | `6ddcd912-...` | Stable (set by policy) |
| Top permit type 2023 | Short Form Bldg Permit (12,362) | `6ddcd912-...` | Immutable |

---

## Scoring Methodology

Each assertion is scored **pass/fail** with the tolerances noted above. Overall scores:

- **Level 1 (Structure)**: All-or-nothing per assertion. Target: 100%.
- **Level 2 (Facts)**: Exact match where tolerance = Exact; within range otherwise. Target: ≥95%.
- **Level 3 (Multi-step)**: Ranked by correctness of names, counts, and ordering. Target: ≥90%.
- **Level 4 (Cross-dataset)**: Directional correctness required; quantitative within tolerance. Target: ≥80%.

### Aggregate Score
```
Overall = (0.15 × L1) + (0.35 × L2) + (0.30 × L3) + (0.20 × L4)
```

Level 2 and 3 get the most weight because they test the core use case — accurate data retrieval. Level 4 gets less weight because cross-dataset queries are harder and more sensitive to interpretation.

---

## Stability Classification

| Class | Definition | Examples | Re-verification Cadence |
|-------|-----------|----------|------------------------|
| **Immutable** | Historical data that will never change | 311 2023 counts, Crime 2023 counts | Never |
| **Very Stable** | Changes only with major policy/Census updates | Neighborhood count, largest neighborhood | Annually |
| **Stable** | Changes slowly over time | Population totals, permit type list | Every 6 months |
| **Slowly Growing** | Increases gradually | Blue Bike station count | Quarterly |

---

## How to Run

1. **Manual**: Ask each prompt to a Claude session with the Boston Open Data MCP connected. Compare the response against the expected answer and tolerance.
2. **Automated (evals.json)**: Use the companion `evals.json` file with the skill-creator eval framework. Each eval specifies the prompt, expected output, and machine-checkable expectations.
3. **Regression**: Re-run after MCP server updates, schema changes, or data refreshes. Focus on Level 1 (structural) first — if schemas break, everything else will too.

---

## Next Steps

- [ ] Convert this plan into `evals.json` format for automated grading
- [ ] Add Level 5: Natural language questions that require the MCP to infer which dataset to use (e.g., "Is Boston getting safer?" requires choosing crime data and comparing years)
- [ ] Add cross-city benchmarking evals using Pittsburgh and San Jose MCP servers
- [ ] Add edge-case evals: empty results, malformed filters, nonexistent neighborhoods
- [ ] Add latency/performance benchmarks
