---
name: prompt-examples
description: "Example prompts organized by phase and complexity. Use as templates or starting points. Each shows what skills activate and what happens."
---

# Prompt Examples — How to Use the City Analysis Skills

## Quick Reference

| Goal | Example Prompt | Skills Activated |
|------|---------------|-----------------|
| Explore what data exists | "What data does Boston have on [topic]?" | Problem Framing |
| Define a problem | "Help me frame the problem of [issue]" | Problem Framing |
| Analyze data | "Analyze [dataset] by neighborhood" | Policy Analysis |
| Equity check | "Is there an equity issue with [service]?" | Policy Analysis |
| Write a memo | "Write a memo to [audience] about [finding]" | Communication |
| Create a brief | "Create a policy brief on [topic]" | Communication |
| Compare to peer cities | "How does Boston compare to Pittsburgh on [metric]?" | Benchmarking |
| Full investigation | "Investigate [issue] and recommend action" | All phases |

---

## Problem Framing Prompts

### Data Discovery
```
"What data does Boston have on housing development?"
```
→ search_datasets(), get_dataset_info() across relevant domains
→ Returns overview of available datasets, resource IDs, key fields, limitations

### Scoping a Question
```
"I'm concerned about whether building permits are being approved equitably 
across neighborhoods. Help me figure out the right questions to ask before 
I dig into the data."
```
→ Stakeholder mapping, assumption interrogation, data landscape assessment
→ Returns problem statement, analytical questions, identified datasets

### Reframing a Predetermined Solution
```
"The Mayor wants to launch a new 311 mobile app to improve service delivery 
in underserved neighborhoods. Before we build it, help me think about 
whether that's the right solution."
```
→ Assumption interrogation: Is the problem app access, or resource allocation?
→ Examines 311 usage data by channel and neighborhood
→ Surfaces alternative problem framings

### Cross-Agency Problem Scoping
```
"Three departments (PWD, Parks, and Transportation) all handle different 
types of street and sidewalk issues through 311. We're getting complaints 
about cases falling through the cracks. Help me map this problem."
```
→ Cross-agency problem framing
→ Maps handoff points between departments using 311 department/queue fields
→ Identifies where cases get stuck or redirected

---

## Policy Analysis Prompts

### Simple Descriptive
```
"How many 311 requests were there in Dorchester in 2024, and what were 
the most common types?"
```
→ Queries 2024 311 data filtered by neighborhood, returns volume + type breakdown

### Comparative Analysis
```
"Compare 311 on-time performance across Boston's five largest neighborhoods 
for 2024. Calculate per-capita request rates using the population estimates."
```
→ Queries 311 data per neighborhood; cross-references population dataset
→ Returns per-capita rates, on-time comparisons, with appropriate caveats

### Equity Investigation
```
"I want a rigorous analysis of whether 311 response times show geographic 
disparities that correlate with race or income. Use neighborhood demographics 
as a proxy and be honest about the limitations of that approach."
```
→ Full equity analysis workflow
→ Cross-references 311 performance with neighborhood demographic profiles
→ Explicitly addresses ecological fallacy and reporting bias
→ Labels all claims with confidence levels

### Trend Analysis Across System Change
```
"How has 311 performance changed from 2022 through early 2026? Note that 
the system changed in October 2025 and field names are different."
```
→ Queries multiple yearly resources with different schemas
→ Maps legacy field names to new system field names
→ Reports trends with caveat about system transition

### Multi-Dataset Investigation
```
"What's the relationship between building permit activity and 311 noise/
construction complaints in each neighborhood? Control for population."
```
→ Queries building permits by neighborhood
→ Queries 311 filtered by construction-related types
→ Cross-references population data for per-capita rates
→ Reports correlations with explicit causation caveats

---

## Communication Prompts

### Quick Summary
```
"Summarize the key 311 findings for Roxbury in a one-page data summary."
```
→ One-page data summary template; key numbers table, findings, limitations

### Executive Memo
```
"Write a one-page memo to the Chief of Staff recommending a pilot program 
to improve 311 response times in three underserved neighborhoods. Base it 
on the analysis we just did."
```
→ Executive memo template; bottom line + recommendation up front
→ Includes equity note, specific next steps; creates .docx file

### Community-Facing Report
```
"Create a plain-language fact sheet about 311 response times in Mattapan 
that I can share at a community meeting. Include how residents can give 
feedback."
```
→ Community report template; plain language, simple visuals
→ Engagement hooks and feedback mechanism included

