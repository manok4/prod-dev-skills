---
name: tdd-docs
description: >
  Technical Design Document generator. Use when the user wants to create, write, or draft a TDD
  for a product or system. Takes a PRD as primary input and translates business requirements into
  a complete technical specification covering architecture, database schema, ETL pipelines, API
  design, deployment, and implementation details. Guides the user through technical discovery —
  asking questions one at a time — then produces a structured TDD using a proven template.
  Triggers on: 'write a TDD', 'create a TDD', 'technical design document', 'I need a TDD',
  'draft a technical design', 'create a tech spec', or any request to produce a technical
  specification from a PRD. Also triggers when the user says '/tdd-docs'.
user_invocable: true
---

# TDD Document Generator

Generate complete, production-quality Technical Design Documents by translating a PRD into an engineering blueprint. Read the PRD, examine available data sources and codebase context, ask targeted technical questions to fill gaps, then produce a structured TDD covering architecture, schema, pipelines, APIs, deployment, and implementation roadmap.

<HARD-GATE>
Do NOT write the TDD until you have:
1. Read and understood the source PRD
2. Examined any available sample data, existing codebase, or project templates
3. Completed the technical discovery phases with the user
4. Presented a technical summary for user approval
Every TDD goes through this process — no exceptions, regardless of how much context the user provides upfront.
</HARD-GATE>

## Usage

```
/tdd-docs <prd-path>
```

## Arguments

$ARGUMENTS - The path to a PRD file (e.g., `docs/PRD-MyApp.md`).

If no path is provided:
1. Search for PRD files: `Glob: docs/**/PRD-*.md`
2. If exactly one PRD found, use it
3. If multiple found, ask the user to pick one via AskUserQuestion
4. If none found, inform the user and suggest running `/prd-docs` first

---

## Process

```
[1] Ingest PRD     → Read the PRD and extract technical requirements
[2] Explore        → Examine data sources, codebase, templates, and existing docs
[3] Discover       → Ask technical questions phase-by-phase (one at a time)
[4] Synthesize     → Present a technical summary for approval
[5] Draft          → Write the TDD using the template
[6] Review         → Walk through each section, revise per feedback
[7] Deliver        → Save to docs/ directory alongside the PRD
```

## Phase 1: Ingest PRD

Read the full PRD and extract the technical foundation:

