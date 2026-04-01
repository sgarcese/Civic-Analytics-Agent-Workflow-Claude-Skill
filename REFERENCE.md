---
name: boston-data-reference
description: "Quick reference for Boston Open Data datasets, field names, schema differences, and cross-referencing strategies. Also includes San Francisco, Seattle, and DC dataset discovery guides for benchmarking. Consult before any analysis to identify available data and plan cross-dataset or cross-city joins."
---

# Boston Open Data — Quick Reference Guide

## Dataset Directory by Domain

### City Services & 311

| Resource / Year | Resource ID | Notes |
|-----------------|-------------|-------|
| 311 — NEW SYSTEM (Oct 2025+) | `254adca6-64ab-4c5c-9fc0-a6da622be185` | New schema |
| 311 — 2025 Legacy (Jan–Oct 2025) | `9d7c2214-4709-478a-a2e8-fb2020a5bb94` | Old schema |
| 311 — 2024 | `dff4d804-5031-443a-8409-8344efd0e5c8` | Old schema |
| 311 — 2023 | `e6013a93-1321-4f2a-bf91-8d8a02f1e62f` | Old schema |
| 311 — 2022 | `81a7b022-f8fc-4da5-80e4-b160058ca207` | Old schema |

### ⚠️ Critical: 311 Schema Change (October 2025)

| Concept | Legacy (2011–Oct 2025) | New System (Oct 2025+) |
|---------|----------------------|----------------------|
| Case ID | `case_enquiry_id` | `case_id` |
| Open date | `open_dt` | `open_date` |
| Close date | `closed_dt` | `close_date` |
| SLA target | `sla_target_dt` | `target_close_date` |
| Service type | `type` (+ `subject`, `reason`) | `service_name` (+ `case_topic`) |
| Department | `department` | `assigned_department` |
| Queue | `queue` | `assigned_team` |
| Location | `location` | `full_address` |
| Street | `location_street_name` | `street_name` |
| ZIP | `location_zipcode` | `zip_code` |
| Source channel | `source` | `report_source` |
| Neighborhood | `neighborhood` | `neighborhood` ✓ same |
| On-time | `on_time` | `on_time` ✓ same |
| Case status | `case_status` | `case_status` ✓ same |
| PWD district | `pwd_district` | `public_works_district` |
| Council district | `city_council_district` | `city_council_district` ✓ same |
| Lat/Long | `latitude`, `longitude` | `latitude`, `longitude` ✓ same |

**When analyzing across the transition: query each system separately and reconcile.**

---

### Demographics & Population

| Dataset | Resource ID | Notes |
|---------|-------------|-------|
| Population — Neighborhood (2025) | `b0543358-d03f-4682-bf0c-658ea4573d6f` | 24 neighborhoods, 259 fields |
| Population — City Level | Get via `get_dataset_info("2025-boston-population-estimates-city-level")` | Citywide totals |
| Population — Census Tract | Get via `get_dataset_info("2025-boston-population-estimates-census-tract-level")` | Finest granularity |
| Digital Equity Survey | Get via `get_dataset_info("2024-digital-equity-survey")` | Digital access data |

### Performance Management

#### Budget & Staffing Datasets

| Dataset | Resource ID | Coverage | Use |
|---------|-------------|----------|-----|
| Operating Budget (FY26) | `8f2971f0-7a0d-401d-8376-0289e3b810ba` | FY23 actual, FY24 actual, FY25 appropriation, FY26 budget | Planned/appropriated budget by Cabinet → Department → Program → Expense Category |
| Employee Earnings Report | `29b3544f-752a-4cb1-a6af-a1de153d20a0` | Annual (multiple CSVs per year) | Per-employee comp, overtime, headcount proxy — filter by `DEPARTMENT_NAME` |
| Checkbook Explorer | `d22fdd5c-7e4c-41b7-a3eb-dfc57a87b245` | FY2011–present, updated regularly | Actual transaction-level spending by dept/account — use for actuals, not appropriations |
| Discretionary Spending | `e44079a8-b64d-4aa8-8570-6a69398892c3` | FY2019–FY2026 | Vendor/supplier spend by dept — useful for procurement efficiency |
| CityScore (Full Metrics) | `dd657c02-3443-4c00-8b29-56a40cfe7ee4` | Real-time, timestamped | KPI scores with numerator/denominator — **fallback only** when no direct operational dataset exists |

#### Service Domain → Performance Data Map

