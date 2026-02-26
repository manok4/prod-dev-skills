# [Product Name] — Product Requirements Document

**Version**: [x.y]
**Date**: [YYYY-MM-DD]
**Status**: Draft | In Review | Approved
**Author**: [Name]
**Reviewers**: [Names]
**Related**: [Design Doc](../plans/YYYY-MM-DD-<topic>-design.md) — High-Level Design from brainstorming

---

## TL;DR

<!-- 2-3 sentences. What is this product, who is it for, and what problem does it solve? Write this last — it should be the elevator pitch distilled from the full document. -->

---

## Table of Contents

<!-- Auto-generate or manually list sections. Include only sections relevant to your project. -->

---

## 1. Problem Statement

### Context

<!-- Describe the current state. What exists today? What process or system is in place? Who are the people involved and what are their roles? Keep it factual — no solution language yet. -->

### Pain Points

<!-- Bulleted list of specific, measurable problems. Each pain point should be grounded in evidence (user interviews, data, observed behavior). Quantify where possible. -->

- [Pain point 1 — with data if available]
- [Pain point 2]
- [Pain point 3]

### Narrative (Day in the Life)

<!-- Tell the story of a real user going through the current painful process. 1-2 paragraphs. This makes the problem tangible for stakeholders who haven't lived it. Then briefly describe how the future state changes this experience. -->

---

## 2. Goals

### Business Goals

<!-- Measurable outcomes the business expects. Use the format: [Verb] + [metric] + [target] + [timeframe]. -->

- [ ] [Goal 1 — e.g., "Reduce reconciliation error rate by 40% within 6 months of launch"]
- [ ] [Goal 2]
- [ ] [Goal 3]

### User Goals

<!-- What do users want to achieve? Written from their perspective. -->

- [User goal 1 — e.g., "Reconcile weekly reports without manual spreadsheet work"]
- [User goal 2]
- [User goal 3]

### Non-Goals (Explicit Scope Exclusions)

<!-- What this product intentionally does NOT do. Prevents scope creep and sets expectations. -->

- [Non-goal 1 — e.g., "No agent-facing mobile application"]
- [Non-goal 2]
- [Non-goal 3]

---

## 3. Users & Stakeholders

### Personas

<!-- Define the primary and secondary personas. For each: role, access level, key needs, and how they interact with the system. -->

| Persona | Role | Access Level | Primary Need |
|---------|------|-------------|--------------|
| [Persona 1] | [Role description] | [Access tier] | [What they need from this product] |
| [Persona 2] | | | |

### User Stories

<!-- Format: "As a [persona], I want [action], so that [outcome]." Group by persona. Prioritize: Must Have / Should Have / Nice to Have. -->

**[Persona 1]**

- *Must Have*: As a [persona], I want [action], so that [outcome].
- *Must Have*: As a [persona], I want [action], so that [outcome].
- *Should Have*: As a [persona], I want [action], so that [outcome].

**[Persona 2]**

- *Must Have*: As a [persona], I want [action], so that [outcome].

---

## 4. Business Rules & Domain Logic

<!-- Document the domain-specific rules that govern how the system behaves. These are NOT features — they are constraints and truths about the business that the system must respect. Skip this section if the domain is straightforward. -->

- [Rule 1 — e.g., "Medicare ID is the universal unique identifier across all systems"]
- [Rule 2 — e.g., "An agent NPN is unique within a tenant, not globally"]
- [Rule 3]

---

## 5. Functional Requirements

<!-- Group by feature area. Each group gets a priority (Critical | High | Medium | Low) and a brief description of what it does, NOT how it's implemented. Implementation details belong in the TDD. -->

### 5.1 [Feature Group Name] — Priority: [Critical|High|Medium|Low]

**Description**: [1-2 sentences — what this feature does and why it matters.]

- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

### 5.2 [Feature Group Name] — Priority: [Critical|High|Medium|Low]

**Description**: [1-2 sentences.]

- [Requirement 1]
- [Requirement 2]

<!-- Repeat for each feature group. -->

---

## 6. User Experience

### 6.1 Core User Journey

<!-- Step-by-step walkthrough of how the primary persona uses the product. This is the "happy path." Number each step. -->

1. **[Step name]**: [What the user does and what the system responds with.]
2. **[Step name]**: [...]
3. **[Step name]**: [...]

