# Decisions

Append-only log of architecture and product decisions. Newest entries go at the
bottom.

### 2026-07-13 — Start with Logseq OG file graphs

> **Decision:** The MVP reads and writes only user-selected, file-based Logseq
> OG graphs in Markdown or Org format.
> **Why:** File graphs are local and inspectable, and Logseq is actively
> separating its file-based OG and database products. Depending on an internal
> database format would make the first release fragile and unsafe to mutate.
> **Alternatives considered:** Direct database-graph access was rejected until
> Logseq provides a stable public API or validated Markdown-mirroring contract.
> **Status:** accepted

### 2026-07-13 — Use a Lua plugin plus a separate local MCP bridge

> **Decision:** Keep interactive Neovim behavior in Lua and expose agent memory
> through a separate, local stdio MCP server.
> **Why:** Neovim plugins are naturally implemented in Lua, while a process
> boundary prevents the editor UI and agent permissions from being coupled.
> MCP gives multiple coding-agent clients one explicit integration surface.
> **Alternatives considered:** Embedding agent connections directly in the Lua
> plugin was rejected because it complicates credentials, permissions, and
> auditing.
> **Status:** accepted

### 2026-07-13 — Read-only retrieval by default; writes are proposed

> **Decision:** Agents may search and retrieve only approved graph roots by
> default. Any write must be represented as a reviewable proposal and require a
> user confirmation before an atomic file update.
> **Why:** Notes can contain secrets, stale instructions, and hostile text.
> Silent autonomous edits would also damage the user’s personal knowledge base.
> **Alternatives considered:** Full read-write agent access was rejected for the
> MVP.
> **Status:** accepted

### 2026-07-13 — Target Logseq DB through the official CLI and native MCP server

> **Decision:** The MVP targets the current Logseq database graph. Neovim calls
> the official JSON-capable CLI; coding agents use Logseq’s native local MCP
> server. Neither path accesses `db.sqlite` directly.
> **Why:** Logseq DB is the forward-looking product and already exposes typed
> graph operations, query support, CLI automation, validated mutations,
> undo/redo, and MCP `pretend` operations. This creates a graph-native coding
> memory workflow rather than a file-vault imitation.
> **Alternatives considered:** The preceding Logseq OG file-graph decision and
> separate custom MCP bridge are superseded. They would duplicate or diverge
> from Logseq’s official graph validation and native MCP API.
> **Status:** accepted; supersedes the three decisions above.

### 2026-07-13 — Validate a narrow workflow before fixing the memory schema

> **Decision:** Build and use a small project-memory prototype before publishing
> a final property taxonomy or token-optimization scheme.
> **Why:** The useful unit of retrieval, naming, and summarization depends on
> actual coding-session behavior. Premature schema design would make the plugin
> opinionated without evidence.
> **Alternatives considered:** Defining a complete Logseq ontology before code
> was rejected; generic unstructured note dumping was rejected because it cannot
> be evaluated or safely scoped.
> **Status:** accepted