| Service Domain | Primary Data Source | Dataset ID / Search Term |
|---------------|--------------------|-----------------------|
| Basic services: trash, recycling, streetlights, rodents, potholes, graffiti, abandoned vehicles | 311 (filter by `type` or `service_name`) | See 311 resource IDs in City Services section above |
| Code enforcement | Code violations | `search_datasets("code violations")` — confirm resource ID before querying |
| Permitting | Building permits | `approved-building-permits` |
| Public safety | 911 dispatch + Crime incidents | `911-daily-dispatch-count-by-agency`, `crime-incident-reports` |
| All other services | CityScore (fallback) | `dd657c02-3443-4c00-8b29-56a40cfe7ee4` |

#### Performance Management Notes

- **Boston Fiscal Year:** July 1 – June 30. FY26 = July 1, 2025 – June 30, 2026.
- **Budget vs. Actuals:** Operating Budget = appropriated/planned. Checkbook Explorer = actual transactions drawn. Always use the right source for the right question.
- **Headcount proxy:** Employee Earnings counts payroll entries in a year — not authorized FTE positions. Includes part-year, part-time, transitional employees. Always caveat.
- **Department name consistency warning:** Department names vary across datasets. "Public Works" may appear as "PWD", "Dept of Public Works", or similar. Always verify the exact string via schema before filtering.
- **Overtime threshold:** Overtime > 15% of total gross compensation is a staffing stress flag for further investigation, not proof of understaffing.

---

### Housing & Development

| Dataset ID | Description |
|-----------|-------------|
| `approved-building-permits` | Building permit approvals with address, type, value |
| `article80-development-projects` | Major development projects requiring Article 80 review |
| `assessing-online` | Property assessments |
| `zoning-reform-impact-tracker` | Zoning reform tracking |
| `zoning-board-of-appeal-tracker` | ZBA decisions |

### Public Safety

| Dataset ID | Description |
|-----------|-------------|
| `crime-incident-reports` | BPD incident reports (2015+) |
| `911-daily-dispatch-count-by-agency` | Daily dispatch volumes |
| `boston-police-department-fio` | Field Interrogation/Observation records |
| `boston-police-department-firearm-recovery-counts` | Firearm recovery stats |

### Transportation & Mobility

| Dataset ID | Description |
|-----------|-------------|
| `blue-bikes-system-data` | Bike share trip data |
| `blue-bike-stations` | Station locations |
| `boston-bicycle-network-2023` | Bike infrastructure GIS |

### Environment & Climate

