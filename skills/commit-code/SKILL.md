---
name: commit-code
description: >
  Enhanced git commit workflow with code review, security scanning, documentation updates,
  and smart branch-aware push. Use when the user wants to commit, review, and push code changes.
  Invoked via /commit-code [doc_file1, doc_file2, ...]
user_invocable: true
---

# Commit Code

Lint and validate early, review code changes, update documentation, conduct code review, commit, and push.

## Usage
```
/commit-code [doc_file1, doc_file2, ...]
```

## Arguments
$ARGUMENTS - Optional comma-separated relative paths to documentation files to update (e.g., `docs/features/FR20-unit-integration-testing.md, docs/architecture.md`)

## Instructions

When this skill is invoked, follow these steps **in order**. Do NOT skip steps.

---

### Step 1: Pre-flight Safety Checks

Run these commands in parallel to assess repository state:

```bash
git status
git branch --show-current
git diff --stat
```

**Gate checks** (stop and inform user if any fail):
- **Merge conflicts**: If `git status` shows merge conflicts, STOP and inform the user
- **Dirty submodules**: If submodules have uncommitted changes, warn the user
- **No changes**: If `git status` shows a clean working tree, inform the user there's nothing to commit

Record the current branch name for use in the push step.

---

### Step 2: Review Code Changes

Run to get full diff details:

```bash
git diff
git diff --cached
```

Analyze the changes to understand:
- What files were modified, added, or deleted
- What functionality was changed
- The scope and purpose of the changes
- Whether changes are logically cohesive or should be split into separate commits

If changes span unrelated features/fixes, **suggest splitting into multiple commits** before proceeding.

---

### Step 3: Lint & Type Validation

Run automated quality checks early — catch mechanical issues before the code review sees the diff.

**3a. Detect the project type** by checking for these files (in parallel):

```bash
cat package.json 2>/dev/null | head -80
ls next.config.* biome.json .eslintrc* eslint.config.* tsconfig.json .prettierrc* 2>/dev/null
```

**3b. Determine and run the appropriate lint command:**

1. **Check `package.json` scripts first** — if a `lint` script exists, prefer it:
   ```bash
   npm run lint
   ```
   This covers most React projects (Next.js, Vite, CRA) since they typically define a `lint` script that wraps their configured linter.

2. **If no `lint` script exists, detect by project type:**
   - **Next.js** (has `next` in dependencies and `next.config.*`): `npx next lint`
   - **ESLint config present** (`.eslintrc*` or `eslint.config.*`): `npx eslint . --max-warnings=0`
   - **Biome** (`biome.json`): `npx biome check .`
   - **Non-JS projects**: Check for `pyproject.toml` (ruff/flake8), `Cargo.toml` (clippy), etc.

3. **Run type checking** if `tsconfig.json` exists:
   ```bash
   npx tsc --noEmit
   ```

4. **Run formatter check** if configured:
   - If `format:check` or `prettier` script in `package.json`: `npm run format:check`
   - If `.prettierrc*` exists but no script: `npx prettier --check "src/**/*.{ts,tsx,js,jsx}"`

**3c. Scope to changed files where possible.** For large projects, scope lint to only the changed files to reduce noise and speed up execution:

```bash
git diff --name-only --diff-filter=d | grep -E '\.(ts|tsx|js|jsx)$'
```

Then pass the file list to the linter (e.g., `npx eslint file1.tsx file2.ts`). Fall back to a full project lint if file-scoped linting is not supported by the tool.

**Gate check:**
- If lint or typecheck **fails with errors**, present the errors to the user and **ask whether to fix or proceed**
- If only warnings, note them but proceed
- If no validation tools are detected in the project, skip this step silently

---

### Step 4: Identify Documentation to Update

**If `$ARGUMENTS` is provided:**
- Parse the comma-separated list of documentation file paths
- Verify each file exists; warn if any don't exist and continue with existing ones

**If `$ARGUMENTS` is empty or not provided:**
- Search for relevant documentation files in `docs/` and `claudedocs/` directories
- Look for files that relate to the changed code
- Use Glob to find markdown files in those directories
- If no relevant docs are found, skip to Step 6

**Always check these files regardless of `$ARGUMENTS`:**

1. **Architecture docs** — check for `docs/architecture.md` or `ARCHITECTURE.md`. If the code
   changes affect system architecture (new components, changed data flow, new integrations,
   modified API surface, new services or modules), add the architecture doc to the update list.
   Skip if changes are purely internal (bug fixes, style, tests, refactors within existing components).

2. **CLAUDE.md** — check for `CLAUDE.md` at the project root. Only add to the update list if
   the code changes introduce something that directly affects how a developer (or Claude) works
   in this project. See Step 5 for the strict criteria.

---

### Step 5: Update Documentation

For each identified documentation file:
1. Read the current content
2. Determine what sections need updating based on the code changes
3. Update the documentation to reflect:
   - New features or functionality
   - Changed behavior or APIs
   - Updated configuration or usage instructions
   - Architecture changes
4. Preserve existing content that's still accurate
5. Only make changes that are directly relevant to the code changes

**Important:** Do NOT create new documentation files unless explicitly requested. Only update existing files.

#### 5a: Architecture Doc Updates (`docs/architecture.md`)

Update the architecture doc when the code changes introduce:
- A new top-level component, service, or module
- A new external integration or dependency
- Changed data flow between existing components
- New or modified API endpoints that alter the system surface
- Database schema changes (new tables, changed relationships)
- New infrastructure (queues, caches, workers)

**Do NOT update** for: internal refactors that don't change component boundaries, bug fixes,
test additions, style changes, or dependency version bumps.

