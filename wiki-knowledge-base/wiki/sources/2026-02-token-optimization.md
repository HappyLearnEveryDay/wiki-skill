---
type: source
title: "AI 编程技巧：最大化 Token 利用"
date_ingested: "2026-04-20"
medium: 文章
tags: [token, 上下文管理, cursor, 提问技巧, skill]
---

# AI 编程技巧：最大化 Token 利用

**来源**：星辰技术专项组 | 2026年2月

## TL;DR

讲解 Token 的基本概念、上下文工作机制，以及在 [[../entities/cursor]] 等 AI 编程工具中高效使用 Token 的核心技巧，包括提问方式、工具选择、模型分层使用和对话管理策略。

## 核心主张

### Token 与上下文
- 1 Token ≈ 3-4 个英文字母 或 0.5-1 个中文字。
- AI 每次回答都会重新读取整个对话历史——轮次越多、引用文件越大，每次请求消耗越多。
- 超出上下文上限后模型会失去早期历史，导致记忆/理解偏差。Claude 4.5 Sonnet 上限约 200K Token。

### 高效提问技巧
- **明确需求**：指定角色、明确格式、提供背景，避免 AI 生成大量无用内容。
- **先确认再执行**：让 AI 先问问题再动手，避免返工。
- **Plan 模式处理复杂任务**：先出计划、再执行，避免方向错误。
- **Debug 模式调试**：先增加打印、再分析日志，系统定位问题。
- **精确引用**：只引用相关代码片段而非整个文件。

### 模型分层使用
- 简单任务（格式化、语法检查）用便宜快速模型；复杂任务（架构、算法）用高级模型（如 Claude Opus 4.6）。
- 日常开发可节省 60-80% Token 成本。

### 对话管理
- 上下文使用量到 60% 时开新对话。
- 一个会话只处理一个任务，避免多任务干扰。
- 完成任务后清理不相关文件引用。

### OpenSkills / Skills
- [[../concepts/agent-skills]] 是模块化、可复用的指令集，只在需要时加载，节省 Token。
- 用 `openskills install` 安装，用 `openskills sync` 同步到 AGENTS.md。

## 页面触及
- [[../entities/cursor]]
- [[../concepts/token-optimization]]
- [[../concepts/agent-skills]]
- [[../concepts/context-engineering]]
