# Prior Art

This project combines patterns from existing agentic software engineering systems and research. It does not claim that the individual components are new.

The gap this project focuses on is the lightweight operating model that connects those components for file-first, multi-agent development.

## GitHub Copilot Coding Agent

GitHub Copilot's cloud agent model supports assigning coding tasks, working in branches, and producing pull requests for review. GitHub also documents custom agents and repository instructions.

Useful ideas:

- issue and PR as durable work surfaces;
- branch-based agent work;
- human review before integration;
- repository instructions and custom agents as harnesses.

References:

- https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent
- https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions
- https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-custom-agents

## Claude Code Skills and Subagents

Claude Code documents skills as reusable capability bundles and subagents as a way to delegate work with isolated context.

Useful ideas:

- skills as project or task harnesses;
- subagents with bounded role and context;
- explicit instructions for repeatable behavior.

References:

- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/agent-sdk/subagents

## SWE-agent and SWE-bench

SWE-agent and SWE-bench focus on real software engineering tasks grounded in GitHub issues, repositories, tests, and patches.

Useful ideas:

- real GitHub issues as task units;
- interface design matters for agent performance;
- validation should be tied to repository truth and tests.

References:

- https://arxiv.org/abs/2405.15793
- https://arxiv.org/abs/2310.06770
- https://github.com/swe-agent/swe-agent

## OpenHands

OpenHands is an AI-driven development platform and agent workspace.

Useful ideas:

- agent orchestration around software tasks;
- development environment as part of the agent system;
- heavier control surfaces for teams that need them.

Reference:

- https://github.com/OpenHands/openhands

## AutoGen

AutoGen explores multi-agent conversation frameworks.

Useful ideas:

- multi-agent collaboration is a real design space;
- shared state and long-running task continuity need explicit design;
- conversation alone is not enough for durable engineering state.

Reference:

- https://www.microsoft.com/en-us/research/publication/autogen-enabling-next-gen-llm-applications-via-multi-agent-conversation-framework/

## What This Project Adds

This repository does not replace those systems. It defines a small operating discipline that can be used with many of them:

- Harness Engineering
- Loop Engineering
- Context Engineering
- Gate Engineering

The intended adoption path is incremental: templates and rules first, scripts and automation after the failure patterns are visible.
