---
type: concept
tags: [agent, 架构, 工程化, 运行时, 评估, 约束]
first_seen: "2026-03"
sources:
  - sources/2026-03-harness-engineering
  - sources/2026-03-harness-three-layer
  - sources/2026-03-harness-what-is-it
  - sources/2026-03-agent-long-task
  - sources/2026-04-harness-from-add-to-delete
  - sources/2026-04-harness-and-sdd
  - sources/2026-04-coding-agent-harness-practice
---

# Harness Engineering

围绕 AI Agent 搭建约束、反馈、质量检查的完整运行环境的工程方法论。

## 定义

Mitchell Hashimoto 的朴素定义："每当 Agent 犯错，就工程化一个让它不再犯同样错的方案。"

核心公式（来源：[[../sources/2026-04-coding-agent-harness-practice]]）：
```
coding agent = AI model(s) + harness
```

Harness 包括指令文件、工具/MCP、Skills、Sub-agents、Hooks、反压机制等。

## 三代工程进化（包含关系，非替代）

| 阶段 | 时间 | 定义 |
|------|------|------|
| Prompt Engineering | 2022-2024 | 打磨单次输入指令（few-shot、CoT、角色扮演） |
| [[context-engineering]] | 2025 | 为模型动态构建完整上下文 |
| Harness Engineering | 2026 | 搭建 Agent 的整个工作环境，含约束、反馈、生命周期管理 |

Prompt ⊂ Context ⊂ Harness（包含关系）

## 为什么重要

同一模型只换 Harness，成功率可从 42% → 78%（来源：[[../sources/2026-03-harness-engineering]]）。
当前阶段优化模型外层环境的回报率，可能高于等下一代模型。

## 三层架构（来源：[[../sources/2026-03-harness-three-layer]]）

### 知识层
- [[agents-md]] 做目录而非百科全书（约 100 行）。
- Agent 看不到的知识等于不存在——隐性知识必须显式化到仓库。
- 确定性逻辑通过 Hooks/代码规则/工具权限表达，不塞上下文。

### 约束与流程层
- [[planner-generator-evaluator]] 三角色分离。
- [[sprint-contract]] 把完成标准变成可验证的协议文件。
- Context Reset 对抗上下文焦虑。

### 反馈与运行时层
- Evaluator 用 Playwright 真跑页面，而非自评。
- 评估器去不掉：自评能力提升远跟不上生成能力。
- 可观测性（DevTools、日志查询）进入闭环。

## 两阶段演进（来源：[[../sources/2026-04-harness-from-add-to-delete]]）

- **第一阶段（补）**：识别模型短板，添加 planner/evaluator、sprint contract 等组件兜底。
- **第二阶段（删）**：模型变强后，识别 dead weight，把编排权、上下文管理权、记忆选择权还给模型。

**Build to delete 是工程节奏**：先搭、再验证、最后敢于拆掉。每次模型升级后做 dead weight inventory。

## 与 [[sdd]] 的关系（来源：[[../sources/2026-04-harness-and-sdd]]）

Harness 是引擎，SDD（Spec）是导航仪，两者互补而非竞争。
- Harness 放大输入质量的影响：Spec 好则输出可靠，Spec 缺失则高效产出错误。
- 速度越快，护栏要越坚固。

## 关键反直觉

- 上下文过多反而导致模型表现下降（上下文焦虑）——harness 不是越大越好。
- 建议 ≠ 约束：prompt 里写规范是建议，hooks 里做物理拦截是约束，系统设计必须区分这两层。
- 约束解空间反而让 Agent 更有生产力（Cursor 的发现）。

## 实践入门

1. 从第一个 Agent 犯错开始写规则文件，遇到问题补一条约束。
2. 把隐性知识（架构决策、服务间约定）版本化进仓库。
3. 给 Agent 接真实环境反馈（Playwright/截图/日志），不满足于代码自评。
4. 给 CI 设上限：Agent 修复失败最多允许 2 轮，超出转交人类。
