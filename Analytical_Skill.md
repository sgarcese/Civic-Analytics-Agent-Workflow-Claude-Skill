---
name: city-policy-analysis
description: "Use this skill when the user needs to conduct rigorous data analysis on Boston city data for policy-making purposes. ALWAYS use when the user wants to query or interpret Boston Open Data, make statistical claims about city services, or move from raw data to defensible findings. Inspired by J-PAL's (Abdul Latif Jameel Poverty Action Lab at MIT) evidence-to-policy methodology. Triggers include: 'analyze this data', 'what does the data show', 'run the numbers', 'is there an equity issue', 'compare neighborhoods', 'what are the trends', 'evidence-based analysis', 'policy analysis', requests to query city data, or any situation where the user needs to move from raw data to defensible findings. Also use when the user makes claims that need data validation, or when findings need to be labeled with appropriate confidence levels."
---

# City Policy Analysis — J-PAL Evidence-to-Policy Methodology

## Core Commitment
**Policy recommendations must be grounded in the best available evidence, with honest accounting of what we know and don't know.** Overstating causal claims undermines credibility and leads to bad policy.

---

## Pre-Analysis Checklist

Before writing a single query, confirm:
- [ ] Do I have a clear problem statement from Problem Framing phase (or will I create one now)?
- [ ] Do I have the specific resource IDs I need to query?
- [ ] Have I confirmed field names via `get_datastore_schema()`? **Never guess field names.**
- [ ] Do I have a population/denominator dataset for per-capita calculations?
- [ ] Am I clear on what time period I'm analyzing?

---

## Level 1: Descriptive Analysis — "What Is Happening?"

This is the foundation. Establish facts before explaining them.

**Core questions:** What is the current state? How has it changed over time? How does it vary by geography? By population? What are the outliers?

**MCP Workflow:**
```
Step 1: Confirm schema
→ get_datastore_schema(resource_id)
  Know your fields before querying. Note date, categorical, and geographic fields.

Step 2: Get overall volume and recent records
→ query_datastore(resource_id, limit=20, sort="[date_field] desc")

Step 3: Filter by category to understand composition
→ query_datastore(resource_id, filters={"[category]": "[value]"}, limit=50)
  Repeat for key categories.

Step 4: Temporal analysis
→ query_datastore(resource_id, date_range={"field": "[date_field]",
    "start_date": "YYYY-MM-DD", "end_date": "YYYY-MM-DD"}, limit=1000)

Step 5: Cross-reference with other datasets
→ Repeat discovery and querying for related datasets (e.g., population by neighborhood)
```

**Descriptive reporting standards:**
- Always report total N (number of records)
- Always report the time period covered
- Note if data is a sample or full population
- Report both central tendency (average/median) AND spread (range, distribution)
- Flag data quality issues (missing values, implausible entries)

---

## Level 2: Diagnostic Analysis — "Why Is It Happening?"

Move carefully from description to explanation.

**Approaches:**
1. **Correlation** — Do two things tend to occur together? *Correlation ≠ causation — always state this.*
2. **Comparison** — How do different groups or areas differ? *Group differences may reflect many underlying factors.*
3. **Temporal** — Did something change after a specific event or policy? *Other things change simultaneously — before/after is not proof of cause.*
4. **Decomposition** — What factors contribute to an observed outcome?

### Claim Strength Framework (J-PAL)

| Claim Level | Language to Use | Evidence Required |
|-------------|----------------|-------------------|
| **Strong causal** | "X caused Y" | Randomized evaluation — rarely available in open data |
| **Suggestive causal** | "Evidence suggests X contributed to Y" | Before/after + comparison group + alternatives ruled out |
| **Correlational** | "X is associated with Y" | Systematic co-occurrence in data |
| **Descriptive** | "The data shows X" | Observed patterns |
| **Hypothetical** | "One possible explanation is..." | Logical reasoning consistent with data |

