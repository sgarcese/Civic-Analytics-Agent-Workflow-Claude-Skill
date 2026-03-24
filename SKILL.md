---
name: city-analysis-workflow
description: "Master workflow skill for City of Boston policy analysis and civic innovation. ALWAYS use this skill for any request involving Boston city data, city services, neighborhood equity, public policy, government performance, 311 analysis, housing, safety, transportation, or any civic issue — even if the user hasn't explicitly asked for a 'full analysis'. This skill orchestrates four sub-skills: city-problem-framing (Bloomberg-inspired), city-policy-analysis (J-PAL-inspired), city-communication (GovLab/InnovateUS-inspired), and city-benchmarking (cross-city comparison using Pittsburgh and San Jose data). Use this skill for: 'full analysis', 'policy brief', 'data-driven recommendation', 'city improvement project', 'investigate [issue]', 'compare Boston to other cities', 'what does the data show', 'help me write a memo about', or any request that combines problem definition, data analysis, and communication for government or civic purposes."
---

# City Policy Analysis — Master Orchestrator

## Four-Phase Integrated Framework

| Phase | Source Methodology | When to Use | Reference File |
|-------|-------------------|-------------|----------------|
| **1. FRAME** | Bloomberg Center for Public Innovation (JHU) | Problem is undefined or needs scoping | `Problem_Framing_Skill.md` |
| **2. ANALYZE** | J-PAL, MIT — Evidence-to-Policy | Running numbers, finding patterns, equity analysis | `Analytical_Skill.md` |
| **3. COMMUNICATE** | The GovLab (NYU) / InnovateUS | Writing memos, briefs, dashboards, community reports | `Communication_Skill.md` |
| **4. BENCHMARK** | Cross-city comparison using Boston + Pittsburgh + San Jose data | Comparing Boston to peer cities, learning from elsewhere | `Benchmarking_Skill.md` |
| **5. PERFORM** | Results for America / PerformanceStat (CitiStat) | Budget × staffing × service outcomes: cost-per-outcome, workload-per-FTE, efficiency trends | `Performance_Management_Skill.md` |

> **Always read the relevant sub-skill file before beginning each phase.**

---

## Quick Decision Router

```
User Request
│
├─ "What data does Boston have on..." / "Help me define the problem"
│   → Read Problem_Framing_Skill.md → Run Phase 1
│
├─ "Analyze / run the numbers / what does the data show / is there an equity issue"
│   → Read Analytical_Skill.md → Run Phase 2
│
├─ "Write a memo / create a brief / make a dashboard / present these findings"
│   → Read Communication_Skill.md → Run Phase 3
│
├─ "Compare Boston to other cities / how does Boston rank / what works elsewhere"
│   → Read Benchmarking_Skill.md → Run Phase 4
│
├─ "Budget vs. performance / cost per outcome / workload per FTE / are we getting results / staffing efficiency / how much does it cost to / is the department understaffed / overtime analysis"
│   → Read Performance_Management_Skill.md → Run Phase 5
│
└─ "Full analysis / investigate / give me a recommendation / policy project"
    → Run all relevant phases in sequence
```

---

## MCP Tool Reference — All Three Cities

### Boston Open Data (Primary)
```
search_datasets(query)           → Discover datasets by topic
get_dataset_info(dataset_id)     → Find resource IDs and metadata
get_datastore_schema(resource_id)→ Get exact field names before querying
query_datastore(resource_id, filters={}, sort="", limit=100, date_range={})
```

### Pittsburgh Open Data (Benchmarking)
```
PGH Open Data MCP:ckan__search_datasets(query)
PGH Open Data MCP:ckan__get_dataset(dataset_id)
PGH Open Data MCP:ckan__get_schema(resource_id)
PGH Open Data MCP:ckan__query_data(resource_id, ...)
```

### San Jose Open Data (Benchmarking)
```
San Jose MCP:ckan__search_datasets(query)
San Jose MCP:ckan__get_dataset(dataset_id)
San Jose MCP:ckan__get_schema(resource_id)
San Jose MCP:ckan__query_data(resource_id, ...)
```

**⚠️ ALWAYS confirm field names via schema before querying any dataset in any city.**

---

## Standard MCP Sequence (All Cities)
```
1. search_datasets("topic")         → find dataset IDs
2. get_dataset_info("dataset-id")   → find queryable resource IDs
3. get_datastore_schema(resource_id)→ confirm EXACT field names
4. query_datastore(resource_id, ...) → retrieve records
```

---

## Boston 311 Schema Cheat Sheet (Critical)

The 311 system changed in October 2025. Field names differ:

| Concept | Legacy (2011–Oct 2025) | New System (Oct 2025+) |
|---------|----------------------|----------------------|
| Open date | `open_dt` | `open_date` |
| Close date | `closed_dt` | `close_date` |
| Service type | `type` | `service_name` |
| Department | `department` | `assigned_department` |
| Neighborhood | `neighborhood` | `neighborhood` (same) |
| On-time | `on_time` | `on_time` (same) |

Key resource IDs: `dff4d804-...` (2024), `9d7c2214-...` (Jan–Oct 2025), `254adca6-...` (New System, Oct 2025+)

---

## Cross-Phase Quality Standards

### Rigor (J-PAL): Every claim is grounded in data or clearly labeled as interpretation. Confidence level stated. Limitations named, not buried.

### Human-Centeredness (Bloomberg): Problem framed around people's lived experience. Recommendations are implementable by real city staff.

### Inclusivity (GovLab): Equity lens applied. Plain-language versions exist. Feedback mechanisms included.

### Transparency: Data sources cited with IDs. Methodology reproducible. Findings shareable as open knowledge.

---

## Supporting Files in This Skill Set

| File | Purpose |
|------|---------|
| `Problem_Framing_Skill.md` | Bloomberg methodology: scope, stakeholders, assumptions |
| `Analytical_Skill.md` | J-PAL methodology: descriptive → diagnostic → equity |
| `Communication_Skill.md` | GovLab/InnovateUS: memos, briefs, dashboards, engagement |
| `Benchmarking_Skill.md` | Cross-city comparison using Pittsburgh and San Jose data |
| `TEMPLATES.md` | Fill-in-the-blank templates for 6 output types |
| `CHECKLISTS.md` | Pre-flight and review checklists for all phases |
| `PROMPTS.md` | Example prompts organized by phase and complexity |
| `REFERENCE.md` | Boston dataset directory, field names, cross-referencing |
| `Performance_Management_Skill.md` | Results for America / PerformanceStat: budget × staffing × outcomes efficiency analysis |
| `EXAMPLE-311-equity.md` | Complete worked example: 311 response equity analysis |

---

## When Creating Documents
- Word docs (.docx): Also read `/mnt/skills/public/docx/SKILL.md`
- Presentations (.pptx): Also read `/mnt/skills/public/pptx/SKILL.md`
- Spreadsheets (.xlsx): Also read `/mnt/skills/public/xlsx/SKILL.md`
- Dashboards (React/HTML): Also read `/mnt/skills/public/frontend-design/SKILL.md`
