---
type: source
title: "编码 Agent Harness 工程实践"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, 最佳实践, agents-md, subagent, hooks, 代码质量, task-design]
---

# 编码 Agent Harness 工程实践

**来源**：星辰技术专项组 | 2026年4月  
**原文**：编码 Agent Harness Engineering 最佳实践

## TL;DR

目前 [[../concepts/harness-engineering]] 领域最全面的中文综述之一，覆盖 12 个主题：核心公式 `coding agent = AI model(s) + harness`；指令文件、渐进式信息披露、Sub-agent 上下文隔离、Hooks 确定性控制、反压机制、任务设计与代码健康度等。

## 核心主张

### Harness 定义
- 核心公式：`coding agent = AI model(s) + harness`
- Harness 包括指令文件、工具/MCP、Skills、Sub-agents、Hooks、反压机制等。
- 目标：减少错误、提升可验证性、控制上下文消耗，**把正确行为变成默认行为**。

### 指令文件（AGENTS.md / CLAUDE.md）最佳实践
- ETH Zurich 研究：LLM 自动生成的 agentfile 反而降低性能并增加 20%+ 成本；人工编写的也仅改善约 4%。
- **什么有效**：命令优先、定义"完成"标准、按任务组织规则、保持简短（< 150 行）。
- **什么无效**：散文段落、模糊指令（"尽量""优雅地"）、矛盾约束、代码库概述目录（Agent 自己能探索）。

### 渐进式信息披露
- AGENTS.md 应做目录索引而非百科全书。
- OpenAI 仓库使用 88 个独立 AGENTS.md 文件，root 文件约 100 行只做索引。
- Skills 机制体现按需加载：Agent 决定需要时才加载相关文件。

### Sub-agents 与上下文隔离
- Sub-agent 核心价值是**上下文隔离**，不是角色分工。
- "前端工程师"+"后端工程师"这种角色化切法不起作用。
- 可用便宜模型跑 Sub-agent，贵模型留在父线程做规划。

### Hooks 与反压
- Hooks 是确定性控制流：CLAUDE.md 的规则是建议，Hook 是强制执行。
- **关键模式**：成功时静默（不消耗上下文），失败时才输出错误。
- 反压机制：避免早期 4000 行成功输出淹掉 Agent 的任务上下文。
- 质量控制三层：实时审查 → 提交前审查 → PR 预检查。

### 任务设计
- **80% 的 agentic 工作流失败**发生在任务设计和上下文阶段，而非 Agent 本身。
- Agent-Ready 任务需 4 部分：成功定义、显式作用域、不可推断的约束、相关背景。

### 代码健康度与 Agent 效果
- CodeScene 研究：代码健康度 ≥ 9.5/10 时 Agent 准确度约 95%；7.0 时仅约 60%。
- 不是"用 Agent 改善代码"，而是"先改善代码让 Agent 发挥"。
- Code SEO：同义词注释、避免重复文件名、完整词优于缩写。

## 核心启发
- 上下文是稀缺资源：几乎所有最佳实践都围绕"减少上下文消耗"展开。
- 代码质量是 Agent 效果的前提条件，呈非线性关系。

## 页面触及
- [[../concepts/harness-engineering]]
- [[../concepts/agents-md]]
- [[../concepts/subagent]]
- [[../concepts/agent-skills]]
- [[../entities/anthropic]]
