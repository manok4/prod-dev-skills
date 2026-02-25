# PRD Discovery Question Bank

Organized by topic. Ask one question at a time. Skip questions already answered by provided context. Prefer AskUserQuestion with multiple-choice options where noted.

---

## 1. Product Identity

**Goal**: Understand what we're building in one sentence.

- "In one sentence, what is this product or feature?" *(open-ended)*
- "Who is the primary audience?" *(multiple-choice if user has hinted at options, else open-ended)*
  - Options could be: Internal team, External customers, Both, Other
- "What's the one-liner problem this solves?"

**Exit when you can fill**: TL;DR, Product name, Target audience

---

## 2. Current State

**Goal**: Understand what exists today and where it breaks down.

- "Is there an existing system or process this replaces?" *(multiple-choice)*
  - Options: Yes (manual process), Yes (existing software), Greenfield (nothing exists), Other
- "What's the current workflow? Walk me through the steps someone does today."
- "Where does the current process break down? What's the biggest pain point?"
- "Can you quantify the pain? (time wasted, error rates, money lost, user complaints)" *(open-ended)*

**Exit when you can fill**: Problem Statement (Context + Pain Points), Narrative

---

## 3. Goals & Success

**Goal**: Define measurable outcomes.

- "What business outcome matters most?" *(multiple-choice)*
  - Options: Reduce cost, Increase revenue, Save time, Reduce errors, Improve user experience, Other
- "What does success look like 6 months after launch? What's different?"
- "Are there specific metrics you'd want to track?" *(open-ended)*
  - Prompt with examples: "e.g., error rate drops by X%, processing time from Y to Z"
- "What would make this project a failure?"

**Exit when you can fill**: Business Goals (with targets), User Goals, Success Metrics

---

## 4. Scope Boundaries

**Goal**: Explicitly define what's NOT in scope.

- "What should this product explicitly NOT do?" *(open-ended)*
- "Are there adjacent features people might expect that you want to exclude?" *(multiple-choice if context suggests options)*
- "Is there a future phase where excluded features might come in?" *(yes/no + details)*

**Exit when you can fill**: Non-Goals section

---

## 5. Users & Personas

**Goal**: Identify who uses it and their permission model.

- "How many distinct user types interact with this system?" *(multiple-choice)*
  - Options: 1 (single user type), 2-3 roles, 4+ roles with hierarchy, Other
- For each role: "What's their title/role, what do they need to do, and what should they NOT be able to see or do?"
- "Is there an admin or super-user who sees everything?" *(yes/no)*
- "Are there external users (customers, vendors, partners) or is this internal only?" *(multiple-choice)*
  - Options: Internal only, Internal + external, External only, Other

**Exit when you can fill**: Personas table, User Stories, Role-Based Access Matrix

---

## 6. Domain Rules

**Goal**: Capture business logic the system must respect. Skip if the domain is simple.

- "Are there business rules or constraints that the system must enforce? Things that are always true in your domain."
- "Are there unique identifiers that matter? (e.g., account IDs, license numbers, national IDs)"
- "Are there lifecycle states that entities move through? (e.g., draft > active > closed)"
- "Are there any regulatory or compliance requirements?"

**Exit when you can fill**: Business Rules & Domain Logic section

---

## 7. Features & Priorities

**Goal**: Build a prioritized feature list.

- "What are the must-have features — the ones without which this product is useless?" *(open-ended, expect a list)*
- "What are the nice-to-haves — useful but can ship without them?"
- For each feature group: "How would you rank its priority?" *(multiple-choice)*
  - Options: Critical (can't launch without), High (should be in v1), Medium (v1 or v2), Low (future)
- "Are there features you've seen in similar products that you want or explicitly don't want?"

**Exit when you can fill**: Functional Requirements (grouped, prioritized)

---

## 8. Data & Integrations

**Goal**: Map data sources and external system connections.

- "What data does this system consume? Where does it come from?" *(open-ended)*
- "What file formats are involved?" *(multiple-choice)*
  - Options: CSV, Excel, JSON/API, PDF, Database, Manual entry, Other
- "How often does data arrive?" *(multiple-choice)*
  - Options: Real-time, Daily, Weekly, On-demand/manual, Other
- "Does this system need to integrate with any external services?" *(open-ended)*
- "Are there data quality issues you know about? (duplicates, missing fields, inconsistent formats)"

**Exit when you can fill**: Data Sources table, Integration Points table

---

## 9. UX & Frontend

**Goal**: Understand the user experience requirements.

- "What are the key pages or screens this product needs?" *(open-ended, expect a list)*
- "Walk me through the most important user journey — what does someone do from login to task completion?"
- "Is this primarily a dashboard, data entry tool, workflow tool, or something else?" *(multiple-choice)*
  - Options: Dashboard/analytics, Data entry/management, Workflow/process tool, Communication tool, Hybrid, Other
- "Any accessibility requirements or design constraints?" *(multiple-choice)*
  - Options: Standard web accessibility (WCAG 2.1), Mobile-responsive required, Must match existing brand/design system, No specific requirements, Other
- "Are there products you like the look and feel of that we should reference?"

**Exit when you can fill**: Pages & Views table, Core User Journey, UX Considerations

---

## 10. Technical Direction

**Goal**: Capture stack preferences and architectural constraints.

- "Do you have a preferred tech stack or are you open to recommendations?" *(multiple-choice)*
  - Options: Specific stack in mind, Open to recommendations, Must match existing codebase, Other
- If specific: "What's the stack?" *(open-ended)*
- "Where should this be hosted?" *(multiple-choice)*
  - Options: Cloud (AWS/GCP/Azure), PaaS (Railway/Render/Vercel), Self-hosted, No preference, Other
- "What's the security model?" *(multiple-choice)*
  - Options: Simple login (email/password), SSO/OAuth, Role-based access, Multi-tenant isolation, Other
- "Any budget constraints for infrastructure?" *(open-ended)*
- "Are there any performance requirements? (response times, concurrent users, data volume)"

**Exit when you can fill**: Technology Stack, System Architecture, Security & Data Privacy, Infrastructure & Cost

---

## 11. Delivery & Risks

**Goal**: Understand phasing, team, and known risks.

- "Is there a target launch date or deadline?" *(open-ended)*
- "Who's building this? What's the team composition?" *(open-ended)*
- "Should this be delivered in phases or all at once?" *(multiple-choice)*
  - Options: Phased (incremental delivery), Single release (big bang), MVP first then iterate, Other
- "What keeps you up at night about this project? What could go wrong?"
- "Are there any external dependencies? (third-party APIs, vendor contracts, data from other teams)"

**Exit when you can fill**: Implementation Roadmap, Team Composition, Risks & Mitigations, Open Questions
