---
name: task-creator
description: Create, organize, list, and move repository todo files using the canonical .todo workflow.
---

# Task Creator

Manage work items under `.todo/`. The repository's `.todo/TEMPLATE.md` is
canonical; `references/task-schema.md` explains the same contract for agents
that load only this skill.

## Rules

1. Write every created task to disk. Do not merely describe it in chat.
2. Name files `TODO_{six_digit_id}_{snake_case_name}.md`.
3. Place files in `.todo/active/`, `.todo/backlog/`, `.todo/blocked/`, or
   `.todo/completed/`; the `# Status` value must match the folder.
4. Check every state folder for duplicates and the highest ID. Increment that
   ID while preserving six digits; start at `TODO_000001` when none exist.
5. Do not delete tasks. Move them when their state changes.
6. Before moving a task to completed, fill `# Result` and confirm every
   acceptance criterion.
7. Never modify product/source code as part of task-management-only work.

## Create

Extract:

- goal and type: `task | bug | chore | spike`
- priority: 1 now through 4 someday
- effort: `XS | S | M | L | XL`
- testable acceptance criteria
- expected files, dependencies, references, workspace, and owners

Ask for clarification only when acceptance criteria or a high-impact boundary
cannot be inferred safely. For a large feature, propose a task breakdown and
wait for confirmation before writing multiple tasks.

New work goes to `.todo/active/` only when work begins now; otherwise use
`.todo/backlog/`. A delegated task must include the exact workspace directory
and branch so the assignee can verify both before its first write.

## Move

- Start: backlog to active; set `# Status` to `active`.
- Block: move to blocked; set status and record the blocker.
- Defer: active to backlog; set status and explain why.
- Complete: fill Result, set status to completed, then move to completed.

Do not move a task without user authorization or an explicit workflow request
that necessarily includes the move.

## List

Show active tasks first, then backlog. Include ID, type, goal, priority, effort,
and blockers. Keep the output concise.
