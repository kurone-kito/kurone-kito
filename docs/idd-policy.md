# IDD Policy Configuration

This repository uses the following IDD policies:

## Development Branch

**Branch**: `master`

## Merge Policy

**Policy**: `fully_autonomous_merge`

## PR Review Policy

**Profile**: `copilot-advisory`

## Review-Thread Resolution Policy

**Policy**: `fast-agent-resolve`

## Critique-Loop Profile

**Profile**: `distributed-defaults`

## Credential Scope

**Scope**: `Repository-scoped gh CLI OAuth token,
Write-collaborator-equivalent (contents, issues, pull-requests
read/write, actions read); no publish or deployment secrets required`

## Claim Timing

- **claim-stale-age**: 24 h (distributed default)
- **claim-heartbeat-interval**: 12 h (distributed default)

## CI Wait Policy

- **running timeout**: `PT30M` / 30 min (distributed default, not
  confirmed by this hearing item)
- **generation timeout**: `PT10M` / 10 min (distributed default, not
  confirmed by this hearing item)
- **rerun policy**: `rerun-once`

## Issue-Author Approval Gate

**Selection**: `enabled-by-default`

## Maintainer Approval Actor Policy

**Policy**: `owners-and-maintainers-only`

## Issue-Authoring Companion

**Status (this core-bootstrap issue, #18)**: `not installed` — temporary,
core-bootstrap-only override. The hearing's real, operator-confirmed
answer is `installed`, but issue #18 defers the actual file install to a
separate follow-up issue.

**Target state (for #28 to read)**: `installed`, native destination
`.agents/skills/issue-authoring/` (single root only; not duplicated into
`.claude/skills/`). Issue #28 installs the companion files under this
destination and flips this status to `installed` once it merges.

## Helper Runtime Profile

**Profile**: `ephemeral-npx`

**Package spec (pinned)**:
`github:kurone-kito/idd-skill#adad8ae43c5a1b6fc3a100ce384c8a84a8d5139d`
(`helperRuntime.packageSpec` in `.github/idd/config.json`). Every
`ephemeral-npx` helper invocation resolves to this exact upstream commit
instead of the mutable default archive URL
(`https://codeload.github.com/kurone-kito/idd-skill/tar.gz/refs/heads/main`),
closing the wider-execution-surface gap documented in
`.github/workflows/post-merge-cleanup.yml` and
`docs/idd-helper-scripts.md`'s "Profile Wiring Surface" section. Bump
this pin deliberately (a separate, reviewed change) when adopting a
newer upstream commit — do not let it drift silently.

## IDD Label Names

**Selection**: `distributed-defaults`

## Up-to-Date-Head Ruleset

**Policy**: `disabled`

## Bootstrap Execution Mode

**Mode**: `issue-mediated`
