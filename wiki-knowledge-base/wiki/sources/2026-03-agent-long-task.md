---
type: source
title: "Agent 长任务防失真"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, agent, 长任务, 上下文焦虑, evaluator, sprint-contract]
---

# Agent 长任务防失真

**来源**：星辰技术专项组 | 2026年3月  
**原文**：如何让单个 Agent 做长任务不失真：Anthropic 给出了一套更工程化的答案

## TL;DR

拆解 [[../entities/anthropic]] 的 Harness 设计，核心解决两类失真：**上下文焦虑**（任务未完成就提前收工）和**自我评估偏差**（模型倾向于夸自己）。最值得关注的是 Sprint Contract 和 Rubric 两个把主观质量压成可打分维度的机制。

## 核心主张

### 两类失真

**上下文焦虑（Context Anxiety）**
- 模型接近上下文边界时会下意识收尾，任务未完成就进入"交卷"状态。
- 这是目标漂移，不是单纯遗忘。

**自我评估偏差（Self-evaluation Bias）**
- 模型评价自己产出时偏正面，"能跑"被误判为"达标"。
- compaction（压缩历史）和 context reset（彻底换会话+结构化交接）是不同层面的治理手段。

### [[../concepts/planner-generator-evaluator]] 三代理分工
- **规划者（Planner）**：只约束"做什么"和"做到什么程度"，不抢 Generator 的实现决策。
- **生成者（Generator）**：执行具体实现。
- **评估者（Evaluator）**：保持怀疑态度，调校使其挑刺比自我检查容易得多。

### [[../concepts/sprint-contract]]
- 把"完成标准"从口头对齐变成可验证的协议文件。
- 任何维度低于阈值即判定失败，退回重做。

### 效果与成本
- V1（Opus 4.5）：单 Agent ~20 分钟/$9，完整 Harness ~6 小时/$200；成本差 20 倍，但核心功能可用性差距明显。
- V2（Opus 4.6）：去掉 Sprint 结构，生成器连续跑 2 小时+，QA 代理仍能抓到"最后一公里"问题。

### 脚手架增减逻辑
- 组件存在是因为当前模型还做不到；模型变强后要持续重估是否仍在"承重"。
- Opus 4.5 → 4.6：很多任务已在生成器独立能力范围内，evaluator 调用频率降低，但仍不可去掉。

## 页面触及
- [[../entities/anthropic]]
- [[../concepts/harness-engineering]]
- [[../concepts/planner-generator-evaluator]]
- [[../concepts/sprint-contract]]
- [[../concepts/context-engineering]]
