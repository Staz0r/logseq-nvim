# Agent Skills

This directory contains skills intentionally maintained as part of the project
workflow. A fresh clone should not require another developer's home directory,
session history, credentials, or package caches.

## Bundled

- `task-creator` — creates and moves todo files using the repository's
  `.todo/TEMPLATE.md` schema.

## Third-party skills

Do not copy an entire personal `~/.agents/skills` or `~/.codex/skills`
directory into a project. For each external skill:

1. Record its official source and version or commit.
2. Check its license before vendoring or redistribution.
3. Prefer the provider's installer or package mechanism.
4. Review scripts and MCP configuration before enabling them.
5. Keep tokens, local paths, caches, histories, and generated plugin files out
   of Git.

Example inventory:

| Skill | Source/version | Installation | Vendored? |
|---|---|---|---|
| <<name>> | <<official URL + version>> | <<command or documented process>> | no |

Treat UI review skills such as Impeccable as optional development tooling unless
the project deliberately depends on a pinned, redistributable version.
