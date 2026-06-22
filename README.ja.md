# Agentic Development Control Plane Engineering

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Agentic Development Control Plane Engineering は、AI coding agents を使ってソフトウェアを開発するときに、重要な作業状態をチャット履歴の中に埋もれさせないための軽量な運用モデルです。

この方法論では、AI 開発を control plane の問題として扱います。

- agent は生成、編集、テスト、推論を行う；
- control plane はスコープ、コンテキスト、状態遷移、引き継ぎ、検証を制御する。

目的は agent に「もっと注意して」と頼むことではありません。外部の harness、loop、context surface、gate を設計し、慎重な作業が自然なデフォルトになるようにすることです。

## なぜ必要か

現代の coding agent は issue から作業を開始し、コードを編集し、pull request を作成し、review を受けることができます。それでもチームは同じ失敗パターンに直面します。

- 長時間の worker session がコンテキストを失う；
- agent ごとに “done” の意味が違う；
- 実装サマリーがリポジトリ上の事実を追い越す；
- handoff が再開可能な指示ではなく、ただの文章要約になる；
- 複数 worker が ownership なしで共有ファイルを編集する；
- closed issue や merged pull request が verified product state と誤解される。

このプロジェクトは、これらの失敗に対する実践的な engineering discipline を定義します。

## 4 つの Engineering Pillars

| Pillar | 目的 | 例 |
|---|---|---|
| Harness Engineering | agent の行動にガードレールを付ける | Skills、custom instructions、PR templates、file locks、check scripts |
| Loop Engineering | intake から closure まで作業を再現可能にする | Issue -> branch -> draft PR -> review -> closure |
| Context Engineering | セッションをまたいで作業を再開可能にする | Handoffs、next-session bootstrap blocks、bounded reading order |
| Gate Engineering | 証拠に基づいて状態をアップグレードする | MODULE_READY、DEV_READY、REVIEW、READY_CANDIDATE、MERGED、STATE_MISMATCH |

## Core Operating Model

参照モデルは以下の役割を想定します。

| Role | Responsibility |
|---|---|
| Business owner | スコープ、優先順位、最終承認 |
| PM / gatekeeper agent | スコープ管理、review、検証、状態アップグレード判断 |
| Coding leader agent | 技術タスク分解、worker 調整、file ownership、開発側 integration |
| Module worker agent | 単一 issue、module、または file-boundary の実装と self-test |
| Review agent | 高リスク変更に対する任意の独立 review |

重要な境界：

> Coding leader は “development-side ready” と言える。PM / gatekeeper は “review passed” と言える。merge、release、production action を承認できるのは owner または委任された release authority だけである。

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

1. 1 つの repository と小さな development wave を選ぶ。
2. owner、PM / gatekeeper、coding leader、module workers を定義する。
3. operating model を参照する project-specific agent instruction または skill を追加する。
4. scope、non-scope、verification、risk、closure を必須にする issue / PR template を使う。
5. すべての worker handoff に Next Session Bootstrap block を含める。
6. module workers は MODULE_READY を出す。
7. coding leader は DEV_READY を出す。
8. PM / gatekeeper は REVIEW または READY_CANDIDATE を出す。
9. issue、PR、docs、repository truth の不一致を STATE_MISMATCH として扱う。

最初は 1 coding leader + 2 module workers から始めることを推奨します。handoff と review loop が安定してから並列度を上げてください。

## Design Principles

- 事実を外部化する。重要な状態は issue、PR、docs、handoff files、checks に置く。
- development readiness と review readiness を分ける。
- governance が他プロジェクトに漏れてはいけない場合、skills と instructions は project-scoped にする。
- 長い chat recap より structured completion cards を使う。
- 現在の session を終える前に、次の session が起動しやすい状態を作る。
- code diff だけでなく state transition を review する。
- 繰り返し起きる失敗を template または check に変える。

## What This Is Not

- GitHub、pull requests、tests、人間の ownership の代替ではない。
- 完全な multi-agent platform ではない。
- すべての project に同じ process が合うという主張ではない。
- prompt engineering だけではない。harness、loop、context、gate engineering である。

## Prior Art

この作業は GitHub Copilot coding agents と custom instructions、Claude Code skills と subagents、SWE-agent、SWE-bench、OpenHands、AutoGen などの agentic software engineering system と research から着想を得ています。詳しくは [Prior Art](docs/06-prior-art.md) を参照してください。

この repository の貢献は、それらの考え方を lightweight で file-first な multi-agent workflow に接続する operating model です。

## Update Log

- 2026-06-22: English、中文、日本語、한국어 の多言語 README entry point を追加。
- 2026-06-22: four-pillar method、operating model、prior-art map、handoff templates を初回公開。

## License

Apache-2.0。See [LICENSE](LICENSE).
