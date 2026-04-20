---
type: source
title: "Claude Code 源码深度分析"
date_ingested: "2026-04-20"
medium: 文章
tags: [claude-code, 源码, 架构, 工具系统, 权限, mcp, agent]
---

# Claude Code 源码深度分析

**来源**：星辰技术专项组 | 2026年3月  
**基于**：GitHub 仓库 instructkr/claude-code（2026年3月31日 npm source map 泄露镜像）1884 个 TypeScript 文件，约 512,664 行代码

## TL;DR

对 [[../entities/claude-code]] 源码的全面分析，覆盖架构总览、Tool 系统、QueryEngine、Agent 协调器、权限系统、Bridge 系统、上下文系统、MCP 集成、Skills/插件系统等核心模块，以及 Feature Flag 死代码消除、Prompt Cache 稳定性设计等技术亮点。

## 架构总览

```
用户输入 → QueryEngine → LLM API (Anthropic)
              ↑              ↓
          Tool 系统 ← → 权限系统
              ↓
         Agent 协调器（多 Agent）
              ↓
         MCP 集成 / Bridge（IDE 远程）
```

**核心设计原则**：
- Feature Flag 驱动的死代码消除（编译期特性门控）
- Tool-First 架构：所有能力都建模为统一 Tool 接口
- 权限分层：配置规则 → 自动分类器 → 用户交互弹窗
- React + Ink 终端 UI

## Tool 系统

- 内置 43 个工具，包括 AgentTool、BashTool、FileReadTool、FileEditTool、GlobTool、GrepTool、SkillTool 等。
- `shouldDefer: true` 标记的工具延迟加载，模型需要时先调用 ToolSearch 检索。
- 工具排序保持字母序，确保 Prompt Cache 前缀稳定。

## QueryEngine

- 管理整个对话生命周期：系统提示词组装、用户输入处理、API 调用循环。
- 自动压缩（autoCompact）、Snip 压缩、响应式压缩（REACTIVE_COMPACT）三种上下文管理策略。

## Coordinator（多 Agent 编排）
- Orchestrator-Worker 模式：Coordinator 接需求，Workers 执行具体任务。
- **关键原则**："Always synthesize — your most important job. Never write 'based on your findings'."
- Worker 工具集受 `ASYNC_AGENT_ALLOWED_TOOLS` 白名单限制。
- 支持 `worktree` 和 `remote` 两种隔离模式。

## 权限系统（三层递进）
1. 配置规则（alwaysAllow / alwaysDeny / alwaysAsk）
2. 自动分类器（AI 驱动的安全判断）
3. 用户交互弹窗（最终兜底）

Bash 工具额外进行 AST 解析、只读验证、沙箱判断、破坏性命令检测。

## 技术亮点
- Feature Flag 死代码消除：一套代码库，不同产品产出不同二进制。
- Prompt Cache 稳定性：工具按字母序排列，保持缓存前缀不变。
- 工具结果持久化：大结果自动写文件，Claude 收到预览+路径而非完整内容。

## 安全启示
- 纵深防御：工具白名单 → 配置规则 → 自动分类器 → 用户确认 → 沙箱。
- source map 泄露教训：生产产物不应包含 `.map` 文件。

## 页面触及
- [[../entities/claude-code]]
- [[../entities/anthropic]]
- [[../concepts/multi-agent-architecture]]
- [[../concepts/harness-engineering]]
- [[../concepts/agent-skills]]