1. **Read the PRD** using the Read tool
2. **Extract these elements** (noting what's present vs. missing):
   - **Problem & goals** — what are we solving and what does success look like
   - **Users & roles** — personas, access levels, role hierarchy
   - **Business rules** — domain constraints that affect schema and logic
   - **Functional requirements** — feature groups with priorities
   - **Data sources** — what data enters the system, formats, frequency
   - **UX requirements** — pages, views, user journeys
   - **Technical direction** — any stack preferences, hosting, security model stated in the PRD
   - **Infrastructure & cost** — budget constraints, provider preferences
   - **Implementation phases** — roadmap from the PRD
   - **Risks** — known risks that need technical mitigation
   - **Open questions** — unresolved decisions from the PRD
3. **Identify technical gaps** — what the PRD leaves unspecified that the TDD must decide:
   - Exact database schema (DDL)
   - ETL pipeline design and parser specifications
   - Matching/processing algorithms with pseudocode
   - API endpoint contracts
   - Auth implementation details
   - Deployment configuration
   - Data normalization rules
   - Field-level mappings across data sources

Summarize what you extracted and what gaps remain. This sets the agenda for discovery.

## Phase 2: Explore

Before asking questions, gather available technical context:

- **Sample data files**: Check `resources/`, `data/`, `samples/`, or directories referenced in the PRD. Read sample files to understand actual field names, formats, and quirks.
- **Existing codebase**: If a project already exists, read CLAUDE.md, scan `backend/`, `frontend/`, check `package.json` / `pyproject.toml` / `Cargo.toml` for stack information.
- **Project templates**: If the PRD references a scaffolding template, research it (via Context7 or web search) to document what it provides.
- **Related docs**: Check `docs/`, `claudedocs/` for existing technical documentation.
- **Design docs**: Check `docs/plans/` for brainstorming/design documents that may contain technical decisions.

Summarize findings. Many discovery questions may already be answerable from this exploration.

## Phase 3: Discover

Work through the discovery topics below **one question at a time**. Adapt based on what you already know from Phases 1 and 2 — skip questions whose answers are already clear.

**Questioning rules:**
- One question per message — never batch multiple questions
- Prefer multiple-choice (via AskUserQuestion) when options are enumerable
- Use open-ended questions for architecture/design discussions
- Lead with the most impactful question in each topic
- Skip topics the PRD or exploration already answered
- If the user says "that's enough" or "just write it", move to Phase 4 with what you have
- If the user provides sample data files, read them immediately and extract field-level detail

**Discovery topics (in order):**

See [references/questioning-guide.md](references/questioning-guide.md) for the full question bank organized by topic. The topics are:

1. **Data sources & formats** — field-level schemas, file formats, normalization needs
2. **Tech stack & foundation** — framework, template, ORM, package managers, linters
3. **Database design** — schema decisions, PK strategy, isolation, indexes, extensions
4. **Processing pipeline** — ETL flow, matching algorithms, dedup, error handling
5. **API design** — style, conventions, pagination, real-time needs
6. **Authentication & authorization** — method, tokens, role enforcement, database-level security
7. **Frontend architecture** — SPA vs SSR, data grids, charting, API client generation
8. **Infrastructure & deployment** — hosting, CI/CD, secrets, compliance, scheduled jobs
9. **Risks & edge cases** — technical risks, performance concerns, failure modes

**Adaptive depth:** Scale discovery to the project's complexity. A simple CRUD app may only need topics 2, 3, 5, and 6. A multi-tenant platform with ETL pipelines needs all of them. Gauge from the PRD's complexity how deep to go.

**Data source deep-dive:** If the PRD lists multiple data sources (especially with different formats), this is the highest-priority discovery topic. Read every available sample file and build complete field tables. Ask the user to provide any missing samples.

## Phase 4: Synthesize

Once discovery is sufficient, present a structured technical summary:

```
## Technical Design Summary

**Product**: [name]
**Source PRD**: [path]

### Architecture
- **Pattern**: [e.g., Monolith, Microservices, Serverless]
- **Backend**: [framework + language]
- **Frontend**: [framework + language]
- **Database**: [engine + provider]
- **Key libraries**: [list the critical ones]

### Data Model
- **Tables**: [count] across [scope levels]
- **Key entities**: [list primary entities]
- **Isolation**: [e.g., RLS, schema-per-tenant, application-level]

### Data Pipeline
- **Sources**: [count] with [formats]
- **Processing**: [sync/async/batch]
- **Matching**: [strategy summary]

### API Surface
- **Style**: [REST/GraphQL/tRPC]
- **Endpoint groups**: [list]
- **Auth**: [method]

### Deployment
- **Hosting**: [providers]
- **Est. cost**: [monthly]
- **Compliance**: [requirements]

### Open Decisions
- [Any unresolved technical choices]
```

Ask the user: "Does this technical direction look right? Anything to change before I draft the full TDD?"

Revise until approved.

## Phase 5: Draft

Use the TDD template in [references/tdd-template.md](references/tdd-template.md) as the structural backbone. Apply these rules:

### Section Inclusion Rules

**Always include** (every TDD needs these):
- 1. Executive Summary
- 4. System Architecture
- 5. Technology Stack
- 6. Database Schema
- 13. Authentication & Authorization
- 14. API Design
- 15. Deployment & Infrastructure
- 16. Implementation Roadmap
- 18. Risks & Mitigations

**Include if applicable** (based on project complexity):
- 2. Business Context — include if the domain has non-obvious rules or workflows
- 3. Data Sources — include if the system ingests external data (field tables required)
- 7. ETL Pipeline — include if the system has data ingestion or transformation
- 8-10. Domain-specific sections — include for complex processing logic (matching, scoring, reconciliation). Name these sections based on the domain.
- 11. Frontend Application — include if the frontend has significant complexity (role-based views, heavy grids, real-time features)
- 12. Integration/AI section — include if there are LLM integrations, external API integrations, or complex system-to-system interactions
- 17. Cost Analysis — include if hosting cost is a concern or the PRD has a budget section
- 19. Appendix — include for field mappings, cross-reference tables, or bulk reference data

**Never include empty sections** — if a section doesn't apply, omit it entirely. Renumber remaining sections sequentially.

### Content Rules

- **DDL must be complete** — every CREATE TABLE statement should be copy-pasteable into a migration. Include all columns, types, constraints, defaults, and indexes.
- **Architecture diagrams in ASCII** — use box-drawing characters for system architecture, pipeline flows, and processing diagrams. These must render correctly in markdown.
- **Algorithm pseudocode** — for matching, scoring, or complex processing logic, include structured pseudocode with conditions and confidence scores.
- **API tables must be complete** — every endpoint with method, path, and description. Group by domain.
- **Environment variables** — list all required env vars with placeholder descriptions, never actual values.
- **No placeholder text** — every section must have real content or be removed. No "[TODO]" in the output.
- **No invented metrics** — if a number wasn't provided or calculated, write "TBD" or "To be validated".
- **Mirror the PRD's language** — use the same terminology, entity names, and role names.
- **Cross-reference the PRD** — link back to specific PRD sections where relevant (e.g., "See PRD Section 5.3 for business requirements").
- **Roadmap aligns with PRD** — the TDD implementation roadmap should map to the PRD's phases but with technical deliverables.

### Scaling Rules

- **Simple project** (CRUD app, <5 tables, single user type): ~1,000-3,000 words. Skip sections 2, 3, 7-12, 17, 19.
- **Medium project** (multiple entities, 2-3 roles, some processing): ~3,000-8,000 words. Include sections as needed.
- **Complex project** (multi-tenant, ETL, matching, multiple data sources): ~8,000-20,000+ words. Include all applicable sections with full detail.

## Phase 6: Review

Present the TDD in logical groups, asking after each:

1. **Sections 1-2** (Executive Summary, Business Context) — "Does the technical framing capture the business correctly?"
2. **Sections 3-5** (Data Sources, Architecture, Tech Stack) — "Are the data source details accurate? Does the architecture and stack look right?"
3. **Section 6** (Database Schema) — "Does the schema cover all entities and relationships? Any missing fields or tables?"
4. **Sections 7-10** (Pipeline, Processing, Domain sections) — "Does the processing logic match your expectations? Any edge cases missing?"
5. **Sections 11-14** (Frontend, Integration, Auth, API) — "Do the API contracts and frontend architecture work?"
6. **Sections 15-19** (Deployment, Roadmap, Cost, Risks, Appendix) — "Does the deployment plan and timeline look right?"

Revise per feedback before moving on.

## Phase 7: Deliver

Save the final TDD to the project's documentation directory:
- Default path: `docs/TDD-<product-name>.md` (same directory as the PRD)
- If the PRD is at `docs/requirements/PRD-X.md`, save at `docs/requirements/TDD-X.md`
- Match the PRD's location pattern

After saving:
1. **Cross-link the documents** — add a `**Related**:` line to the PRD header pointing to the TDD, and vice versa. Edit the PRD file to add the link if it doesn't already exist.
2. **Report completion** — summarize what was created and suggest next steps (e.g., `/prd-to-features` to generate feature docs).

---

## Key Principles

- **PRD is the source of truth** — the TDD implements what the PRD specifies. If you disagree with a PRD decision, flag it but don't silently change it.
- **One question at a time** — never overwhelm with batched questions
- **Multiple choice when possible** — faster for the user, more structured for you
- **Skip what you know** — if the PRD or codebase already answered it, don't re-ask
- **Read sample data** — if sample files exist, read them. Field tables built from real data are vastly more accurate than ones built from descriptions.
- **Complete DDL** — the database schema section should be directly usable for migrations. No abbreviated or pseudo-schemas.
- **No fabrication** — unknown values are "TBD", not invented. Don't guess data volumes, costs, or performance characteristics.
- **Adapt depth to complexity** — a simple app gets a lightweight TDD, a platform gets comprehensive detail
- **ASCII diagrams** — all architecture and flow diagrams use ASCII box-drawing characters that render in any markdown viewer
- **Mirror PRD terminology** — use the same entity names, role names, and domain terms as the PRD