**With administrative open data (like Boston's), most findings will be descriptive or correlational. Be transparent about this every time.**

---

## Level 3: Equity Analysis — "Who Benefits and Who Is Burdened?"

Average effects mask important distributional differences. This step is **mandatory**.

**Equity Analysis Checklist:**
- [ ] **Geographic equity:** Do outcomes vary by neighborhood? Do historically underserved areas fare differently?
- [ ] **Racial/ethnic equity:** Can neighborhood demographics serve as a proxy? (note limitations)
- [ ] **Economic equity:** Do outcomes differ by income level?
- [ ] **Access equity:** Who is represented in the data? Who might be missing?
- [ ] **Temporal equity:** Are some groups waiting longer?

**Boston Equity Data Strategy:**
- Cross-reference with `2025-boston-population-estimates-neighborhood-level` (resource ID: `b0543358-d03f-4682-bf0c-658ea4573d6f`) for 24-neighborhood demographic profiles
- Use `2024-digital-equity-survey` for digital access analysis
- Census tract resource for finer geographic granularity

**Equity Caveats (always include):**
- Ecological fallacy: Neighborhood patterns don't necessarily apply to individuals
- Reporting bias: Open data reflects reported events, which may differ from actual incidence
- Missing populations: People with less access to government services may be underrepresented

---

## Level 4: Counterfactual Thinking

To evaluate a policy, ask: what would have happened without it?

1. **Baseline trend:** What's the trajectory without intervention? Is the problem getting better or worse on its own?
2. **Comparison groups:** Are there natural comparison cases? (Neighborhoods that did/didn't receive intervention; before/after a policy change)
3. **Dose-response:** If an intervention varies in intensity, do outcomes vary accordingly?
4. **Confounders:** What else changed at the same time? List plausible alternative explanations.

---

## Level 5: Evidence Synthesis Template

```markdown
## Summary of Findings

### What the Data Shows (Descriptive)
[Clear, factual summary of patterns. N = X records, period = Y]

### What Might Explain This (Diagnostic)
[Possible explanations, each labeled with claim strength level]

### Who Is Most Affected (Equity)
[Distributional analysis across geography, demographics, access]

### What Would Happen Without Action (Counterfactual)
[Baseline trends and projections]

### What We Don't Know
[Questions that require additional data or deeper analysis]

### Limitations and Caveats
[Honest accounting — what this analysis CANNOT tell us]

### Implications for Policy
[What decision-makers can reasonably conclude from this evidence]
```

---

## Common MCP Query Patterns

### Neighborhood comparison
```
query_datastore(resource_id, filters={"neighborhood": "Roxbury"}, limit=200)
query_datastore(resource_id, filters={"neighborhood": "Back Bay"}, limit=200)
# Compare volumes, rates, outcomes — always per-capita when comparing areas
```

### On-time performance (311)
```
# Legacy system (pre-Oct 2025):
query_datastore(resource_id, filters={"neighborhood": "Mattapan", "on_time": "ONTIME"}, limit=5)
query_datastore(resource_id, filters={"neighborhood": "Mattapan", "on_time": "OVERDUE"}, limit=5)

# New system (Oct 2025+): same on_time field, but use open_date/close_date not open_dt/closed_dt
```

### Cross-dataset (per-capita rates)
```
# Step 1: Count by neighborhood in primary dataset
query_datastore(primary_id, filters={"neighborhood": "Dorchester"}, limit=5)  # note total_count

# Step 2: Get population
query_datastore("b0543358-d03f-4682-bf0c-658ea4573d6f", limit=30)  # population by neighborhood

# Step 3: Divide count / population → per-capita rate
```

---

## Analytical Pitfalls to Avoid

| Pitfall | Remedy |
|---------|--------|
| **Cherry-picking** | Report ALL major findings, including uncomfortable ones |
| **Denominator neglect** | Always calculate per-capita rates when comparing areas |
| **Simpson's Paradox** | Check subgroup patterns before reporting aggregates |
| **Survivorship bias** | Consider what happened to cases that dropped out |
| **False precision** | Use ranges for uncertain quantities; round appropriately |
| **Causal overclaiming** | Label every finding with its appropriate confidence level |

---

## Handoff to Communication Phase

Pass these to `Communication_Skill.md`:
1. ✅ Key findings organized by confidence level
2. ✅ Equity analysis results
3. ✅ Clear statement of limitations
4. ✅ Specific, actionable recommendations (or questions for further investigation)
5. ✅ Data tables ready for formatting
6. ✅ Target audience identified
7. ✅ Success criteria from problem framing phase

## J-PAL Core Values
1. **Intellectual honesty** — Report what you find, not what you hoped to find
2. **Transparency** — Show your work; describe methodology; acknowledge limitations
3. **Humility** — Administrative data rarely proves causation
4. **Equity focus** — Always ask who benefits and who is burdened
5. **Action orientation** — Analysis should inform decisions, not just describe situations
6. **Iterative learning** — Frame recommendations as hypotheses to test
