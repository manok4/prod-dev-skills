# Feature Documentation Template

Unified template for all modes. Adapt sections based on mode:

- **DOCUMENT mode**: Fill all sections from code analysis. Use definitive language.
- **NEW mode**: Fill sections with planned/proposed details. Use forward-looking language. Include implementation plan and future sections.
- **UPDATE mode**: Surgically update only sections that have discrepancies. Preserve existing accurate content. Add changelog entry.

---

## Template

```markdown
# [Feature Name]

**Status:** [Implemented | In Development | Beta | Deprecated]
**Version:** [version from package/code]
**Last Updated:** [current date]
**Owner:** [team/person if known]

---

## Table of Contents
- [Overview](#overview)
- [Scope](#scope)
- [Acceptance Criteria](#acceptance-criteria)
- [User Flow](#user-flow)
- [Architecture](#architecture)
- [Technical Details](#technical-details)
- [Configuration](#configuration)
- [Dependencies](#dependencies)
- [Usage Examples](#usage-examples)
- [Testing](#testing)
- [Security Considerations](#security-considerations)
- [Performance](#performance)
- [Edge Cases & Error States](#edge-cases--error-states)
- [Error Handling](#error-handling)
- [Common Issues & Solutions](#common-issues--solutions)
- [Limitations](#limitations)
- [Quick Reference](#quick-reference)
- [Changelog](#changelog)
- [References](#references)

---

## Overview
- What the feature does (3-4 sentences)
- Why it exists (business/user value)
- When/where it's used
- Key characteristics or constraints

## Scope
- **In scope**: What is part of this feature
- **Out of scope**: What is explicitly NOT included
- **Prerequisites**: Features or infrastructure that must exist first
- **Depends on**: APIs, data, or services this feature needs

## Acceptance Criteria

Numbered list of specific, testable criteria. Each must be independently verifiable.

### Must-Have (P0)
1. User can [action] and sees [result]
2. System [validates/enforces] [rule]
3. [State change] persists after [trigger]

### Should-Have (P1)
4. [Secondary capability]
5. [Edge case handling]

### Nice-to-Have (P2)
6. [Polish item]

Each criterion must be a concrete behavior, not a vague quality.
- "User can filter by date range and sees results update within 500ms"
- NOT "Filtering works well"

## User Flow

### Happy Path
1. **Entry point**: User navigates to [page] and sees [initial state]
2. **Action**: User clicks [element] / fills [form] / selects [option]
3. **Feedback**: System shows [loading state / optimistic update]
4. **Result**: User sees [success state / new content / confirmation]
5. **Next step**: User can now [follow-up action]

### Alternate Paths
- **Path B**: If user [alternative action], then [what happens]
- **Path C**: If user [cancels/goes back], then [what happens]

## Architecture
- High-level component diagram (Mermaid):
  ```mermaid
  graph TD
      A[Component A] -->|data flow| B[Component B]
      B --> C[Component C]
  ```
- Data flow and interactions between components
- External dependencies and integrations
- Technology stack

## Technical Details

### Core Files
| File | Responsibility |
|------|---------------|
| `path/to/file.ts` | Description |

### Key Implementation
- Core algorithms or logic flows
- Important functions/classes with file:line references
- **Show real code, not descriptions.** Include actual implementation snippets from the codebase
  (or proposed code for NEW mode), not summaries like "add validation" or "handle errors."
  Every snippet must be copy-pasteable and syntactically correct.

### API Reference (if applicable)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/resource` | Description |

Request/response examples for each endpoint.

### Component Reference (if applicable)
- Component hierarchy
- Props/state interfaces
- Event handlers

## Configuration
| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `CONFIG_KEY` | string | `"default"` | Description |

## Dependencies
| Package | Version | Purpose |
|---------|---------|---------|
| `library` | `^1.0.0` | Description |

## Usage Examples

### Basic Usage
```language
// example code
```

### Advanced Patterns
```language
// advanced example
```

## Testing

### Test Coverage
| Test File | Type | Coverage |
|-----------|------|----------|
| `path/to/test` | Unit | Description of what it covers |

### Running Tests
```bash
# command to run tests
```

### Manual Testing Checklist
- [ ] Step 1: Verify X works
- [ ] Step 2: Verify Y works

## Security Considerations
- Authentication/authorization requirements
- Input validation approach
- Data protection measures
- Known security constraints

## Performance
- Key performance characteristics
- Optimization techniques used
- Caching strategy (if any)
- Scalability considerations

## Edge Cases & Error States

### Input Validation
- Empty/missing required fields → [error message]
- Invalid format (email, URL, etc.) → [error message]
- Input too long/short → [error message]

### System Errors
- Network failure → [retry behavior, offline message]
- Server error (500) → [error UI, retry option]
- Timeout → [timeout message, recovery]

### Permission & State
- Unauthorized access → [redirect, error message]
- Stale data / concurrent edit → [conflict resolution]
- Empty state (no data yet) → [empty state UI, CTA]
- Loading state → [skeleton / spinner / optimistic update]
- Rate limited → [throttle message]

## Error Handling
| Error | Code | Cause | Resolution |
|-------|------|-------|------------|
| ErrorName | CODE | Why it happens | How to fix |

## Common Issues & Solutions
- Known limitations
- Debugging tips
- FAQ

## Limitations
- Current constraints
- Edge cases
- Planned improvements

## Quick Reference
One-page cheat sheet:
- Key commands/API calls
- Configuration essentials
- Troubleshooting quick tips

## Changelog

### [Date]
- Initial documentation / Updated sections / etc.

## References
- Related documentation links
- External resources
- Design documents
```

---

## NEW Mode Additions

**NEW mode only** — add these sections after "Limitations":

```markdown
## Implementation Plan

Break the implementation into bite-sized tasks. Each task targets one component
and follows this structure:

### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.ts`
- Modify: `exact/path/to/existing.ts:45-67`
- Test: `tests/exact/path/to/test.ts`

**Step 1: Write the failing test**
```language
// complete, copy-pasteable test code
```

**Step 2: Verify test fails**
Run: `<exact command>`
Expected: FAIL with "<expected error>"

**Step 3: Implement**
```language
// complete, minimal implementation code
```

**Step 4: Verify test passes**
Run: `<exact command>`
Expected: PASS

**Granularity rule:** Each step should be a single action (2-5 minutes).
"Write the test" is a step. "Implement the feature" is too big — split it.

## Deployment Plan
- Deployment strategy
- Environment configurations
- Database migrations
- Rollback procedure
- Feature flag rollout plan

## Monitoring Plan (if applicable)
- Key metrics to monitor
- Alerting configuration
- Health check endpoints
- SLA/SLO definitions

## Future Enhancements
- Planned features
- Potential improvements
- Technical debt items
```

---

## Section Relevance Rules

Not every section applies to every feature. Apply these rules:

- **Skip Acceptance Criteria** in DOCUMENT mode for purely internal/backend utilities
- **Skip User Flow** if not a user-facing feature (e.g., internal library, build tooling)
- **Skip Edge Cases** in DOCUMENT mode if the feature has no user input or external interaction
- **Skip API Reference** if the feature has no endpoints or public API
- **Skip Component Reference** if not a frontend feature
- **Skip Performance** if not performance-sensitive
- **Skip Monitoring Plan** if not a service/backend feature
- **Skip Deployment Plan** if documenting a library/utility

For skipped sections, do NOT include empty sections. Simply omit them.
