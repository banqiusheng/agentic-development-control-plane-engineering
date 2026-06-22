# Agentic Development Control Plane Engineering

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Agentic Development Control Plane Engineering은 AI coding agents로 소프트웨어를 개발할 때 중요한 작업 상태가 채팅 기록 속에 사라지지 않도록 돕는 가벼운 운영 모델입니다.

이 방법론은 AI 개발을 control plane 문제로 봅니다.

- agent는 생성, 편집, 테스트, 추론을 수행한다;
- control plane은 범위, 컨텍스트, 상태 전이, handoff, 검증을 제어한다.

목표는 agent에게 “더 조심해 달라”고 부탁하는 것이 아닙니다. 외부 harness, loop, context surface, gate를 설계해 신중한 작업이 기본 경로가 되도록 만드는 것입니다.

## 왜 필요한가

현대의 coding agent는 issue에서 작업을 시작하고, 코드를 수정하고, pull request를 만들고, review를 받을 수 있습니다. 그래도 팀은 반복적으로 같은 실패 패턴을 만납니다.

- 장시간 실행되는 worker session이 컨텍스트를 잃는다;
- agent마다 “done”의 의미가 다르다;
- 구현 요약이 repository truth보다 앞서간다;
- handoff가 재시작 가능한 지시가 아니라 서술형 요약이 된다;
- 여러 worker가 ownership 없이 공유 파일을 수정한다;
- closed issue나 merged pull request가 verified product state로 오해된다.

이 프로젝트는 이런 실패 패턴을 다루기 위한 실용적인 engineering discipline을 정의합니다.

## 네 가지 Engineering Pillars

| Pillar | 목적 | 예시 |
|---|---|---|
| Harness Engineering | agent 행동에 guardrail을 둔다 | Skills, custom instructions, PR templates, file locks, check scripts |
| Loop Engineering | intake부터 closure까지 반복 가능한 흐름을 만든다 | Issue -> branch -> draft PR -> review -> closure |
| Context Engineering | session을 넘어 작업을 재시작 가능하게 만든다 | Handoffs, next-session bootstrap blocks, bounded reading order |
| Gate Engineering | 증거 기반으로 상태를 업그레이드한다 | MODULE_READY, DEV_READY, REVIEW, READY_CANDIDATE, MERGED, STATE_MISMATCH |

## Core Operating Model

참조 모델은 다음 역할을 가정합니다.

| Role | Responsibility |
|---|---|
| Business owner | 범위, 우선순위, 최종 승인 |
| PM / gatekeeper agent | 범위 제어, review, 검증, 상태 업그레이드 판단 |
| Coding leader agent | 기술 작업 분해, worker 조율, file ownership, 개발 측 integration |
| Module worker agent | 단일 issue, module, file-boundary 구현 및 self-test |
| Review agent | 고위험 변경에 대한 선택적 독립 review |

중요한 경계:

> Coding leader는 “development-side ready”라고 말할 수 있다. PM / gatekeeper는 “review passed”라고 말할 수 있다. merge, release, production action은 owner 또는 위임받은 release authority만 승인할 수 있다.

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

1. 하나의 repository와 작은 development wave를 선택한다.
2. owner, PM / gatekeeper, coding leader, module workers를 정의한다.
3. operating model을 가리키는 project-specific agent instruction 또는 skill을 추가한다.
4. scope, non-scope, verification, risk, closure 필드를 요구하는 issue / PR template을 사용한다.
5. 모든 worker handoff에 Next Session Bootstrap block을 포함한다.
6. module worker는 MODULE_READY를 생성한다.
7. coding leader는 DEV_READY를 생성한다.
8. PM / gatekeeper는 REVIEW 또는 READY_CANDIDATE를 생성한다.
9. issue, PR, docs, repository truth가 맞지 않으면 STATE_MISMATCH로 처리한다.

처음에는 1 coding leader + 2 module workers로 시작하는 것을 권장합니다. handoff와 review loop가 안정된 뒤에만 동시성을 늘리세요.

## Design Principles

- 사실을 외부화한다. 중요한 상태는 issue, PR, docs, handoff files, checks에 둔다.
- development readiness와 review readiness를 분리한다.
- governance가 다른 프로젝트에 새어 나가면 안 되는 경우 skills와 instructions는 project-scoped로 둔다.
- 긴 chat recap보다 structured completion cards를 사용한다.
- 현재 session을 끝내기 전에 다음 session이 쉽게 시작할 수 있게 만든다.
- code diff뿐 아니라 state transition을 review한다.
- 반복되는 실패를 template 또는 check로 바꾼다.

## What This Is Not

- GitHub, pull requests, tests, human ownership의 대체물이 아니다.
- 완전한 multi-agent platform이 아니다.
- 하나의 process가 모든 project에 맞는다는 주장이 아니다.
- prompt engineering만이 아니다. harness, loop, context, gate engineering이다.

## Prior Art

이 작업은 GitHub Copilot coding agents와 custom instructions, Claude Code skills와 subagents, SWE-agent, SWE-bench, OpenHands, AutoGen 등 agentic software engineering systems와 research에서 영감을 받았습니다. 자세한 내용은 [Prior Art](docs/06-prior-art.md)를 참고하세요.

이 repository의 기여는 그런 아이디어들을 lightweight, file-first multi-agent workflow에 연결하는 operating model입니다.

## Update Log

- 2026-06-22: English, 中文, 日本語, 한국어 다국어 README entry point 추가.
- 2026-06-22: four-pillar method, operating model, prior-art map, handoff templates 최초 공개.

## License

Apache-2.0. See [LICENSE](LICENSE).
