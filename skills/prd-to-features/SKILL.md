---
name: prd-to-features
description: >
  Convert a PRD and TDD into individual feature documentation files. Uses the PRD for functional
  requirements, user experience flows, and implementation phases, and the TDD for tech stack,
  database schema, API contracts, and infrastructure details. Generates a foundational Phase 0
  project-setup feature (from TDD) before any user-story features, then produces sequentially
  numbered docs/features/FRXX-feature-name.md files using the feature-docs NEW mode template.
  TDD is optional but strongly recommended — without it, foundational setup cannot be fully generated.
  Triggers on: 'convert PRD to features', 'break down PRD', 'generate features from PRD',
  'PRD to feature docs', or '/prd-to-features'.
user_invocable: true
---

# PRD to Features

Convert a completed PRD and TDD into individual, implementable feature documentation files.
Uses the PRD for functional requirements (Section 5), user experience flows (Section 6),
and implementation phases (Section 10). Uses the TDD for tech stack, database schema, API
contracts, and infrastructure — generating a foundational Phase 0 project-setup feature before
any user-story features. Produces sequentially numbered feature docs using the feature-docs
NEW mode template. TDD is optional but strongly recommended; without it, foundational setup
features require manual tech stack input from the user.

## Usage

```
/prd-to-features <prd-path> [--tdd <tdd-path>]
```

## Arguments

$ARGUMENTS - The path to a PRD file (e.g., `docs/PRD-MyApp.md`), optionally followed by `--tdd <tdd-path>`.

**PRD resolution** (if no PRD path provided):
1. Search for PRD files: `Glob: docs/**/PRD-*.md`
2. If exactly one PRD found, use it
3. If multiple found, ask the user to pick one via AskUserQuestion
4. If none found, inform the user and exit

**TDD resolution** (if no `--tdd` flag provided):
1. Search for TDD files: `Glob: docs/**/TDD-*.md`
2. If exactly one TDD found in the same directory as the PRD, use it automatically
3. If multiple found, ask the user to pick one via AskUserQuestion
4. If none found, warn the user:

   > "No TDD found. Without a TDD, I cannot generate a complete Phase 0 project-setup feature
   > covering tech stack, database, and infrastructure setup. I'll need to ask you a few questions
   > about your tech stack to fill in the gaps. Providing a TDD is strongly recommended."

   Then proceed to Step 1 where the missing TDD is handled by asking the user directly.

---

## Process

```
[1] Load & Parse     → Read the PRD and TDD, extract structured data (ask user for tech stack if no TDD)
[2] Extract Features → Identify features from Sections 5, 6, 10 + foundational setup from TDD
[3] Build Plan       → Phase 0 (project-setup) first, then map remaining features to phases
[4] Verify           → Present extraction to user for confirmation
[5] Generate         → Invoke /feature-docs new for each feature in order (FR01 project-setup first)
[6] Summary          → Output results with cross-references
```

### Step 1: Load & Parse

1. **Validate the PRD path** — confirm the file exists and is a markdown file
2. **Read the full PRD** using the Read tool
3. **Parse PRD sections** — identify and extract content from these sections:
   - **Section 5: Functional Requirements** — feature groups with priorities
   - **Section 6: User Experience** — core user journey, pages/views, role-based access, UX considerations
   - **Section 10: Implementation Roadmap** — phases with deliverables and dependencies
   - **Section 3: Users & Stakeholders** — personas and user stories (for feature context)
   - **Section 4: Business Rules & Domain Logic** — constraints features must respect
   - **Section 8: Technical Architecture** — tech stack and design context
   - **Section 2: Goals** — business/user goals and non-goals
4. **Identify the product name** from the PRD title/metadata
5. **Load the TDD** (if available) using the Read tool. Extract these technical sections:
   - **Database schema** — table DDL, indexes, constraints, RLS policies, data isolation strategy
   - **API endpoints** — method, path, description for each endpoint group
   - **ETL pipeline** — parser specifications, normalization rules, processing flow
   - **Technology stack** — backend/frontend/database/infrastructure components with versions
   - **Domain processing** — matching algorithms, calculation rules, pseudocode
   - **Auth implementation** — token lifecycle, role enforcement, RLS details
   - **Deployment config** — services, environment variables, scheduled jobs
   - **Data source field mappings** — appendix cross-reference tables

   These TDD details are used in Step 2 (Source D) to build the Phase 0 project-setup feature
   and in Step 5 to enrich the feature context brief passed to feature-docs.

