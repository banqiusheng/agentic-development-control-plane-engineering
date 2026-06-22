# Harness Engineering

Harness Engineering builds the guardrails that make agent behavior predictable.

An agent harness is the set of instructions, templates, permissions, checks, and ownership rules that shape what an agent does before it starts editing.

## Harness Components

| Component | Purpose |
|---|---|
| Project-scoped skill or custom instruction | Load the right workflow without affecting unrelated projects |
| AGENTS / CLAUDE / repository instructions | Define local rules that every agent can find |
| Issue template | Make work intake structured |
| PR template | Force scope, non-scope, evidence, and closure fields |
| File ownership table | Prevent workers from editing the same files |
| Check scripts | Turn repeated mistakes into deterministic checks |
| Handoff templates | Prevent prose summaries from becoming the only state |

## Project-Scoped, Not Global

Do not make every governance rule global.

If a method applies only to one repository or program, scope the skill or custom instruction to that project. Otherwise, the harness will interfere with unrelated work.

Good trigger:

```text
Use this when working in the commercial-real-estate-crm repository on M0/E0 development management, review, handoff, or agent coordination.
```

Risky trigger:

```text
Use this for all Claude Code development.
```

## Harness Before Parallelism

Parallel workers should not start until the harness answers:

- What issue or module does this worker own?
- Which files or directories can it edit?
- Which files are forbidden?
- What does MODULE_READY mean?
- What is the exact completion card?
- What must be handed to the next session?

If those answers are not durable, add harness before adding workers.

## Minimum Harness Checklist

```text
Project:
Issue:
Role: PM / gatekeeper | coding leader | module worker | review agent
Allowed files:
Forbidden files:
State labels available:
Required verification:
Required completion artifact:
Next-session bootstrap required: yes / no
```
