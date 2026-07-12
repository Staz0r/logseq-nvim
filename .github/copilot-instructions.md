# GitHub Copilot instructions

The canonical ruleset for this repo is **`AGENTS.md`** — read it. Copilot loads
this file automatically; it only restates the essentials.

- One logical change at a time; small, reviewable diffs.
- Branch → PR → merge; never commit to `main`.
- Conventional Commits (`type(scope): subject`). **No AI attribution / no
  `Co-Authored-By` for any AI.**
- Tests ship with the code they cover.
- Match the existing code's style, naming, and comment density.

Full workflow, testing policy, and PR format: see `AGENTS.md` and
`.github/pull_request_template.md`.
