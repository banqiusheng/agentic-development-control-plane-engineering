# Agentic Development Control Plane Engineering

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Agentic Development Control Plane Engineering 是一套轻量级 AI 开发控制面工程方法，用来让 AI coding agents 参与软件开发时，不把关键工作丢进聊天历史里。

它把 AI 开发看作一个控制面问题：

- agent 负责生成、编辑、测试和推理；
- 控制面负责约束范围、上下文、状态流转、交接和验证。

目标不是反复提醒 agent “更小心一点”，而是设计外部 harness、loop、context surface 和 gate，让严谨工作成为默认路径。

## 为什么需要它

现代 coding agent 已经可以从 issue 开始工作、改代码、开 pull request、接受 review。但团队仍然会遇到同一类问题：

- 长时间运行的 worker 会话丢失上下文；
- 不同 agent 对 “done” 的理解不一致；
- 实现摘要跑在仓库事实前面；
- handoff 只是叙事总结，而不是可重启指令；
- 多个 worker 同时改共享文件，却没有 ownership；
- 已关闭 issue 或已合并 PR 被误认为已经通过验证。

本项目为这些问题定义一套可执行的工程纪律。

## 四个工程支柱

| 支柱 | 解决什么 | 示例 |
|---|---|---|
| Harness Engineering | 给 agent 行为加外骨骼 | Skills、自定义指令、PR 模板、file lock、检查脚本 |
| Loop Engineering | 让工作从入口到闭环可重复 | Issue -> branch -> draft PR -> review -> closure |
| Context Engineering | 让工作可以跨会话重启 | Handoff、Next Session Bootstrap、有界阅读顺序 |
| Gate Engineering | 用证据控制状态升级 | MODULE_READY、DEV_READY、REVIEW、READY_CANDIDATE、MERGED、STATE_MISMATCH |

## 核心运行模型

参考模型包含这些角色：

| 角色 | 职责 |
|---|---|
| Business owner | 范围、优先级、最终授权 |
| PM / gatekeeper agent | 范围控制、review、验证、状态升级判断 |
| Coding leader agent | 技术拆解、worker 协调、文件 owner、开发侧集成 |
| Module worker agent | 单个 issue、模块或文件边界内的实现和自测 |
| Review agent | 高风险变更的可选独立 review |

关键边界：

> Coding leader 可以说“开发侧 ready”。PM / gatekeeper 可以说“review passed”。只有 owner 或被授权的发布负责人才能授权 merge、release 或 production 动作。

## 仓库地图

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

## 快速开始

1. 选择一个仓库和一个小开发 wave。
2. 定义角色：owner、PM / gatekeeper、coding leader、module workers。
3. 添加项目级 agent instruction 或 skill，并指向本运行模型。
4. 使用 issue 和 PR 模板，强制填写范围、非范围、验证、风险和 closure 字段。
5. 要求每个 worker handoff 都包含 Next Session Bootstrap block。
6. 让 module worker 只产出 MODULE_READY。
7. 让 coding leader 只产出 DEV_READY。
8. 让 PM / gatekeeper 产出 REVIEW 或 READY_CANDIDATE。
9. 把 issue、PR、docs 和仓库事实不一致的情况标为 STATE_MISMATCH。

建议从 1 个 coding leader + 2 个 module workers 开始。只有当 handoff 和 review loop 跑顺以后，再扩大并发。

## 设计原则

- 事实外部化：重要状态放进 issue、PR、docs、handoff 文件和检查脚本。
- 区分开发侧 ready 和 review ready。
- 当治理规则不应影响其他项目时，skill 和指令必须项目级隔离。
- 用结构化完成卡替代长聊天复盘。
- 结束当前会话前，先让下一会话容易启动。
- review 状态流转，不只 review code diff。
- 把重复失败沉淀成模板或检查。

## 它不是什么

- 它不是 GitHub、pull request、测试或人类 ownership 的替代品。
- 它不是完整多 agent 平台。
- 它不声称一套流程适合所有项目。
- 它不只是 prompt engineering，而是 harness、loop、context 和 gate engineering。

## 前线参考

本项目受到 GitHub Copilot coding agents、自定义指令、Claude Code skills 和 subagents、SWE-agent、SWE-bench、OpenHands、AutoGen 等 agentic software engineering 系统和研究启发。详见 [Prior Art](docs/06-prior-art.md)。

本仓库的贡献是把这些思路连接成一套轻量、file-first、适合多 agent 开发的运行模型。

## 更新日志

- 2026-06-22：新增 English、中文、日本語、한국어 多语言 README 入口。
- 2026-06-22：首次公开发布四支柱方法、运行模型、前线参考和 handoff 模板。

## License

Apache-2.0。见 [LICENSE](LICENSE)。
