<!--
Title = the Conventional Commit subject (e.g. "feat(auth): add PIN login").
Required sections: Summary, Closes, and exactly one of Test plan or Validation.
Delete the unused verification section and optional sections that do not fit.
No AI attribution — describe the work, don't sign it.
-->

## Summary

One–two lines: what changed + why. Bold the **TODO_XXXXXX** it closes, then a
bullet per component/behavior.

### Decisions recorded (`docs/DECISIONS.md`)   <!-- only if a decision was logged -->
1. *Title* — one-line rationale.

### AI partition   <!-- only on multi-agent tasks -->
- **Codex** — …
- **Gemini** — …
- **Claude** — …

### Review fixes applied (this branch)   <!-- only if review iterations happened -->
| Issue | Fix |
|---|---|
| … | … |

## Test plan

<!-- Keep for code, configuration, schema, CI, or behavior changes. -->

- [ ] lint / static analysis — clean
- [ ] tests — N / N pass (baseline + new)
- [ ] <key cases covered>
- [ ] <deferred / manual check — say why it's deferred>

## Validation

<!-- Keep instead of Test plan for documentation-only changes. -->

- [ ] links and anchors checked where applicable
- [ ] terminology and project-memory consistency reviewed
- [ ] formatting / `git diff --check` — clean

<!-- Optional, add when they fit:
## Risk / Rollout   — migrations, data-affecting changes, flags, how to revert
## Deferred / Follow-ups   — out of scope + the todo it's parked in
## Breaking changes   — API / schema / behavior downstream must adapt to
## Screenshots / UX   — for visible UI changes
-->

## Closes

- TODO_XXXXXX — <scope>
