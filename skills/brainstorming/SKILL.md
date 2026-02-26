---
name: brainstorming
description: "You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation."
---

# Brainstorming Ideas Into High-Level Designs

Turn ideas into high-level designs through natural collaborative dialogue. Understand context, research prior art, ask questions one at a time, challenge assumptions, then present the recommended direction for approval.

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it. This applies to EVERY project regardless of perceived simplicity.
</HARD-GATE>

## When NOT to Use

Skip brainstorming when:
- Requirements are crystal clear and the user has already designed the solution
- Only one obvious approach exists (e.g., "rename this variable")
- It's an urgent bug fix with a known root cause
- The user explicitly says "just do it" or "skip the design phase"

Even "simple" projects benefit from a brief design check — but if the above apply, don't force it.

## Scope Boundaries

Brainstorming produces a high-level design — the "what" and "why", not the "how." Stay at the level of direction and rationale.

**Out of scope (belongs in PRD or TDD):**
- Detailed requirements lists or user stories
- User flow diagrams with error/edge cases
- Data models, schema design, or ERDs
- Library comparisons with adoption verdicts
- Architecture diagrams or component hierarchies
- API contracts or endpoint design
- Non-functional requirements with specific numbers
- Error handling strategies or testing plans

If the conversation starts drifting into these areas, note them as open questions for the next phase and move on.

## Checklist

Complete these steps in order:

1. **Explore project context** — check files, docs, recent commits
2. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
3. **Research** — find reference projects, proven patterns, and relevant libraries
4. **Propose 2-3 approaches** — informed by research, with trade-offs and your recommendation
5. **Challenge the approaches** — devil's advocate before committing
6. **Present high-level design** — recommended direction with reasoning, get user approval
7. **Write high-level design** — save to `docs/plans/YYYY-MM-DD-<topic>-design.md`
8. **Hand off** — ask user if they want to proceed to `prd-docs` (new project) or `feature-docs` (feature implementation), or come back later

## Process Flow

```
Explore context ──► Ask questions ──► Research prior art
                                            │
                                            ▼
                                   Propose 2-3 approaches
                                            │
                                            ▼
                                   Challenge approaches
                                   (devil's advocate)
                                            │
                                            ▼
                            ┌──► Present high-level design
                            │           │
                            │           ▼
                            │   User approves?
                            │     │          │
                            │    no          yes
                            │     │          │
                            └─────┘          ▼
                                  Write high-level design
                                            │
                                            ▼
                                    Hand off to next skill
                              (prd-docs or feature-docs)
```

## The Process

**Understanding the idea:**
- Check the current project state first (files, docs, recent commits)
- Ask questions one at a time to refine the idea
- Prefer multiple choice questions when possible, but open-ended is fine too
- Only one question per message — if a topic needs more exploration, break it into multiple questions
- Focus on understanding: purpose, constraints, success criteria
- See [references/socratic-questions.md](references/socratic-questions.md) for structured question templates

**Research:**

Once you understand what's being built, research before proposing approaches. See [references/research-guide.md](references/research-guide.md) for the full research protocol. In summary:

- Does this problem have **well-known solutions**? What approaches have others taken?
- Are there **libraries** that handle the hard part? (existence and maturity, not API details)
- What **mistakes have others made**? What anti-patterns should we avoid?
- Use Context7 for current library/framework documentation when relevant

Present research findings to the user before proposing approaches:
- "I found [project X] which solves a similar problem using [approach]. Here's what we can learn from it..."
- "There's a well-known pattern for this called [pattern]. It works by..."
- "[Library X] handles the hard part of this. It's mature and actively maintained."

**Skip research when**: The problem is purely internal/project-specific with no external parallels (e.g., "reorganize our config files"), or the user has already researched and is sharing their findings.

**Exploring approaches:**
- Propose 2-3 different approaches with trade-offs
- Ground proposals in research findings — reference the projects, patterns, or libraries discovered
- Present options conversationally with your recommendation and reasoning
- Lead with your recommended option and explain why

**Challenging the approaches (devil's advocate):**

Before presenting the design, stress-test the recommended approach:

- **Hidden assumptions**: "This assumes X. What if X is wrong?"
- **Failure modes**: "What breaks at 10x scale? What's the single point of failure?"
- **Simpler alternative**: "Could we solve 80% of this with something much simpler?"
- **Maintenance burden**: "In 2 years, will anyone understand why this was built this way?"
- **Security surface**: "What attack surface does this introduce?"

Share findings with the user. If a critical flaw is found, revise the approach before presenting the design.

**Presenting the high-level design:**
- Present the recommended direction — the problem being solved, the approach chosen, and why it was chosen over alternatives
- Keep it conversational, not structured — this is a checkpoint, not a deliverable
- Scale to complexity: a few sentences if straightforward, a couple of paragraphs if nuanced
- Reference research findings where they informed the recommendation
- Ask whether the direction looks right before writing it up
- Be ready to go back and clarify if something doesn't make sense
- Do NOT drift into detailed requirements, user flows, data models, or architecture — note those as open questions for the next phase

## After the High-Level Design

Write the validated high-level design to `docs/plans/YYYY-MM-DD-<topic>-design.md` using the template in [references/high-level-design-template.md](references/high-level-design-template.md). Commit the document to git.

**Hand off to the next skill.** The brainstorming session ends here — do NOT continue into detailed requirements, technical design, or implementation.

Ask the user if they'd like to proceed to the next skill now or come back to it later:

- **New project**: Invoke the `prd-docs` skill to begin detailed product requirements
- **Feature for existing project**: Invoke the `feature-docs` skill to plan the feature implementation

Do NOT auto-invoke either skill. Wait for the user's explicit confirmation.

## Key Principles

- **One question at a time** — don't overwhelm with multiple questions
- **Multiple choice preferred** — easier to answer than open-ended when possible
- **Research before proposing** — ground approaches in real-world evidence, not just intuition
- **YAGNI ruthlessly** — remove unnecessary features from all designs
- **Explore alternatives** — always propose 2-3 approaches before settling
- **Challenge before committing** — devil's advocate catches hidden flaws
- **Stay high-level** — capture the "what" and "why", leave the "how" for PRD and TDD
- **Incremental validation** — present the direction, get approval before writing it up
- **Be flexible** — go back and clarify when something doesn't make sense

## Common Pitfalls

**Information overload** — Asking many questions at once prevents conversation flow. One question per message.

**Single approach** — Jumping to one solution suggests you haven't explored alternatives. Always present 2-3 options with trade-offs.

**Over-engineering** — For 100 users/day, a monolith with PostgreSQL is sufficient. Don't reach for microservices, Kubernetes, or Kafka unless the scale demands it. Start simple.

**Ignoring existing code** — Read the codebase before proposing architecture. Consistency with existing patterns beats novelty unless there's a compelling reason to change.

**Skipping research** — Proposing approaches without checking how others have solved the same problem leads to reinventing the wheel or missing proven patterns.

**Skipping the challenge step** — Unchallenged ideas often have hidden flaws. Always stress-test before presenting as the final design.
