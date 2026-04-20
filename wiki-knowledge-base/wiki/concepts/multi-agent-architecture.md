---
type: concept
tags: [multi-agent, orchestrator, worker, 并行, 协作]
first_seen: "2026-02"
sources:
  - sources/2026-02-openclaw-multi-agent
  - sources/2026-03-claude-code-source-analysis
  - sources/2026-03-subagent-custom
  - sources/2026-03-harness-engineering
---

# Multi-Agent 架构

多个 AI Agent 协同工作，通过任务分工、上下文隔离和并行执行提升复杂任务的完成质量与效率。

## 核心模式

### Orchestrator-Worker 模式
- **Orchestrator（编排器）**：接收需求、分析任务、派发子任务、综合结果。
- **Worker（执行器）**：接收自包含的指令，独立执行具体任务，汇报结果。

在 [[../entities/claude-code]] 中称为 Coordinator 模式，关键原则：
> "Always synthesize — your most important job. Never write 'based on your findings'."
> 
> Coordinator 必须理解 Worker 的研究结果，而不是直接转发。
（来源：[[../sources/2026-03-claude-code-source-analysis]]）

### [[planner-generator-evaluator]] 模式
三角色分离，专为解决代码生成中的质量评估问题。

## [[subagent]] vs MultiAgent

| | Subagent | MultiAgent |
|---|---|---|
| 生命周期 | 临时（任务完成后关闭） | 常驻（有独立工作区） |
| 隔离 | 上下文隔离 | 完全独立（工作区、认证、记忆） |
| 配置 | 自然语言派发即可 | 需配置 agents.list + bindings |

## 并行的价值

- 独立的子任务可并行执行，大幅缩短总时长。
- 研究类任务可同时启动多个专家 Subagent 并行调研。
- 来源：[[../sources/2026-03-subagent-custom]]

## 在 OpenClaw 中的实现

真正 MultiAgent 需要：
1. 设置多 agent：修改 `agents.list`
2. 设置多聊天通道：修改 `channels.accounts`
3. 绑定 channel 和 agent：修改 `bindings`

注意：企微不支持多 agent bindings 路由，飞书可以。
来源：[[../sources/2026-02-openclaw-multi-agent]]

## 实践建议

- 没有最优方案，按需选择或组合：单 Agent 简单任务、Subagent 隔离探索、MultiAgent 并行角色。
- Sub-agent 核心价值是**上下文隔离**，"前端工程师 + 后端工程师"这种角色化切法实际不起作用。
- 来源：[[../sources/2026-04-coding-agent-harness-practice]]
