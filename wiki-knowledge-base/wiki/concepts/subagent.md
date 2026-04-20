---
type: concept
tags: [agent, 上下文隔离, 工具权限, 工作流, claude-code]
first_seen: "2026-02"
sources:
  - sources/2026-02-openclaw-multi-agent
  - sources/2026-03-subagent-custom
  - sources/2026-04-coding-agent-harness-practice
  - sources/2026-03-claude-code-source-analysis
---

# Subagent

由主 Agent 临时派发的独立子 Agent，在隔离的上下文窗口中执行特定任务，完成后关闭。

## 核心价值

**上下文隔离**，不是角色分工（来源：[[../sources/2026-04-coding-agent-harness-practice]]）：
- 子任务的中间过程不污染主线程。
- 父 Agent 只看到 prompt 和最终结果。
- 可用便宜模型跑 Subagent，贵模型留在父线程做规划。

## 与 MultiAgent 的区别

| | Subagent | MultiAgent |
|---|---|---|
| 生命周期 | 临时，用完即关 | 常驻，独立工作区 |
| 创建方式 | 主 Agent 自然语言派发 | 需配置 agents.list + bindings |
| 隔离程度 | 上下文隔离 | 完全独立（工作区、认证、记忆） |
| 适用场景 | 一次性委托执行 | 长期独立角色 |

## 在 Claude Code 中创建（来源：[[../sources/2026-03-subagent-custom]]）

### 三种方式
1. `/agents` 交互式命令（推荐）
2. 手动创建 `.claude/agents/<name>.md`
3. CLI `--agents` 标志（临时会话）

### 文件结构
```yaml
---
name: code-reviewer
description: "PROACTIVELY review code after any modification. Check quality, security, and style."
tools: [Read, Grep, Glob]
model: claude-opus-4-7
permissionMode: default
---

You are a strict code reviewer...
```

### Description 是成败关键
- 必须写明触发条件，含 `PROACTIVELY when...` 关键词。
- 职责单一原则：3-5 个核心 Subagent 为理想数量。
- Description 即 API 契约：触发条件越精确，调用越可靠。

### 文件位置与作用域
- `.claude/agents/`：项目级（可共享给团队）
- `~/.claude/agents/`：用户级（跨项目）
- CLI `--agents`：会话级（优先级最高）

## 高级特性

- **持久化记忆**：`memory: user/project/local` 跨会话学习
- **Git Worktree 隔离**：`isolation: worktree` 在临时分支运行，保护主分支
- **链式调用**：planner → coder → reviewer → tester
- **并行调用**：研究类任务同时启动多个专家 Subagent

## 注意事项

- Subagent 的数据信息必须独立存放，不能存进主 Agent 的 memory（浪费 Token）。
- Subagent 工具集应最小化（只读型只需 Read/Grep/Glob）。
