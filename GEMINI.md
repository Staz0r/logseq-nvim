# GEMINI.md

**Read `AGENTS.md` first — it is the canonical ruleset** for this repo
(workflow, todos, git/PR rules, testing policy, RTK, multi-AI partition). This
file adds nothing new; it exists so the Gemini CLI loads the rules.

Key reminders (full detail in `AGENTS.md`):
- One task at a time; branch → PR → merge, never commit to `main`.
- Conventional Commits; **no AI attribution** in commits or PRs.
- Tests ship in the same PR as the code. PR body follows
  `.github/pull_request_template.md`.
- When acting as the **second-model reviewer** on a risky PR (money / auth /
  crypto / migration / security), record a one-line note of what you checked.
- Before project work, create or reference a `TODO_XXXXXX` in `.todo/active/`.
