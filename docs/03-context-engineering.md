# Context Engineering

Context Engineering keeps agent work bounded, restartable, and fact-based.

The key idea:

> Long chat history is not a reliable state store.

Important context must be externalized into durable surfaces: issues, PRs, docs, logs, commits, test output, handoff files, and closure packs.

## Context Surfaces

| Surface | Good for | Bad for |
|---|---|---|
| Issue | Task scope, owner, blockers, state comments | Long implementation logs |
| PR | Diff, review, verification, closure evidence | Project-wide memory |
| Docs | Stable policy, architecture, operating rules | Raw session history |
| Handoff file | Restarting a paused or overloaded session | Permanent product truth by itself |
| Chat | Short coordination | Authoritative state |

## Next Session Bootstrap

Every handoff should include a block the next session can paste directly.

The first line should trigger the right project-scoped skill or instruction. This avoids hoping the next agent will infer the right behavior.

Example:

```text
Use <project-skill-name>.

Role: Module Worker
Repository: /absolute/path/to/repo
Issue: #123
Branch: feature/example
PR: #456
Handoff: /absolute/path/to/handoff.md

Before editing, report:
- role confirmed
- current branch and commit
- allowed files
- forbidden files
- last completed step
- next smallest step
```

## Handoff Validity

A handoff is invalid if it lacks:

- role;
- repository path;
- issue or task id;
- branch and current commit;
- files changed;
- files still locked;
- verification already run;
- blockers;
- next-session bootstrap.

If the handoff is invalid, the next session should enter PROCESS_BLOCKED rather than continue editing.

## Context Budget Rule

The higher the concurrency, the less context each participant should carry.

Module workers should carry the least context:

- issue scope;
- allowed files;
- local implementation details;
- verification commands.

The coding leader should carry integration context.

The PM / gatekeeper should carry scope, evidence, state, and risk context.
