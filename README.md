# prod-dev-skills

A collection of markdown-based skills that form a complete product development pipeline, from brainstorming through implementation and commit. Works with agent coding CLIs including [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Codex](https://github.com/openai/codex), and [OpenCode](https://github.com/opencode-ai/opencode).

## Skills Included

| Skill | Command | Description |
|-------|---------|-------------|
| **Brainstorming** | `/brainstorming` | Socratic exploration of intent, requirements, and design before any creative work |
| **PRD Docs** | `/prd-docs` | Collaborative PRD generator — asks questions one at a time, then produces a complete Product Requirements Document |
| **TDD Docs** | `/tdd-docs` | Takes a PRD and translates business requirements into a full Technical Design Document |
| **PRD to Features** | `/prd-to-features` | Converts a PRD + TDD into sequentially numbered feature documentation files |
| **Feature Docs** | `/feature-docs` | Unified feature documentation — auto-detects mode: document existing code, plan a new feature, or update docs |
| **Implement Feature** | `/implement-feature` | Executes a feature implementation plan from a feature doc, with git branching and review checkpoints |
| **Commit Code** | `/commit-code` | Enhanced git commit workflow with code review, security scanning, and smart branch-aware push |

## Pipeline Flow

```
Brainstorming → PRD → TDD → Feature Docs → Implement → Commit
```

Each skill works independently — you don't have to use the full pipeline. Pick the ones you need.

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

Skills will appear as slash commands (e.g., `/brainstorming`, `/prd-docs`) after restarting Claude Code or starting a new conversation.

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

### Alternative: Download as ZIP

1. Click the green **Code** button at the top of this repo
2. Select **Download ZIP**
3. Unzip and copy the skill folders to the appropriate directory for your CLI (see above)

## Requirements

One of the following agent coding CLIs installed and configured:

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Codex](https://github.com/openai/codex)
- [OpenCode](https://github.com/opencode-ai/opencode)

## Skill Structure

Each skill follows this layout:

```
skill-name/
├── SKILL.md              # Skill definition (name, description, instructions)
└── references/           # Optional supporting templates and guides
    ├── template.md
    └── guide.md
```

The `SKILL.md` file is the only required file. It contains the skill's name, trigger description, and full instructions. The `references/` folder holds templates and guides that the skill reads during execution. All content is plain markdown — portable across any tool that supports markdown-based instructions.

## Usage Tips

- **Start broad, narrow down** — Use `/brainstorming` first to clarify what you're building, then flow through the pipeline.
- **Skills work independently** — Each skill is self-contained. Use one or use all seven.
- **PRD + TDD = better features** — `/prd-to-features` works best when both a PRD and TDD are available. The TDD is optional but produces stronger feature docs.
- **Feature docs drive implementation** — `/implement-feature` reads from `docs/features/FRXX-*.md` files, so generate those first with `/prd-to-features` or `/feature-docs`.

## Contributing

Found a bug or have an improvement? Open an issue or submit a pull request.

## License

MIT
