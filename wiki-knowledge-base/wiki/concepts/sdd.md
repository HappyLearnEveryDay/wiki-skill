---
type: concept
tags: [规范驱动开发, spec, 知识管理, 返工, 代码质量]
first_seen: "2026-04"
sources:
  - sources/2026-04-harness-and-sdd
---

# SDD（规范驱动开发）

Spec-Driven Development，以规格说明（Spec）为核心驱动的软件开发方法。在 [[harness-engineering]] 时代，SDD 被视为 Harness 的"导航仪"，两者互补而非竞争。

## 核心理念

SDD 优化的是**"正确交付总成本"**，而非"第一版代码速度"：
- 前移写 Spec 消灭后移返工和猜测成本。
- 不是让人更快写代码，而是让 Agent 第一次就写对。

## Spec 在 Harness 中的三重角色（来源：[[../sources/2026-04-harness-and-sdd]]）

### 推理地图
提供渐进式披露的结构化指引，让 Agent 从高层概览按需深入细节，而非被信息淹没或从零推理。

### 语义约束载体
Linter 只能检查格式层面约束；跨服务错误码含义、字段业务语义等**必须**通过 Spec 显式记录，否则 Agent 只能"合理猜测"。

### 反馈回路判据
[[../concepts/harness-engineering]] 飞轮（犯错→诊断→修复→不再犯）的第一步"发现错误"需要正确性判据，Spec 的 WHEN/THEN Scenario 就是对照标准。

## 放大器效应

Harness 放大输入质量的影响：
- Spec 好 → 输出可靠一致
- Spec 缺失 → 高效产出错误

引擎越强，导航仪越重要。速度越快，护栏要越坚固。

## 知识资产化

Spec 把设计决策和跨服务约定，从"人脑 + 一次性 AI 对话"转化为仓库中**版本化的结构化资产**：
- 对抗人员流失（新人接手时 Agent 仍可基于 Spec 正确工作）
- 对抗会话流失（Vibe Coding 中随意提需求导致设计决策随会话消失）

## Spec 漂移

Spec 漂移比代码漂移更隐蔽：
- 代码错误会报错，Spec 漂移不会报错。
- 沉默地让下一个 Agent 基于过时定义生成代码。
- 需要建立一致性检测机制（CI 脚本定期扫描 Spec-Code 偏差）。

## 实践建议

1. **Spec Review 是上游控制**：在 Spec 层发现问题修改成本极低，应把部分 Code Review 精力前移。
2. **[[../concepts/agents-md]] 做目录而非百科**：控制在约 100 行，细节通过 Spec 文件分散管理。
3. **建立 Spec-Code 一致性检查**：用脚本或 CI 流程定期扫描偏差。
4. **追问"AI 还缺什么"**：遇到 Bug 时首要追问是"AI 完成这件事时还缺什么 Spec"。
