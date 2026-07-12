# Implementation Plan

This plan turns the roadmap into small PRs with evidence gates. It deliberately
keeps the plugin lightweight: Lua owns the Neovim experience; Logseq’s official
CLI and native MCP server own graph data and mutations.

## Phase 1 — Prove the CLI-backed Neovim loop

**PRs:** health check → graph selection → search/show → query view.

1. Implement `:LogseqHealth` to verify `nvim` version, `logseq --version`, CLI
   JSON output, configured graph availability, and actionable failure messages.
2. Add an async Lua CLI adapter with argument arrays—never shell-concatenated
   user input.
3. Add `:LogseqSearch`, `:LogseqOpenPage`, and `:LogseqOpenBlock` using the
   official CLI.
4. Add headless Neovim tests against recorded JSON fixtures and a disposable DB
   graph where practical.

**Exit gate:** a developer can locate and inspect a page or block from Neovim
faster than manually switching to Logseq, with clear errors when Logseq is not
available.

## Phase 2 — Prove a useful coding-memory loop

**PRs:** minimal convention → project-memory query → session handoff view.

1. Start with only four concepts: project, decision, handoff, and lesson.
2. Store each as native Logseq nodes/tags/properties, but do not freeze names
   until the prototype has been used on real projects.
3. Build `:LogseqProjectMemory` from named queries with a strict result limit
   and source IDs.
4. Use it for at least one week across active projects; record retrieval misses,
   redundant fields, and token cost.

**Exit gate:** the next-day coding session can recover relevant context without
manually searching several pages or past agent transcripts.

## Phase 3 — Prove the native-MCP handoff workflow

**PRs:** documented setup → read-only context → preview UI → approved writes.

1. Configure Logseq’s local native MCP server; keep its bearer token outside
   the repository.
2. Validate actual client compatibility with Codex and Agy, one client at a
   time, against the installed Logseq version.
3. Require MCP `pretend` for a proposed handoff/decision write.
4. Show the proposed change in Neovim; only an explicit user action runs the
   real mutation.

**Exit gate:** an agent can propose a useful handoff and the user can approve or
reject it without losing control of the graph.

## Phase 4 — Optimize only after measurements

**PRs:** telemetry schema → retrieval tuning → optional cache.

Measure locally and opt-in only:

- query latency and returned bytes;
- whether retrieved nodes were used, irrelevant, or missing;
- edits required after an agent’s proposed handoff;
- repeated fields/tags that should become stable conventions.

Then improve property names, query templates, summaries, and result limits.
Do not introduce embeddings, a second database, a custom MCP server, or a cache
until a measured bottleneck justifies it.

## Hallucination and uncertainty controls

No honest process can give a fixed probability that an AI will not hallucinate.
Instead, every claim is assigned an evidence level and gated accordingly.

| Risk | Why it is uncertain | Control before merge |
|---|---|---|
| Logseq CLI/MCP capability | DB features and docs are evolving | Test against the pinned installed Logseq version; record command/tool output in fixtures |
| Agent-client compatibility | Codex/Agy MCP support and approval behavior differ | Maintain a small compatibility matrix; verify one client/version at a time |
| Graph schema usefulness | Names and properties may not match real retrieval needs | Treat Phase 2 as a one-week experiment; defer public schema stability |
| AI-generated graph content | Agents can invent facts or follow hostile note text | Use provenance, bounded retrieval, `pretend`, visible diffs, and user approval |
| Plugin correctness | Neovim and CLI edge cases are easy to assume | Test argument construction, JSON normalization, failure paths, and headless commands |

The policy is simple: **no claim about a Logseq API, agent behavior, or memory
benefit becomes product behavior until it has a reproducible test or a recorded
manual validation.**
