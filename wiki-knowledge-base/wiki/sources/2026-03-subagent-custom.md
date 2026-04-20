---
type: source
title: "Subagent 自定义入门"
date_ingested: "2026-04-20"
medium: 文章
tags: [subagent, claude-code, yaml, 工具权限, 上下文隔离, 工作流]
---

# Subagent 自定义入门

**来源**：星辰技术专项组 | 2026年3月  
**原文**：5 分钟上手 Claude 自定义 Subagents（作者：ElioYue）

## TL;DR

系统讲解 [[../entities/claude-code]] 中自定义 [[../concepts/subagent]] 的创建方法与配置选项，涵盖三种创建方式、完整 YAML 字段说明和四个实战案例。Subagent 通过独立上下文窗口 + 工具白名单实现职责隔离。

## 核心主张

### Subagent 的核心价值
- **独立上下文窗口**：每个 Subagent 运行在隔离的上下文中，不污染主对话。
- **工具权限最小化**：只读型只需 Read/Grep/Glob，修改型可加 Write/Edit/Bash。
- **模型灵活选择**：Opus（深度推理）/ Sonnet（平衡）/ Haiku（快速低成本）。

### 三种创建方式
1. `/agents` 交互式命令（推荐）
2. 手动创建 `.md` 文件（YAML frontmatter + Markdown）
3. CLI `--agents` 标志（临时会话）

### 文件位置与作用域
- `.claude/agents/`：项目级，可共享给团队
- `~/.claude/agents/`：用户级，跨项目
- CLI 标志：会话级，优先级最高

### Description 编写是成败关键
- 模糊 description 会导致 Claude 无法判断何时委派。
- 善用 `PROACTIVELY` 关键词：鼓励 Claude 主动判断并自动委派。
- **职责单一原则**：3-5 个核心 Subagent 为理想数量，每个只做一件事。

### YAML 配置字段
- 必填：`name`、`description`
- 可选：`tools`、`model`、`permissionMode`、`maxTurns`、`skills`、`memory`、`hooks`、`isolation`、`background`

### 高级特性
- 持久化记忆：`memory: user/project/local` 让 Subagent 跨会话学习
- Git Worktree 隔离：`isolation: worktree` 在临时分支中运行
- 链式调用：planner → coder → reviewer → tester
- 并行调用：研究类任务可并行启动多个专家 Subagent

## 核心启发
- AI 工作流的本质是"分工"而非"全能"：与其让一个 AI 做所有事，不如拆成多个专精角色的协作链。
- Description 即 API 契约：触发条件越精确，调用方越可靠。
- 成本可控的 AI 编排已成为现实：Haiku 处理简单任务，Opus 处理复杂推理。

## 页面触及
- [[../entities/claude-code]]
- [[../concepts/subagent]]
- [[../concepts/multi-agent-architecture]]
