# [Feature Name] Design Document

**Date**: YYYY-MM-DD
**Author**: [Name]
**Status**: Draft | In Review | Approved

---

## Overview

Brief description of what is being designed and why.

**Goal**: What problem does this solve?

**Non-Goals**: What is explicitly out of scope?

---

## Requirements

### Functional Requirements
- Requirement 1
- Requirement 2

### Non-Functional Requirements
- [Constraint 1 — e.g., "Must handle 500 concurrent users"]
- [Constraint 2 — e.g., "Must work offline"]

### Success Criteria
- How will success be measured?
- What does "done" look like?

---

## Approach Comparison

| Approach | Pros | Cons | Complexity | Chosen |
|----------|------|------|------------|--------|
| Option 1 | Benefits | Drawbacks | Low/Med/High | |
| Option 2 | Benefits | Drawbacks | Low/Med/High | |
| Option 3 | Benefits | Drawbacks | Low/Med/High | |

**Decision**: Why this approach was chosen.

**Assumptions challenged**:
- [Assumption tested and result]
- [Assumption tested and result]

---

## User Flow

### Primary Flow (Happy Path)

```
[Entry point] --> [Step 1] --> [Step 2] --> [Step 3] --> [Success state]
```

1. **Entry point**: How does the user arrive here? (navigation, link, redirect)
2. **[Step 1]**: What the user sees and does
3. **[Step 2]**: What the system responds with
4. **[Step 3]**: What the user confirms or completes
5. **Success state**: What the user sees when done

### Alternate Flows

- **[Alternate path A]**: If the user [does X instead], then [what happens]
- **[Alternate path B]**: If the user [cancels / goes back], then [what happens]

### Error & Edge Case Flows

- **[Error case 1]**: If [condition], user sees [error state / message / recovery option]
- **[Empty state]**: If no data exists yet, user sees [empty state with CTA]
- **[Permission denied]**: If user lacks access, they see [redirect / message]

### Key UX Decisions

<!-- Capture the "why" behind UX choices made during brainstorming — these are easy to lose. -->

- [Decision 1 — e.g., "Inline editing vs. modal: chose inline because users need to edit multiple rows quickly"]
- [Decision 2 — e.g., "Optimistic update on save: chose this over loading spinner because latency is low"]
- [Decision 3 — e.g., "Progressive disclosure: advanced filters hidden behind toggle to reduce cognitive load"]

---

## Research & Prior Art

<!-- Record the research findings that informed the chosen approach. This preserves the "why" so future developers understand what was evaluated and what evidence backed the decision. -->

### Reference Projects

| Project | Link | What We Learned |
|---------|------|-----------------|
| [Project 1] | [URL] | [Key insight or pattern borrowed] |
| [Project 2] | [URL] | [Key insight or pattern borrowed] |

### Libraries & Tools Evaluated

| Library | Verdict | Rationale |
|---------|---------|-----------|
| [Library A] | Adopted | [Why — e.g., "Handles fuzzy matching with 95%+ accuracy, MIT licensed, active maintenance"] |
| [Library B] | Rejected | [Why not — e.g., "GPL license incompatible with our project" or "Overkill for our scale"] |
| Build our own | Rejected | [Why not — e.g., "Would take 2 weeks to match what Library A provides out of the box"] |

### Patterns Applied

- **[Pattern name]** — [How it's used in this design and why. e.g., "Row-Level Security for tenant isolation — standard approach for multi-tenant SaaS, avoids application-layer filtering bugs"]
- **[Anti-pattern avoided]** — [What we explicitly chose NOT to do and why. e.g., "Avoided shared-schema-with-tenant-column without RLS — too easy to leak data across tenants"]

---

## Open Questions

- [ ] [Unresolved question 1]
- [ ] [Unresolved question 2]

---

## References

- [Link to related design docs, prior art, inspiration]
- [Link to feature-docs output once implementation planning begins]
