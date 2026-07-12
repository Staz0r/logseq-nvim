# Agent Instructions (canonical)

> This is the **single source of truth** for how any AI assistant works in this
> repo. The per-tool files — `CLAUDE.md`, `GEMINI.md`,
> `.github/copilot-instructions.md` — are thin and point here. Codex reads this
> file (`AGENTS.md`) natively. Edit the rules HERE; don't fork them per tool.

## Project

- **What:** Neovim workspace for the current Logseq database graph, with a
  project-memory workflow for coding agents through Logseq’s native MCP server.
- **Stack:** Lua/Neovim plugin; Logseq CLI with JSON output; Logseq DB graph;
  native local HTTP MCP server.
- **Build/run:** Not implemented yet. The first feature branch must document
  its chosen toolchain and exact lint/test commands in this section.

Architecture and domain rules are current working defaults, not final law. If a
better rule is chosen, record it in `docs/DECISIONS.md` and update the docs.

## Project Memory — read at the start of every session

- `docs/PROGRESS.md` — current phase, active task, blocker, next task.
- `docs/DECISIONS.md` — architecture/product decisions + rationale (append-only).
- `docs/SESSION_LOG.md` — append one line at session end (include the todo ID).
- `docs/SCHEMA.md` — data contract; update **before** schema/migration work. *(if the project has one)*
- `docs/ROADMAP.md` — phases + what's shipped vs gaps.

Keep memory current as part of the task, not as later cleanup: set `PROGRESS.md`
when work starts and ends, append `SESSION_LOG.md` at session end, record durable
architecture/product choices in `DECISIONS.md`, and mark roadmap items shipped only
after verification. Do not use these files as raw chat transcripts.

## Work in small steps — one task at a time

- **One logical change per branch / PR.** Never a massive block of unrelated
  work in a single PR — it's unreviewable and hard to revert.
- Build → **test + lint green** → commit → PR → merge → *then* the next task.
  Don't start task N+1 until N is verified and committed.
- Prefer many small, separately-reviewable PRs over one big one. If a task grows,
  split it (e.g. "core" PR + "UI" PR).

## Todo Tracking

Every session has a todo in `.todo/active/` (see `.todo/TEMPLATE.md` for the
shape). Use the `task-creator` skill if available.

- New work → create a todo first. Continuing work → reference the existing
  `TODO_XXXXXX` ID before editing files.
- Keep `docs/PROGRESS.md` aligned with the active todo; put the todo ID in
  `docs/SESSION_LOG.md` at session end.
- Moving a task to `.todo/completed/`: set `# Status: completed` and fill
  `# Result` **first**. Folder and field must always match.
- Skip a todo only for tiny clarification-only answers that change no files.
- Any todo that **delegates to an agent** carries a `# Workspace` section
  (dir + branch). The agent verifies `pwd` and `git branch` match it BEFORE the
  first write. Never work in the wrong checkout.
- **Confirm-files gate (high blast radius only).** For changes that are
  expensive to get wrong (MCP permission/token boundaries, graph mutation
  approval, CLI command safety, secret handling, or direct database access), the
  agent lists the exact files it will touch + a one-line plan and **waits for
  confirmation before the first write**. Routine tasks skip this.

## Skills

- Project-maintained skills live under `.agents/skills/` and may be committed
  when they are portable, reviewed, and intentionally part of this workflow.
- Third-party or machine-installed skills are dependencies, not project source.
  Record their source and installation steps in `.agents/README.md`; vendor
  them only when their license permits redistribution and pinning the files is
  an explicit project decision.
- Never commit skill caches, authentication state, MCP credentials, personal
  paths, session history, or generated plugin output.
- When a skill's schema overlaps a repository template, update both in the same
  PR and validate that they agree.

## Multi-AI partition

Work is split across assistants (e.g. Claude / Codex / Gemini / Copilot). When a
task is multi-agent:

- State who does what in the todo `# Owners` and the PR `### AI partition`.
- **Second-model review for risky PRs** (MCP security, graph writes, path
  traversal/symlink handling, secret handling, or database-graph adapters):
  a *different model than the author* reviews before merge and records a one-line
  note of what it checked. The author-model is blind to its own blind spots.
- Each assistant follows THIS file. The per-tool files don't add rules, only
  tool-specific usage notes.

## Git Workflow

### Branches
- `main` is always stable/deployable — **never commit directly to it**.
- Naming: `<type>/<short-description>` (`feat/auth-flow`, `fix/api-timeout`).
- One branch per logical feature/fix. Delete after merge.

