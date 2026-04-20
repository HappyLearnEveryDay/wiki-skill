---
type: entity
category: 组织
tags: [ai-company, claude, harness, 研究]
first_seen: "2026-03"
sources:
  - sources/2026-03-harness-engineering
  - sources/2026-03-agent-long-task
  - sources/2026-04-harness-from-add-to-delete
  - sources/2026-03-claude-code-source-analysis
---

# Anthropic

AI 安全公司，Claude 系列模型和 [[claude-code]] 的开发商。在 [[../concepts/harness-engineering]] 领域有重要工程实践。

## Harness 工程实践

### GAN 式三 Agent 架构
[[../concepts/planner-generator-evaluator]]：Planner 扩展规格，Generator 逐功能实现，Evaluator 用 Playwright 真机操作验收。
- 根本洞察：让独立 evaluator 变严格，远比让 generator 学会自我批评容易。
- 来源：[[../sources/2026-03-harness-engineering]]

### 效果数据
- solo 模式 20 分钟/$9 → 完整 Harness 6 小时/$200，核心功能可用性差距明显。
- Opus 4.5 → Opus 4.6：很多任务已在生成器独立能力范围内，Sprint 结构可精简，但 evaluator 仍不可去掉。
- 来源：[[../sources/2026-03-agent-long-task]]

### 两阶段 Harness 演进
1. 第一阶段（V1）：补短板——planner/evaluator、context reset、sprint contract。
2. 第二阶段（V2）：删 dead weight——编排权、上下文管理权、记忆选择权还给模型。
- 来源：[[../sources/2026-04-harness-from-add-to-delete]]

### 分层控制哲学
从 CLAUDE.md → Skills → Hooks → Harness → Auto Mode，每一层都在把不同类型的判断从"模型自觉"外移到系统机制。

## Claude 模型系列

| 模型 | 定位 |
|------|------|
| Claude Opus 4.7 | 最强推理，适合复杂规划 |
| Claude Sonnet 4.6 | 平衡性能与成本 |
| Claude Haiku 4.5 | 快速低成本，适合简单任务 |
