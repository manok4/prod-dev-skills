---
name: feature-docs
description: >
  Unified feature documentation skill. Auto-detects mode: document existing code,
  plan a new feature, or update existing docs from code changes.
  Use when the user wants to create, plan, or update feature documentation.
user_invocable: true
---

# Feature Docs

Unified skill for all feature documentation workflows: document existing features,
plan new features, or update existing documentation to match code reality.

## Usage
```
/feature-docs <mode?> <target> [options...]
```

## Arguments

$ARGUMENTS - Parsed as follows:

**Explicit mode (optional):**
- `document <feature-name> "<description>"` - Document an existing codebase feature
- `new <feature-name> "<description>" [doc-links...]` - Plan/document a new feature
- `update <doc-path>` - Update existing docs from code changes

**Auto-detect mode (if no mode keyword given):**
- If first argument is a path to an existing `.md` file → **update** mode
- If first argument is a feature name and matching code exists in codebase → **document** mode
- If first argument is a feature name and no matching code found → **new** mode

---

## Process

### Step 1: Parse Arguments and Detect Mode

1. Check if first token is `document`, `new`, or `update` → explicit mode
2. If no mode keyword, auto-detect:
   - Check if the argument is a file path that exists → **update** mode
   - Search codebase: `Glob: **/*{feature-name}*` and `Grep: "{feature-name}"`
   - If matching source code found → **document** mode
   - If no matching source code → **new** mode
3. Extract remaining arguments: feature-name, description, doc-path, doc-links

Announce detected mode before proceeding:
> "Detected mode: **[document|new|update]** for feature: [name]"

### Step 2: Project Context Discovery

Gather project context (run in parallel):

1. **Project structure**: Glob for `**/README.md`, `**/ARCHITECTURE.md`
2. **Existing docs**: Glob for `docs/**/*.md`
3. **Package info**: Read `package.json`, `Cargo.toml`, `pyproject.toml`, or `go.mod`
4. **Architecture docs**: Read `docs/architecture.md` or `ARCHITECTURE.md` if present
5. **Current branch**: `git branch --show-current`
6. **Existing doc patterns**: Check formatting/style of existing feature docs for consistency

### Step 3: Mode-Specific Analysis

#### Mode: DOCUMENT (existing feature)

1. **Discover all related files** using parallel searches:
   - Glob for files with feature name in path
   - Grep for feature name in source files (exclude node_modules, .git, dist, build)
   - Grep for related class/function/component names
   - Search for test files: `**/*{feature-name}*.test.*`, `**/*{feature-name}*.spec.*`
   - Search for config references in `.env*`, `*.yaml`, `*.toml`, `*.json`

2. **Read and analyze key files**: core implementation, tests, config, migrations, API routes

3. **Extract technical details**: public API surface, dependencies, config options, error handling, data models

#### Mode: NEW (planned feature)

1. **Research context**: review doc-links, check existing patterns, identify integration points
2. **Define feature scope**: break down requirements, acceptance criteria, target users, new components
3. **Optional research** (WebSearch if needed): similar implementations, security, accessibility, performance

#### Mode: UPDATE (sync docs with code)

1. **Parse existing documentation**: read doc, extract feature name, last updated date, documented paths
2. **Discover current code state** (same search strategy as DOCUMENT mode)
3. **Analyze git history since last update**:
   ```bash
   git log --since="[last-update-date]" --pretty=format:"%h %s" -- [related-files]
   ```
4. **Compare documentation vs code reality**: missing, outdated, incorrect, obsolete

### Step 4: Generate/Update Documentation

Use the template from [documentation-template.md](references/documentation-template.md).

The template includes:
- **Unified template** — all standard sections (Overview through References)
- **NEW mode additions** — Implementation Plan, Deployment, Monitoring, Future Enhancements
- **Section relevance rules** — which sections to skip based on feature type

### Step 5: Cross-Reference Management

1. **Search for related feature docs** in `docs/features/`:
   - Features that depend on this one
   - Features this one depends on
   - Features in the same domain

2. **Add cross-references** in the References section

3. **Update related docs** (UPDATE mode only):
   - If changes affect other documented features, note it in the summary
   - Do NOT auto-edit other docs — flag them for the user

### Step 6: Quality Validation

Run these checks before saving:

- [ ] All referenced file paths exist in the codebase
- [ ] Code examples use correct syntax for the project's language
- [ ] No sensitive information (API keys, passwords, internal URLs)
- [ ] Mermaid diagrams are syntactically valid
- [ ] Table of contents matches actual sections
- [ ] Terminology is consistent throughout
- [ ] Version numbers match codebase
- [ ] Status reflects actual feature state
- [ ] No empty sections (remove rather than leave blank)
- [ ] Formatting follows existing doc patterns in the project

### Step 7: Save Documentation

**File location:**
- **NEW/DOCUMENT mode**: `docs/features/[FRXX]-[feature-name].md`
  - Auto-increment `FRXX` by scanning: `Glob: docs/features/FR*.md`
  - Find highest FR number, increment by 1. If none exist, start at FR01.
- **UPDATE mode**: Edit the file in place at its existing path.

Create `docs/features/` if it doesn't exist.

**UPDATE mode**: Preserve existing structure and style.
**DOCUMENT/NEW mode**: Do NOT overwrite existing docs without user confirmation.

### Step 8: Output Summary

```markdown
## Feature Documentation Summary

**Mode:** [Document | New | Update]
**Feature:** [feature-name]
**File:** docs/features/[FRXX]-[feature-name].md

### What was done
- [List of sections created/updated]

### Files Analyzed
- [List of source files read during analysis]

### Cross-References
- [Related docs that link to/from this one]
- [Docs that may need updating due to this feature]

### Quality Check
- All file references valid
- No sensitive data
- Consistent formatting
- (or warnings if any checks failed)

### Next Steps
- [Review the documentation]
- [Any manual steps needed]
- [Related docs to update]
```

---

## Error Handling

- Feature name matches no code AND no mode specified → ask user to clarify
- Update target file doesn't exist → inform user, offer to create in document mode
- Codebase search returns >100 files → ask user to narrow scope
- No `docs/features/` directory → create it, inform user
- Existing doc would be overwritten → ask for confirmation

## Examples

```
# Document an existing feature
/feature-docs rate-limiting "Token bucket rate limiter for API endpoints"
/feature-docs document auth "Authentication and authorization system"

# Plan a new feature
/feature-docs new websocket-notifications "Real-time push notifications via WebSocket"
/feature-docs new dark-mode "System-wide dark mode with theme persistence" docs/design-system.md

# Update existing docs
/feature-docs docs/features/FR12-rate-limiting.md
/feature-docs update docs/features/FR05-auth.md
```
