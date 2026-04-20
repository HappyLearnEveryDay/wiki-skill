---
type: source
title: "Harness 与 SDD"
date_ingested: "2026-04-20"
medium: 文章
tags: [harness, sdd, spec, agents-md, 规范驱动开发, 知识管理]
---

# Harness 与 SDD

**来源**：星辰技术专项组 | 2026年4月  
**原文**：Harness Engineering 不是替代方案，而是 SDD 的终极放大器

## TL;DR

厘清 [[../concepts/harness-engineering]] 与 [[../concepts/sdd]] 的关系——两者不是竞争，而是引擎与导航仪的互补。核心洞察：Agent 看不到的就不存在，Spec 把隐性知识显式写进仓库，是 Harness 长期有效的关键前提；引擎越强，导航仪越重要。

## 核心主张

### Harness 是工程化错误预防系统
- Harness 不是"让 AI 自己干"，而是把"支撑 Agent 正确工作的所有要素"显式写进代码仓库。
- **可见性约束**：Agent 运行时无法访问的内容等于不存在——Google Docs、聊天记录、人脑中的知识对 Agent 无效。

### Spec 在 Harness 中的三重角色
1. **推理地图**：提供渐进式披露的结构化指引，让 Agent 从高层概览按需深入细节。
2. **语义约束载体**：Linter 只能检查格式层面，跨服务错误码含义、字段业务语义必须通过 Spec 显式记录。
3. **反馈回路判据**：Harness 飞轮（犯错→诊断→修复→不再犯）的第一步"发现错误"需要 Spec 的 WHEN/THEN Scenario 作为对照标准。

### Harness 时代 Spec 比以往更重要
- 放大器效应：Harness 放大输入质量的影响——Spec 好则输出可靠一致，Spec 缺失则高效产出错误。
- 消灭返工：SDD 优化的是"正确交付总成本"而非"第一版代码速度"。
- 知识资产化：Spec 把设计决策从"人脑+一次性 AI 对话"转化为版本化的结构化资产。

### 四个关键实践启示
1. **审查重心前移**：Spec Review 是上游控制，在 Spec 层发现问题修改成本极低。
2. **AGENTS.md 做目录而非百科**：大型 AGENTS.md 有四个系统性问题（挤上下文、过多失效、立即腐烂、难以核实），应控制在约 100 行。
3. **Spec 漂移需主动检测**：Spec 漂移比代码漂移更隐蔽，不会报错只会沉默地让下一个 Agent 基于过时定义生成代码。
4. **追问"AI 还缺什么"**：遇到 Bug 时首要追问不是"怎么让人做对"，而是"AI 完成这件事时还缺什么"。

## 页面触及
- [[../concepts/harness-engineering]]
- [[../concepts/sdd]]
- [[../concepts/agents-md]]
- [[../entities/anthropic]]