When updating, match the existing document's style and structure. Add to existing sections
rather than creating new ones when possible.

#### 5b: CLAUDE.md Updates

CLAUDE.md must stay lean. Only update it when the code changes introduce something a developer
**must know to work in this project correctly**. Apply this strict filter:

**Update CLAUDE.md for:**
- New CLI commands or scripts added to `package.json` / `Makefile` / equivalent
- Changed build, test, or lint commands
- New environment variables that are required
- New project conventions established by the change (e.g., a new file naming pattern, a new
  directory convention)
- Changed directory structure at the top level
- New required setup steps

**Do NOT update CLAUDE.md for:**
- Individual feature descriptions (those belong in feature docs)
- Temporary implementation details
- Information already documented elsewhere (architecture docs, README)
- Anything that would make CLAUDE.md longer without being directly actionable
- Dependency additions (unless they require special setup)

When updating, keep additions to 1-3 lines. If an existing entry needs updating, edit in place
rather than adding a new entry.

---

### Step 6: Code Review

Conduct a thorough code review of all staged and unstaged changes. Use the `code-reviewer` subagent via the Task tool.

Review criteria:
- **Correctness**: Logic errors, off-by-one errors, null/undefined handling
- **Security**: Injection vulnerabilities, improper input validation, exposed secrets
- **Performance**: Unnecessary loops, missing indexes, N+1 queries, memory leaks
- **Code quality**: Naming clarity, DRY violations, dead code, proper error handling
- **Breaking changes**: API contract changes, removed exports, changed return types

**Gate check:**
- If **critical issues** are found (security vulnerabilities, data loss risks, logic errors), present findings to the user and **ask whether to proceed or fix first**
- If only **minor suggestions** are found, include them in the summary but proceed

---

### Step 7: Security Scan

Scan the diff for accidentally included sensitive data:

```bash
git diff --cached --name-only
git diff --name-only
```

**Check for these patterns in changed/added files:**
- `.env`, `.env.*` files
- Files containing `API_KEY`, `SECRET`, `PASSWORD`, `TOKEN`, `PRIVATE_KEY` patterns
- `credentials.json`, `serviceAccount.json`, `*.pem`, `*.key` files
- Hardcoded secrets, connection strings with passwords
- AWS access keys (`AKIA...`), private keys (`-----BEGIN`)

**Gate check:**
- If sensitive files or secrets are detected, **STOP and warn the user**. List the specific files/patterns found
- Do NOT proceed to staging until the user confirms or excludes the files

---

### Step 8: Stage Changes Selectively

**Do NOT use `git add -A` or `git add .`**

Instead:
1. Get the list of changed files from `git status`
2. Exclude any files that match sensitive patterns (`.env`, `*.key`, etc.)
3. Stage files by name:

```bash
git add file1.ts file2.ts docs/updated.md
```

4. Show the user what was staged:

```bash
git diff --cached --stat
```

---

### Step 9: Craft Commit Message

Based on the code changes, create a commit message following conventional commits format:

- `feat:` - New feature
- `fix:` - Bug fix
- `refactor:` - Code refactoring
- `docs:` - Documentation only
- `chore:` - Maintenance tasks
- `perf:` - Performance improvement
- `test:` - Adding/updating tests
- `ci:` - CI/CD changes
- `build:` - Build system changes

**Format:**
```
<type>(<scope>): <description>    # 50 chars max

<body>                             # Explain WHY, not WHAT
                                   # Wrap at 72 chars

<footer>                           # Breaking changes, issue refs
```

If multiple types of changes exist, use the most significant one as the prefix. For breaking changes, append `!` after the type: `feat(api)!: restructure response format`.

Use a HEREDOC for the commit message to preserve formatting:
```bash
git commit -m "$(cat <<'EOF'
<commit message here>
EOF
)"
```

---

### Step 10: Push with Smart Branch Detection

Determine push target based on the current branch (recorded in Step 1):

**If on a feature branch** (anything other than `main` or `master`):
```bash
git push origin <current-branch>
```
If the branch has no upstream, use:
```bash
git push -u origin <current-branch>
```

**If on `main` or `master`:**
```bash
git push origin main
```

**Push failure handling:**
- If push is rejected due to remote changes: suggest `git pull --rebase origin <branch>` then retry
- If push fails for other reasons: show the error and suggest resolution

---

### Step 11: Output Summary

Provide a summary in this format:

```
## Commit Summary

**Commit:** <short_hash> - <commit_message>
**Branch:** <branch_name>

### Code Changes
- <list of code files changed with brief description>

### Documentation Updated
- <list of doc files updated with what was changed>
- (or "No documentation changes" if none)

### Code Review
- <critical/minor findings summary, or "No issues found">

### Security Scan
- ✅ No sensitive data detected
- (or list of warnings/exclusions)

### Validation
- ✅ Lint passed | ⚠️ Lint warnings | ❌ Lint failed (proceeded by user choice)
- ✅ Types passed | ⏭️ No type checker configured
- (or "No validation tools detected")

### Push Status
- ✅ Pushed to <branch_name> successfully
- (or error details)
```

---

## Error Handling

- If `git status` shows no changes → inform user, exit
- If merge conflicts detected → STOP, inform user, exit
- If specified doc files don't exist → warn, continue with existing
- If code review finds critical issues → present findings, ask user
- If secrets detected in diff → STOP, warn user, list files
- If lint/typecheck fails → present errors, ask user
- If push fails → show error, suggest resolution
- If on detached HEAD → warn user, suggest creating a branch first
