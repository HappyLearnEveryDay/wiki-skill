---
type: source
title: "Harness 三层架构拆解"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, 知识层, 约束层, 反馈层, openai, anthropic, evaluator]
---

# Harness 三层架构拆解

**来源**：星辰技术专项组 | 2026年3月  
**原文**：大家都在讲 Harness，但它到底该怎么理解

## TL;DR

通过对比 OpenAI Codex 和 [[../entities/anthropic]] 的工程实践，把 [[../concepts/harness-engineering]] 拆解为三个有边界的工程层级：**知识层**、**约束与流程层**、**反馈与运行时层**。同一模型不改 Prompt，只换 Harness，产出质量差距可达 20 倍以上。

## 三层架构

### 层一：知识层——让隐性经验变成显式工件
- [[../concepts/agents-md]] 不是百科全书：OpenAI 从巨石文档转向约 100 行的"内容目录"，指向结构化 `docs/` 目录。
- **对 Agent 来说，看不见的知识等于不存在**：Slack 讨论、资深工程师脑中的判断，未编码进仓库就等于没有。
- 确定性逻辑别塞上下文：能通过 Hooks、代码规则、工具权限表达的，交给外部系统处理。
- 文档防腐：OpenAI 专门跑 doc-gardening Agent 扫描过时文档、自动发起修复 PR。

### 层二：约束与流程层——职责分离与架构护栏
- [[../concepts/planner-generator-evaluator]] 三角色分离：不是为了并行更快，而是避免"一边生成一边自我表扬"。
- Sprint 契约：每个 Sprint 有 27 条验收标准，Generator 和 Evaluator 先谈好"做到什么算完成"再动手。
- Context Reset：长任务中定期清空上下文窗口、通过结构化工件交接状态，对抗"上下文焦虑"。
- OpenAI 的机械约束：分层依赖方向由 linter 自动拦截违规 PR。

### 层三：反馈与运行时层——让模型有感官
- Evaluator 用 Playwright **真跑页面**：不是读代码打分，而是点按钮、查 DOM。
- 开箱即用的 Claude 是很差的 QA Agent：需多轮迭代调教 evaluator 保持怀疑态度。
- 可观测性进闭环：OpenAI 把 Chrome DevTools、LogQL、PromQL 接入运行时。
- **评估器去不掉**：不管模型多强，自评能力提升远跟不上生成能力的提升。

## 核心启发
- Harness 是长出来的，不是设计出来的：从第一个犯错开始补。
- Prompt → Context → Harness 是包含关系，不是替代关系。
- NLAH 方向（自然语言描述控制逻辑 + Runtime 执行）值得关注。

## 页面触及
- [[../concepts/harness-engineering]]
- [[../concepts/agents-md]]
- [[../concepts/planner-generator-evaluator]]
- [[../concepts/sprint-contract]]
- [[../entities/anthropic]]
