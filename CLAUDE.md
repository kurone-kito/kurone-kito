# Guidelines for AI Agents

## Immediate rules

- Match the conversational language to the user's language.
- Write comments and documentation in English unless there is a clear
  project-specific reason otherwise.
- If uncertainty, hidden risk, or missing context blocks a safe change,
  stop and ask a concise question before proceeding.

## IDD Workflow

This project uses Issue-Driven Development (IDD) with parallel AI
agents. Start with [docs/idd-workflow.md](docs/idd-workflow.md) for the
cross-agent entry path and phase routing.

Before starting IDD work, open
`.github/instructions/idd-overview-core.instructions.md`. Open the routed
phase file manually when the current step changes.

Doing ad-hoc engineering outside a formal IDD claim (a direct fix, a PR,
a review reply)? The "Wake-up discipline" section of
`.github/instructions/idd-ci.instructions.md` (no self-polling while
waiting on CI or bot review) still applies — open it whenever a commit
you pushed is waiting on either.

See [docs/idd-policy.md](docs/idd-policy.md) for this repository's
recorded IDD policy decisions (merge policy, review policy, credential
scope, and the rest).
