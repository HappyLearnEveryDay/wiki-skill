---
type: source
title: "Harness Engineering 又是啥？"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, context-engineering, prompt-engineering, 多agent架构, mitchell-hashimoto]
---

# Harness Engineering 又是啥？

**来源**：星辰技术专项组 | 2026年3月

## TL;DR

将 [[../concepts/harness-engineering]] 定义为围绕 AI Agent 搭建约束、反馈、质量检查的完整运行环境。核心洞察：模型可能已不是瓶颈，你给它搭的环境才是；上下文过多反而导致模型表现下降，harness 不是越大越好。

## 核心主张

### 三代工程进化（包含关系，非替代）
- **Prompt Engineering**（2022-2024）：对马说的话
- **[[../concepts/context-engineering]]**（2025）：帮马看路的一切
- **[[../concepts/harness-engineering]]**（2026）：缰绳、马鞍、围栏和道路本身

Context 是 Harness 的子集；Harness 还管理约束、反馈循环和质量检查。

### 实际效果数据
- LangChain：coding agent 在 Terminal Bench 2.0 从 52.8% → 66.5%，模型完全没换，只改了系统提示词、工具配置和中间件钩子。
- OpenAI Codex 团队：3 名工程师 5 个月用 Codex 产出 100 万行代码，估算速度是传统方式 10 倍。

### 两大主流 Harness 架构
- **Martin Fowler 三块拆分**：上下文工程（给地图不给说明书）+ 架构约束（代码检查器硬拦截）+ 垃圾回收（专职 Agent 找矛盾和违规）
- **[[../entities/anthropic]] 三 Agent 架构**：规划者 + 生成者 + 评估者，灵感来自 GAN，专门训练 evaluator 挑刺

### Mitchell Hashimoto 方法
每次 Agent 犯错就工程化一个方案让它再也犯不了同样的错，配置文件是活的、一直在长。

### 关键反直觉洞察
- 上下文焦虑：Claude Sonnet 4.5 在上下文过多时表现反而变差，**harness 不是越大越好**。
- 建议与约束是完全两回事：prompt 里写"请注意代码规范"是建议，hooks 里做物理拦截是约束。

## 页面触及
- [[../concepts/harness-engineering]]
- [[../concepts/context-engineering]]
- [[../entities/anthropic]]
- [[../concepts/planner-generator-evaluator]]
- [[../concepts/agents-md]]
