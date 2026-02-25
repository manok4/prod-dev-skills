# [Product Name] — Product Requirements Document

**Version**: [x.y]
**Date**: [YYYY-MM-DD]
**Status**: Draft | In Review | Approved
**Author**: [Name]
**Reviewers**: [Names]

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

<!-- Group by feature area. Each group gets a priority (Critical | High | Medium | Low) and a brief description of what it does, NOT how it's implemented. Implementation details go in Section 8. -->

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

## 8. Technical Architecture

<!-- This section bridges product requirements to engineering. Detailed enough that an engineer can start building, but not so deep that it belongs in a separate design doc. For large systems, link to a separate Technical Design Document. -->

### 8.1 System Architecture

<!-- High-level architecture diagram (ASCII, Mermaid, or link to diagram tool). Show major components and data flow. -->

```
[Architecture diagram here]
```

### 8.2 Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| [Backend] | [e.g., FastAPI, Python 3.12+] | [API, ETL processing] |
| [Frontend] | [e.g., Vite + React, TypeScript] | [SPA dashboard] |
| [Database] | [e.g., PostgreSQL 16] | [Primary data store] |

### 8.3 Data Sources

<!-- For each external data source: file format, frequency, key fields, and known quirks. -->

| Source | Format | Frequency | Key Identifier | Notable Quirks |
|--------|--------|-----------|---------------|----------------|
| [Source 1] | [CSV] | [Weekly] | [Field name] | [e.g., "Date field uses Excel serial numbers"] |

<!-- If field mappings are extensive, move them to an appendix. -->

### 8.4 Database Schema

<!-- Core tables with key columns, relationships, and indexing strategy. Full DDL goes in an appendix or linked design doc. -->

#### [Table Name]
- **Scope**: [Global | Tenant-scoped]
- **Purpose**: [1 sentence]
- **Key columns**: [List the important ones]
- **Indexes**: [Notable indexes]

### 8.5 API Design

<!-- List core endpoint groups. Full endpoint documentation goes in an API spec (OpenAPI/Swagger). -->

| Group | Key Endpoints | Auth Required |
|-------|--------------|---------------|
| [Auth] | `POST /auth/login`, `POST /auth/refresh` | No (login), Yes (refresh) |
| [Resource 1] | `GET /resource`, `GET /resource/{id}` | Yes |

### 8.6 Security & Data Privacy

- **Authentication**: [Method — e.g., JWT with refresh tokens]
- **Authorization**: [Strategy — e.g., RLS + API middleware, role hierarchy]
- **Data isolation**: [How tenant data is separated]
- **PII handling**: [What PII exists, how it's protected]
- **Secrets management**: [env vars, no secrets in code/git]

### 8.7 Integration Points

<!-- External systems this product connects to, and how. -->

| System | Integration Method | Direction | Frequency |
|--------|-------------------|-----------|-----------|
| [System 1] | [Batch CSV import] | [Inbound] | [Weekly] |

---

## 9. Infrastructure & Cost

### Deployment

| Service | Provider | Tier | Est. Cost |
|---------|----------|------|-----------|
| [Backend] | [e.g., Railway] | [Starter] | [~$5/mo] |
| [Frontend] | [e.g., Netlify] | [Free] | [$0] |
| [Database] | [e.g., Neon] | [Free] | [$0] |

### Cost Scaling Triggers

<!-- When does cost jump? What triggers an upgrade? -->

| Trigger | Current State | Upgrade Path | New Cost |
|---------|--------------|-------------|----------|
| [e.g., DB > 5 GB] | [0 GB] | [Paid tier] | [$X/mo] |

### Environment Variables

<!-- List required env vars (NO actual values). -->

```
DATABASE_URL=<connection string>
JWT_SECRET_KEY=<random key>
```

---

## 10. Implementation Roadmap

### Team Composition

| Role | Count | Responsibilities |
|------|-------|-----------------|
| [Backend engineer] | [1-2] | [ETL, API, matching engine] |
| [Frontend engineer] | [1] | [React dashboard, data grids] |

### Phases

#### Phase 1: [Name] — [Duration]

**Deliverables**:
- [ ] [Deliverable 1]
- [ ] [Deliverable 2]

**Dependencies**: [What must exist before this phase starts]

**Exit criteria**: [How you know this phase is done]

#### Phase 2: [Name] — [Duration]

**Deliverables**:
- [ ] [Deliverable 1]
- [ ] [Deliverable 2]

**Dependencies**: [Phase 1 complete, plus any external dependencies]

**Exit criteria**: [...]

<!-- Repeat for each phase. -->

---

## 11. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | [High/Med/Low] | [High/Med/Low] | [Specific mitigation strategy] |
| [Risk 2] | | | |

---

## 12. Open Questions

<!-- Decisions that haven't been made yet. Track who owns each question and when it needs resolution. -->

| Question | Owner | Needed By | Resolution |
|----------|-------|-----------|------------|
| [Question 1] | [Name] | [Phase X] | [Pending / Resolved: answer] |

---

## Appendices

### A. Data Source Field Mappings

<!-- Detailed field-by-field mapping tables for each data source. Move here to keep the main document scannable. -->

### B. Glossary

<!-- Domain-specific terms and their definitions. -->

| Term | Definition |
|------|-----------|
| [Term 1] | [Definition] |

### C. Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| [1.0] | [Date] | [Name] | [Initial draft] |
