# IDD Policy Configuration

This repository uses the following IDD policies:

## Development Branch

**Branch**: `master`

## Merge Policy

**Policy**: `fully_autonomous_merge`

**Enforcement (recorded 2026-09-16, issue #31, PR #37)**: at bootstrap
(#18/PR #29) this policy was opted into while `master` had no
GitHub-enforced required status check and `required_approving_review_count:
0` — the gate existed only by IDD convention, not by server-side
requirement. Issue #31 closed that gap: `master` now has classic branch
protection requiring the `lint` status check
(`required_status_checks.checks: [{context: "lint", app_id: 15368}]`,
`strict: false`, matching this repository's `Up-to-Date-Head Ruleset:
disabled` policy) with `enforce_admins: true`, so the requirement applies
even to the automation token's own admin-level access — not merely to
non-admin contributors. This coexists with, and does not replace or
weaken, the two active rulesets (`main`, `features`) and their
`copilot_code_review` / `pull_request` rules. Verified via
`gh api repos/kurone-kito/kurone-kito/branches/master/protection` and
`gh api repos/kurone-kito/kurone-kito/rulesets/20747408` (rules unchanged,
same `updated_at`) rather than by convention alone.

This GitHub-side mutation (repository administration write) was applied
directly by the operator's own authenticated session as a one-off,
explicitly authorized action outside the normal autonomous IDD claim/A4.5
scope — see the Credential Scope section below, which this repository's
autonomous IDD sessions remain bound by.

**Acknowledgment recorded (2026-09-17, issue #40)**: `mergePolicyAck:
"fully_autonomous_merge"` is now set in `.github/idd/config.json`,
confirming the opt-in already documented above and silencing
`idd-doctor`'s diagnostics-only reminder.

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

## Required-Check-Read Trust

**Policy**: `ciGate.trustEmptyProtectionReads: false` (reverted to the
fail-closed default 2026-09-16, issue #31/PR #37, superseding the
`true` value recorded 2026-09-16 for issue #19's PR #32).
`ciGate.trustSourcePinnedRequiredChecks: true` (recorded the same day,
issue #31/PR #37).

**`trustEmptyProtectionReads` history.** By default, a `404` from the
branch-protection or ruleset read endpoints is treated as unreadable
(same as a `403`), since neither endpoint documents `403` as a possible
response and a `404` can mask a permission failure. This repository
briefly opted into trusting a genuine `404` as "no required checks
configured" (`true`) between issue #19's PR #32 and issue #31's PR #37
(both 2026-09-16), while `master` had no classic branch
protection and the automation token's read access to the relevant
endpoints had been separately verified (`admin: true`; rulesets list
and detail endpoints both `200`). Issue #31 removed the underlying
condition this opt-in existed for: `master` now has classic branch
protection (see Merge Policy above), so `GET
/repos/{owner}/{repo}/branches/master/protection` now genuinely
returns `200`, not `404` — there is no longer an empty-read case for
this flag to trust. Leaving it `true` would only be a latent risk: a
future regression that accidentally removes or breaks that protection
would produce a real `404` that this flag would silently trust as
"nothing configured" instead of holding. The flag was reverted to
`false` (the fail-closed default) for that reason, not merely left in
place because "there's no harm."

**`trustSourcePinnedRequiredChecks`.** Issue #31's classic-protection
required check pins the `lint` context to a specific producer
(`app_id: 15368`, the standard GitHub Actions app) rather than
accepting any check named `lint` from any source — the security-safer
choice, but one the IDD readiness gate downgrades to
`source-pinned`/`unknown` by default even when the check is green,
since this codebase does not itself verify producer identity anywhere
in its check-run reads. Verified out-of-band before opting in: `.github/workflows/lint.yml`
is this repository's only workflow producing a check named `lint`, and
GitHub Actions (`app_id: 15368`) is the sole possible producer for a
same-repository workflow run — no external integration can post a
same-named check under that `app_id`. Revalidate this flag (confirm no
new workflow or external integration could produce a same-named `lint`
check) if `lint.yml` is ever renamed, split, or if a second CI producer
is introduced.

## Issue-Author Approval Gate

**Selection**: `enabled-by-default`

## Maintainer Approval Actor Policy

**Policy**: `owners-and-maintainers-only`

## Issue-Authoring Companion

**Status**: `installed`

**Destination**: `.agents/skills/issue-authoring/` (single root only; not
duplicated into `.claude/skills/`, `.opencode/skills/`, or any other root).

Issue #18 (core bootstrap) recorded a temporary `not installed` override
while deferring the actual file install to a follow-up issue. Issue #28
fetched the companion files (`SKILL.md` and the three `references/*.md`
files) from the pinned upstream commit
`adad8ae43c5a1b6fc3a100ce384c8a84a8d5139d`, installed them byte-identical
at the destination above, and flipped this status to `installed`.

**Authoring journal** (recorded 2026-09-16): `issueAuthoring.journalIssue:
"kurone-kito/kurone-kito#38"`. Issue #38 is a comment-only, non-IDD-work-item
tracking issue (carries `status:blocked-by-human` so Discover never
selects it) that a standalone authoring set with no existing
roadmap/anchor uses to record its pre-create publication-intent record,
per the companion contract's Stage 1 set protocol. Set directly by the
operator's own authenticated session (a one-off config change, not an
autonomous IDD claim) so the companion's full ownership-marker protocol
could be used for standalone issue drafting rather than falling back to
an ad hoc simplified path.

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

## Worktree Guard

**Status**: `enabled` (`worktreeGuard.enabled: true`,
`worktreeGuard.refuseBaseBranchCommits: false` in
`.github/idd/config.json`). The opt-in `.githooks/` hook set refuses a
commit or push made from the **primary** worktree while `HEAD` is on an
implementation branch (`issue/*` or `roadmap-audit/*`), enforcing the B1
disposable-worktree rule locally. `refuseBaseBranchCommits` stays `false`
by intentional choice, as decided in #19: the repository's base branch
(`master`) remains committable directly from the primary worktree;
enabling the stricter mode is left to a separate, not-yet-filed future
policy decision.

`core.hooksPath` is local, per-clone git configuration and is **not**
committed — each fresh clone or ephemeral agent checkout must run

```sh
git config core.hooksPath .githooks
```

once to wire the guard. See
[docs/onboarding/optional-host-setup.md](onboarding/optional-host-setup.md#optional--enable-the-local-worktree-guard)
for the full per-clone activation steps, Windows caveats, and how to
coexist with an existing hook manager.