6. **If no TDD is available**, ask the user for foundational tech stack details via AskUserQuestion:
   - Frontend framework (e.g., Next.js, React, Vue, SvelteKit)
   - Backend framework (e.g., Node/Express, Python/FastAPI, Convex)
   - Database (e.g., PostgreSQL, MongoDB, Convex, Supabase)
   - Hosting/infrastructure (e.g., Vercel, AWS, Railway)
   - Auth provider (e.g., Clerk, Auth0, NextAuth)
   - Any other key tooling (e.g., ORM, CSS framework, monorepo tool)

   Use these answers to populate the Phase 0 project-setup feature in place of TDD-derived details.

### Step 2: Extract Features

Features come from three sources. Merge and deduplicate across all three.

#### Source A: Functional Requirements (Section 5)

Each `### 5.X [Feature Group Name] — Priority: [Level]` becomes a candidate feature:
- **Feature name**: derived from the feature group name (kebab-case for file naming)
- **Priority**: Critical / High / Medium / Low (from the PRD)
- **Description**: the `**Description**:` line
- **Requirements**: the bulleted requirements list

#### Source B: User Experience (Section 6)

Extract UI/UX-facing features that may not have explicit Section 5 entries:
- **Pages & Views (6.2)**: each page/screen listed in the table becomes a potential feature or is merged into an existing feature from Source A
- **Core User Journey (6.1)**: map journey steps to features — each major journey step should be covered by at least one feature
- **Role-Based Access (6.3)**: attach role/permission context to relevant features
- **UX Considerations (6.4)**: attach accessibility, onboarding, and error handling requirements to relevant features

**Merge rules:**
- If a page/view clearly maps to a Section 5 feature group, attach it as UX context to that feature
- If a page/view has no corresponding Section 5 entry, create a new feature for it (e.g., a dashboard page, settings page, onboarding flow)
- Every step in the Core User Journey (6.1) must be traceable to at least one feature. Flag any orphaned steps.

#### Source C: Implementation Roadmap (Section 10)

Each phase lists deliverables. Map each deliverable to a feature from Sources A/B:
- If a deliverable matches an existing feature, assign that feature to the phase
- If a deliverable introduces something not in Sources A/B (e.g., "Set up CI/CD", "Database migrations"), note it — these are likely covered by the Phase 0 project-setup feature (Source D)

**Phase assignment rules:**
- Every feature must be assigned to exactly one phase (except Phase 0 which is handled separately)
- Features not mentioned in any phase deliverable get assigned based on their priority:
  Critical → Phase 1, High → earliest available phase, Medium/Low → later phases
- Preserve the phase ordering from the PRD

#### Source D: Foundational Setup (from TDD or user input)

This source always produces exactly **one feature**: `project-setup`. It is always assigned to **Phase 0: Foundation** and always gets the first FR number (FR01). This feature represents everything needed before any user-story feature can be built.

**If TDD is available**, extract and consolidate these into the project-setup feature:
- **Project scaffolding** — repo initialization, monorepo structure (if applicable), directory layout, dev tooling (linting, formatting, pre-commit hooks)
- **Frontend foundation** — framework setup (e.g., Next.js app, React with Vite), routing configuration, CSS/design system setup (e.g., Tailwind, shadcn/ui), app shell with layout components
- **Backend foundation** — API framework setup (e.g., Express, FastAPI, Convex functions), middleware configuration, base route structure, error handling patterns
- **Database setup** — database provisioning, schema creation (initial migrations or schema definition), ORM/client configuration, seed data structure, connection pooling
- **Auth scaffolding** — auth provider integration (e.g., Clerk, Auth0), middleware for protected routes, role/permission model setup
- **Infrastructure** — hosting/deployment configuration, environment variable templates (.env.example), CI/CD pipeline basics, dev/staging environment setup

**If no TDD is available**, use the tech stack answers gathered from the user in Step 1 to populate the same categories. The feature will be less detailed but still establishes the project foundation.

**What goes in project-setup vs. later features:**
- project-setup covers *installing, configuring, and wiring up* the tech stack so it runs
- It does NOT implement business logic, user flows, or feature-specific endpoints
- Example: project-setup creates the database and auth middleware; FR02+ implements specific tables, specific API routes, and specific UI pages

### Step 3: Build Plan

1. **Scan for existing feature docs**: `Glob: docs/features/FR*.md`
   - Find the highest existing FR number
   - New features start numbering from highest + 1 (or FR01 if none exist)

2. **Create Phase 0: Foundation** — this phase always exists and always comes first:
   - Contains exactly one feature: **project-setup** (from Source D)
   - This feature is always the first FR number (FR01, or next available if existing FR files are found)
   - If no TDD was provided and the user supplied tech stack answers, use those answers as the basis

3. **Order remaining features** (Phase 1+ from PRD):
   - Primary sort: phase number (Phase 1 first)
   - Secondary sort within phase: priority (Critical > High > Medium > Low)
   - Tertiary sort: order of appearance in the PRD

