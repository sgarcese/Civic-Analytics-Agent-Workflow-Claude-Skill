---
name: quality-checklists
description: "Pre-flight and review checklists for all four phases of city analysis. Use BEFORE starting each phase (pre-flight) and BEFORE delivering outputs (review). Encodes quality standards from Bloomberg, J-PAL, GovLab/InnovateUS, and cross-city benchmarking."
---

# Quality Checklists

Use at two points: **before starting** (pre-flight ✈️) and **before delivering** (review ✅).

---

## Phase 1: Problem Framing

### Pre-Flight ✈️
- [ ] Do I understand the triggering event? (Why is this being asked now?)
- [ ] Have I resisted jumping to a solution or querying data immediately?
- [ ] Do I know who is asking and what decision they need to make?
- [ ] Have I considered whose perspective is shaping the problem definition?

### Review Before Handoff ✅
- [ ] **Problem statement written** — human-centered, specific, measurable, bounded, honest
- [ ] **Stakeholders mapped** — affected residents, frontline staff, decision-makers, partners
- [ ] **Equity dimensions identified** — who is disproportionately affected?
- [ ] **At least 3 assumptions** explicitly stated and questioned
- [ ] **Data landscape assessed** — relevant datasets identified with resource IDs
- [ ] **Schema examined** — field names confirmed via schema tool
- [ ] **Analytical questions listed** — specific, answerable with available data
- [ ] **Known limitations noted** — what the data cannot tell us
- [ ] **Success criteria defined** — what "better" looks like

### Red Flags 🚩
- Problem statement sounds like a solution ("We need an app that...")
- No equity dimension identified
- Analysis questions are unanswerable with available data
- Stakeholder map only includes people inside government

---

## Phase 2: Data Analysis

### Pre-Flight ✈️
- [ ] Do I have a clear problem statement?
- [ ] Do I have specific resource IDs to query?
- [ ] Have I confirmed field names via schema tool — not guessed?
- [ ] Do I have a population/denominator dataset for per-capita calculations?
- [ ] Am I clear on what time period I'm analyzing?

### Review Before Handoff ✅

**Data Quality**
- [ ] Total N reported (how many records analyzed)
- [ ] Time period clearly stated
- [ ] Data freshness noted (when was this last updated?)
- [ ] Missing values flagged (any key fields with high null rates?)
- [ ] Obvious outliers or quality issues called out

**Descriptive Findings**
- [ ] Central tendency reported (average and/or median)
- [ ] Spread/distribution reported (range, variation)
- [ ] Both raw counts AND rates/per-capita figures where comparing groups
- [ ] Time trends examined (not just a single snapshot)
- [ ] Geographic patterns examined (across neighborhoods)

**Diagnostic Claims**
- [ ] Every claim labeled with appropriate confidence level:
  - "The data shows X" → Descriptive
  - "X is associated with Y" → Correlational
  - "Evidence suggests X may contribute to Y" → Suggestive
  - "One possible explanation is..." → Hypothetical
- [ ] **NO unsupported causal claims** ("X caused Y" without experimental evidence)
- [ ] Alternative explanations considered for key findings
- [ ] Confounders acknowledged

**Equity Analysis**
- [ ] Findings disaggregated by neighborhood at minimum
- [ ] Neighborhoods compared in context of demographic profiles
- [ ] Reporting/access bias discussed (who might be undercounted?)
- [ ] Ecological fallacy caveat included if using geographic proxies
- [ ] Asset-based framing used (not deficit framing)

**Limitations**
- [ ] Honest limitations section written (not perfunctory boilerplate)
- [ ] Clear distinction: what data shows vs. what we wish it showed
- [ ] Key data gaps identified

### Red Flags 🚩
- Causal language without experimental evidence
- Raw counts compared without population adjustment
- No equity/distributional analysis
- Only citywide averages reported (masks neighborhood variation)
- Limitations section missing or reads as boilerplate
- Cherry-picked time periods or neighborhoods
- Claims based on very small N without uncertainty acknowledgment

---

## Phase 3: Communication

### Pre-Flight ✈️
- [ ] Primary audience named specifically?
- [ ] Decision or action this communication should support is clear?
- [ ] Correct format selected for this audience?
- [ ] Appropriate level of technical detail determined?
- [ ] Political or timing considerations noted?

### Review Before Delivery ✅