### Commits — Conventional Commits
`type(scope): imperative subject` (under 50 chars, capitalized, no trailing period).

Types: `feat` `fix` `docs` `style` `refactor` `perf` `test` `chore` `ci` `build`.

- Imperative mood ("add" not "added"). Atomic — one logical change per commit.
- Body (wrapped ~72) explains **why + what** when not obvious.
- **No AI attribution.** Never add `Co-Authored-By:` / signature lines for any
  AI. Commits are authored by the human dev even when an AI drafted them.

### Pull Requests
- Title = the Conventional Commit subject.
- Body follows `.github/pull_request_template.md` (GitHub pre-fills it). Required:
  **Summary**, **Closes**, and exactly one verification section:
  - **Test plan** for code, configuration, schema, CI, or behavior changes.
  - **Validation** for documentation-only changes; record relevant link,
    formatting, consistency, and diff checks without claiming application tests.
  Add optional sections only when they fit.
- Self-review the full diff before merge. No AI attribution in the body either.

## Editing Files

- **Default to Edit/Write tools** — the harness tracks state, shows diffs, and
  fails loudly on a missing target, so a bad edit is caught.
- **Shell edits (`sed -i`, `perl -i`, heredocs) are fine** for bulk find-replace,
  generated content, or throwaway probes. Two guards: (1) never use one to skip
  Read-before-edit on a normal single-file change; (2) verify the result —
  `sed`/`perl` fail **silently** on a non-matching pattern.

## RTK Usage (token-saving shell wrapper)

RTK is the default shell-command wrapper for this workspace (if installed).

- Prefix shell commands with `rtk`: `rtk git ...`, `rtk rg ...`, and your
  build/test tools (e.g. `rtk <build-tool> test`).
- `rtk gain` / `rtk gain --history` for token-savings stats.
- `rtk proxy <cmd>` only when you need an unfiltered raw command.
- Project-specific output filters live in `.rtk/filters.toml` (committed, so they
  travel with the repo). Add filters for your verbose commands (test/lint/build).

## Testing Policy

Full playbook: `docs/TESTING_GUIDE.md`. Non-negotiables:

- Tests ship in the **same PR** as the code under test — no deferred test todos.
- A PR that adds graph parsing, indexing, MCP tools, or write coordination
  **without tests does not merge**.
- Graph mutations must use the official Logseq CLI/MCP validation path, be
  previewed before agent application, and preserve the user’s undo/redo story;
  MCP reads must be project-scoped and bounded.
- Coverage gate: parser, index, write-coordinator, and MCP-tool modules ≥ 80%
  line coverage; below → the PR explains why.
- Second-model review required for the risky paths listed above.

## Generated code (if the stack uses codegen)

Generated indexes, test snapshots, package output, and dependency directories
are **gitignored — never committed**. CI and every clone regenerate them.

- After `git pull` / fresh clone / branch switch, run the documented dependency
  install and generation commands for the selected implementation toolchain.
- CI runs any required generation before tests. Never `git add` generated
  output.

<!-- CODEGRAPH_START -->
## CodeGraph (if a `codegraph_*` MCP server is configured)

A tree-sitter knowledge graph of every symbol/edge/file. Sub-millisecond reads
with structure grep can't give. Prefer it for **structural** questions; use
grep/read only for literal text or once a file is already open.

| Question | Tool |
|---|---|
| "Where is X defined? / find symbol X" | `codegraph_search` |
| "What calls Y?" / "What does Y call?" | `codegraph_callers` / `codegraph_callees` |
| "How does X reach Y? / trace the flow" | `codegraph_trace` |
| "What breaks if I change Z?" | `codegraph_impact` |
| "Show Y's signature/source" | `codegraph_node` |
| "Focused context for an area" | `codegraph_context` |
| "Several symbols' source at once" | `codegraph_explore` |

Rules of thumb: answer architecture questions with `codegraph_context` then one
`codegraph_explore`; for a flow use `codegraph_trace` from→to then one
`codegraph_explore`. Trust results — don't re-verify with grep. Index lags
writes ~500ms; don't re-query immediately after an edit.

If `.codegraph/` doesn't exist the server returns "not initialized" — offer to
run `codegraph init -i`.
<!-- CODEGRAPH_END -->

## MCP Servers

- **context7** (`resolve-library-id`, `get-library-docs`): fetch up-to-date
  library docs before implementing/referencing any third-party package. Prefer
  it over training knowledge for API signatures, codegen, and version behavior.
