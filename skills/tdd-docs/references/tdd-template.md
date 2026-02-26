# [Product Name] — Technical Design Document

**Version**: [x.y]
**Date**: [YYYY-MM-DD]
**Status**: Draft | In Review | Approved
**Author**: [Name]
**Change Log**: [Brief change description]
**Related**: [PRD-ProductName.md](PRD-ProductName.md) — Product Requirements Document

---

## Table of Contents

<!-- Auto-generate or manually list sections. Include only sections relevant to your project. -->

---

## 1. Executive Summary

### Problem

<!-- 1-2 paragraphs summarizing the business problem from the PRD. Keep it concise — this is context for engineers, not the full problem statement. Reference the PRD for complete business context. -->

### Solution

<!-- Numbered list of what the system does at the highest level. Focus on technical capabilities, not business outcomes. Each item should be one sentence. -->

### Key Metrics

<!-- Quantitative parameters that drive technical decisions: data volume, processing cadence, user count, tenant count, growth expectations. These are the numbers that inform architecture choices. -->

- **Data volume**: [e.g., ~5,000 records/week across all sources]
- **Processing cadence**: [e.g., Weekly batch + on-demand uploads]
- **Users**: [e.g., 5-10 internal users, non-technical]
- **Tenants**: [e.g., Starting with 1, designed for N]
- **Key rate**: [e.g., ~25% breakage rate, 99% auto-match target]

---

## 2. PRD Context

<!-- Brief cross-reference to the PRD. Do NOT reproduce full business context, workflows, or stakeholder tables — those live in the PRD. Only list the specific business rules and role details that directly drive technical decisions in this document. -->

**Source PRD**: [PRD-ProductName.md](PRD-ProductName.md)

### Business Rules Affecting Technical Design

<!-- Only rules that drive schema decisions, validation logic, or processing flows. Reference PRD Section 4 for the complete list. -->

- [Rule — e.g., "Medicare ID is the universal unique identifier" → drives PK strategy and cross-source matching]
- [Rule — e.g., "Agent NPN is unique per tenant, not globally" → drives composite unique constraint]

### Roles & Access Levels

<!-- Summary of roles relevant to auth implementation. See PRD Section 3 for full personas and user stories. -->

- **[Role 1]** — [access level relevant to auth/RLS design]
- **[Role 2]** — [access level]

---

## 3. Data Sources

<!-- One subsection per data source. Each includes: file pattern, source/frequency, full field table, and key observations. This level of detail is necessary for parser implementation. -->

### 3.X [Source Name]

**File**: `[file pattern]`
**Source**: [Where it comes from and how often]
**Volume**: [Records per period]
**Description**: [1-2 sentences on what this data represents]

| Field | Type | Example | Notes |
|-------|------|---------|-------|
| `field_name` | [Type] | [Example value] | [Parsing notes, format quirks] |

**Key observations**:
- [Observation about format, naming, quirks]
- [Fields that need special handling]
- [Relationships to other sources]

<!-- Repeat for each data source. -->

---

## 4. System Architecture

<!-- ASCII diagram showing the full system: data sources → backend layers → database → frontend. Include tenant context, ETL pipeline, auth, and external integrations. This is the single most important diagram in the document. -->

```
[Architecture diagram here — use ASCII box drawing characters]
```

---

## 5. Technology Stack

### 5.0 Project Foundation

<!-- If scaffolded from a template or existing project, document what the template provides vs. what this project adds. Include a decision table for any choices that deviate from the template defaults. -->

**What the template/foundation provides:**
- [Item 1]
- [Item 2]

**What this project adds on top:**
- [Item 1]
- [Item 2]

**Key decisions (deviations from defaults):**

| Decision | Default/Original | Chosen Approach | Rationale |
|----------|-----------------|-----------------|-----------|
| [Decision] | [Default] | [Choice] | [Why] |

### Backend

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| [Component] | **[Technology]** | [Version] | [Purpose] |

### Frontend

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| [Component] | **[Technology]** | [Version] | [Purpose] |

### Database

| Component | Technology | Purpose |
|-----------|-----------|---------|
| [Component] | **[Technology]** | [Purpose] |

### Infrastructure

| Component | Technology | Cost | Purpose |
|-----------|-----------|------|---------|
| [Component] | **[Technology]** | [Cost] | [Purpose] |

---

## 6. Database Schema

<!-- Full DDL for every table. Group by scope (global, tenant-scoped, platform). Include indexes, constraints, and RLS policies where applicable. This section should be copy-pasteable into a migration file. -->

