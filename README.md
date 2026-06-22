# Agentic Development Control Plane Engineering

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Agentic Development Control Plane Engineering is a lightweight operating model for shipping software with AI coding agents without letting work disappear into chat history.

It treats AI development as a control-plane problem:

- agents generate, edit, test, and reason;
- the control plane constrains scope, context, state transitions, handoffs, and verification.

The goal is not to make agents "more careful" by asking nicely. The goal is to design the external harness, loops, context surfaces, and gates that make careful work the default path.

## Why This Exists

Modern coding agents can work from issues, edit code, open pull requests, and review changes. Teams still hit the same failure modes:

- long-running worker sessions lose context;
- "done" means different things to different agents;
- implementation summaries outrun repository truth;
- handoffs are prose recaps instead of restartable instructions;
- multiple workers edit shared files without ownership;
- a closed issue or merged pull request is mistaken for verified product state.

This project defines a practical engineering discipline for those failure modes.

## The Four Engineering Pillars

| Pillar | Purpose | Examples |
|---|---|---|
| Harness Engineering | Put guardrails around agent behavior | Skills, custom instructions, PR templates, file locks, check scripts |
| Loop Engineering | Make work repeatable from intake to closure | Issue -> branch -> draft PR -> review -> closure |
| Context Engineering | Keep work restartable across sessions | Handoffs, next-session bootstrap blocks, bounded reading order |
| Gate Engineering | Control state upgrades with evidence | MODULE_READY, DEV_READY, REVIEW, READY_CANDIDATE, MERGED, STATE_MISMATCH |

## Core Operating Model

The reference model assumes these roles:

| Role | Responsibility |
|---|---|
| Business owner | Scope, priority, final authorization |
| PM / gatekeeper agent | Scope control, review, verification, state upgrade decisions |
| Coding leader agent | Technical task split, worker coordination, file ownership, development-side integration |
| Module worker agent | Single issue, module, or file-boundary implementation and self-test |
| Review agent | Optional independent review for high-risk changes |

Important boundary:

> A coding leader can say "development-side ready." A PM / gatekeeper can say "review passed." Only the owner or delegated release authority can authorize merge, release, or production action.

## Repository Map

- [Concept](docs/00-concept.md)
- [Harness Engineering](docs/01-harness-engineering.md)
- [Loop Engineering](docs/02-loop-engineering.md)
- [Context Engineering](docs/03-context-engineering.md)
- [Gate Engineering](docs/04-gate-engineering.md)
- [Operating Model](docs/05-operating-model.md)
- [Prior Art](docs/06-prior-art.md)
- [Node Closure Pack template](templates/node-closure-pack.md)
- [Next Session Bootstrap template](templates/next-session-bootstrap.md)
- [Worker Handoff template](templates/worker-handoff.md)
- [PR template](templates/pr-template.md)

## Quick Start

1. Pick one repository and one small development wave.
2. Define the roles: owner, PM / gatekeeper, coding leader, module workers.
3. Add a project-specific agent instruction or skill that points to the operating model.
4. Use issue and PR templates that require scope, non-scope, verification, risk, and closure fields.
5. Require every worker handoff to include a Next Session Bootstrap block.
6. Let module workers produce MODULE_READY.
7. Let the coding leader produce DEV_READY.
8. Let the PM / gatekeeper produce REVIEW or READY_CANDIDATE.
9. Treat mismatch between issue, PR, docs, and repository truth as STATE_MISMATCH.

Start with one coding leader and two module workers. Increase concurrency only after the handoff and review loop works.

## Design Principles

- Externalize facts. Important state belongs in issues, PRs, docs, handoff files, and checks.
- Separate development readiness from review readiness.
- Keep skills and instructions project-scoped when governance should not leak into other projects.
- Prefer structured completion cards over long chat recaps.
- Make the next session easy to start before ending the current one.
- Review state transitions, not just code diffs.
- Convert repeated failures into templates or checks.

## What This Is Not

- It is not a replacement for GitHub, pull requests, tests, or human ownership.
- It is not a full multi-agent platform.
- It is not a claim that one process fits every project.
- It is not prompt engineering alone. It is harness, loop, context, and gate engineering.

## Prior Art

This work is inspired by existing agentic software engineering systems and research, including GitHub Copilot coding agents and custom instructions, Claude Code skills and subagents, SWE-agent, SWE-bench, OpenHands, and AutoGen. See [Prior Art](docs/06-prior-art.md).

The contribution here is the operating model that connects those ideas into a lightweight, file-first workflow for teams running multiple coding agents.

## Update Log

- 2026-06-22: Added multilingual README entry points for English, Chinese, Japanese, and Korean.
- 2026-06-22: Initial public release with the four-pillar method, operating model, prior-art map, and handoff templates.

## License

Apache-2.0. See [LICENSE](LICENSE).
