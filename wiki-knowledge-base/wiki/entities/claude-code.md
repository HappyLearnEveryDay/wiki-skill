---
type: entity
category: 工具
tags: [ai-coding, cli, anthropic, agent, mcp]
first_seen: "2026-02"
sources:
  - sources/2026-02-openclaw-deploy
  - sources/2026-03-claude-code-source-analysis
  - sources/2026-03-claude-code-hidden-commands
  - sources/2026-03-subagent-custom
---

# Claude Code

[[../entities/anthropic]] 开发的终端 AI 编程助手，基于 Bun 运行时，以 CLI 形式运行。

## 基本信息

- **开发商**：[[anthropic]]
- **形态**：CLI / 终端工具
- **技术栈**：TypeScript/TSX，约 512,664 行，1884 个文件（2026年3月泄露源码）
- **运行时**：Bun

## 架构

### Tool-First 设计
所有能力（文件读写、Shell 执行、Agent 调度等）统一建模为 Tool 接口，内置 43 个工具。
（来源：[[../sources/2026-03-claude-code-source-analysis]]）

### 核心模块
- **QueryEngine**：LLM 交互核心，管理对话生命周期与上下文压缩。
- **Coordinator 模式**：Orchestrator-Worker 多 Agent 架构，支持并行任务编排。
- **权限系统**：三层递进——配置规则 → 自动分类器 → 用户交互弹窗。
- **MCP 集成**：支持 stdio/sse/http/ws 等传输协议，工具动态发现。
- **Bridge 系统**：允许从 IDE 或远程环境控制 Claude Code 实例。

### Feature Flag
通过编译期特性门控，一套代码库可产出不同产品形态（CLI vs 内部版）。

## 对话模式

| 模式 | 说明 |
|------|------|
| Agent | 主动分析并跨文件修改代码，需用户 accept |
| Plan | 先出计划，确认后再执行 |
| Ask | 问答模式，用户决定是否采纳 |

## 隐藏命令（部分）

| 命令 | 功能 |
|------|------|
| `/btw` | 免污染提问，不写入对话历史 |
| `/rewind` | 代码/对话分离回退 |
| `/model opusplan` | 规划用 Opus、执行用 Sonnet |
| `/simplify` | 三维并行代码审查 |
| `/branch` | 对话分叉（原 /fork） |
| `/loop` | 定时循环任务 |
| `/remote-control` | 手机远程操控终端会话 |

（来源：[[../sources/2026-03-claude-code-hidden-commands]]）

## 自定义 Subagent

支持通过 `.claude/agents/*.md` 定义项目级 Subagent，配置工具白名单、模型、隔离模式等。
（来源：[[../sources/2026-03-subagent-custom]]）

## 部署

- 可在 Ubuntu 虚拟机中运行（见 [[../entities/openclaw]] 部署指南）。
- 支持通过 CC-Switch 快速切换所使用的模型配置。
