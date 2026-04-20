---
type: concept
tags: [skill, 模块化, 按需加载, workflow, anthropic]
first_seen: "2026-02"
sources:
  - sources/2026-02-token-optimization
  - sources/2026-03-agent-skills-6
  - sources/2026-03-agent-skill-5-modes
  - sources/2026-04-coding-agent-harness-practice
  - sources/2026-03-claude-code-source-analysis
---

# Agent Skills

模块化、可复用的 AI Agent 指令集，用于扩展 Agent 在特定任务领域的能力。

## 定义

Skill 是包含 `SKILL.md` 文件的目录，定义了处理特定任务的详细指令。核心特性：
- **渐进式披露（Progressive Disclosure）**：技能内容只在需要时加载，不常驻上下文。
- **可复用**：一次安装，多个项目使用；拷贝目录即可复用。

## 结构

```
skill-name/
├── SKILL.md          # 主入口，定义任务说明和调度逻辑
├── references/       # 按需加载的参考资料、规范文档
└── assets/           # 素材资源
```

## 五种设计模式（来源：[[../sources/2026-03-agent-skill-5-modes]]）

| 模式 | 解决问题 | 核心机制 |
|------|----------|----------|
| **Tool Wrapper** | 特定框架/SDK 不稳定 | 规则存 `references/`，按需加载 |
| **Generator** | 文档结构不统一 | 模板在 `assets/`，调度流程 |
| **Reviewer** | 审查标准难维护 | Checklist 独立于流程，可替换 |
| **Inversion** | 模型急于生成 | 强制先采访后动手 |
| **Pipeline** | 模型跳过中间验证 | 设 gate condition，不允许跳步 |

## 推荐必装 Skills（来源：[[../sources/2026-03-agent-skills-6]]）

1. **Frontend Design**（Anthropic 官方）— 解决 AI 前端审美问题
2. **办公四件套 docx/xlsx/pdf/pptx**（Anthropic 官方）— 文档处理标准化
3. **Web Access**（@一泽，2000+ star）— 联网搜索 + 浏览器操控
4. **PUA** — 专治 AI 摆烂，四级压力升级
5. **Claude-mem** — 给 Agent 装长期记忆
6. **Skill-Creator** — 元工具，帮你构建个性化 Skill

**核心洞察**：通用技能在贬值，封装个人经验的 Skill 才是真正的护城河。

## 安装（OpenSkills）

```bash
# 安装 OpenSkills CLI
npm install -g openskills

# 安装 Skills（从 Anthropic 市场或 GitHub）
openskills install anthropics/skills

# 同步到 AGENTS.md（告诉 Agent 有哪些 Skill 可用）
openskills sync
```

## 在 Claude Code 中的实现（来源：[[../sources/2026-03-claude-code-source-analysis]]）

`BundledSkillDefinition` 支持：
- frontmatter 元数据（description、allowedTools、model 等）
- `$ARGUMENTS` 参数替换
- 上下文执行模式（inline / fork 子进程）
- MCP 服务器可注册 Skills

大量工具通过 `shouldDefer: true` 标记为延迟加载，模型需要时先调用 ToolSearch 检索。
