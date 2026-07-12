# PR Review Checklist

Use the sections that match the change. Record concrete findings in the PR or
task; do not check boxes mechanically.

## Scope and behavior

- [ ] The diff implements one logical task and its acceptance criteria.
- [ ] Unrelated refactors, generated output, credentials, and local state are absent.
- [ ] Error, empty, loading, permission, and retry paths are handled where relevant.
- [ ] Backward compatibility and downstream consumers were considered.

## Security and data

- [ ] Authorization is enforced at the trusted boundary, not only in UI code.
- [ ] Responses, logs, analytics, and client payloads do not leak protected data.
- [ ] Inputs are validated and sensitive values are not committed.
- [ ] Migrations and destructive operations include rollback or recovery notes.

## Verification

- [ ] Tests assert the behavior and important failure cases.
- [ ] Lint, static analysis, tests, and build checks appropriate to the change pass.
- [ ] Documentation-only changes use Validation rather than claiming app tests.
- [ ] Risky changes received the required second-model review.

## Maintainability

- [ ] The implementation follows existing boundaries and naming.
- [ ] New dependencies are necessary, current, licensed, and documented.
- [ ] Project memory, decisions, schema, and follow-up todos are updated.
- [ ] The full diff was self-reviewed before requesting merge.
