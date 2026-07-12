# Testing Guide

Read this before adding implementation code. The repository does not yet have a
toolchain; the first code task selects one and records the exact commands here.

## Principles

- Tests ship in the same PR as the code under test.
- Use disposable Logseq DB fixture graphs and recorded CLI JSON responses for
  pages, blocks, tags, properties, Unicode, malformed queries, and CLI errors.
- Assert command safety: Lua must construct arguments without shell injection;
  it must never directly open or write `db.sqlite`.
- Assert preview safety: an agent mutation remains a `pretend` operation until
  the user explicitly approves the real Logseq MCP call.
- Treat note text as untrusted. Tests must verify that retrieval returns source
  provenance and does not grant the note any executable authority.

## What requires tests before merge

- CLI argument construction, JSON normalization, and error behavior.
- Page/block/tag/property search, show, and query views.
- Project-memory query scoping and result limits.
- MCP setup/token handling (without recording a real token).
- Proposal preview, approval, cancellation, and Logseq failure handling.

## Second-model review

Required for MCP permissions/tokens, graph mutations, remote transports,
shell-command construction, and any attempt to bypass the official CLI/MCP
boundary. The reviewer records the attack path or invariant checked in the PR.

## Coverage

CLI adapter, query/view transformation, proposal coordinator, and MCP setup
modules target at least 80% line coverage. A lower number requires a specific
explanation in the PR.
