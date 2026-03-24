---
name: city-communication
description: "Use this skill when the user needs to communicate city data findings, write policy memos, create public-facing reports, build dashboards, or prepare briefings for any stakeholder audience. ALWAYS use when findings need to be translated into a deliverable document, presentation, or engagement product. Inspired by The GovLab (Northeastern + NYU) and InnovateUS methodologies for effective democratic communication and civic engagement. Triggers include: 'write a memo', 'create a brief', 'present these findings', 'make a dashboard', 'community report', 'stakeholder briefing', 'plain-language version', 'create a fact sheet', 'write this up for the Mayor', 'prepare slides', 'public-facing report', or any situation where analysis needs to become a specific deliverable. Also use when the user needs to design engagement processes or incorporate community voice."
---

# City Communication & Engagement — GovLab / InnovateUS Methodology

## Core Commitment
**Data-driven communication should be consequential, not performative.** It should actually help people make better decisions and participate meaningfully in governance.

---

## Step 1: Identify Audience and Format

Before writing, answer these questions:
- Who is the **primary audience**? (name them specifically)
- What **decision or action** should this communication support?
- What **format** does this audience need?
- What is the appropriate **level of technical detail**?

### Audience → Format → Tone Guide

| Audience | Format | Tone |
|----------|--------|------|
| **Mayor / Chief of Staff** | 1-page executive memo | Direct, confident, bottom-line-first |
| **Department Head** | 2-3 page policy memo | Professional, operational, specific |
| **City Council** | Policy brief + talking points | Balanced, evidence-rich, politically aware |
| **Cross-agency working group** | Presentation + discussion guide | Collaborative, open questions |
| **Community organizations** | Community report or fact sheet | Accessible, respectful, empowering |
| **General public / residents** | Dashboard, infographic, fact sheet | Clear, visual, jargon-free (8th grade level) |
| **Media** | Press-ready summary | Factual, quotable, contextual |
| **Research partners** | Technical appendix | Precise, transparent, reproducible |

---

## Step 2: The Data Story Arc (GovLab Framework)

Every communication — regardless of format — should follow this narrative structure:

```
1. HOOK — Why should the audience care?
   Human impact, urgency, or a surprising finding.
   "In the last year, residents in three neighborhoods waited twice as long for pothole repairs."

2. CONTEXT — What's the landscape?
   Scope, history, how the system works, comparison point.

3. EVIDENCE — What did we find?
   Findings in order of importance (not chronological).
   Each finding: data reference + plain-language interpretation.

4. MEANING — Why does this matter?
   Equity, cost, trust, quality of life. Connect numbers to lived experience.

5. ACTION — What should we do?
   Specific, feasible recommendations. Who, what, when.

6. ENGAGEMENT — What's next for the audience?
   How can they participate, question, or verify?
```

---

## Step 3: Format-Specific Guidance

### Executive Memo (1 page maximum)

**Key rule: Bottom line up front. They may only read the first paragraph.**

```
TO:     [Name, Title]
FROM:   [Name, Office]
DATE:   [Date]
RE:     [Specific, actionable subject — not vague]

BOTTOM LINE
[1-2 sentences: Finding + Recommendation. Must stand alone.]

BACKGROUND
[2-3 sentences: Why this analysis was done. What triggered it.]

KEY FINDINGS
[Finding 1: One sentence of evidence + one sentence of implication.]
[Finding 2: Same structure.]
[Finding 3: Same structure.]

EQUITY IMPACT
[2-3 sentences: Who is most affected. Disproportionate burden. 
Implications of acting vs. not acting.]

RECOMMENDATION
[Specific action: Who does it / By when / Resources needed / Expected outcome]

REQUESTED DECISION
[One sentence: "Please approve..." with specific date.]
```

### Policy Brief (3-5 pages)

**Key rule: Evidence-rich but accessible. Support every claim.**

Structure: Executive Summary → The Problem → What the Data Shows → Equity Analysis → What Other Cities Have Done → Recommendations → Limitations & Next Steps → Data Sources

**See TEMPLATES.md for the complete fill-in-the-blank version.**

### Community Report / Fact Sheet (1-2 pages)

**Key rules:** Reading level = 8th grade. Use visuals. Include feedback mechanisms. Consider translation.

Boston's primary non-English languages: **Spanish, Haitian Creole, Chinese, Vietnamese, Portuguese, Cape Verdean Creole**

Structure: Plain-language headline → What we looked at → What we found → What this means for you → What's being done → How to participate → The data behind this

**See TEMPLATES.md for the complete fill-in-the-blank version.**

### Data Dashboard / Interactive Artifact