### 6.0 Data Architecture Overview

<!-- If multi-tenant or multi-scope, document the isolation strategy first. -->

| Table | Scope | Rationale |
|-------|-------|-----------|
| `table_name` | [Global / Tenant-scoped / Platform] | [Why this scope] |

<!-- If using RLS or similar isolation: -->

```sql
-- Example isolation policy
[SQL for RLS, tenant filtering, or equivalent]
```

### 6.X [Table Group Name]

#### `table_name`
[1-sentence purpose. Scope notation.]

```sql
CREATE TABLE table_name (
    -- Full DDL here
);

CREATE INDEX idx_name ON table_name(column);
```

<!-- Repeat for each table. Include role/permission tables, audit tables, and any lookup/reference tables. -->

---

## 7. ETL Pipeline

### 7.1 Pipeline Overview

<!-- ASCII flow diagram showing the full ETL pipeline from file upload to database writes to post-processing. Number each step. -->

```
[Pipeline diagram here]
```

### 7.2 Parser Specifications

<!-- Table summarizing each parser: source file pattern, key challenges, configuration dependencies. -->

| Parser | Source File Pattern | Key Challenges | Config Used |
|--------|-------------------|----------------|-------------|
| `[ParserName]` | `[pattern]` | [Challenges] | [Config lookups] |

### 7.3 Data Normalization Rules

<!-- Tables showing normalization mappings for: names, dates, codes, statuses, or any domain-specific values that vary across sources. -->

#### [Normalization Category]
| Source Format | Example | Normalized |
|---------------|---------|------------|
| [Format] | [Example] | [Result] |

<!-- Repeat for each normalization category. -->

---

## 8. [Domain-Specific Processing Section]

<!-- Name this section based on the domain's core processing logic. Examples: "Record Matching & Reconciliation", "Order Processing Pipeline", "Risk Scoring Engine". Include algorithm pseudocode, decision trees, and confidence thresholds. -->

### 8.1 [Algorithm/Strategy Name]

<!-- Pseudocode or structured description of the core algorithm. Include tiers, thresholds, and fallback strategies. -->

#### [Tier/Step 1]
```
IF [condition]
THEN → [action] (confidence: [score])
```

### 8.2 [Processing Flow]

<!-- ASCII diagram showing the end-to-end processing flow for the domain's core operation. -->

```
[Flow diagram here]
```

---

## 9. [Domain-Specific Tracking Section]

<!-- Name based on what the system tracks. Examples: "Commission Tracking", "Inventory Management", "Subscription Lifecycle". Include type breakdowns, calculation rules, and edge cases. -->

### 9.1 [Type Breakdown]

| Type | Description | Recipient | Timing |
|------|------------|-----------|--------|
| [Type] | [Description] | [Who] | [When] |

### 9.2 [Calculation/Processing Rules]

<!-- Tables or rules for amounts, rates, and edge cases. -->

| [Category] | [Sub-category] | [Amount/Rule] | [Direction/Effect] |
|------------|----------------|----------------|-------------------|
| [Value] | [Value] | [Value] | [Value] |

### 9.3 [Edge Case Rules]

<!-- Clawbacks, reversals, cancellations, or domain-specific edge cases. -->

- [Rule 1]
- [Rule 2]

---

## 10. [Secondary Domain Section — Optional]

<!-- If the system has a second major domain concern (e.g., funding requests alongside commissions), give it its own section. Follow the same pattern: workflow, business rules, cross-cutting concerns. -->

### 10.1 Workflow

```
[Workflow diagram]
```

### 10.2 Business Rules

- [Rule 1]
- [Rule 2]

---

## 11. Frontend Application

### 11.1 [Access/Tenant Model]

<!-- If the frontend has role-based or tenant-based behavior differences, document them here. -->

| User Role | Context | UI Behavior |
|-----------|---------|-------------|
| [Role] | [Context] | [Behavior] |

### 11.2 Pages & Views

<!-- One subsection per page/view. Include key elements, data sources, and interactions. -->

#### [Page Name]
- **[Element group]**: [Description of elements]
- **[Element group]**: [Description]
- **[Interactions]**: [Key user interactions]

### 11.3 Role-Based Access

| Feature | [Role 1] | [Role 2] | [Role 3] |
|---------|----------|----------|----------|
| [Feature] | [Access] | [Access] | [Access] |

---

## 12. [Integration/AI Section — Optional]

