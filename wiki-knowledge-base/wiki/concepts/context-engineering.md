---
type: concept
tags: [上下文, token, agent, 工程化]
first_seen: "2026-02"
sources:
  - sources/2026-02-token-optimization
  - sources/2026-03-harness-what-is-it
  - sources/2026-03-harness-engineering
  - sources/2026-04-harness-from-add-to-delete
  - sources/2026-03-claude-code-hidden-commands
---

# Context Engineering

为 AI 模型动态构建完整上下文的工程方法，是 Prompt Engineering 的进阶，是 [[harness-engineering]] 的子集。

## 定义

Context Engineering（2025）：不只是打磨单次输入指令，而是动态构建完整上下文——相关文件、历史对话、工具定义、检索结果等。

## 上下文的工作机制

AI 不会真正"记忆"——每次响应都重新读取整个对话历史：
- 对话轮次越多、引用文件越大，每次请求消耗越多 Token。
- 超出上下文上限后模型会失去早期历史，导致记忆偏差（Claude 4.5 Sonnet 上限约 200K Token）。

## 上下文焦虑（Context Anxiety）

模型接近上下文边界时会下意识收尾，任务未完成就进入"交卷"状态。
- 这是目标漂移，不是单纯遗忘。
- 治理手段：compaction（压缩历史）vs context reset（彻底换会话 + 结构化交接）——两者层面不同。
- 来源：[[../sources/2026-03-agent-long-task]]

## 核心管理原则

### 渐进式信息披露（Progressive Disclosure）
- [[agents-md]] 做目录索引，不塞百科全书。
- [[agent-skills]] 按需加载：只在需要时加载相关知识，不一直占用上下文。
- 来源：[[../sources/2026-04-coding-agent-harness-practice]]

### 精确引用
- 只引用相关代码片段，而非整个文件。
- 用 `@文件名:函数名` 精确到函数级别，而非整个目录。
- 来源：[[../sources/2026-02-token-optimization]]

### 对话管理
- 上下文使用量达到 60% 时开新对话。
- 一个会话只处理一个任务，避免多任务干扰。
- 完成任务后清理不相关的文件引用。

### Subagent 隔离
[[subagent]] 的核心价值是上下文隔离——子任务的中间过程不污染主线程。
来源：[[../sources/2026-04-coding-agent-harness-practice]]

## 与 Harness Engineering 的关系

Context Engineering ⊂ [[harness-engineering]]：Harness 在上下文之外还管理约束、反馈循环和质量检查。