**When building as React artifact or HTML:**
- Lead with the most important metric
- Provide context (comparison values, trends, benchmarks)
- Allow neighborhood filtering at minimum
- Include "About This Data" section
- Use accessible color palettes (color-blind safe)
- Add last-updated date and data source attribution
- Also read `/mnt/skills/public/frontend-design/SKILL.md` for visual quality

### Presentation Deck (10-12 slides maximum)

One main point per slide. More visuals, fewer words. Speak the analysis, show the evidence.

```
Slide 1:  Title + One-Line Finding
Slide 2:  Why This Matters (the human hook)
Slide 3:  The Problem (human-centered framing)
Slide 4:  What We Looked At (brief — data, methods)
Slides 5-8: Key Findings (one per slide: visual + 3 bullet annotations)
Slide 9:  Equity Dimensions
Slide 10: Recommendations (specific, with ownership)
Slide 11: Discussion Questions (genuine — questions you want input on)
Slide 12: Data Sources & Contact
```

---

## Step 4: Inclusive Communication Standards

**Plain Language Checklist:**
- [ ] Main finding is clear within 30 seconds of reading
- [ ] No jargon without definition
- [ ] Active voice used throughout
- [ ] Sentences under 25 words
- [ ] Concrete examples used

**Equity Communication Rules:**
- Use asset-based framing, not deficit framing
- Name structural factors, not just individual behaviors
- Include community voice, not just institutional perspective

**Asset vs. Deficit Framing:**

| Instead of (Deficit) | Use (Asset-Based) |
|---------------------|-------------------|
| "Residents fail to report issues" | "Residents face barriers to accessing 311" |
| "High-crime neighborhoods" | "Neighborhoods with historically under-resourced public safety" |
| "Low-performing schools" | "Schools with unmet resource needs" |
| "Digital divide" | "Communities with fewer digital access opportunities" |

---

## Step 5: Engagement Design (GovLab Collective Intelligence)

Engagement should be **consequential**, not performative. Design for:

1. **Ask genuine questions** — Questions the audience can actually influence
2. **Provide data to ground conversation** — Share what you know and don't know
3. **Lower barriers** — Multiple channels (online, in-person, written, verbal), multiple languages, async options
4. **Close the loop** — Always report back: "Here's what we heard. Here's what we did. Here's why."
5. **Enable collective intelligence** — Combine institutional data with community knowledge

**Engagement Format Options:**

| Format | Best For | Reach | Depth |
|--------|---------|-------|-------|
| Community meeting | Deliberation, relationship-building | Low | High |
| Online survey | Broad input on specific questions | High | Low |
| Data walk / public dashboard | Data literacy, resident analysis | Moderate | Moderate |
| Co-design workshop | Solution development with affected communities | Low | Very High |
| Participatory budgeting | Resource allocation decisions | Moderate | High |

---

## Step 6: Open Governance Commitments

Every communication should reinforce open, transparent governance:

1. **Cite the data** — Dataset names, resource IDs, and Boston Open Data Portal link
2. **Describe the methodology** — Filters applied, assumptions made, alternatives considered
3. **Invite scrutiny** — "We welcome corrections and alternative interpretations"
4. **Enable reuse** — Share in formats others can build on

---

## Quality Standards

| Standard | What It Means |
|----------|--------------|
| **Accuracy** | Every number traceable to a specific data query; no claims exceed evidence |
| **Clarity** | Main point unmistakable in 30 seconds; technical terms defined |
| **Actionability** | Recommendations specific enough to implement (who/what/when) |
| **Inclusivity** | Plain-language version exists; engagement is genuine |
| **Transparency** | Data sources cited; methodology described; uncertainties acknowledged |

---

## Document Creation Notes

- For Word docs (.docx): Also read `/mnt/skills/public/docx/SKILL.md`
- For Presentations (.pptx): Also read `/mnt/skills/public/pptx/SKILL.md`
- For Spreadsheets (.xlsx): Also read `/mnt/skills/public/xlsx/SKILL.md`
- For Dashboards (React/HTML): Also read `/mnt/skills/public/frontend-design/SKILL.md`

## GovLab / InnovateUS Core Values
1. **Consequential engagement** — Communication leads to action, not just awareness
2. **Data as public infrastructure** — Open data belongs to everyone; make it usable by everyone
3. **Collective intelligence** — Best solutions combine institutional data with community knowledge
4. **Democratic legitimacy** — Transparency and participation build trust
5. **Co-design over consultation** — Build with communities, not just for them
6. **Iterate and learn** — Treat communication as a skill to improve, not a box to check
