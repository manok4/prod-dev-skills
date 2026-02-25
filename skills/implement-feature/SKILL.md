---
name: implement-feature
description: >
  Execute a feature implementation plan from a feature doc (docs/features/FRXX-*.md).
  Creates a git branch from main, parses the plan, reads architecture and project docs,
  asks clarifying questions, then executes tasks in batches with review checkpoints.
  Use when you have a written feature doc ready to implement. Triggers on:
  'implement feature', 'start implementing', 'build this feature', 'execute the plan',
  'implement FRXX', or '/implement-feature'.
user_invocable: true
---

# Implement Feature

Execute a written feature implementation plan through structured, batched task execution
with review checkpoints. This skill bridges the gap between planning (feature docs) and
working code.

**Announce at start:** "I'm using the implement-feature skill to build this feature."

<HARD-GATE>
Do NOT write any implementation code until you have:
1. Read and understood the feature doc
2. Read architecture docs and project context
3. Created the feature branch from an up-to-date main
4. Raised any concerns or questions with the user
5. Received explicit approval to begin implementation
Every feature goes through this process — no exceptions.
</HARD-GATE>

## Usage

```
/implement-feature <feature-doc-path>
```

## Arguments

$ARGUMENTS - The path to a feature doc file (e.g., `docs/features/FR03-user-auth.md`).

**Feature doc resolution** (if no path provided):
1. Search for feature docs: `Glob: docs/features/FR*.md`
2. If exactly one found, confirm with user before using it
3. If multiple found, present a numbered list and ask the user to pick one via AskUserQuestion
4. If none found, inform the user and suggest running `/prd-to-features` or `/feature-docs new` first

---

## Process

```
[1] Load & Context    → Read the feature doc + architecture docs + project context
[2] Branch            → Create feature branch from up-to-date main
[3] Review & Clarify  → Critically review the plan, ask clarifying questions
[4] Execute Batches   → Implement tasks in batches of 3, with checkpoints
[5] Verify            → Run tests, lint, type checks after each batch
[6] Report            → Present progress, get feedback, continue or adjust
[7] Complete          → Final verification, summary, suggest commit/PR
```

### Step 1: Load & Context

Gather all context needed to implement the feature. Run these in parallel:

1. **Read the feature doc** — the full file including Implementation Plan, Acceptance Criteria,
   Architecture, Technical Details, Dependencies, and Configuration sections
2. **Read project docs** (in parallel):
   - `CLAUDE.md` — project conventions, commands, patterns
   - `docs/architecture.md` or `ARCHITECTURE.md` — system architecture
   - `README.md` — project overview and setup
   - `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` — dependencies and scripts
3. **Read related feature docs** — any features listed in the "Depends on" or "Prerequisites"
   sections of the feature doc. Understand what they implemented and what APIs/patterns they
   established.
4. **Scan project structure** — `tree -L 3 -I 'node_modules|.git|dist|build|.next|__pycache__'`
   to understand where files belong
5. **Read the PRD and TDD** if referenced in the feature doc's References section — skim for
   context relevant to this feature

**Extract from the feature doc:**
- **Implementation Plan** — the ordered list of tasks with steps
- **Acceptance Criteria** — what "done" looks like (P0, P1, P2)
- **Architecture** — component diagram, data flow, tech decisions
- **Core Files** — files to create and modify
- **Dependencies** — packages needed
- **Configuration** — env vars, config changes required
- **Testing** — test files, commands, manual checklist

If the feature doc has no Implementation Plan section, inform the user:
> "This feature doc doesn't include an Implementation Plan with tasks. I can still implement
> it based on the Architecture and Technical Details sections, but the process will be less
> structured. Want me to proceed, or would you prefer to update the feature doc first
> (run `/feature-docs update <path>`)?"

### Step 2: Branch

Create an isolated branch for the feature implementation:

1. **Check current git state**:
   ```bash
   git status
   git branch --show-current
   ```

2. **Stash any uncommitted work** (if dirty working tree):
   - Warn the user: "You have uncommitted changes. I'll stash them before switching branches."
   - `git stash push -m "implement-feature: stashed before FRXX-feature-name"`

3. **Update main**:
   ```bash
   git checkout main
   git pull origin main
   ```

4. **Create and checkout the feature branch**:
   ```bash
   git checkout -b feature/FRXX-feature-name
   ```
   - Branch name format: `feature/FRXX-feature-name` (e.g., `feature/FR03-user-auth`)
   - Derived from the feature doc filename

5. **Confirm branch creation**:
   > "Created branch `feature/FR03-user-auth` from up-to-date `main`."

