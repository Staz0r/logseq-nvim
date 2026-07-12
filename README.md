# logseq-nvim

`logseq-nvim` is a Neovim-native workspace for the current **Logseq database
graph**. It uses Logseq’s official CLI/API as the graph authority and adds the
editor UX and coding-agent workflow that database-native Logseq users need.

## Why this can be better than an Obsidian integration

The goal is not “Obsidian.nvim for a different Markdown syntax.” Logseq DB
offers typed properties, first-class tags, stable block identities, graph
queries, validated mutations, local-first sync, and a native MCP server. The
plugin can make those graph semantics visible in Neovim instead of reducing
them to text-file links.

## Product thesis

One user-owned database graph can be both a personal knowledge base and a
durable coding-memory system. Neovim should be the fast keyboard-first client;
the official Logseq MCP server should be the agent connection; Logseq itself
should remain the only database writer.

## MVP

- A Lua Neovim plugin that calls the official `logseq` CLI with JSON output.
- DB graph discovery, page/block search, block-tree navigation, backlinks and
  queries in Neovim.
- Project-memory views built from configured tags/properties, not filename
  conventions.
- A safe integration guide for Logseq’s native local MCP server with Codex,
  Agy, and other compatible clients.
- Agent writes use Logseq MCP’s `pretend` mode first, then require user review
  and explicit approval.

`logseq-nvim` will not parse or write `db.sqlite` directly, and does not depend
on Logseq OG Markdown graphs.

## Status

Research and architecture are complete; implementation has not started. Read
[research](docs/RESEARCH.md), [architecture](docs/ARCHITECTURE.md), and the
[roadmap](docs/ROADMAP.md) before proposing code.

## Open source and prototype-first

This project is intended for public open-source collaboration under the MIT
License. The next goal is not a complete Logseq replacement: it is one useful
vertical slice that a developer can use daily—project-memory retrieval in
Neovim plus a reviewed native-MCP handoff. We will refine property names,
schemas, and token-efficient retrieval from observed usage, not invent a final
ontology before the prototype exists.

## References

- [Logseq DB documentation](https://github.com/logseq/docs/blob/master/db-version.md)
- [Logseq CLI documentation](https://github.com/logseq/logseq/blob/master/docs/cli/logseq-cli.md)
- [Neovim Lua plugin guide](https://neovim.io/doc/user/lua-plugin/)
- [Model Context Protocol specification](https://modelcontextprotocol.io/specification/)
