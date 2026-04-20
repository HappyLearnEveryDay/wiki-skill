---
type: source
title: "Cursor 入门文档"
date_ingested: "2026-04-20"
medium: 文档
tags: [cursor, 入门, 插件, 配置, erlang, 隐私]
---

# Cursor 入门文档

**来源**：星辰技术专项组 | 2026年2月

## TL;DR

[[../entities/cursor]] 的团队内部入门指南，涵盖安装注册、常用插件、配置文件导入、隐私设置（内部项目必须开 Privacy Mode）、文件索引优化、User Rules 配置、模型选择及日常开发实用案例。

## 核心主张

### 重要配置
- **隐私模式**（Privacy Mode Legacy）：打开公司内部项目**必须**开启，防止代码泄露。
- **文件索引优化**：在 `.cursorignore` 排除 `*.beam`、`*.meta`、`zone/merge/dets/` 等非必要文件，加快索引速度。
- **User Rules**：配置通用规则让 AI 回答更符合团队习惯（精简、不过度设计、Always respond in Chinese-simplified 等）。

### 三种对话模式
- **Agent**：主动分析并跨文件修改代码，需要 accept。
- **Plan**：先生成计划，明确后再交 Agent 执行。
- **Ask**：问答模式，自己选择是否采纳。

### 常用插件
- 中文包、GitLens、Better Comments Next、Color Highlight
- Erlang: ELP（体验优于 Erlang LS）

### 日常实用案例
- AI 代码审核：扫描文件夹检查逻辑错误和注释不一致
- 由图生成前端代码（截图 + 提示）
- 分析函数调用链（30 分钟缩短到 5-10 分钟）
- Debug 模式：加打印 → 跑测 → 分析日志 → 修复
- 快速搭建基础代码框架（给出详细需求一次成型）

## 页面触及
- [[../entities/cursor]]
- [[../concepts/ai-coding-workflow]]
- [[../concepts/token-optimization]]
