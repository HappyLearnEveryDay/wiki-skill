---
type: source
title: "Anthropic Harness：从补到删"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, anthropic, 模型升级, dead-weight, 放权]
---

# Anthropic Harness：从补到删

**来源**：星辰技术专项组 | 2026年4月  
**原文**：Anthropic 的 Harness，已经进入新阶段：只用三招，开始从"补"转向"删"

## TL;DR

解读 [[../entities/anthropic]] 前后两篇 Harness 文章的内在变化——从"拼命补短板"到"开始删 dead weight"。核心论点：Harness 进入第二阶段，很多组件的存在不是因为永远正确，而是因为填上了某一代模型的能力缺口；缺口消失后组件该重新评估。

## 核心主张

### 两阶段：先补后删
- **第一阶段**（前篇）：回答"该补什么"——planner/evaluator、context reset、sprint contract，全是在替模型兜底。
- **第二阶段**（本篇）：回答"该删什么"——编排、上下文管理、记忆选择，哪些可以还给模型。
- **关键转折**：Sonnet 4.5 的 context anxiety 到 Opus 4.5 消失了，原来的 reset 机制从护栏变成 dead weight。

### 模式一：先用模型已经会的东西
- bash + text editor 是基座：Claude 3.5 Sonnet 仅靠这两个工具拿到 SWE-bench 49%，Opus 4.5 到 80.9%。
- 如果一个问题可以用通用工具解决，别急着固化成框架逻辑——过早专用化会卡住新模型。

### 模式二：三类控制权还给模型
- **编排权**：让 Claude 自己写代码串联工具调用，BrowseComp 准确率从 45.3% → 61.6%。
- **上下文管理权**：Skills 实现 progressive disclosure（先目录后细节）；subagents 隔离子任务。
- **记忆选择权**：compaction 让模型自己总结；Opus 4.6 记忆质量远超 Sonnet 3.5，从流水账变成战术笔记。

### 模式三：放权但必须有边界
- **成本边界**：缓存命中率是预算纪律，静态前置、动态后置，缓存 Token 成本仅为基础输入的 10%。
- **安全边界**：越难回滚的动作越值得做成声明式专用工具，而非藏在 bash 里。
- **可观测性边界**：类型化工具让系统可审计、可追踪、可回放。

### 核心启发
- **Build to delete 是工程节奏**：先搭、再验证、最后敢于拆掉，比"只加不减"更接近真正的工程实践。
- 每次模型升级后做 dead weight inventory。
- Harness 的可能性空间不会缩小，只是在移动：旧边界消失的同时，新边界问题会冒出来。

## 页面触及
- [[../entities/anthropic]]
- [[../concepts/harness-engineering]]
- [[../concepts/context-engineering]]
- [[../concepts/subagent]]
