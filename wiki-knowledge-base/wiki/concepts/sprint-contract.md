---
type: concept
tags: [harness, 验收协议, 质量控制, 评估, 完成标准]
first_seen: "2026-03"
sources:
  - sources/2026-03-agent-long-task
  - sources/2026-03-harness-three-layer
  - sources/2026-04-harness-from-add-to-delete
---

# Sprint Contract

[[../entities/anthropic]] 提出的验收协议机制，把"完成标准"从口头对齐变成可验证的协议文件，使 [[../concepts/planner-generator-evaluator]] 架构中的验收标准可操作化。

## 定义

Sprint Contract 是一个在 Sprint 开始前由 Planner 和 Evaluator 共同确认的文件，明确规定：
- 本次 Sprint 要交付什么（交付物范围）
- 每个维度的验收标准（通常 27 条左右）
- 各维度的通过阈值

**任何维度低于阈值即判定失败，退回重做。**

来源：[[../sources/2026-03-agent-long-task]]

## 设计目的

解决**自我评估偏差**：模型倾向于认为自己的产出"还不错"，尤其在 UI 设计等主观任务上。Sprint Contract 将主观质量压成可打分维度，使验收标准客观可验证。

## 与 Rubric（评分准绳）的关系

- **Sprint Contract**：验收协议，定义"做什么"和"做到什么程度"。
- **Rubric**：评分准绳，提供具体的打分维度和权重（如设计质量/原创性/工艺水平/功能性）。

两者配合，把"看起来完成"和"真正完成"区分开来。

## 随模型演进的变化

- Opus 4.5：Sprint Contract 几乎每轮都必要。
- Opus 4.6：Sprint 结构可以去掉（Generator 连续跑 2 小时+），但 Evaluator 仍然必要。

**工程启示**：Sprint Contract 是对当前模型能力缺口的补偿，应随模型变强而重新评估是否还在"承重"。
（来源：[[../sources/2026-04-harness-from-add-to-delete]]）

## 可迁移的思路

即使是单 Agent，也可以在 prompt 中定义"完成标准清单"，模拟 Sprint Contract 的效果：
- 开新会话让模型对照 Rubric 评估产出质量（context reset 后独立评估）。
- 完成长任务后，在新会话中让模型对照明确标准进行审查。
