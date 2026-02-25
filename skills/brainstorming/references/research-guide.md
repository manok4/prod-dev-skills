# Research Guide

How to research prior art, reference implementations, and proven patterns before proposing approaches. Run this after understanding the idea but before proposing solutions.

---

## When to Research

**Always research when:**
- Building a feature that likely exists in other projects (auth, dashboards, ETL, chat, etc.)
- Choosing between libraries, frameworks, or architectural patterns
- The problem space is unfamiliar or has known complexity (concurrency, matching, real-time sync)
- The user hasn't specified an approach and is open to recommendations

**Skip research when:**
- The problem is purely internal (reorganizing files, renaming, config changes)
- The user has already researched and is sharing their findings
- It's a trivial change where research adds no value
- Time pressure makes research impractical (user said "keep it quick")

---

## Research Protocol

### Step 1: Find Reference Projects

Search for open source projects that solve the same or a similar problem. Use WebSearch with targeted queries:

**Query patterns:**
- `"[problem domain] open source [language/framework]"` — e.g., "commission reconciliation open source python"
- `"[feature type] github [tech stack]"` — e.g., "ETL pipeline github fastapi polars"
- `"awesome [domain]" github` — e.g., "awesome data-pipeline github" (curated lists)
- `"[specific pattern] example implementation"` — e.g., "fuzzy name matching example implementation python"
- `site:github.com [problem] [language]` — restrict to GitHub

**What to extract from each project:**
- Architecture pattern used (monolith, microservices, event-driven, etc.)
- Key libraries and why they were chosen
- Schema design or data model approach
- What worked well (based on stars, issues, and documentation quality)
- What didn't work (based on open issues, abandoned forks, or migration notices)

**Present to user as:**
> "I found **[project name]** ([link]) — it solves [similar problem] using [approach]. Key takeaway: [what we can learn]. It has [X stars / active maintenance / known issues]."

### Step 2: Find Libraries and Tools

Search for libraries that handle the hard parts of the problem:

**Query patterns:**
- `"best [language] library for [task]"` — e.g., "best python library for fuzzy matching"
- `"[library A] vs [library B] [year]"` — e.g., "polars vs pandas 2025"
- `"[framework] [feature] plugin"` — e.g., "fastapi file upload plugin"

**Use Context7** (if available) to look up current documentation for libraries already in the project or being considered. Never rely on training data for API usage — always verify.

**What to extract:**
- What the library does and its maturity (version, last release, download count)
- API surface — is it simple or complex to integrate?
- Trade-offs vs. building it yourself
- Licensing (MIT/Apache = safe, GPL = check with user, proprietary = flag)

**Present to user as:**
> "For [task], **[library]** handles this well — [what it does]. It's [actively maintained / widely used / MIT licensed]. Alternative: **[library B]** which [trade-off]. Or we build it ourselves, which [trade-off]."

### Step 3: Find Patterns and Anti-Patterns

Search for established patterns in the problem space:

**Query patterns:**
- `"[problem] design pattern"` — e.g., "multi-tenant data isolation design pattern"
- `"[problem] best practices [year]"` — e.g., "commission tracking best practices 2025"
- `"[problem] anti-patterns"` — e.g., "ETL pipeline anti-patterns"
- `"how [company] built [feature]"` — e.g., "how stripe built commission tracking" (engineering blogs)

**What to extract:**
- Named patterns (e.g., "saga pattern" for distributed transactions, "RLS" for tenant isolation)
- Common mistakes others made and how to avoid them
- Performance or scale considerations from real-world deployments

**Present to user as:**
> "The standard approach for [problem] is [pattern name] — it works by [brief explanation]. Common pitfall: [anti-pattern]. [Company X] wrote about their experience with this: [key insight]."

---

## Research Output Format

After researching, present a concise summary before proposing approaches:

```
## Research Findings

**Reference projects:**
- [Project 1] ([link]) — [what it does, what we can learn]
- [Project 2] ([link]) — [what it does, what we can learn]

**Relevant libraries:**
- [Library A] — [what it does, maturity, trade-off]
- [Library B] — [alternative, trade-off]

**Patterns discovered:**
- [Pattern name] — [brief description, when to use it]
- [Anti-pattern] — [what to avoid and why]

**Key insight:** [The single most important thing the research revealed]
```

Keep it short — 3-5 bullet points total is ideal. The goal is to inform the approach proposals, not to write a literature review.

---

## Connecting Research to Proposals

When proposing 2-3 approaches (the next step), explicitly reference research findings:

- "Based on how [project X] handles this, Option A uses [pattern]..."
- "Option B leverages [library] which handles [hard part] for us..."
- "Option C avoids the [anti-pattern] we saw in [project Y] by..."

This grounds proposals in evidence rather than intuition.
