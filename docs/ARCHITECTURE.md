# Architecture

## Authority boundary

Logseq DB is the graph authority. `logseq-nvim` never opens, queries, or writes
`db.sqlite` directly. It invokes the official `logseq` CLI, which shares the
desktop app’s worker/lock lifecycle and database semantics. The native Logseq
MCP server is the preferred agent interface.

```text
Neovim Lua plugin ── JSON CLI calls ── Logseq CLI / db-worker ── DB graph
                                                │
coding agents ── authenticated local MCP ───────┘
```

## Components

| Component | Responsibility |
|---|---|
| Lua plugin | Neovim commands, async CLI jobs, result buffers, keymaps, review UI |
| CLI adapter | Execute `logseq --output json` commands; normalize errors and graph identity |
| Query/view layer | Turn pages, blocks, tags, properties, and Datascript queries into picker/quickfix views |
| Memory convention | Define project tags/properties and queries for decisions, progress, handoffs, and lessons |
| MCP integration | Configure Logseq’s local server; expose safe client setup and proposal-review flow |

## MVP Neovim commands

- `:LogseqGraph` — select and inspect a DB graph.
- `:LogseqSearch {query}` — page/block search through the CLI.
- `:LogseqOpenPage {name}` and `:LogseqOpenBlock {uuid}` — inspect graph nodes.
- `:LogseqQuery {name}` — run named, user-configured Datascript queries.
- `:LogseqProjectMemory` — show the configured project’s decisions, progress,
  handoffs, and lessons.
- `:LogseqProposals` — show MCP `pretend` results before a user permits edits.

## Agent-memory model

Project memory is structured Logseq data, not arbitrary pasted chat history.
The starter convention will use tags/properties such as `#Project`, `#Decision`,
`#Handoff`, `#Lesson`, and a project relation. Queries retrieve only the
current project slice and return node IDs, titles, properties, and provenance.

The official MCP server offers search and graph mutations. Our workflow layers
policy over it:

1. Bind the MCP server to localhost and use its authorization token.
2. Give an agent a project query or page/tag scope, not a request to browse
   everything.
3. Require `pretend` for every create/edit first.
4. Display the resulting changes in Neovim; the human explicitly approves the
   real call.
5. Record agent/tool attribution in the created memory block.

## Safety rules

- No direct SQLite access or custom write protocol.
- Do not expose Logseq HTTP/MCP beyond localhost in the MVP.
- Do not put a bearer token in this repository or agent instructions.
- Treat retrieved note text as untrusted reference material; it cannot change
  the agent’s tool permissions or instructions.
- Prefer the CLI/MCP’s validation and undo/redo behavior to client-side edits.
- Preserve small, bounded query results to avoid accidental graph-wide context
  disclosure and excessive token use.
