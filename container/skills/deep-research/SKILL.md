---
name: deep-research
description: Conduct comprehensive multi-source research with citation tracking and verification. Use when user needs thorough analysis, synthesis, competitive analysis, market research, or investigation requiring 10+ sources. Triggers on "deep research", "research thoroughly", "comprehensive analysis", "investigate". Do NOT use for simple lookups or quick facts.
---

# Deep Research

A 7-phase research system that produces decision-grade, auditable, citation-backed research outputs. Adapted from standardhuman/deep-research-skill and 199-biotechnologies patterns.

## Decision Gate (Execute First)

```
Is this a simple lookup? → STOP. Use WebSearch directly.
Is this debugging? → STOP. Use standard tools.
Does this need 10+ sources? → CONTINUE with this skill.
```

## Research Types

| Type | When | Time | Approach |
|------|------|------|----------|
| **A: Lookup** | Single fact, known source | 1-2 min | WebSearch only |
| **B: Synthesis** | Aggregation, no judgment | 5-15 min | 3-5 parallel searches |
| **C: Analysis** | Judgment, perspectives needed | 15-30 min | Plan + parallel deep search |
| **D: Investigation** | Novel, conflicting evidence | 30-60 min | Full 7-phase pipeline |

## The 7-Phase Pipeline

### Phase 0: Classify
Determine research type (A/B/C/D) from the question. Type A stops here.

### Phase 1: Scope
Capture or infer:
- Core question (one sentence)
- Use case (what decision does this inform?)
- Audience (technical / executive / general)
- Scope (geography, timeframe, inclusions/exclusions)
- Citation level (strict / standard / light)

### Phase 1.5: Hypothesize
Generate 3-5 testable hypotheses:
- What are the likely answers?
- What evidence would confirm/disconfirm each?
- Track probability as evidence accumulates

### Phase 2: Plan
- Break into 3-7 sub-questions
- Plan search queries for each
- Identify source types needed (academic, news, government, industry)
- Set budgets (max searches per sub-question)

**Tool Selection Per Sub-Question:**
- Factual data → WebSearch (fast)
- Full article content → WebFetch top URLs
- Interactive/paywalled → agent-browser
- Academic papers → Python with Semantic Scholar/arXiv APIs
- Business data → Python with SEC EDGAR/OpenCorporates APIs

### Phase 3: Execute
Run searches in parallel where possible. For each source:
- Rate quality: A (systematic reviews) → E (SEO spam)
- Extract key claims with page references
- Note contradictions immediately

### Phase 4: Triangulate
**The 2-Source Rule:**
- Critical claims need 2+ independent sources
- 5 articles citing 1 report = 1 source, not 5
- Contradictions are data — document them, don't hide them

### Phase 5: Synthesize
Write the report with these required sections:
- Executive summary (2-3 paragraphs, <250 words)
- Findings by sub-question (evidence-rich prose, not bullet lists)
- Synthesis and insights (patterns, implications)
- Limitations and caveats (gaps, uncertainties)
- Recommendations (if applicable)
- Sources (every URL cited, with quality grade)

### Phase 6: Quality Check
Before delivering, verify:
- [ ] Every claim has a source URL
- [ ] Critical claims have 2+ independent sources
- [ ] Contradictions are explained, not hidden
- [ ] Confidence levels are assigned where appropriate
- [ ] No placeholder text (TBD, TODO, [citation needed])
- [ ] Prose-first: <20% bullet points, >80% flowing narrative

### Phase 7: Deliver
- Save report to `/workspace/group/reports/{topic}-{date}.md`
- Update `/workspace/group/reports/INDEX.md`
- Present executive summary in chat
- Offer to dive deeper into any section

## Source Quality Ratings

| Grade | Examples |
|-------|---------|
| **A** | Systematic reviews, RCTs, official regulations, government data |
| **B** | Cohort studies, reputable journalism, industry reports |
| **C** | Expert opinions, conference talks, white papers |
| **D** | Preprints, blog posts from domain experts |
| **E** | Anecdotal, speculative, SEO content |

## Claim Types

| Type | Verification |
|------|-------------|
| **C1 Critical** | Full citation + 2-source verification + confidence tag |
| **C2 Supporting** | Citation required |
| **C3 Context** | Cite if non-obvious |

## Anti-Hallucination Rules

- Every factual claim MUST cite a specific source: "According to [Source](URL)..."
- Distinguish FACTS (from sources) from SYNTHESIS (your analysis)
- Never fabricate citations. Say "No sources found for X" if you can't find it.
- Label speculation: "This suggests..." not "Research shows..."
- Prefer recent sources (current year)

## Writing Standards

- **Narrative-driven**: Flowing prose, not bullet dumps
- **Precision**: "reduced mortality 23% (p<0.01)" not "significantly improved"
- **Economy**: No fluff. Every word carries intention.
- **Honest about gaps**: "I couldn't find data on X" is acceptable

## Report Template

```markdown
# {Topic}

## Executive Summary
{2-3 paragraphs: core findings, key stats, main uncertainties}

## {Sub-Question 1}
{Evidence-rich prose with inline citations}

## {Sub-Question 2}
{...}

## Synthesis & Insights
{Patterns, novel insights, implications beyond individual sources}

## Limitations
{Gaps, conflicting sources, uncertainties, assumptions}

## Recommendations
{If applicable: immediate actions, next steps, further research needed}

## Sources
{All URLs cited, with quality grade}
[1] Author/Org (Year). "Title". [Grade A] URL
[2] ...
```
