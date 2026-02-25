---
name: prd-docs
description: "Product Requirements Document generator. Use when the user wants to create, write, or draft a PRD for a new product, feature, or system. Triggers on: 'write a PRD', 'create a PRD', 'product requirements document', 'I need a PRD', 'draft requirements for', or any request to produce a structured product specification. Guides the user through collaborative discovery — asking questions one at a time — then produces a complete PRD using a proven template. Also triggers when the user says '/prd-docs'."
---

# PRD Document Generator

Generate complete, production-quality Product Requirements Documents through collaborative discovery. Ask targeted questions to extract requirements, then produce a structured PRD covering problem, goals, users, features, UX, technical architecture, risks, and roadmap.

<HARD-GATE>
Do NOT write the PRD until you have completed the discovery phases and the user has confirmed the gathered information is sufficient. Every PRD goes through discovery first — no exceptions, regardless of how much context the user provides upfront.
</HARD-GATE>

## Process

```
[1] Orient        → Understand what exists (files, docs, prior context)
[2] Discover      → Ask questions phase-by-phase (one at a time)
[3] Synthesize    → Present a structured summary for approval
[4] Draft         → Write the PRD using the template
[5] Review        → Walk through each section, revise per feedback
[6] Deliver       → Save to docs/ directory
```

## Phase 1: Orient

Before asking anything, check for existing context:
- Read any files the user has referenced or linked
- Check for existing PRDs, design docs, or READMEs in the project
- Look at `docs/` and `claudedocs/` directories if they exist
- Scan CLAUDE.md for project conventions

Summarize what you already know and what gaps remain. This prevents re-asking things the user has already documented.

## Phase 2: Discover

Work through the discovery topics below **one question at a time**. Adapt based on what you already know from Phase 1 — skip questions whose answers are already clear.

**Questioning rules:**
- One question per message — never batch multiple questions
- Prefer multiple-choice (via AskUserQuestion) when options are enumerable
- Use open-ended questions for narrative/context topics
- Lead with the most impactful question in each topic
- Skip topics the user has already answered through provided context
- If the user says "that's enough" or "just write it", move to Phase 3 with what you have

**Discovery topics (in order):**

See [references/questioning-guide.md](references/questioning-guide.md) for the full question bank organized by topic. The topics are:

1. **Product identity** — What is it, who is it for, what problem does it solve
2. **Current state** — What exists today, what's broken, what's the pain
3. **Goals & success** — Business outcomes, user outcomes, measurable targets
4. **Scope boundaries** — What's explicitly out of scope (non-goals)
5. **Users & personas** — Who uses it, what roles exist, what permissions
6. **Domain rules** — Business logic, constraints, data relationships
7. **Features & priorities** — What it does, grouped and prioritized
8. **Data & integrations** — Data sources, formats, external systems
9. **UX & frontend** — Key pages, user journeys, accessibility needs
10. **Technical direction** — Stack preferences, hosting, security model
11. **Delivery & risks** — Phasing, team, budget, known risks

**Adaptive depth:** Not every PRD needs all 11 topics explored deeply. A small feature PRD may only need topics 1-5 and 7. A platform PRD needs all of them. Gauge from the user's answers how deep to go.

## Phase 3: Synthesize

Once discovery is sufficient, present a structured summary:

```
## Discovery Summary

**Product**: [name / one-liner]
**Problem**: [1-2 sentences]
**Target users**: [personas list]
**Key goals**: [3-5 bullet points]
**Non-goals**: [what's excluded]
**Core features** (prioritized):
  1. [Critical] ...
  2. [High] ...
  3. [Medium] ...
**Technical direction**: [stack, hosting, key constraints]
**Open questions**: [anything unresolved]
```

Ask the user: "Does this capture it? Anything to add or change before I draft the PRD?"

Revise until approved.

## Phase 4: Draft

Use the PRD template in [references/prd-template.md](references/prd-template.md) as the structural backbone. Apply these rules:

- **Include only sections that are relevant** — delete template sections that don't apply
- **Scale section depth to complexity** — a simple feature gets 1-2 sentences per section; a platform gets full detail
- **Use the user's language** — mirror their terminology, don't impose jargon
- **No placeholder text** — every section must have real content or be removed. No "[TODO]" in the output.
- **No invented metrics** — if the user didn't provide a number, don't fabricate one. Write "TBD" or "To be validated" instead.
- **Business rules get their own section** — if the domain has non-obvious rules, include Section 4
- **Appendices for bulk detail** — field mappings, data schemas, glossaries go in appendices to keep the main doc scannable

## Phase 5: Review

Present the PRD section by section, asking after each major group:
- Sections 1-3 (Problem, Goals, Users) — "Does the framing look right?"
- Sections 4-6 (Domain, Features, UX) — "Are the features and priorities accurate?"
- Sections 7-11 (Technical, Cost, Roadmap, Risks) — "Does the technical direction and plan work?"

Revise per feedback before moving on.

## Phase 6: Deliver

Save the final PRD to the project's documentation directory:
- Default path: `docs/PRD-<product-name>.md`
- If `claudedocs/` exists and is the project convention, use that instead
- Ask the user if they want it saved elsewhere

## Key Principles

- **One question at a time** — never overwhelm with batched questions
- **Multiple choice when possible** — faster for the user, more structured for you
- **Skip what you know** — if context was provided, don't re-ask it
- **Adapt depth to scope** — small feature = lightweight PRD, platform = comprehensive
- **No fabrication** — unknown metrics are "TBD", not invented numbers
- **User's words** — use their terminology, not generic product-speak
