# prod-dev-skills

A collection of [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills that form a complete product development pipeline, from brainstorming through implementation and commit.

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

### Option 1: Clone the Repo (Recommended)

Clone this repository, then copy the skills you want into your Claude Code skills directory:

```bash
# Clone the repo
git clone https://github.com/manok4/prod-dev-skills.git

# Copy all skills
cp -r prod-dev-skills/skills/* ~/.claude/skills/

# Or copy individual skills
cp -r prod-dev-skills/skills/brainstorming ~/.claude/skills/
cp -r prod-dev-skills/skills/prd-docs ~/.claude/skills/
```

### Option 2: Download a Single Skill

If you only need one skill, you can download just its folder:

1. Navigate to the [`skills/`](skills/) directory in this repo
2. Open the skill folder you want (e.g., [`brainstorming/`](skills/brainstorming/))
3. Download the `SKILL.md` file and any files in the `references/` subfolder
4. Place them in `~/.claude/skills/<skill-name>/`

### Option 3: Download as ZIP

1. Click the green **Code** button at the top of this repo
2. Select **Download ZIP**
3. Unzip and copy the skill folders:

```bash
unzip prod-dev-skills-main.zip
cp -r prod-dev-skills-main/skills/* ~/.claude/skills/
```

### Verify Installation

After installing, restart Claude Code or start a new conversation. The skills will appear as slash commands (e.g., `/brainstorming`, `/prd-docs`).

Verify by running any skill command or checking `/help`.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed and configured

## Skill Structure

Each skill follows this layout:

```
skill-name/
├── SKILL.md              # Skill definition (name, description, instructions)
└── references/           # Optional supporting templates and guides
    ├── template.md
    └── guide.md
```

The `SKILL.md` file is the only required file. It contains the skill's name, trigger description, and full instructions. The `references/` folder holds templates and guides that the skill reads during execution.

## Usage Tips

- **Start broad, narrow down** — Use `/brainstorming` first to clarify what you're building, then flow through the pipeline.
- **Skills work independently** — Each skill is self-contained. Use one or use all seven.
- **PRD + TDD = better features** — `/prd-to-features` works best when both a PRD and TDD are available. The TDD is optional but produces stronger feature docs.
- **Feature docs drive implementation** — `/implement-feature` reads from `docs/features/FRXX-*.md` files, so generate those first with `/prd-to-features` or `/feature-docs`.

## Contributing

Found a bug or have an improvement? Open an issue or submit a pull request.

## License

MIT
