# prod-dev-skills

A structured planning workflow for building software with coding agents. Seven markdown-based skills that take a project from idea to shipped code — brainstorming, requirements, technical design, feature planning, implementation, and commit.

Works with [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Codex](https://github.com/openai/codex), and [OpenCode](https://github.com/opencode-ai/opencode).

## Why Planning Over Scaffolding

The quality of what a model produces is directly tied to the quality of the plan you give it. For anything with real complexity, the model produces significantly better code when it understands the full picture first.

This workflow goes through a structured planning process. Each step produces a document that becomes the context for the next step, and eventually becomes the documentation for the project itself. Plans become documentation. Less duplicate work.

## Workflow

### New Project

For a new project, the full pipeline looks like this:

```
Brainstorming → PRD → TDD → PRD to Features → Implement → Commit
```

### Adding Features

Once a project exists, the workflow for adding features is shorter:

```
Brainstorming → Feature Docs → Implement → Commit
```

Each skill works independently — you don't have to use the full pipeline. Pick the ones that fit your situation.

## Skills

| Skill | Command | What it does |
|-------|---------|--------------|
| **Brainstorming** | `/brainstorming` | Prevents premature implementation. Explores the idea, proposes 2–3 approaches with trade-offs, runs a devil's advocate step, then writes a design document to `docs/plans/`. |
| **PRD Docs** | `/prd-docs` | Turns brainstorming output into a Product Requirements Document. Works through 11 discovery topics one question at a time. Adapts depth to the project. |
| **TDD Docs** | `/tdd-docs` | Translates a PRD into an engineering blueprint — architecture, database schema, API contracts, deployment, and implementation roadmap. |
| **PRD to Features** | `/prd-to-features` | Breaks the PRD and TDD into sequentially numbered feature docs under `docs/features/`. Creates a Phase 0 project-setup feature first, then user-facing features in phase order. |
| **Feature Docs** | `/feature-docs` | Three modes: plan a new feature, document existing code, or update docs after code changes. Plans become documentation after implementation. |
| **Implement Feature** | `/implement-feature` | Reads a feature doc, creates a git branch, and implements in batches of 3 tasks with test/lint/type-check after each batch. You review and give feedback between batches. |
| **Commit Code** | `/commit-code` | Lints, type-checks, updates relevant docs, runs a fresh sub-agent for code review, scans for secrets, then stages, commits, and pushes. |

## Installation

### Step 1: Clone the Repo

```bash
git clone https://github.com/manok4/prod-dev-skills.git
```

### Step 2: Install for Your CLI

Each skill is a folder containing a `SKILL.md` file and optional `references/`. Choose the instructions for your tool:

#### Claude Code

Copy skill folders into your Claude Code skills directory:

```bash
# Copy all skills
cp -r prod-dev-skills/skills/* ~/.claude/skills/

# Or copy individual skills
cp -r prod-dev-skills/skills/brainstorming ~/.claude/skills/
cp -r prod-dev-skills/skills/prd-docs ~/.claude/skills/
```

Skills appear as slash commands (e.g., `/brainstorming`, `/prd-docs`) after restarting Claude Code or starting a new conversation.

#### Codex

Copy skill folders into your Codex instructions directory:

```bash
# Copy all skills
cp -r prod-dev-skills/skills/* ~/.codex/skills/

# Or copy individual skills
cp -r prod-dev-skills/skills/brainstorming ~/.codex/skills/
```

Alternatively, reference the `SKILL.md` content in your project's `AGENTS.md` file.

#### OpenCode

Copy skill folders into your OpenCode instructions directory:

```bash
# Copy all skills
cp -r prod-dev-skills/skills/* ~/.opencode/skills/

# Or copy individual skills
cp -r prod-dev-skills/skills/brainstorming ~/.opencode/skills/
```

Alternatively, reference the `SKILL.md` content in your project's configuration.

#### Download as ZIP

1. Click the green **Code** button at the top of this repo
2. Select **Download ZIP**
3. Unzip and copy the skill folders to the appropriate directory for your CLI (see above)

## Documentation Structure

These skills produce documentation in a consistent layout. Every project follows this structure:

```
docs/
├── requirements/    → PRD and TDD
├── features/        → feature documents (plans that become documentation)
├── plans/           → brainstorming designs
├── architecture.md  → living architecture document, updated after each feature
└── readme.md        → index for all documents in the folder
```

The `features/` folder is where the planning-to-documentation lifecycle plays out. Each feature starts as an implementation plan. After the feature ships and `commit-code` runs, the same file gets updated to reflect what was actually built. Documentation stays grounded in reality because it evolved from the plan.

## Tips

- **Start with brainstorming** — The `/brainstorming` skill prevents premature implementation. Use it to clarify what you're building before flowing into the pipeline.
- **PRD + TDD together produce stronger features** — `/prd-to-features` works without a TDD, but with one it generates more complete feature docs including infrastructure setup.
- **Cross-model validation** — Send the PRD and TDD to a different model for review before starting implementation. Different models surface different gaps and challenge different assumptions.
- **Feature docs drive implementation** — `/implement-feature` reads from `docs/features/FRXX-*.md` files, so generate those first with `/prd-to-features` or `/feature-docs`.
- **Customize the skills** — These skills are plain markdown. Models are good at writing and modifying skills based on your requirements, so adapt them to your workflow.

## Contributing

Found a bug or have an improvement? Open an issue or submit a pull request.

## License

MIT
