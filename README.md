# Civic Analytics Skills for Claude

> **Evidence-based city policy analysis, powered by Claude AI and live open government data.**

A set of Claude Skills that guide analysts through rigorous, equity-focused city policy work — from scoping a problem to publishing findings. Built on world-class public innovation methodologies and connected to live open data from Boston, San Francisco, Seattle, and Washington DC via Model Context Protocol (MCP).

---

## What This Is

City analysts spend too much time wrestling with data portals and too little time asking the right questions. These skills flip that equation.

Each skill encodes a proven methodology and handles the mechanics of data discovery, querying, and quality control — so analysts can focus on judgment, equity, and communication. Claude acts as an expert collaborator who knows the data, knows the methods, and knows the audience.

---

## The Five-Phase Framework

| Phase | Skill File | Methodology | Core Question |
|-------|-----------|-------------|--------------|
| **1. Frame** | `Problem_Framing_Skill.md` | Bloomberg Centers (JHU & HKS) | Are we solving the right problem? |
| **2. Analyze** | `Analytical_Skill.md` | J-PAL at MIT | What does the evidence actually show? |
| **3. Communicate** | `Communication_Skill.md` | The GovLab (NYU & Northeastern) | Who needs to know what, in what format? |
| **4. Benchmark** | `Benchmarking_Skill.md` | Cross-city comparison (SF, Seattle, DC) | Is this a Boston problem or every city's problem? |
| **5. Perform** | `Performance_Management_Skill.md` | Results for America / PerformanceStat | Are we getting results for our investment? |

The master orchestrator (`SKILL.md`) routes automatically between phases based on your request. You can run a single phase or the full workflow end-to-end.

---

## Source Methodologies

**Bloomberg Centers for Public Innovation** — *Johns Hopkins University & Harvard Kennedy School*
Problem framing, stakeholder mapping, assumption interrogation, human-centered scoping. Prevents the most common city innovation failure: solving the wrong problem well.

**J-PAL — Abdul Latif Jameel Poverty Action Lab at MIT**
Five levels of evidence — descriptive, diagnostic, equity, counterfactual, synthesis — with explicit claim-strength labeling. Ensures analysts never overstate what administrative data can prove.

**The GovLab — New York University & Northeastern University**
Data collaboratives, collective intelligence, consequential engagement, democratic legitimacy. Translates analysis into formats that create real civic action.

**InnovateUS**
Accessible public communication, co-design methodology, plain-language standards. Bridges the gap between expert findings and community understanding.

**Results for America / PerformanceStat (CitiStat)**
Budget × staffing × service outcomes. Connects actual expenditures and employee headcount to delivered results. Surfaces cost-per-outcome, workload-per-FTE, and multi-year efficiency trends to ground investment decisions in evidence.

---

## Data Sources

Skills connect to open government data via MCP servers — no manual downloads, no API keys. Data flows directly into analysis on demand.

| City | Role | MCP Server | Platform |
|------|------|-----------|----------|
| **Boston** | Primary analysis city | Boston Open Data MCP | CKAN/Socrata |
| **San Francisco** | Peer benchmarking | San Francisco Open Data | Socrata |
| **Seattle** | Peer benchmarking | Seattle Open Data | Socrata |
| **Washington DC** | Peer benchmarking | DC Open Data | ArcGIS |

Boston coverage includes: 311 service requests, building permits, public safety, demographics, housing, transportation, environment, operating budget, and employee earnings. SF, Seattle, and DC were selected as peers for comparable population scale, urban density, and data infrastructure depth.

---

## Repository Structure

```
.
├── SKILL.md                         # Master orchestrator — start here
├── Problem_Framing_Skill.md         # Phase 1: Bloomberg methodology
├── Analytical_Skill.md              # Phase 2: J-PAL evidence framework
├── Communication_Skill.md           # Phase 3: GovLab/InnovateUS methods
├── Benchmarking_Skill.md            # Phase 4: Cross-city comparison (SF, Seattle, DC)
├── Performance_Management_Skill.md  # Phase 5: Results for America / PerformanceStat
├── TEMPLATES.md                     # 6 fill-in-the-blank output formats
├── CHECKLISTS.md                    # Pre-flight & review checklists
├── PROMPTS.md                       # 25+ example prompts by complexity
├── REFERENCE.md                     # Dataset catalog, field names, schema guide
├── EXAMPLE-311-equity.md            # Complete worked example end-to-end
└── index.html                       # Project overview page
```