| Dataset ID | Description |
|-----------|-------------|
| `berdo-reporting-2019` (and later years) | Building energy reporting |
| `2016-land-cover` | Land use data |
| Multiple sea-level-rise datasets | Climate projections (9", 21", 36" scenarios) |

---

## Boston 24 Neighborhoods Reference

| Neighborhood | Est. Population | Key Characteristics |
|-------------|----------------|---------------------|
| Allston | ~34K | Student-heavy, dense rental |
| Back Bay | ~19K | High-income, historic, commercial |
| Beacon Hill | ~10K | Historic, residential, state government |
| Brighton | ~46K | Residential, student population |
| Charlestown | ~18K | Mixed-income, waterfront, historic |
| Chinatown | ~14K | Immigrant community, dense, transit-adjacent |
| Dorchester | ~83K | Largest neighborhood, diverse, multiple sub-areas |
| Downtown | ~11K | Commercial core, growing residential |
| East Boston | ~44K | Immigrant communities, airport-adjacent, gentrifying |
| Fenway | ~39K | Institutional (hospitals, universities), young |
| Hyde Park | ~33K | Residential, suburban feel, southern border |
| Jamaica Plain | ~38K | Mixed-income, arts community, diverse |
| Longwood | ~2K | Medical/academic campus |
| Mattapan | ~28K | Predominantly Black community, residential |
| Mission Hill | ~17K | Mixed student/family, medical adjacent |
| North End | ~11K | Historic, Italian-American, tourist destination |
| Roslindale | ~33K | Residential, growing diversity, village center |
| Roxbury | ~56K | Predominantly Black/Latino, historic, cultural center |
| South Boston | ~40K | Rapidly changing, waterfront development |
| South Boston Waterfront | ~12K | New development, commercial/residential |
| South End | ~34K | Mixed-income historic, LGBTQ+ community |
| West End | ~7K | Dense residential, government center adjacent |
| West Roxbury | ~30K | Suburban residential, older population |
| Harbor Islands | <1K | Parkland, seasonal |

---

## Cross-Referencing Strategy

### Geographic Joins
Most Boston datasets include `neighborhood` as a text field — your primary join key.

```
# Per-capita rate calculation
# Step 1: Count records by neighborhood in primary dataset
query_datastore(primary_id, filters={"neighborhood": "Roxbury"}, limit=5)  # note total_count

# Step 2: Get neighborhood population
query_datastore("b0543358-d03f-4682-bf0c-658ea4573d6f", limit=30)

# Step 3: rate = neighborhood_count / neighborhood_population * 10000
```

⚠️ Watch for neighborhood name variations: "South Boston Waterfront" vs. "Seaport"; "Downtown" vs. "Downtown / Financial District". Always verify with: `query_datastore(resource_id, fields=["neighborhood"], limit=100)`

---

## San Francisco Open Data — Key Datasets

Portal: https://data.sfgov.org
MCP: `San Francisco Open Data` (Socrata)

To discover datasets: `San Francisco Open Data:socrata__search_datasets(query)`

| Topic | Search Terms | Notes |
|-------|-------------|-------|
| 311 / Service Requests | "311 cases", "service requests" | SF311 case data; includes status and resolution fields |
| Building Permits | "building permits" | Active building permits with approval dates |
| Crime | "police incidents", "crime" | SFPD incident data |
| Budget / Spending | "budget expenditure", "controller spending" | City Controller publishes detailed spending data |
| Payroll / Staffing | "employee compensation", "payroll", "salaries" | Annual employee compensation data |
| Demographics | "census", "population", "demographics" | ACS and city estimates |
| Transportation | "bay wheels", "bike share", "traffic" | Mobility and transit datasets |

**San Francisco Context:** ~870K residents; dense urban; tech-sector economy; Socrata platform; strong civic data infrastructure; comparable union workforce; higher labor costs than Boston — normalize dollar figures before cross-city efficiency comparisons.

---

## Seattle Open Data — Key Datasets

Portal: https://data.seattle.gov
MCP: `Seattle Open Data` (Socrata)

To discover datasets: `Seattle Open Data:socrata__search_datasets(query)`

| Topic | Search Terms | Notes |
|-------|-------------|-------|
| Service Requests / 311 | "service requests", "customer service", "find it fix it" | Seattle's FindIt FixIt app generates service request data |
| Building Permits | "building permits", "permits issued" | Active building permit data |
| Crime | "crime data", "police report", "SPD" | Seattle Police Department data |
| Budget / Spending | "budget", "expenditure", "spending" | City budget and spending datasets |
| Payroll / Staffing | "payroll", "employee wages", "city employee" | Annual employee compensation |
| Demographics | "census", "population", "demographics" | ACS and city estimates |
| Transportation | "bike share", "transit", "transportation" | Mobility datasets |

**Seattle Context:** ~750K residents; tech-driven economy; Socrata platform; strong open data culture; Pacific Northwest climate affects infrastructure demands; comparable innovation economy to Boston.

---

## Washington DC Open Data — Key Datasets

Portal: https://opendata.dc.gov
MCP: `DC Open Data` (ArcGIS)

To discover datasets: `DC Open Data:arcgis__search_datasets(query)`

**Note:** DC uses ArcGIS — query syntax differs from Boston and Socrata-based cities. Use `arcgis__query_data` and `arcgis__get_aggregations` instead of `query_datastore` or `query_dataset`.

| Topic | Search Terms | Notes |
|-------|-------------|-------|
| 311 / Service Requests | "311 service requests", "DC311" | DC311 (DCii) system data |
| Building Permits | "building permits", "construction permits" | DCRA permit data |
| Crime | "crime incidents", "MPD crime" | Metropolitan Police Department data |
| Budget / Spending | "budget", "expenditure", "agency spending" | DC Office of the Chief Financial Officer data |
| Payroll / Staffing | "payroll", "employee compensation" | Employee compensation data |
| Demographics | "census", "population" | ACS and DC Office of Planning estimates |
| Transportation | "capital bikeshare", "transportation" | Mobility and transit datasets |

**DC Context:** ~690K residents; most similar population to Boston; city + state hybrid government (handles some functions other cities delegate to state/county); ArcGIS platform; sophisticated performance management culture (CitiStat model — Boston's CitiStat was modeled on DC's); federal government presence distorts some per-capita metrics.

---

## Standard MCP Query Sequence (All Cities)

```
1. search_datasets("topic")           → find dataset IDs
2. get_dataset_info("dataset-id")     → find queryable resource IDs + metadata
3. get_datastore_schema(resource_id)  → get EXACT field names + types (NEVER skip)
4. query_datastore(resource_id, ...)  → retrieve records
```

---

## Data Quality Notes

- **311 reporting bias:** Usage varies by neighborhood, age, language, digital access. High volume = high reporting, not necessarily more problems.
- **Crime data:** Reflects reported/recorded incidents. Reporting rates vary by crime type and community-police relationships.
- **Building permits:** Formal system only; unpermitted work not captured.
- **Population estimates:** 2025 Boston estimates address Census undercounts; may differ from Census Bureau figures.
- **Geographic assignment:** Some records may have incorrect geocoding. Check outliers.
- **Cross-city comparability:** Data collection practices, definitions, and completeness differ across cities. Document all assumptions in any cross-city comparison.
