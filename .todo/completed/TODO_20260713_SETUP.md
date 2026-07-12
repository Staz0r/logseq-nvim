# Goal

Make the fresh public repository ready for disciplined prototype work: replace
the placeholder CI with lightweight integrity checks, publish a phase plan with
explicit uncertainty controls, and create the local Warp workspace.

# Type

chore

# Priority (1-4)

1

# Status

completed

# Effort

S

# Workspace

- dir: /home/user/Projects/logseq-nvim
- branch: ci/workspace-plan
- CI and planning only; no Logseq graph or plugin implementation

# Owners

- **Codex** — add CI, planning, and local workspace configuration.

# Acceptance Criteria

1. CI validates repository integrity without requiring an unselected plugin
   toolchain.
2. The delivery plan has phase gates, validation evidence, and explicit
   controls for uncertain API/agent behavior.
3. Warp can open a project workspace with Codex, Agy, and terminal panes.

# Files

- .github/workflows/ci.yml
- .gitignore
- docs/IMPLEMENTATION_PLAN.md
- ~/.local/share/warp-terminal/launch_configurations/logseq_nvim.yaml

# Result

Replaced the placeholder CI with dependency-free repository-integrity checks,
added the implementation plan with evidence gates and uncertainty controls, and
created the Logseq Nvim Warp launch configuration. The CI passed after its
placeholder check was narrowed to actual project documents.