4. **Assign FRXX numbers** sequentially starting after Phase 0:
   - Phase 0: FR01 (project-setup)
   - Phase 1 features: FR02, FR03, FR04...
   - Phase 2 features: FR05, FR06... (continuing from Phase 1)
   - And so on

5. **Build the feature manifest** — a structured list:

```
Phase 0: Foundation
  FR01 - project-setup        | Priority: Critical | Source: TDD (tech stack, schema, infra)

Phase 1: [Phase Name]
  FR02 - feature-name         | Priority: Critical | Source: Section 5.1, 6.2
  FR03 - feature-name         | Priority: High     | Source: Section 5.3, 6.1

Phase 2: [Phase Name]
  FR04 - feature-name         | Priority: High     | Source: Section 5.2
  FR05 - feature-name         | Priority: Medium   | Source: Section 6.2 (new)
```

### Step 4: Verify

Present the complete extraction to the user for confirmation. This is a **hard gate** — do not proceed without user approval.

Display the feature manifest using this format:

```markdown
## PRD Feature Extraction

**Source PRD**: [PRD filename]
**Source TDD**: [TDD filename or "Not available"]
**Product**: [Product name]
**Total features**: [count]
**Phases**: [count]

---

### Phase 0: Foundation

| # | Feature | Priority | Source | Description |
|---|---------|----------|--------|-------------|
| FR01 | project-setup | Critical | TDD / user input | Project scaffolding, tech stack, DB, auth, infra |

### Phase 1: [Phase Name] — [Duration if specified]

| # | Feature | Priority | PRD Source | Description |
|---|---------|----------|------------|-------------|
| FR02 | feature-name | Critical | 5.1, 6.2 | Brief description |
| FR03 | feature-name | High | 5.3, 6.1 | Brief description |

### Phase 2: [Phase Name] — [Duration if specified]

| # | Feature | Priority | PRD Source | Description |
|---|---------|----------|------------|-------------|
| FR04 | feature-name | High | 5.2 | Brief description |

---

### UX Coverage Check
- Core User Journey steps: [X] / [Total] covered
- Pages & Views: [X] / [Total] covered
- Orphaned journey steps: [list any uncovered steps, or "None"]
- Orphaned pages: [list any uncovered pages, or "None"]

### Notes
- [Any merge decisions made (e.g., "Combined 'search' page with 'data-explorer' feature")]
- [Any infrastructure features added from Phase deliverables]
```

Ask: **"Does this capture all the features correctly? Any to add, remove, rename, reorder, or reassign to a different phase?"**

Revise until the user confirms.

### Step 5: Generate Feature Docs via feature-docs Skill

After user confirmation, generate all feature docs by invoking the **feature-docs** skill in NEW mode for each feature. This skill handles template structure, section relevance, quality validation, auto-increment naming, and cross-referencing.

**For each feature in the manifest (in FRXX order):**

1. **Prepare a feature context brief** — a structured summary of all PRD content relevant to this feature. This brief is passed as context when invoking the feature-docs skill.

   The brief should include:
   - **Feature name** (kebab-case)
   - **Description** (1-2 sentences from PRD)
   - **Priority** (Critical / High / Medium / Low)
   - **Phase assignment** (which phase and position)
   - **Functional requirements** — the specific requirements from Section 5 for this feature
   - **UX context** — relevant pages/views from Section 6.2, user journey steps from Section 6.1, role access from Section 6.3, UX considerations from Section 6.4
   - **Business rules** — constraints from Section 4 that apply to this feature
   - **Technical context** — relevant stack, APIs, data models, security from Section 8
   - **User stories** — stories from Section 3 that map to this feature
   - **Goals** — business/user goals from Section 2 that this feature supports
   - **Risks** — relevant risks from Section 11
   - **Dependencies** — other FRxx features this depends on or that depend on it
   - **PRD reference** — source PRD path and specific section numbers
   - **TDD context** (if TDD is available) — include these additional details:
     - **Database tables** — relevant CREATE TABLE DDL, indexes, and constraints from the TDD schema section
     - **API endpoints** — specific endpoint contracts (method, path, description) from the TDD API section
     - **ETL/pipeline details** — parser specifications, normalization rules, and processing flow relevant to this feature
     - **Algorithm/processing logic** — matching tiers, pseudocode, confidence thresholds from domain-specific TDD sections
     - **Auth requirements** — RLS policies, role enforcement details, token handling from the TDD auth section
     - **Data source fields** — field mappings from the TDD appendix relevant to this feature's data handling
     - **Infrastructure** — deployment services, environment variables, scheduled jobs from the TDD deployment section
     - **TDD reference** — source TDD path and specific section numbers