<!-- For LLM integrations, external API integrations, or complex system integrations. Include architecture diagrams, safety guardrails, and configuration. -->

### 12.1 Architecture

```
[Integration architecture diagram]
```

### 12.2 Configuration

<!-- Business definitions, training data, configuration tables, or API contracts for the integration. -->

| [Key] | [Value] | [Technical Mapping] |
|-------|---------|---------------------|
| [Item] | [Definition] | [How it maps to the system] |

### 12.3 Safety Guardrails

<!-- Security, rate limiting, isolation, and audit requirements for the integration. -->

1. [Guardrail 1]
2. [Guardrail 2]

---

## 13. Authentication & Authorization

### 13.1 Auth Implementation

<!-- Method, token lifecycle, password handling, session management. -->

- **Method**: [e.g., JWT with refresh tokens]
- **Token lifecycle**: [e.g., 15-min access + 7-day refresh]
- **Password storage**: [e.g., argon2 via pwdlib]
- **User management**: [e.g., Admin-created, no self-registration]
- **Session**: [e.g., Stateless JWT]

### 13.2 Authorization

<!-- Role hierarchy, enforcement layers, and how tenant/scope isolation works at both the API and database levels. -->

**[Tier 1 — Platform Level]**
- [Description of platform-level auth]

**[Tier 2 — Tenant/Scope Level]**
- [Description of scoped auth]

**Enforcement Layers**:
- **Database**: [e.g., RLS policies on all scoped tables]
- **API**: [e.g., Middleware extracts context from JWT, sets session variables]

---

## 14. API Design

### 14.1 Core Endpoints

<!-- Grouped by domain. Method, endpoint, description. Include all CRUD, search, aggregation, and special endpoints. -->

#### [Domain Group]
| Method | Endpoint | Description |
|--------|----------|-------------|
| [METHOD] | `[/path]` | [Description] |

### 14.2 Common Query Parameters

<!-- Standard parameters shared across list endpoints. -->

- `page` / `per_page` — Pagination
- `sort` / `order` — Sorting
- `search` — Full-text search
- [Domain-specific filters]

---

## 15. Deployment & Infrastructure

### 15.1 Services

| Service | Provider | Tier | Configuration |
|---------|----------|------|---------------|
| [Service] | [Provider] | [Tier] | [Key config details] |

### 15.2 Environment Variables

```
# [Group]
VARIABLE_NAME=<description>
VARIABLE_NAME=<description>

# [Group]
VARIABLE_NAME=<description>
```

### 15.3 Scheduled Jobs

| Job | Schedule | Description |
|-----|----------|-------------|
| [Job] | [Cron schedule] | [What it does] |

---

## 16. Implementation Roadmap

<!-- Technical deliverables mapped to the PRD's phases (see PRD Section 9). Do NOT duplicate the PRD's phase descriptions, dependencies, or exit criteria — only list the technical tasks for each phase. -->

### Phase 1: [Name from PRD]- [ ] [Technical task 1 — e.g., "Create database migration for core tables"]
- [ ] [Technical task 2 — e.g., "Implement auth middleware with JWT"]

### Phase 2: [Name from PRD]- [ ] [Technical task 1]
- [ ] [Technical task 2]

<!-- Repeat for each PRD phase. -->

---

## 17. Cost Analysis

### Monthly Operating Costs

| Service | Cost | Notes |
|---------|------|-------|
| [Service] | [Cost] | [Notes] |
| **Total** | **[Total]** | |

### Cost Scaling Triggers

| Trigger | Current | Upgrade Path | New Cost |
|---------|---------|-------------|----------|
| [Trigger] | [Current state] | [What changes] | [New cost] |

---

## 18. Technical Risks & Mitigations

<!-- Technical risks only — performance bottlenecks, data migration concerns, dependency risks, failure modes. Product and business risks are in PRD Section 10. -->

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| [Technical risk] | [High/Med/Low] | [High/Med/Low] | [Specific technical mitigation] |

---

## 19. Appendix: [Domain-Specific Reference Data]

<!-- Field mappings, identifier cross-references, status mappings, or any bulk reference data that supports the main document. Keep appendices focused — one per topic. -->

### [Mapping Category]

| Source | Field Name | Format Example |
|--------|-----------|----------------|
| [Source] | `[field]` | [Example] |

### [Second Mapping Category]

| [Key] | [Value 1] | [Value 2] | [Value N] |
|-------|-----------|-----------|-----------|
| [Row] | [Value] | [Value] | [Value] |