**Content Quality**
- [ ] Main finding clear within first 30 seconds of reading
- [ ] Every number traceable to specific data query
- [ ] No claims exceed what evidence supports
- [ ] Recommendations are specific (who, what, when)
- [ ] Alternatives considered and addressed
- [ ] Data sources cited with enough detail to reproduce

**Audience Fit**
- [ ] Tone matches audience (executive vs. community vs. technical)
- [ ] Technical jargon eliminated or defined for non-technical audiences
- [ ] Length appropriate for audience
- [ ] Visuals support narrative (not decorative)
- [ ] "So what / now what" is unmistakable

**Equity & Inclusion**
- [ ] Plain language used in public-facing materials (8th grade target)
- [ ] Deficit framing avoided; asset-based language used
- [ ] Translation needs considered for Boston's language communities
- [ ] Digital access barriers considered
- [ ] Community voice included or acknowledged as appropriate

**Engagement & Transparency**
- [ ] Feedback mechanism included
- [ ] Data sources linked or referenced
- [ ] Methodology described at appropriate detail level
- [ ] Engagement is genuine (asking questions the audience can influence)

**Document Quality**
- [ ] Professional formatting (consistent headers, clean layout)
- [ ] No typos or calculation errors
- [ ] Visuals are accessible (color-blind safe, labeled)
- [ ] File format appropriate for audience

### Red Flags 🚩
- Main finding buried below the fold
- Recommendations are vague ("explore options")
- No equity analysis in communication
- Public-facing document uses unexplained government jargon
- No mechanism for audience to respond or verify
- Misleading visuals (truncated axes, cherry-picked comparisons)

---

## Phase 4: Benchmarking

### Pre-Flight ✈️
- [ ] Is the metric I'm comparing clearly defined?
- [ ] Have I verified that all three cities measure this metric similarly?
- [ ] Have I identified the appropriate denominator (per capita, per unit, etc.)?
- [ ] Have I noted key structural differences between cities?

### Review Before Delivery ✅

**Comparability**
- [ ] Each city's exact metric definition documented
- [ ] Comparability confidence rated (High / Moderate / Low)
- [ ] Key structural differences between cities listed
- [ ] Time periods align (or misalignment explicitly noted)
- [ ] Data quality differences across cities acknowledged

**Analysis**
- [ ] All metrics normalized by appropriate denominator
- [ ] City population figures used for per-capita calculations
  (Boston ~675K, Pittsburgh ~300K, San Jose ~1M)
- [ ] Both absolute and per-capita numbers reported
- [ ] Data source for each city cited (dataset ID + portal)

**Claims and Language**
- [ ] Comparative claims use hedged, appropriate language
- [ ] No causal attribution ("City X's policy caused their better performance")
- [ ] Peer city practices framed as "worth exploring" not "proven models"
- [ ] "Benchmark" framed as a starting point for investigation, not a verdict

**Recommendations**
- [ ] Each benchmark finding connects to a specific investigable hypothesis for Boston
- [ ] Next steps proposed before acting on benchmarks (e.g., site visits, practitioner interviews)
- [ ] Cost/resource context noted where available

### Red Flags 🚩
- Comparing metrics with different definitions without acknowledging it
- Drawing causal conclusions from cross-city comparison
- Omitting structural differences between cities
- Cherry-picking peer cities based on desired outcome
- Using absolute numbers to compare cities of different sizes without normalizing

---

## Final Cross-Phase Review

Before any analysis is considered complete:

### The Rigor Test (J-PAL)
- [ ] Could a skeptical analyst challenge any finding? If so, have you addressed it?
- [ ] Is the weakest link in the evidence chain clearly identified?
- [ ] Would you be comfortable if this analysis were made fully public?

### The Human Test (Bloomberg)
- [ ] If a resident read this, would they recognize their experience in it?
- [ ] Does this connect to something that matters in people's daily lives?
- [ ] Can a city employee actually act on these recommendations?

### The Democracy Test (GovLab/InnovateUS)
- [ ] Does this increase transparency about how the city works?
- [ ] Does it create an opportunity for meaningful public participation?
- [ ] Is the underlying data accessible for others to verify and build on?

### The Equity Test (All Frameworks)
- [ ] Who benefits from this analysis and its recommendations?
- [ ] Who might be harmed or overlooked?
- [ ] Have historically marginalized communities' perspectives been centered?
- [ ] Does this analysis challenge or reinforce existing power dynamics?
