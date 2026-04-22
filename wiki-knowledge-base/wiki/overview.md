# 星辰 AI 应用手册 — Wiki 知识库

**来源**：星辰技术专项组发布的 AI 应用文章与工具指南（2026年2月-4月）  
**维护方式**：LLM 编写并维护，人类提供资料与提问

---

## 这个知识库讲什么

星辰技术专项组围绕 AI 编程工具的**实战应用**与**方法论演进**，整理了两条主线：

### 主线一：工具实践

如何在日常开发中高效使用 AI 编程工具：

- **[[entities/cursor]]** — 功能完整的 AI 编程编辑器，含隐私设置、插件配置、日常实用案例
- **[[entities/trae]]** — 字节发布的国内友好替代，适合零基础入门
- **[[entities/openclaw]]** — 支持通讯平台接入的 AI Agent，含 Multi-Agent 配置
- **[[entities/claude-code]]** — [[entities/anthropic]] 的终端 AI 编程助手，含源码架构与隐藏命令

核心实践概念：[[concepts/token-optimization]] · [[concepts/ai-coding-workflow]] · [[concepts/agent-skills]]

### 主线二：方法论（Harness Engineering）

2026 年最重要的 AI Agent 工程方法论演进：

> **核心公式**：`coding agent = AI model(s) + harness`

三代工程进化（包含关系）：
```
Prompt Engineering (2022-2024)
  ↳ Context Engineering (2025)
      ↳ Harness Engineering (2026)
```

关键概念：
- [[concepts/context-engineering]] — 为 AI 模型动态构建完整上下文的工程方法
- [[concepts/harness-engineering]] — 围绕 Agent 搭建约束、反馈、质量检查的完整运行环境
- [[concepts/planner-generator-evaluator]] — 三角色分离架构，GAN 式解法
- [[concepts/agents-md]] — 知识层核心文件，100 行目录而非百科
- [[concepts/sprint-contract]] — 验收协议，把完成标准变成可验证文件
- [[concepts/multi-agent-architecture]] — 多 Agent 协同工作架构
- [[concepts/subagent]] — 上下文隔离的核心机制
- [[concepts/sdd]] — 规范驱动开发，Harness 的"导航仪"

---

## 核心洞察速查

| 洞察 | 出处 |
|------|------|
| 同一模型只换 Harness，成功率可从 42% → 78% | [[sources/2026-03-harness-engineering]] |
| 上下文过多反而导致模型表现下降 | [[sources/2026-03-harness-what-is-it]] |
| Harness 两阶段：先补后删，随模型变强而减轻 | [[sources/2026-04-harness-from-add-to-delete]] |
| Sub-agent 核心价值是上下文隔离，不是角色分工 | [[sources/2026-04-coding-agent-harness-practice]] |
| 80% 的 agentic 工作流失败在任务设计阶段 | [[sources/2026-04-coding-agent-harness-practice]] |
| 代码健康度 ≥ 9.5/10 时 Agent 准确度约 95%，7.0 时约 60% | [[sources/2026-04-coding-agent-harness-practice]] |
| 通用技能在贬值，封装个人经验的 Skill 是真正护城河 | [[sources/2026-03-agent-skills-6]] |
| 建议 ≠ 约束：prompt 是建议，hooks 是强制执行 | [[sources/2026-03-harness-what-is-it]] |

---

## 如何使用这个 Wiki

- **找工具指南** → 查 `entities/` 下的工具页面
- **找概念解释** → 查 `concepts/` 下的方法论页面
- **找具体文章** → 查 `sources/` 下的原文摘要
- **提问** → 直接问，LLM 会从 wiki 中综合回答并引用来源