---

## Quickstart

### 1. Clone the repository

```bash
git clone https://github.com/sgarcese/Civic-Analytics-Agent-Workflow-Claude-Skill.git
```

### 2. Connect your MCP data sources

Add the Boston, San Francisco, Seattle, and DC open data MCP servers to your Claude environment. See your Claude configuration for MCP setup instructions.

### 3. Upload the skills to Claude

Add the `.md` skill files to your Claude project. The master orchestrator in `SKILL.md` will route to sub-skills automatically.

### 4. Start analyzing

```
# Full end-to-end workflow
"Investigate whether Boston's 311 response times are equitable across
neighborhoods. Frame the problem, analyze the data, compare to Seattle,
and write a brief for the Mayor."

# Single-phase analysis
"Analyze Boston building permit approval times by neighborhood for 2024.
Flag any equity concerns."

# Cross-city benchmark
"Compare Boston, San Francisco, Seattle, and DC on 311 closure rates.
What can Boston learn from the better performers?"

# Performance management
"What is Boston's cost per resolved 311 case in Public Works for FY25?
How does workload per employee compare to San Francisco?"

# Communication package
"Write a 1-page memo for the Mayor AND a community fact sheet for
Roxbury residents from the same 311 equity analysis."
```

More prompts in [`PROMPTS.md`](PROMPTS.md), organized from simple to expert-level.

---

## Design Principles

**Rigorous** — Every claim carries a confidence level. Limitations are stated, not buried. Administrative data rarely proves causation; these skills say so explicitly.

**Human-centered** — Problems are framed around residents' lived experience. Success is measured by resident outcomes, not bureaucratic process metrics.

**Equity-focused** — Every analysis asks who benefits and who is burdened. Asset-based framing is required. Geographic proxies are noted alongside their limits.

**Transparent** — Data sources are cited with resource IDs. Methodology is reproducible. Findings are structured for open sharing.

**Actionable** — Communication is designed to create action, not just awareness. Recommendations specify who, what, and when. Feedback loops are built in.

---

## Important: Boston 311 Schema Change

The Boston 311 system changed in October 2025. Field names differ between legacy and new records. Always check [`REFERENCE.md`](REFERENCE.md) or use the schema cheat sheet in `SKILL.md` before querying.

| Concept | Legacy (2011–Oct 2025) | New System (Oct 2025+) |
|---------|----------------------|----------------------|
| Open date | `open_dt` | `open_date` |
| Close date | `closed_dt` | `close_date` |
| Service type | `type` | `service_name` |
| Department | `department` | `assigned_department` |

---

## Supporting Materials

**[`Performance_Management_Skill.md`](Performance_Management_Skill.md)** — Phase 5 skill. Connects budget expenditures, employee headcount, and service outcomes to compute cost-per-outcome, workload-per-FTE, and multi-year efficiency trends. Also the foundation for cross-city performance benchmarking via `Benchmarking_Skill.md`.

**[`TEMPLATES.md`](TEMPLATES.md)** — Six fill-in-the-blank output formats: executive memo, policy brief, one-pager, community fact sheet, benchmark summary, and presentation deck.

**[`CHECKLISTS.md`](CHECKLISTS.md)** — Pre-flight and review checklists for all five phases. Catches the most common analytical errors before they reach stakeholders.

**[`PROMPTS.md`](PROMPTS.md)** — 25+ example prompts organized from simple single-query requests to complex multi-phase policy projects.

**[`REFERENCE.md`](REFERENCE.md)** — Full dataset catalog for all four cities with resource IDs, field names, and the 311 schema change reference table.

**[`EXAMPLE-311-equity.md`](EXAMPLE-311-equity.md)** — A complete worked example showing every phase, every tool call, and every analytical decision for a 311 response equity analysis.

---

## License

MIT License — open source, public domain data.

---

*Built with Claude · Methodologies from Bloomberg Centers, J-PAL, The GovLab, InnovateUS, and Results for America*
