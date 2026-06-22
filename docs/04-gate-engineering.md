# Gate Engineering

Gate Engineering defines what state labels mean and who can advance them.

Without explicit gates, every agent invents its own meaning for "done."

## Reference State Labels

| State | Meaning | Owner |
|---|---|---|
| READY | Work can be picked up | PM / gatekeeper |
| IN_PROGRESS | Work is being edited | Coding leader or module worker |
| DRAFT_PR | PR exists, not ready | Coding leader or module worker |
| MODULE_READY | One module worker completed self-test and completion card | Module worker |
| DEV_READY | Development side believes the work is ready for review | Coding leader |
| REVIEW | PM / gatekeeper review is in progress or required | PM / gatekeeper |
| READY_CANDIDATE | Review passed, awaiting authorized merge or release action | PM / gatekeeper |
| MERGED | Pull request actually merged | GitHub truth |
| BLOCKED | Cannot proceed without missing input, permission, environment, or evidence | Any role can raise; PM verifies |
| STATE_MISMATCH | Issue, PR, docs, or repository truth conflict | PM / gatekeeper |

## State Upgrade Rule

State upgrades require evidence.

Examples:

- MODULE_READY requires a completion card and self-test output.
- DEV_READY requires coding leader integration review.
- READY_CANDIDATE requires PM / gatekeeper review.
- MERGED requires actual repository truth.
- VERIFIED requires the required evidence for that project stage.

## Common Mismatches

| Symptom | Gate response |
|---|---|
| Docs-only PR closes implementation issue | STATE_MISMATCH |
| Worker says ready but no tests are listed | BLOCKED or NEEDS_CHANGES |
| PR claims no scope impact but docs changed stage behavior | NEEDS_CHANGES |
| Issue closed without closure pack | STATE_MISMATCH |
| Local run succeeds but cloud readiness is claimed | NEEDS_CHANGES |

## Gatekeeper Discipline

The gatekeeper should review:

- scope;
- done contract;
- verification;
- secret or PII risk;
- unresolved risks;
- state labels;
- handoff quality;
- required documentation updates.

The gatekeeper should not silently become the coding leader unless explicitly authorized.
