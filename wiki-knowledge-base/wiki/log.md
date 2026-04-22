# Log

## [2026-04-20] lint | 知识库健康检查

**已自动修复（4 项）：**
- `index.md` 概念计数错误：10 → 11（`ai-coding-workflow` 漏计）
- `index.md` 来源分组错误：`2026-03-subagent-custom` 从"2026年4月"移至"Claude Code 专题（2026年3月）"
- `concepts/sdd.md` 链接风格不一致：统一为同目录相对链接 `[[harness-engineering]]`
- `overview.md` 遗漏关键概念：补充 `context-engineering` 与 `multi-agent-architecture`

**需要人工处理（2 项，见下）：**
- `entities/anthropic.md` "分层控制哲学"节无来源引用，且"Auto Mode"概念未见于任何 source 页面——疑为 LLM 合成内容混入 entity 页；建议移至 syntheses/ 或找到对应原始资料后补充引用。
- `concepts/sdd.md` 仅有 1 个来源（违反"≥2 来源才建页"规则），请确认是否属于用户显式要求创建。

**无问题项：**死链、孤立页、来源-概念矛盾——均未发现。

## [2026-04-20] init | 星辰-AI应用手册 wiki 初始化

- 来源：`星辰-AI应用手册/` 目录下 18 篇 `.docx` 文档（实战案例 3 篇，工具文档 3 篇，龙虾工作区文章总结 12 篇）
- 创建目录结构：`wiki/{sources,entities,concepts,syntheses,raw}/`
- 创建 AGENTS.md、overview.md、index.md、log.md
- 创建 18 个 sources 页面
- 创建 5 个 entities 页面（claude-code, cursor, openclaw, anthropic, trae）
- 创建 11 个 concepts 页面（harness-engineering, context-engineering, token-optimization, agent-skills, subagent, multi-agent-architecture, planner-generator-evaluator, agents-md, sprint-contract, sdd, ai-coding-workflow）
- 原始 `.docx` 文件保留在 `星辰-AI应用手册/` 目录（对应 `raw/`）