### 6.2 Pages & Views

<!-- List each page/screen with its key elements. This is NOT a wireframe — it's a feature inventory per page. -->

| Page | Key Elements | Accessible By |
|------|-------------|---------------|
| [Page 1] | [KPI cards, charts, filters] | [Roles] |
| [Page 2] | [Data grid, export, drill-down] | [Roles] |

### 6.3 Role-Based Access Matrix

<!-- Which roles can do what. Rows = features, columns = roles. -->

| Feature | [Role 1] | [Role 2] | [Role 3] |
|---------|----------|----------|----------|
| [Feature 1] | Full | Read | None |
| [Feature 2] | Full | Full | Read |

### 6.4 UX Considerations

<!-- Accessibility, onboarding, error handling UX, responsive design, etc. -->

- **Accessibility**: [Requirements — e.g., WCAG 2.1 AA, keyboard navigation, screen reader support]
- **Onboarding**: [First-time user experience — e.g., guided walkthrough, sample data]
- **Error states**: [How the system communicates errors — e.g., descriptive messages, retry options]

---

## 7. Success Metrics

### Key Performance Indicators

<!-- Metrics that prove the product is working. Tie back to Business Goals in Section 2. -->

| Metric | Baseline (Current) | Target | Measurement Method |
|--------|-------------------|--------|-------------------|
| [Metric 1] | [Current value] | [Target value] | [How measured] |
| [Metric 2] | | | |

### Tracking Plan

<!-- What events/data points will be instrumented to measure the KPIs above. -->

- [Event 1 — e.g., "File upload events: count, type, errors, deduplication triggers"]
- [Event 2 — e.g., "Match tier distribution per import batch"]
- [Event 3]

---

## 8. Technical Direction

<!-- Capture preferences and constraints that inform the Technical Design Document. This is NOT an architecture section — detailed technical design (schema, APIs, deployment) belongs in the TDD. -->

### Stack Preferences

- **Backend**: [Preference or "Open to recommendations"]
- **Frontend**: [Preference or "Open to recommendations"]
- **Database**: [Preference or "Open to recommendations"]
- **Hosting**: [Preference — e.g., "Cloud PaaS", "Self-hosted", "No preference"]

### Data Sources

<!-- High-level overview of what data the system consumes. Field-level detail belongs in the TDD. -->

| Source | Format | Frequency | Key Identifier |
|--------|--------|-----------|---------------|
| [Source 1] | [CSV/Excel/API] | [Weekly/Daily/Real-time] | [Primary key field] |

### Security & Compliance

- **Auth model**: [e.g., "Role-based access, admin-created users"]
- **Compliance**: [e.g., "HIPAA", "SOC 2", "None specific"]
- **Data isolation**: [e.g., "Multi-tenant", "Single-tenant"]

### Budget Constraints

- **Infrastructure budget**: [e.g., "Free tier to start", "$50/mo", "No constraint"]
- **Scaling expectations**: [e.g., "10 users initially, ~100 within a year"]

---

## 9. Implementation Roadmap

### Phases

#### Phase 1: [Name]
**Deliverables**:
- [ ] [Deliverable 1]
- [ ] [Deliverable 2]

**Dependencies**: [What must exist before this phase starts]

**Exit criteria**: [How you know this phase is done]

#### Phase 2: [Name]
**Deliverables**:
- [ ] [Deliverable 1]
- [ ] [Deliverable 2]

**Dependencies**: [Phase 1 complete, plus any external dependencies]

**Exit criteria**: [...]

<!-- Repeat for each phase. -->

---

## 10. Risks & Mitigations

<!-- Focus on product and business risks. Technical risks belong in the TDD. -->

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | [High/Med/Low] | [High/Med/Low] | [Specific mitigation strategy] |
| [Risk 2] | | | |

---

## 11. Open Questions

<!-- Decisions that haven't been made yet. Track who owns each question and when it needs resolution. -->

| Question | Owner | Needed By | Resolution |
|----------|-------|-----------|------------|
| [Question 1] | [Name] | [Phase X] | [Pending / Resolved: answer] |

---

## Appendices

### A. Glossary

<!-- Domain-specific terms and their definitions. -->

| Term | Definition |
|------|-----------|
| [Term 1] | [Definition] |

### B. Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| [1.0] | [Date] | [Name] | [Initial draft] |
