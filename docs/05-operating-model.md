# Operating Model

This document describes a practical role split for multi-agent development.

## Roles

### Business Owner

Owns:

- product direction;
- scope tradeoffs;
- final authorization for merge, release, production, destructive operations, and sensitive access.

### PM / Gatekeeper Agent

Owns:

- task boundaries;
- review criteria;
- state labels;
- evidence verification;
- mismatch detection;
- escalation to the owner.

Does not own:

- technical implementation details;
- worker coordination;
- hidden merge or release action.

### Coding Leader Agent

Owns:

- technical task split;
- worker assignment;
- file ownership;
- integration checks;
- development-side ready.

Does not own:

- product scope expansion;
- final verification;
- merge or release authority unless explicitly delegated.

### Module Worker Agent

Owns:

- one issue, module, or file-boundary task;
- implementation;
- self-test;
- completion card;
- handoff if context runs long.

Does not own:

- other workers' files;
- project state upgrade;
- final completion claims.

### Review Agent

Optional independent reviewer for high-risk changes.

Owns:

- focused review findings;
- additional verification suggestions.

Does not own:

- implementation unless explicitly reassigned;
- merge or release authority.

## Recommended Concurrency

Start with:

```text
1 coding leader
2 module workers
1 PM / gatekeeper
```

Increase to three module workers only after:

- file ownership works;
- handoffs are restartable;
- PR templates are filled correctly;
- review queues do not pile up;
- state mismatch incidents are handled quickly.

## Serial and Parallel Boundaries

Serial:

- define scope;
- define shared architecture;
- define schema and global config ownership;
- integrate;
- review;
- state upgrade.

Parallel:

- independent module implementation;
- local docs for module-specific behavior;
- isolated tests;
- low-conflict scaffolding.

## A Healthy Wave

```text
PM defines wave scope
Coding leader splits tasks
Workers claim file locks
Workers open draft PRs
Workers submit MODULE_READY
Coding leader submits DEV_READY
PM reviews and gates
Owner authorizes merge if needed
Closure packs are archived
```
