# Wiki Conventions

这是"星辰-AI应用手册"的 wiki 知识库，整理自星辰技术专项组发布的 AI 应用文章与工具指南。
涵盖 Harness Engineering、Claude Code、Cursor、OpenClaw 等工具的使用实践与方法论。
由 LLM 维护；人类负责提供原始资料和提问，LLM 负责编写并维护所有页面。

## Layout

- `raw/` — 原始资料目录，不可编辑。
- `sources/` — 每篇原始文档对应一个摘要页，写入后不再大幅修改。
- `entities/` — 具名实体页（工具、产品、组织）。随新来源持续补充事实。
- `concepts/` — 概念页（方法论、模式、理论）。随新来源持续丰富。
- `syntheses/` — 跨来源的对比分析与综合解读。
- `index.md` — 所有页面的目录索引。
- `log.md` — 按时间顺序追加的操作记录。
- `overview.md` — 全局综述与入口。

## Naming

- 所有路径使用小写 kebab-case：`entities/claude-code.md`
- 来源文件以摄入日期为前缀：`sources/2026-03-harness-engineering.md`
- slug 应简短明确，消歧时用后缀：`entities/apple-company.md`

## Frontmatter

每个页面使用 YAML frontmatter，按类型必填字段如下：

- **source**: `type`, `title`, `date_ingested`, `medium`, `tags`
- **entity**: `type`, `category`, `tags`, `first_seen`, `sources`
- **concept**: `type`, `tags`, `first_seen`, `sources`
- **synthesis**: `type`, `tags`, `date`, `draws_on`

## Linking

- 所有内部链接使用 `[[relative/path/without-extension]]`。
- 任意页面中某实体或概念的**第一次出现**必须链接。
- 同页后续出现可使用纯文本。
- `entities/` 和 `concepts/` 中的非显而易见的说法必须引用对应 `[[sources/...]]` 页。

## Page Discipline

- **Entities**：仅记录客观事实，观点放入 syntheses。
- **Concepts**：定义、机制、示例，文字精炼。
- **Sources**：不复制原文，只提炼 TL;DR 和核心主张。
- **Syntheses**：明确标注为解读；列出所参考页面。
- **新建页面**：实体或概念仅在 ≥2 个来源中出现，或用户明确要求时才创建独立页。

## Log Format

每条日志以 `## [YYYY-MM-DD] <action> | <subject>` 开头，
使得 `grep "^## \[" log.md` 可输出干净的时间线。

Actions: `init`, `ingest`, `query`, `lint`, `refactor`, `schema-change`

## Linter Rules

每 ~10 次摄入或用户要求时执行 lint：
- 孤立页面（无入链）
- 死链（目标不存在）
- 页面间矛盾
- 被新来源覆盖的过时说法
- 多页频繁出现但缺少独立页面的术语

删除或合并页面前必须获得用户确认。

## Boundaries

LLM 无需征求即可做的事：
- 创建实体/概念/综合页（满足双来源规则或用户要求）
- 为已有页面添加有引用的事实
- 更新 `index.md` 和 `log.md`
- 修复链接和文件名的拼写错误

需要用户确认：
- 删除任何页面
- 合并两个页面
- 重命名已有页面
- 修改本文件中的约定
- 将某项说法标注为矛盾或已过时