**Error handling:**
- If `main` doesn't exist, try `master`. If neither exists, ask the user for the base branch.
- If `git pull` fails (no remote, auth issues), warn the user and proceed from local main.
- If the branch name already exists, ask: "Branch `feature/FRXX-name` already exists. Switch to it, or create a new one (e.g., `feature/FRXX-name-v2`)?"

### Step 3: Review & Clarify

Critically review the plan before executing. This is the last checkpoint before code is written.

1. **Assess the plan for issues**:
   - Missing dependencies not in `package.json` / equivalent
   - Files to modify that don't exist in the current codebase
   - Steps that reference APIs or patterns not found in architecture docs
   - Ambiguous acceptance criteria
   - Tasks that are too large (should be split)
   - Tasks that conflict with existing code patterns (from CLAUDE.md or codebase analysis)
   - Security concerns (auth, input validation, secrets)

2. **Present concerns** (if any):
   ```
   ## Plan Review

   I've reviewed the implementation plan and have [N] concern(s):

   1. **[Issue]**: [Description and why it matters]
      Suggested fix: [What to do about it]

   2. **[Issue]**: [Description]
      Question: [What I need from you]
   ```

3. **Ask clarifying questions** — anything the plan leaves ambiguous that would block
   implementation. Ask one question at a time. Prefer multiple-choice when options are
   enumerable.

4. **Present implementation summary** for approval:
   ```
   ## Ready to Implement

   **Feature**: [name]
   **Branch**: feature/FRXX-name
   **Tasks**: [N] tasks in the Implementation Plan
   **Estimated batches**: [ceil(N/3)] batches of ~3 tasks each
   **Dependencies to install**: [list or "none"]
   **Files to create**: [count]
   **Files to modify**: [count]

   Shall I begin implementation?
   ```

5. **Wait for explicit approval** before proceeding to Step 4.

### Step 4: Execute Batches

Execute the Implementation Plan tasks in batches. Default batch size: 3 tasks.

**For each batch:**

1. **Announce the batch**:
   > "Starting batch [N]/[total]: Tasks [X]-[Y]"

2. **For each task in the batch**:

   a. **Announce the task**: "Task [N]: [Component Name]"

   b. **Read all files** that the task will modify — understand current state before changing

   c. **Install dependencies** if the task requires new packages (only on first occurrence)

   d. **Follow the plan's steps exactly**:
      - If the plan specifies test-first (TDD), write the test first and verify it fails
      - Implement the code as specified
      - If the plan specifies a verification command, run it
      - If no verification is specified, run the project's test/lint/typecheck commands

   e. **Adapt when the plan doesn't match reality**:
      - File paths may have changed since the plan was written — find the correct path
      - APIs may have evolved — match the current codebase, not the plan's assumptions
      - If adaptation requires a significant departure from the plan, **stop and ask**

   f. **Mark task complete** when verification passes

3. **After each batch**, proceed to Step 5 (Verify) then Step 6 (Report).

**When to stop and ask:**
- A test fails and you can't determine why after one attempt
- A dependency is missing or incompatible
- The plan contradicts the actual codebase
- A file the plan references doesn't exist and you're not sure where it should be
- You're about to make a change that affects files outside the feature's scope
- Security concern (hardcoded secrets, SQL injection, XSS patterns)

**Never:**
- Skip a failing test to move forward
- Modify files outside the feature's scope without asking
- Install packages not mentioned in the plan without asking
- Force through blockers — stop and ask

### Step 5: Verify

After each batch, run verification:

1. **Run the project's test suite** (or the subset relevant to the feature):
   - Check CLAUDE.md for test commands
   - Fall back to: `npm test`, `pytest`, `cargo test`, `go test ./...`

2. **Run linting and type checking**:
   - Check CLAUDE.md for lint/typecheck commands
   - Fall back to: `npm run lint`, `npm run typecheck`, or equivalent

3. **Run the feature doc's specific verification commands** if any are listed in the Testing section

4. **Check acceptance criteria** — mentally verify which P0/P1 criteria this batch addressed

**If verification fails:**
- Fix the issue if it's straightforward (typo, missing import, minor logic error)
- If the fix requires changing the approach, stop and report to the user

### Step 6: Report

Present batch results and wait for feedback.

