---
type: source
title: "Claude Code 隐藏命令"
date_ingested: "2026-04-20"
medium: 文章
tags: [claude-code, 命令, 快捷键, token, 上下文管理]
---

# Claude Code 隐藏命令

**来源**：星辰技术专项组 | 2026年3月  
**原文**：分享10个你可能不知道的 Claude Code 隐藏命令

## TL;DR

梳理了 [[../entities/claude-code]] 中 10 个低感知度但高价值的隐藏命令与快捷键，核心价值在于帮助用户从"知道命令"跃迁到"理解场景"，避免 Token 浪费和效率损失。

## 10 个隐藏命令

| 命令 | 作用 |
|------|------|
| `/btw` | 免污染提问：并行进程回答，不写入对话历史，复用提示缓存 |
| `/rewind` | 代码/对话分离回退：代码回退、对话保留，Claude 记得"此路不通" |
| `/insights` | 使用习惯自分析：生成本地 HTML 报告，分析过去一个月使用模式 |
| `/model opusplan` | 规划用 Opus、执行切回 Sonnet，兼顾质量与额度 |
| `/simplify` | 并行启动三个 Agent 从复用/质量/效率三维审查代码 |
| `/branch` | 对话分叉（原 /fork）：保留当前进度同时探索新方向 |
| `/loop` | 定时循环任务，3 天后自动删除 |
| `/remote-control` (或 `/rc`) | 生成 URL，手机同步操控终端会话（代码在本地运行） |
| `/export` | 导出对话为 Markdown，保存架构讨论和判断过程 |

## 快捷键

- `Ctrl+V`：直接粘贴截图（非 Cmd+V）
- `Ctrl+J` / `Option+回车`：输入框内换行
- `Ctrl+R`：搜索历史 prompt
- `Ctrl+U`：删除整行输入

## 核心启发
- 上下文管理是 AI 编程的核心技能：`/btw` 和 `/rewind` 本质都是管理上下文窗口。
- 工具认知的"冰山效应"：大多数人只用了 Claude Code 表面 20% 的能力。
- 模型组合策略是成本控制关键：`opusplan` 的"规划用强模型、执行用快模型"思路值得推广。

## 页面触及
- [[../entities/claude-code]]
- [[../concepts/token-optimization]]
- [[../concepts/context-engineering]]
