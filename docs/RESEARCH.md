# Research — current Logseq DB, Neovim, and agent memory

Research date: 2026-07-13.

## Recommendation

Target the current **Logseq database graph**, not Logseq OG. The database
product is Logseq’s forward-looking app: it is local-first and adds performance,
sync/collaboration, data-integrity checks, a CLI, plugin APIs, and a native MCP
server. Logseq OG remains the Markdown/file product and is maintained for
reliability rather than new features.

This is a stronger basis than an Obsidian-style vault integration because
`logseq-nvim` can work with real graph entities—typed properties, tags, block
UUIDs, pages, node relationships, and Datascript queries—rather than inferring
structure from text files.

## Verified platform facts

### Logseq DB is a local graph with official automation surfaces

Logseq documents the database graph as locally stored and describes its CLI as
independent of the desktop app. The CLI and desktop coordinate a `db-worker`
and lock lifecycle; the disk SQLite database is the source of truth. The CLI
has JSON/EDN output and supports searching pages/blocks, showing block trees,
queries, and validated create/update/move/remove operations.

**Design consequence:** call the CLI from Lua; never query or mutate SQLite
directly. This protects compatibility, lets Logseq manage validation, and avoids
competing writers.

Sources: [DB product announcement](https://logseq.io/page/b2ad9ce1-9cb7-4436-8083-54cb4516d324/df4dc09d-0a12-4c87-904e-22a9bf4c350a), [DB
documentation](https://github.com/logseq/docs/blob/master/db-version.md), and
[CLI documentation](https://github.com/logseq/logseq/blob/master/docs/cli/logseq-cli.md).

### Logseq already has a native MCP server

Logseq DB documents an optional MCP server that runs from the desktop app or
CLI against a graph. It uses the local HTTP server with a bearer token. It
supports search, pages/tags/properties, block edits, batched operations, and a
`pretend` option. Current-graph changes are undo/redo-able and use app
validations.

**Design consequence:** `logseq-nvim` should not create a competing generic
memory server. It should make the official MCP server safer and easier for
coding workflows: scoped query conventions, client setup, previews, and review.

Source: [Logseq DB MCP-server documentation](https://github.com/logseq/docs/blob/master/db-version.md#MCP-Server).

### Neovim fits as a keyboard-first client

Neovim exposes Lua plugin APIs, user commands, async jobs, and RPC. A Lua
plugin can call the Logseq CLI asynchronously and present graph results through
pickers, quickfix lists, splits, and buffers without owning graph persistence.

Source: [Neovim Lua plugin guide](https://neovim.io/doc/user/lua-plugin/).

## “Better than Obsidian” means graph-native, not feature-count parity

The differentiators worth building are:

1. Typed, queryable project memory instead of folders/frontmatter conventions.
2. Stable block identity and block-level handoffs/decisions.
3. Graph query views for active work, decisions, unanswered questions, and
   lessons across projects.
4. One native graph authority for both Neovim and agents.
5. MCP writes that can be previewed (`pretend`), validated by Logseq, and
   undone in the app.

Do not claim feature superiority until the plugin proves a faster coding-memory
workflow in daily use. The first success metric is: start a coding session,
retrieve the relevant project context, make a reviewed memory update, and
resume accurately the next day without manually finding notes.

## Open prototype questions

1. Which native MCP tools and scopes are available in the released build the
   user runs, versus documented upcoming capabilities?
2. What CLI latency is acceptable for interactive picker/search use; is a
   short-lived cache needed?
3. How should a project relation be represented to make queries ergonomic while
   keeping personal and project memory separate?
4. Can Codex and Agy both use the native Streamable HTTP MCP endpoint with the
   desired approval model?