2. **Invoke the feature-docs skill** in NEW mode:

   ```
   /feature-docs new <feature-name> "<description>" <prd-path>
   ```

   The feature-docs skill will:
   - Follow its own Step 2 (Project Context Discovery)
   - Execute its Step 3 NEW mode analysis, using the PRD as the primary doc-link
   - Generate the feature doc using its own template (references/documentation-template.md)
   - Apply its own section relevance rules (skip sections that don't apply)
   - Include NEW mode additions (Implementation Plan, Deployment Plan, etc.)
   - Run its Step 6 quality validation
   - Save to `docs/features/FRXX-feature-name.md` with auto-incremented numbering (Step 7)

3. **Supplement the feature-docs output** with PRD-specific context:

   After the feature-docs skill generates the doc, ensure these PRD-sourced details are present:
   - **Scope > Prerequisites**: list other FRxx features that must be built first
   - **Scope > Depends on**: cross-reference to dependent features by FRXX number
   - **References**: link back to the source PRD with specific section numbers
   - **References**: link to related FRxx features in the same phase

   If feature-docs missed any of these (because they come from the PRD extraction, not codebase analysis), add them via targeted edits to the generated file.

4. **Proceed to the next feature** in the manifest. Since feature-docs auto-increments FR numbers by scanning existing files, generating features in order naturally produces the correct sequential numbering.

<IMPORTANT>
- Do NOT reimplement the feature-docs template or section logic. The feature-docs skill owns all template decisions.
- Do NOT skip invoking feature-docs and write files directly. Every feature doc must go through the feature-docs skill process.
- The role of this skill (prd-to-features) is extraction, ordering, and orchestration. The role of feature-docs is document generation.
- Pass as much PRD context as possible to feature-docs so it can make informed section and content decisions.
</IMPORTANT>

### Step 6: Summary

After all feature docs are generated, output a summary:

```markdown
## PRD to Features — Complete

**Source PRD**: [path]
**Source TDD**: [path or "Not used"]
**Features generated**: [count]
**Output directory**: docs/features/

### Generated Files

| # | File | Phase | Priority |
|---|------|-------|----------|
| FR01 | docs/features/FR01-project-setup.md | Phase 0 | Critical |
| FR02 | docs/features/FR02-feature-name.md | Phase 1 | Critical |
| FR03 | docs/features/FR03-feature-name.md | Phase 1 | High |
| FR04 | docs/features/FR04-feature-name.md | Phase 2 | High |

### Cross-Reference Map
- FR01 (project-setup) → depends on: none | blocks: all subsequent features
- FR02 → depends on: FR01 | blocks: FR04, FR05
- FR03 → depends on: FR01 | blocks: FR06
- ...

### Implementation Order (Suggested)
Phase 0: FR01 (project-setup — must be completed first)
Phase 1: FR02 → FR03 → ...
Phase 2: FR04 → FR05 → ...

### Next Steps
- Review each feature doc for completeness
- Run `/feature-docs update docs/features/FRXX-name.md` after implementation to sync with code
- Use feature docs as implementation guides for each development cycle
```

---

## Rules

1. **No fabrication** — if the PRD doesn't specify something, use "TBD" or omit the section. Never invent requirements, metrics, or technical details not in the PRD.
2. **User's language** — mirror the PRD's terminology in all feature docs.
3. **No empty sections** — omit sections that have no relevant content rather than leaving placeholders.
4. **Atomic features** — each feature doc should be independently implementable. If a feature is too large, split it. If too small, merge it.
5. **Traceability** — every feature must trace back to at least one PRD section. Every PRD requirement must be covered by at least one feature.
6. **Phase ordering matters** — features are numbered by phase order to enable sequential implementation.
7. **UX completeness** — every page/view and user journey step in the PRD must be covered by a feature. The verification step (Step 4) checks this explicitly.
8. **Create docs/features/ directory** if it doesn't exist.
9. **Respect existing features** — if docs/features/ already has FR files, continue numbering from the highest existing number + 1.
10. **Feature naming** — use kebab-case for file names (e.g., `FR01-user-authentication.md`). Feature names should be descriptive but concise (2-4 words).

## Error Handling

- PRD file not found → inform user, suggest `Glob: docs/PRD-*.md` to find PRDs
- PRD has no Section 5 → inform user the PRD may be incomplete, ask how to proceed
- PRD has no Section 10 (no phases) → assign all features to a single "Implementation" phase, ordered by priority
- Feature count exceeds 20 → warn user and suggest grouping related features
- docs/features/ would overwrite existing files → ask for confirmation before overwriting

## Examples

```
# Convert a PRD to feature docs (auto-detects TDD in same directory)
/prd-to-features docs/PRD-MyApp.md

# Explicitly provide both PRD and TDD
/prd-to-features docs/requirements/PRD-MyApp.md --tdd docs/requirements/TDD-MyApp.md

# Auto-detect both PRD and TDD
/prd-to-features

# PRD only (no TDD available)
/prd-to-features docs/PRD-MyApp.md
```
