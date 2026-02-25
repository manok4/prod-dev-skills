# TDD Discovery Question Bank

Organized by topic. Ask one question at a time. Skip questions already answered by the PRD or provided context. Prefer AskUserQuestion with multiple-choice options where noted.

**Important**: The PRD is the primary input. Most business context should already be known. These questions focus on **technical decisions** that the PRD leaves open or underspecified.

---

## 1. Data Sources & Formats

**Goal**: Get precise field-level understanding of every data source the system will consume.

- "The PRD mentions [N] data sources. Do you have sample files I can examine for each?" *(open-ended)*
- "For [source name], what file format does it arrive in?" *(multiple-choice)*
  - Options: CSV, Excel (.xlsx), JSON, PDF, API response, Other
- "How often does [source name] data arrive?" *(multiple-choice)*
  - Options: Real-time, Daily, Weekly, On-demand/manual upload, Other
- "Are there known data quality issues? (duplicate records, missing fields, inconsistent formats, encoding problems)" *(open-ended)*
- "Do any data sources use proprietary or non-standard formats? (Excel serial dates, packed decimals, custom delimiters)" *(open-ended)*
- "Are there fields that use different names or formats across sources for the same concept?" *(open-ended)*

**Exit when you can fill**: Section 3 (Data Sources) with full field tables and observations

---

## 2. Tech Stack & Foundation

**Goal**: Confirm or decide the technology stack and project foundation.

- "Do you have a preferred tech stack, or should I recommend one based on the requirements?" *(multiple-choice)*
  - Options: Specific stack in mind, Open to recommendations, Must match existing codebase, Other
- "Are you scaffolding from a template or starting from scratch?" *(multiple-choice)*
  - Options: Existing template (specify which), Existing codebase to extend, Greenfield, Other
- If template: "Which template? I'll review it and document what it provides vs. what we add."
- "What's the preferred ORM/database access pattern?" *(multiple-choice if context suggests options)*
  - Options could be: SQLAlchemy, SQLModel, Prisma, Drizzle, Raw SQL, Other
- "Any strong preferences on package managers, linters, or testing frameworks?" *(open-ended)*
- "What's the target deployment environment?" *(multiple-choice)*
  - Options: Cloud Run/Lambda (serverless), Container (Docker/K8s), PaaS (Railway/Render), VPS, Other

**Exit when you can fill**: Section 5 (Technology Stack) with decision rationale

---

## 3. Database Design

**Goal**: Make schema decisions that the PRD doesn't specify.

- "The PRD describes [N] entities. Are there any additional tables or data structures you already have in mind?" *(open-ended)*
- "What's the primary key strategy?" *(multiple-choice)*
  - Options: Auto-increment integers, UUIDs, Domain-specific IDs (e.g., Medicare ID), Composite keys, Other
- "Are there any multi-tenant or data isolation requirements beyond what the PRD describes?" *(open-ended)*
- "Are there fields that need to be encrypted at rest beyond standard database encryption?" *(open-ended)*
- "Do you anticipate needing full-text search, fuzzy matching, or geographic queries?" *(multiple-choice, multiSelect)*
  - Options: Full-text search (pg_trgm / Elasticsearch), Fuzzy matching (Levenshtein / trigram), Geographic (PostGIS), None of these
- "What's the expected data retention policy? How long should raw imports and audit logs be kept?" *(open-ended)*

**Exit when you can fill**: Section 6 (Database Schema) with DDL and index strategy

---

## 4. Processing Pipeline

**Goal**: Design the ETL, matching, or core processing logic.

- "Walk me through the ideal processing flow when new data arrives — what happens step by step?" *(open-ended)*
- "How should duplicate detection work?" *(multiple-choice)*
  - Options: File-level hash (reject re-uploads), Record-level dedup (merge/skip duplicates), Both, Other
- "For record matching across sources, what's the primary matching key?" *(open-ended)*
- "What should happen when a record can't be auto-matched?" *(multiple-choice)*
  - Options: Queue for manual review, Skip and log, Best-effort match with low confidence, Reject the record, Other
- "Are there processing steps that need to be idempotent (safe to re-run)?" *(open-ended)*
- "Should processing be synchronous (user waits) or async (background job with status updates)?" *(multiple-choice)*
  - Options: Synchronous (small files, fast processing), Async with progress (large files), Scheduled batch only, Hybrid (sync for small, async for large), Other

**Exit when you can fill**: Section 7 (ETL Pipeline) and Section 8 (domain processing)

