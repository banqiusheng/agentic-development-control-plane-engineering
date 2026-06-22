# Concept

Agentic Development Control Plane Engineering is the practice of designing the external control surfaces that make AI coding work reliable.

The core claim:

> AI development quality depends as much on harnesses, loops, context surfaces, and gates as it does on model capability.

## The Control Plane View

Traditional prompt engineering focuses on what to ask an agent.

Control plane engineering focuses on what must surround the agent:

- where facts live;
- what the agent is allowed to read or edit;
- how work enters and exits a session;
- what state labels mean;
- who can upgrade state;
- how evidence is preserved;
- how the next session restarts without guessing.

## The Failure Pattern

Most agentic development failures are not pure reasoning failures. They are operating failures:

- the task boundary was not durable;
- evidence lived only in chat;
- ownership of files was ambiguous;
- one agent used "done" to mean "I stopped editing";
- another agent used "done" to mean "verified and safe to merge";
- handoff text could not restart work;
- the next agent had to reconstruct intent from incomplete context.

## The Operating Answer

Use four engineering pillars:

1. Harness Engineering: build guardrails around agent behavior.
2. Loop Engineering: make work repeatable from intake to closure.
3. Context Engineering: make sessions restartable and bounded.
4. Gate Engineering: make state upgrades evidence-based.

These pillars are intentionally small. They can be adopted in an existing GitHub repository without introducing a new platform.

## Minimal Adoption

A project can start with:

- one issue template;
- one PR template;
- one project-scoped skill or custom instruction;
- one Node Closure Pack template;
- one rule: coding agents cannot mark work VERIFIED.

From there, convert every repeated failure into a template or check.
