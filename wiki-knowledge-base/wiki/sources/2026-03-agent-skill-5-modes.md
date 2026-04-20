---
type: source
title: "Agent Skill 五模式"
date_ingested: "2026-04-20"
medium: 文章
tags: [skill, agent, 设计模式, workflow, google-cloud]
---

# Agent Skill 五模式

**来源**：星辰技术专项组 | 2026年3月  
**原文**：Google Cloud Tech 关于 Agent Skill 内容设计

## TL;DR

系统拆解 Google Cloud Tech 提出的五种 Agent [[../concepts/agent-skills]] 设计模式。核心判断：Agent 设计正在从"写提示词"升级为"设计工作流"，真正决定 Skill 质量的是内部逻辑结构，而非格式规范。

## 五种模式

### 1. Tool Wrapper — 按需加载知识的"临时专家"模式
- **解决问题**：模型通用能力强，但在特定框架/SDK/团队规范上不够稳定。
- **设计**：技术栈规则整理进 `references/`，SKILL.md 规定仅相关任务时才加载。
- **本质**：让模型在需要时精确加载正确知识，而非一直占用上下文。

### 2. Generator — 压制自由发挥的"模板驱动交付"模式
- **解决问题**：Agent 每次生成文档结构不统一，风格飘移严重。
- **设计**：`assets/` 放模板，`references/` 放风格指南，SKILL.md 调度流程（先读规则→读模板→问用户→填充）。
- **关键洞察**：团队协作中一致性往往比华丽更值钱。

### 3. Reviewer — 规则驱动的"可替换检查框架"模式
- **解决问题**：审查标准堆在 system prompt 里难以维护和复用。
- **设计**：SKILL.md 负责流程，`references/review-checklist.md` 负责具体标准，输出按 error/warning/info 分级。
- **最大价值**：换 checklist 不用重写整个 Skill，团队经验可版本化、模块化。

### 4. Inversion — "先采访再动手"的需求澄清模式
- **解决问题**：大模型天然倾向于一接到任务就马上生成，导致复杂任务产出"看上去完整"的空方案。
- **设计**：强制进入提问优先阶段，收集完整需求后才允许进入生成阶段。
- **核心**：先把问题问对，再给答案。

### 5. Pipeline — "流程闸门"防止跳步的受约束执行模式
- **解决问题**：复杂任务中模型喜欢跳过中间验证点直接给最终答案。
- **设计**：将任务拆为不能随意跳步的流水线，关键节点设 gate condition。
- **核心价值**：不是期待模型自觉守规矩，而是通过流程设计让它不得不守规矩。

## 核心启发
- Prompt Engineering → **Workflow Engineering**：范式级转变。
- Skill 是团队经验的载体，结构化后 Agent 才能从"看起来能用"变成"长期可用"。
- 可靠性来自结构而非更长说明；护城河不在格式而在逻辑设计。

## 页面触及
- [[../concepts/agent-skills]]
- [[../concepts/harness-engineering]]