```
## Batch [N]/[total] Complete

### Tasks Completed
- [x] Task 1: [name] — [brief summary of what was done]
- [x] Task 2: [name] — [brief summary]
- [x] Task 3: [name] — [brief summary]

### Verification Results
- Tests: [PASS/FAIL] — [details if failed]
- Lint: [PASS/FAIL]
- Types: [PASS/FAIL]

### Acceptance Criteria Progress
- [x] P0-1: [criterion] — covered by Task [N]
- [x] P0-2: [criterion] — covered by Task [N]
- [ ] P1-1: [criterion] — not yet addressed (expected in batch [M])

### Files Changed
- Created: [list]
- Modified: [list]

### Notes
- [Any deviations from the plan and why]
- [Any concerns for upcoming tasks]

Ready for feedback before continuing to batch [N+1].
```

**Based on user feedback:**
- If "looks good" / "continue" → proceed to next batch
- If feedback includes changes → apply changes, re-verify, then report again
- If "stop here" → skip to Step 7 with partial completion summary

### Step 7: Complete

After all batches are done (or the user stops early):

1. **Final verification** — run the full test suite, lint, and typecheck one more time

2. **Acceptance criteria audit**:
   ```
   ## Acceptance Criteria — Final Status

   ### Must-Have (P0)
   - [x] Criterion 1
   - [x] Criterion 2

   ### Should-Have (P1)
   - [x] Criterion 3
   - [ ] Criterion 4 — [reason not addressed]

   ### Nice-to-Have (P2)
   - [ ] Criterion 5 — deferred
   ```

3. **Implementation summary**:
   ```
   ## Implementation Complete

   **Feature**: [name]
   **Branch**: feature/FRXX-name
   **Tasks completed**: [N]/[total]

   ### What Was Built
   - [High-level description of what was implemented]

   ### Files Changed
   | Action | File | Description |
   |--------|------|-------------|
   | Created | path/to/new.ts | [purpose] |
   | Modified | path/to/existing.ts | [what changed] |

   ### Dependencies Added
   - [package@version] — [purpose]

   ### Configuration Changes
   - [env var or config added]

   ### Deviations from Plan
   - [Any changes made and why, or "None"]

   ### Next Steps
   Ready to commit? I'll run `/commit-code` now to review, commit, and push.
   ```

4. **Commit the implementation** — after presenting the summary, ask:

   > "All tasks are complete and verified. Want me to commit the changes now?"

   - If the user approves → invoke `/commit-code` to review, stage, commit, and push
   - If the user wants to review first → show `git diff main` and wait
   - If the user declines → leave changes uncommitted on the feature branch

---

## Rules

1. **Plan is the guide, codebase is the truth** — follow the plan's intent, but adapt to the
   actual codebase when they conflict. Flag significant deviations.
2. **Never implement on main** — always create a feature branch first. No exceptions.
3. **Batch execution with checkpoints** — never run all tasks without stopping for review.
   Default batch size is 3 tasks.
4. **Stop when blocked** — ask for clarification rather than guessing. A wrong guess wastes
   more time than a question.
5. **Test as you go** — run verification after each batch, not just at the end.
6. **No scope creep** — implement what the plan says. Don't add features, refactor unrelated
   code, or "improve" things outside the plan's scope.
7. **Respect project conventions** — follow patterns in CLAUDE.md and the existing codebase.
   If the plan contradicts project conventions, flag it and ask.
8. **Security first** — never hardcode secrets, skip auth checks, or introduce injection
   vulnerabilities, even if the plan says to.
9. **One feature per branch** — each feature doc gets its own branch.
10. **Clean commits** — suggest atomic commits after each batch when appropriate (don't
    auto-commit without asking).

## Error Handling

- **Feature doc not found** → ask for path, suggest `Glob: docs/features/FR*.md`
- **No Implementation Plan section** → offer to proceed from Architecture/Technical Details,
  or suggest updating the feature doc first
- **Main branch can't be updated** → warn and proceed from local main
- **Branch already exists** → ask user: switch to it or create a new variant
- **Test suite fails before implementation** → warn user that existing tests are failing and
  ask how to proceed
- **Dependency installation fails** → report the error, suggest alternatives
- **Plan references non-existent files** → search for the correct file, ask if unsure

## Integration

**Upstream skills (produce the plan this skill executes):**
- **feature-docs** — creates the feature documentation with Implementation Plan
- **prd-to-features** — generates feature docs from PRD + TDD
- **project-kickoff** — orchestrates the full pipeline

**Downstream skills (use after implementation):**
- **commit-code** — commit, review, and push the implementation
- **feature-docs update** — sync the feature doc with the actual implementation
- **code-review** — review the implementation before merging

## Examples

```
# Implement a specific feature doc
/implement-feature docs/features/FR03-user-auth.md

# Auto-detect (single feature doc in project)
/implement-feature

# Auto-detect (multiple feature docs — will prompt for selection)
/implement-feature
```
