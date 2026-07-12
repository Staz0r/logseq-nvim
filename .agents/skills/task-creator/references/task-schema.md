# Task Schema

Every task follows `.todo/TEMPLATE.md`:

```markdown
# Goal
One or two sentences describing the outcome and reason.

# Type
task | bug | chore | spike

# Priority (1-4)
1

# Status
backlog | active | blocked | completed

# Effort
XS | S | M | L | XL

# Dependencies
- blocks: TODO_XXXXXX
- blocked by: TODO_XXXXXX

# Workspace
- dir: /absolute/path/to/worktree
- branch: type/short-description
- high-blast-radius flags when applicable

# Owners
- **Person or agent** — responsibility

# Acceptance Criteria
1. A specific, testable outcome.

# Files
- expected/path

# Notes
- constraints and implementation context

# References
- https://example.com/relevant-source

# Result
Who completed it, files changed, commit/PR, side effects, and completion status.
```

## Field rules

- Omit Dependencies and References when empty.
- Workspace is mandatory for delegated work and recommended for concurrent
  worktrees.
- Status must match the containing folder.
- Result is filled before moving to completed and omitted or left as the
  template placeholder before completion.
- IDs are repository-local, monotonically increasing, and six digits.
