---
type: concept
tags: [指令文件, harness, 知识层, 文档, claude-md]
first_seen: "2026-03"
sources:
  - sources/2026-03-harness-what-is-it
  - sources/2026-03-harness-engineering
  - sources/2026-03-harness-three-layer
  - sources/2026-04-harness-and-sdd
  - sources/2026-04-coding-agent-harness-practice
---

# AGENTS.md / CLAUDE.md

[[../concepts/harness-engineering]] 的知识层核心文件，用于将隐性经验显式化为 Agent 可读的指令。`AGENTS.md` 是通用约定，`CLAUDE.md` 是 [[../entities/claude-code]] 专用的同类文件。

## 核心原则

**对 Agent 来说，看不见的知识等于不存在**（来源：[[../sources/2026-03-harness-three-layer]]）。
Slack 讨论、资深工程师脑中的判断，未编码进仓库就等于没有。

## 最佳实践

### 应该有的内容
- 命令优先（直接可执行的命令，如构建/测试命令）
- 明确的"完成"标准定义
- 按任务类型组织的规则
- 约 100 行的目录索引，指向分散的专项文档

### 不应该有的内容
- 散文段落（模型难以提取行动项）
- 模糊指令（"尽量""优雅地"）
- 矛盾约束
- 代码库概述和目录列表（Agent 自己能探索仓库结构）
- 无验证的风格指南

### 什么无效
ETH Zurich 研究：LLM 自动生成的 agentfile 反而降低性能并增加 20%+ 成本；人工编写的也仅改善约 4%。
（来源：[[../sources/2026-04-coding-agent-harness-practice]]）

## 大型 AGENTS.md 的四个系统性问题（来源：[[../sources/2026-04-harness-and-sdd]]）

1. 挤掉上下文（占用过多 Token）
2. 过多失效条目
3. 立即腐烂（难以保持最新）
4. 难以核实（无法验证规则是否有效）

**解法**：控制在约 100 行，做轻量目录 + 分散的专项文档。

## 文档防腐

OpenAI 专门运行 doc-gardening Agent，定期扫描过时文档、自动发起修复 PR。
（来源：[[../sources/2026-03-harness-three-layer]]）

## 与 Spec（[[sdd]]）的关系

AGENTS.md 是 Harness 的知识层入口，Spec 提供更细粒度的语义约束（字段含义、业务规则、WHEN/THEN Scenario）。两者互补：
- AGENTS.md：目录索引 + 行为约束
- Spec：推理地图 + 语义约束 + 验收判据

## Mitchell Hashimoto 的方法

每次 Agent 犯错就把修复方式写进 AGENTS.md，配置文件是活的、一直在长。
从第一个 Agent 犯错开始补，不需要画完整架构图。
（来源：[[../sources/2026-03-harness-what-is-it]]）
