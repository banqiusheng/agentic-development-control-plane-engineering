# Loop Engineering

Loop Engineering makes agentic work repeatable.

A loop is a bounded path from task intake to closure. It should be simple enough that each agent knows where it is and what it must produce before leaving the loop.

## Reference Development Loop

```text
Issue READY
  -> Branch created
  -> Draft PR opened
  -> Module worker edits and self-tests
  -> MODULE_READY
  -> Coding leader integrates and checks
  -> DEV_READY
  -> PM / gatekeeper review
  -> READY_CANDIDATE
  -> Owner or delegated authority approves merge
  -> MERGED
  -> Node Closure Pack archived
```

## Two Loops, One Project

Most AI development programs need two loops:

### Development Loop

The development loop belongs to the coding leader and module workers.

Its output is DEV_READY.

### Gate Loop

The gate loop belongs to the PM / gatekeeper and owner.

Its output is READY_CANDIDATE, MERGED, BLOCKED, or STATE_MISMATCH.

Keep these separate. A worker can finish code without the project being ready.

## Wave-Based Parallelism

Prefer wave-based parallelism:

1. Define shared boundaries serially.
2. Run two or three low-conflict workers in parallel.
3. Integrate serially.
4. Review serially.
5. Expand concurrency only after the loop works.

Avoid starting many workers before the project has file ownership rules, closure templates, and state labels.

## Loop Smells

- Workers say "done" without a completion card.
- Draft PRs become hidden final PRs.
- Review starts before verification commands are listed.
- Multiple workers edit global config or schema files.
- The next session cannot restart from artifacts.
- The issue closes but the done contract was not met.

Each smell should become a template field, check, or gate.
