---
type: source
title: "Harness Engineering 解析"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, anthropic, openai, stripe, cursor, generator-evaluator, gan]
---

# Harness Engineering 解析

**来源**：星辰技术专项组 | 2026年3月  
**原文**：Anthropic 说：不要再等下一代模型了，立刻马上做 Harness！

## TL;DR

系统梳理 [[../concepts/harness-engineering]] 的概念起源、行业实践和核心价值——同一模型只换"外壳"，成功率可从 42% 跳到 78%。Anthropic、OpenAI、Stripe、Cursor 等团队几乎同时走向同一方向，用 GAN 式的 generator-evaluator 分离架构解决"模型不会自我评价"的根本问题。

## 核心主张

### 三代工程进化
- **Prompt Engineering**（2022-2024）：few-shot、CoT、角色扮演
- **Context Engineering**（2025）：动态构建完整上下文
- **Harness Engineering**（2026）：搭建 Agent 的整个工作环境

Mitchell Hashimoto 定义："每当 Agent 犯错，就工程化一个让它不再犯同样错的方案"

### 行业实证数据
- Nate B Jones：同一模型，只换 Harness，成功率 42% → 78%
- LangChain：Terminal Bench 2.0 从 52.8% → 66.5%，排名从 30+ 进前 5
- [[../entities/anthropic]] 实验：solo 模式 20 分钟/$9 产出核心功能损坏 vs 完整 Harness 6 小时/$200 全功能
- Pi Research：一个下午仅改 Harness，提升了 15 个不同 LLM 的编程能力

**核心结论**：优化模型外面的壳，当前回报率可能比等下一代模型更高。

### Anthropic 的 GAN 式解法
- 根本问题：模型不会评价自己的工作，自信地认为产出很好。
- 解法：把 generator 和 evaluator 拆成独立 Agent。
- Evaluator 用 Playwright 真机操作验收，不是看截图打分。
- 关键洞察：让独立 evaluator 变严格，远比让 generator 学会自我批评容易。

### 行业多团队的 Harness 设计
- **OpenAI Codex**：5 个月 100 万行代码；核心规则——仓库是唯一知识源，架构约束靠 linter 不靠 prompt。
- **Stripe**：每周 1300+ PR 无人值守；CI 最多跑两轮，失败就转交人类。
- **Cursor**：从单 Agent 到递归 Planner-Worker，4 次架构迭代；发现约束解空间反而让 Agent 更有生产力。

### 反方与未来
- Noam Brown（OpenAI）：Harness 像拐杖，统一模型未来终将超越它。
- METR 数据：维护者实际合并率约为自动评分通过率的一半，7 倍能力高估。
- Anthropic 回应：模型变强后旧约束可拆，但新的高阶约束空间打开。

## 页面触及
- [[../concepts/harness-engineering]]
- [[../entities/anthropic]]
- [[../concepts/planner-generator-evaluator]]
- [[../concepts/agents-md]]