---

## 5. API Design

**Goal**: Define the API surface and conventions.

- "What API style are you using?" *(multiple-choice)*
  - Options: REST, GraphQL, tRPC, gRPC, Other
- "Are there existing API conventions to follow? (naming, versioning, error format)" *(open-ended)*
- "Do any endpoints need real-time capabilities?" *(multiple-choice)*
  - Options: WebSocket (streaming), Server-Sent Events, Polling is fine, Other
- "What pagination strategy?" *(multiple-choice)*
  - Options: Offset-based (page/per_page), Cursor-based, Keyset, No preference
- "Are there any external consumers of this API, or is it purely for the frontend?" *(multiple-choice)*
  - Options: Frontend only, Frontend + mobile app, Frontend + third-party integrations, Public API, Other

**Exit when you can fill**: Section 14 (API Design) with endpoint tables

---

## 6. Authentication & Authorization

**Goal**: Nail down the auth implementation details.

- "The PRD describes [N] roles. Is there anything about the permission model that needs more technical detail?" *(open-ended)*
- "What auth method?" *(multiple-choice)*
  - Options: JWT (self-issued), OAuth2 / SSO (external provider), Session-based, API keys, Other
- "Token lifecycle — how long should access tokens live?" *(multiple-choice)*
  - Options: 15 minutes (standard), 1 hour, 24 hours, Other
- "How are users created?" *(multiple-choice)*
  - Options: Self-registration, Admin-created only, Invite link, SSO auto-provision, Other
- "Does the database need its own access control layer (RLS, views) or is API-level enforcement sufficient?" *(multiple-choice)*
  - Options: API-level only, Database RLS (defense-in-depth), Both (belt and suspenders), Other

**Exit when you can fill**: Section 13 (Authentication & Authorization)

---

## 7. Frontend Architecture

**Goal**: Technical frontend decisions not covered in the PRD's UX section.

- "Is the frontend an SPA, SSR, or static site?" *(multiple-choice)*
  - Options: SPA (Vite/CRA), SSR (Next.js/Nuxt), Static (Astro/Hugo), Hybrid, Other
- "What's the data grid requirement? Light tables or heavy grids with grouping/export?" *(multiple-choice)*
  - Options: Light tables (TanStack Table, HTML tables), Heavy grids (AG Grid, Handsontable), Both (light + heavy), Other
- "Any charting/visualization library preference?" *(multiple-choice)*
  - Options: Recharts, Chart.js, D3, Tremor, Nivo, No preference, Other
- "How is the API client generated or managed?" *(multiple-choice)*
  - Options: Auto-generated from OpenAPI (e.g., @hey-api/openapi-ts), Manual axios/fetch, tRPC (type-safe), Other
- "Any state management beyond server state (TanStack Query / SWR)?" *(open-ended)*

**Exit when you can fill**: Section 11 (Frontend Application) with technical specifics

---

## 8. Infrastructure & Deployment

**Goal**: Pin down hosting, CI/CD, and operational concerns.

- "What's the hosting budget?" *(multiple-choice)*
  - Options: Free tier only (~$0), Low-cost (~$10-30/mo), Standard (~$50-200/mo), Enterprise, Other
- "Do you need HIPAA, SOC 2, or other compliance?" *(multiple-choice, multiSelect)*
  - Options: HIPAA (health data), SOC 2, PCI DSS (payment data), GDPR, None, Other
- "How should secrets be managed?" *(multiple-choice)*
  - Options: Cloud secret manager (GCP/AWS), .env files (dev only), Vault, Environment variables (PaaS), Other
- "Do you need scheduled jobs?" *(multiple-choice)*
  - Options: Yes (cron-style), Yes (event-driven), No, Other
- "What's the CI/CD setup?" *(multiple-choice)*
  - Options: GitHub Actions, GitLab CI, Cloud Build, Manual deploy, Other

**Exit when you can fill**: Section 15 (Deployment & Infrastructure) and Section 17 (Cost Analysis)

---

## 9. Risks & Edge Cases

**Goal**: Identify technical risks the PRD may not cover.

- "What's the most technically risky part of this system?" *(open-ended)*
- "Are there any performance bottlenecks you're worried about?" *(open-ended)*
- "What happens if [external dependency] goes down or changes its format?" *(open-ended)*
- "Are there data migration or backfill concerns?" *(open-ended)*

**Exit when you can fill**: Section 18 (Risks & Mitigations) with technical-specific risks
