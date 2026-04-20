---
type: concept
tags: [harness, evaluator, generator, planner, 质量控制, gan]
first_seen: "2026-03"
sources:
  - sources/2026-03-agent-long-task
  - sources/2026-03-harness-engineering
  - sources/2026-03-harness-three-layer
  - sources/2026-03-harness-what-is-it
---

# Planner-Generator-Evaluator

[[../entities/anthropic]] 提出的三角色分离架构，灵感来自 GAN（生成对抗网络），专为解决 AI Agent 在长任务中的质量控制问题。

## 三角色定义

| 角色 | 职责 | 关键约束 |
|------|------|----------|
| **Planner（规划者）** | 扩展产品规格，定义"做什么"和"做到什么程度" | 不干预 Generator 的低层实现决策 |
| **Generator（生成者）** | 逐功能迭代实现 | 不自我评估 |
| **Evaluator（评估者）** | 用 Playwright 真机操作验收，挑刺找问题 | 调校使其保持怀疑态度，不接受"看起来完整"的产出 |

## 存在的根本原因

**自我评估偏差（Self-evaluation Bias）**：模型评价自己产出时偏正面，"能跑"被误判为"达标"。
- 调校一个独立的 evaluator 使其保持怀疑态度，远比让 generator 对自身作品保持批判性容易。
- 执行与判断拆到两个主体是正确的工程直觉，适用于所有长任务场景。

来源：[[../sources/2026-03-agent-long-task]]

## Evaluator 设计要点（来源：[[../sources/2026-03-harness-three-layer]]）

- Evaluator 用 Playwright **真跑页面**（点按钮、查 DOM），不是读代码打分。
- 开箱即用的 Claude 是很差的 QA Agent：需多轮迭代调教，防止 evaluator 自我说服"这不是大事"。
- Bug 报告需精确到行级，而非笼统描述。
- **评估器去不掉**：不管模型多强，自评能力提升远跟不上生成能力的提升。

## 随模型演进的变化（来源：[[../sources/2026-03-agent-long-task]]）

| 版本 | Sprint 结构 | Evaluator |
|------|------------|-----------|
| Opus 4.5 | 必须——evaluator 几乎每轮发挥作用 | 关键 |
| Opus 4.6 | 可去掉——生成器连续跑 2 小时+ | 仍然必要，抓"最后一公里"问题 |

**工程哲学**：好的 Harness 不是不断叠加，而是持续重估；随模型变强持续拆脚手架。

## 主观质量量化（来源：[[../sources/2026-03-agent-long-task]]）

把审美类主观质量拆成可打分维度（如设计质量/原创性/工艺水平/功能性），刻意调整权重。
这个思路可迁移到代码审查、文案评审等场景。
