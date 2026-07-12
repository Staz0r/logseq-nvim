# Roadmap

## Phase 0 — Research and contract

- [x] Confirm current Logseq DB as the product target.
- [x] Select official CLI/API and native MCP server as integration boundaries.
- [x] Define preview-first, user-approved agent writes.

## Phase 1 — CLI-backed Neovim graph workspace

- [ ] Lua plugin skeleton and `:LogseqHealth` for CLI/version/graph checks.
- [ ] JSON CLI adapter for graph list, search, show, and named queries.
- [ ] Neovim picker/quickfix views for pages, blocks, and query results.
- [ ] Fixture-driven adapter tests and headless Neovim command tests.

### Prototype exit criterion

Before expanding scope, use the prototype on a real coding project for at least
one week. It succeeds only if it makes session start/resume faster than manually
finding Logseq pages and produces a trustworthy, reviewable handoff.

## Phase 2 — Coding-memory conventions

- [ ] Observe the prototype’s actual retrieval and handoff needs before fixing
  property names or a public schema.
- [ ] Define an installable graph schema for project, decision, handoff, lesson,
  and active-task nodes from those observations.
- [ ] `:LogseqProjectMemory` view and daily-session handoff flow.
- [ ] Project-scoped query templates with bounded results and provenance.

## Phase 3 — Native MCP workflow

- [ ] Setup wizard/docs for Logseq local MCP server and token handling.
- [ ] Codex/Agy client configuration examples.
- [ ] Neovim proposal preview for MCP `pretend` operations.
- [ ] Explicit approve-and-apply workflow with attribution and undo guidance.

## Phase 4 — Advanced DB-native UX

- [ ] Tag/property editor and validated block capture.
- [ ] Query builder, linked-reference views, and task views.
- [ ] Performance measurements on large graphs and cache design if required.