### Interactive Dashboard
```
"Build a dashboard that lets someone pick a neighborhood and see 311 
performance metrics compared to the citywide average. Include on-time rate, 
most common request types, and volume trends."
```
→ React artifact with neighborhood filter
→ Reads frontend-design skill for visual quality
→ Includes data source attribution and "About This Data" section

### Presentation Deck
```
"Create a 10-slide presentation for the City Council on 311 service equity. 
Start with the human impact, present the evidence, end with three specific 
policy recommendations."
```
→ Presentation template; reads pptx skill
→ Data story arc: HOOK → CONTEXT → EVIDENCE → MEANING → ACTION

### Multi-Audience Communication Package
```
"I need a communication package for the 311 equity findings: (1) a one-page 
executive memo for the Mayor, (2) a 3-page policy brief for Council, and 
(3) a community fact sheet for Mattapan residents. Same analysis, three 
audiences."
```
→ Three distinct outputs from same analysis
→ Each audience-appropriate in tone, detail, and format
→ All cite same data sources; consistent findings

---

## Benchmarking Prompts

### Simple Comparison
```
"How does Boston's 311 on-time rate compare to Pittsburgh's?"
```
→ Reads Benchmarking_Skill.md
→ Searches Pittsburgh MCP for equivalent service request data
→ Normalizes by population; reports with comparability caveats

### Multi-City Benchmark
```
"Compare Boston, Pittsburgh, and San Jose on building permit approval times 
and per-capita permit volume. What can Boston learn from the better performers?"
```
→ Parallel queries across all three city MCPs
→ Normalization table with per-capita figures
→ Structural differences analysis
→ Peer learning recommendations labeled with confidence levels

### Best-Practice Hunt
```
"I want to know if any comparable cities have figured out how to improve 
311 response times in lower-income neighborhoods. Look at what Pittsburgh 
and San Jose have done and what their data shows."
```
→ Searches Pittsburgh and San Jose MCPs for service request data
→ Searches web for policy context (what those cities actually did)
→ Returns benchmark table + policy narrative labeled as preliminary evidence

### Equity Benchmark
```
"Is Boston's neighborhood-level equity gap in 311 response times worse or 
better than Pittsburgh's? Use the most recent data available from both cities."
```
→ Neighborhood-level analysis in both cities
→ Calculates equity gap metric (top vs. bottom quartile neighborhoods)
→ Reports with structural context and comparability caveats

---

## Full Workflow Prompts (All Phases)

### Focused Investigation
```
"Investigate whether Boston's 311 system is serving all neighborhoods 
equitably. Frame the problem, analyze the data, and write a brief 
for the Mayor's office."
```
→ Phase 1: Problem statement, stakeholder map, data discovery
→ Phase 2: Descriptive → diagnostic → equity analysis
→ Phase 3: Executive memo format

### Policy Recommendation with Benchmarks
```
"The City Council is debating whether to reallocate Public Works crews 
based on 311 demand data. Help me build the evidence case — analyze the 
relevant Boston data, see how Pittsburgh handles this, and produce a policy 
brief with honest pros and cons."
```
→ Phase 1: Frame resource allocation question
→ Phase 2: Analyze Boston demand patterns, geographic distribution
→ Phase 4: Pittsburgh comparison for crew allocation approaches
→ Phase 3: Policy brief with recommendations, alternatives, limitations

### Neighborhood Equity Index
```
"I'm working on a neighborhood equity index for the Mayor's budget process. 
I want to combine 311 response times, building permit activity, and population 
demographics to create a composite picture of service delivery equity across 
all 24 neighborhoods. Help me design the methodology, run the analysis, and 
produce both a technical report and a public-facing summary."
```
→ Phase 1: Define what "equity" means operationally; select indicators
→ Phase 2: Multi-dataset analysis with normalization and composite scoring
→ Phase 3: Technical report + community summary + methodology appendix

---

## Tips for Effective Prompts

**Name the audience** — "Write a memo" is OK; "Write a memo to the Chief of Staff who needs to brief the Mayor before a press conference tomorrow" is much better.

**Name the data** — "Analyze 311 data for 2024" is faster than "look at city services data."

**State your constraints** — "I need this in one page" or "this needs to be readable by someone with no data background."

**Ask for honesty** — "Be honest about what the data can't tell us" signals that you want rigorous caveats, not just confident-sounding conclusions.

**Request specific formats** — "Create a .docx memo" or "build a React dashboard" or "give me a markdown brief."

**Iterate across phases** — Start with framing, review the problem statement, then ask for analysis, review findings, then ask for communication. Course-correct between phases.
